---
stage: published
created: 2024-02-25
title: Listing the customers who bought a product on Stripe
---
Stripe is the payments backbone of much of the internet. Since 2021[^1], it also has no-code [payment links](https://stripe.com/docs/api/payment_links/payment_links?lang=node) that you can tie to [products](https://stripe.com/docs/api/products?lang=node). But figuring out which customers bought a certain product is surprisingly non-obvious!

This API, that you might naively expect to exist, **doesn't exist**:
```
GET https://api.stripe.com/v1/products/:id/customers — list customers who bought the product with the given id
```

In what follows, I'll describe two different cases where I've answered this question. 

First, a simple case, applicable only to products sold through payment links. Last, a more general approach.

## Case 1: Using the Checkout Session API with known Payment Links

### Context
In this case, we were selling tickets for the online documentary premiere of [Women Don't Cycle](https://womendontcycle.com). The ticket was represented by a single Stripe *product*, with three different possible *prices* and matching *payment links* for each price.

We needed to get the email addresses of the people who bought the documentary, so we could send them an access link some time before the premiere.

To leverage this method, it is important that you only want to list customers for products sold through payment links.
### Method

Purchases through a Payment Link will have inevitably created a Checkout Session.

The Checkout Session API had all that we needed to retrieve the customers that bought a product:
- sessions can be [listed and filtered by a Payment Link ID](https://stripe.com/docs/api/checkout/sessions/list?lang=node) request parameter.
- [Checkout Session objects](https://stripe.com/docs/api/checkout/sessions/object?lang=node) have some `customer_details`: name, email, address, phone, ...


Since we knew exactly which payment links could be used to buy the ticket product, we could derive our target customers from there.

Here is a Deno TypeScript snippet using the Stripe API:

```ts
// Payment links
const WDC_10_PLINK = "plink_someidA";
const WDC_15_PLINK = "plink_someidB";
const WDC_CHOICE_PLINK = "plink_someidC";

async function main() {
  const sessions =
    // Fetch all Checkout Sessions for each Payment Link
    (await Promise.all(
      [WDC_10_PLINK, WDC_15_PLINK, WDC_CHOICE_PLINK].map((payment_link) =>
        // https://stripe.com/docs/api/checkout/sessions/list?lang=node
        stripe.checkout.sessions.list(
          {
            payment_link,
          },
        )
          // https://github.com/stripe/stripe-node#autopagingtoarray
          // A limit of 1000 in total for each link should suffice.
          .autoPagingToArray({ limit: 1000 })
      ),
    ))
      .flat()
      // Filter out the sessions that succeeded and were paid.
      .filter(({ payment_status, status }) =>
        (payment_status === "no_payment_required" ||
          payment_status === "paid") &&
        status === "complete"
      );

  await Deno.writeTextFile(
    "./wdc-customers.json",
    JSON.stringify(sessions, null, 2),
  );
}
```

This exported all the session data of relevant customers for further processing.

### With the Product ID(s) as input
In the example above, I still manually referenced the payment link IDs. A more automated solution with a product ID as input could be useful if you want to know the stats for multiple products, or if you have many payment links. 

It could work as follows:
1. [List all payment links](https://stripe.com/docs/api/payment_links/payment_links/list?lang=node)
2. For each payment link, [get its line items](https://stripe.com/docs/api/payment_links/line_items?lang=node)
3. Using the line item info, filter out the payment links that apply to the desired product ID(s).
4. Map the Payment Links onto Checkout Session queries, like in the example above.
## Case 2: Listing all sessions or transactions, then filtering down

This solution is more generally applicable, but also more involved.

The idea is to list **all** Checkout Sessions within a period with the [expandable `line_items`](https://docs.stripe.com/api/checkout/sessions/object#checkout_session_object-line_items) property, and then: 
1. filter the sessions by product based on their `line_items`.
2. deriving the `customer_details` from the filtered sessions.

To get the filtered Checkout Sessions, you can loop through the [list API](https://docs.stripe.com/api/checkout/sessions/list), only using the `created` time-frame request parameter.
### "Special" case: subscriptions, Invoices & Web Elements

We also have a subscription product that is *not paid via a Payment Link or hosted Stripe Checkout*, but via Web Elements which directly link to a subscription invoice's Payment Intent.

In this case, I don't think Checkout Sessions are created, which means the above method can't be used. I haven't tried, but it's probably possible to [list all Invoices](https://docs.stripe.com/api/invoices/list) instead, which also come with a [`lines` property](https://docs.stripe.com/api/invoices/object#invoice_object-lines) that can be used to match the bought subscription products. 

### Getting a monthly breakdown of all product sales, using Reports

The ultimate test of the Stripe APIs was a small project we did recently to automate some of our accounting: we wanted to have a PDF for each product, with a table of transactions related to that product, monthly.

I could have used the above mentioned methods for listing Checkout Sessions and Invoices, and then derived product sales (and customers) from that, but I wanted something that would catch *all* transactions with more certainty. It turns out to be possible.

**Reports & Report Runs**

In Stripe, you can request an ["Itemized balance change from activity"](https://stripe.com/docs/reports/report-types/balance#schema-balance-change-from-activity-itemized-3) report for a month, either manually, or with the [Report Runs API](https://docs.stripe.com/api/reporting/report_run). It includes a `payment_intent_id` (and `invoice_id`, if applicable) for each product-related transaction, but also negative Stripe fee rows, without such IDs.

While the API seems well-constructed, I found its response time unreliable: sometimes it finished a report within seconds, sometimes only within an hour. Using time-capped, simple, budget-friendly cloud functions in such a situation meant polling for report completion was only possible with a database and a rather ugly function scheduler, or a more elegant webhook-based solution that kept track of the progress of a report. Both options seemed overengineered.

We concluded that manually requesting the report `.csv` in the Dashboard, and getting it emailed (or downloading it immediately) was the simpler approach to acquire the transaction data monthly. It's even possible to schedule automated reports in the Dashboard itself this way.

We then upload this `.csv` to a cloud function, which processes it:
- It ignores negative transactions (costs)
- For rows with an `invoice_id`, it gets additional product & invoice information with the Invoice API, as described above.
- For rows without an `invoice_id`, but with a `payment_intent_id`, it requests Checkout Sessions for the payment intent ID, and then filters the response to the completed ones, to get to the product information for the row eventually.

With this, each row is enriched with product information, and the data can be further processed to categorize transactions by product.


[^1]: According to Wikipedia https://en.wikipedia.org/wiki/Stripe,_Inc.#Payment_processing