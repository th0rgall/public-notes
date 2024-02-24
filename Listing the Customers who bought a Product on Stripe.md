---
stage: published
---

Stripe is the payments backbone of much of the internet. Since 2021[^1], it also has no-code [Payment Links](https://stripe.com/docs/api/payment_links/payment_links?lang=node) that you can tie to [Products](https://stripe.com/docs/api/products?lang=node). But figuring out which customers bought a certain product is surprisingly non-obvious! 

Here is one method that worked for us, using the Checkout Sessions API.
## Our context
In our case, we were selling tickets for the online documentary premiere of [Women Don't Cycle](https://womendontcycle.com). The ticket was represented by a single Stripe Product, with three different possible Prices and matching Payment Links.

We needed to get the email addresses of the people who bought the documentary, so we could send them an access link some time before the premiere.
## Using the Checkout Session API with Payment Links

The **Checkout Session** API had all that we needed:
- [Checkout Session objects](https://stripe.com/docs/api/checkout/sessions/object?lang=node) have some `customer_details`: name, email, address, phone, ...
- sessions can be [listed and filtered by a Payment Link ID](https://stripe.com/docs/api/checkout/sessions/list?lang=node)

Since we knew exactly which Payment Links could be used to buy the ticket Product, we could derive our target customers from there.

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
          // A limit of 500 in total for each link should suffice.
          .autoPagingToArray({ limit: 500 })
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
In the example above, I still manually referenced the Payment Link IDs. A more automated solution with a Product ID as input could be useful if you want to know the stats for multiple products, or if you have many Payment Links. 

It could work as follows:
1. [List all Payment Links](https://stripe.com/docs/api/payment_links/payment_links/list?lang=node)
2. For each Payment Link, [get its line items](https://stripe.com/docs/api/payment_links/line_items?lang=node)
3. Using the line item info, filter out the Payment Links that apply to the desired product ID(s).
4. Map the Payment Links onto Checkout Session queries, like in the example above.

## Alternatives without payment links

You might have this question as well when you're selling products *without payment links*. In my research for the above, I came the these conclusions:

- Invoices : TODO
- Payment integrated through [Stripe Checkout](https://stripe.com/docs/payments/checkout) but without Payment Links still create checkout sessions, but then you can't figure out the bought products anymore by referring to payment links. I'm not sure how that could be solved!

## Final thoughts

TODO: underwhel
Not as efficient as it can be. Underwhelming dev x for common use case.
Some api's offer searching with filters, here we have to list everything, run multiple queries, unavoidably deal with the N+1 problem, .


[^1]: According to Wikipedia https://en.wikipedia.org/wiki/Stripe,_Inc.#Payment_processing