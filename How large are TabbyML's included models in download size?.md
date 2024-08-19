---
created: 2024-08-19
stage: published
---
I just installed and started playing around with TabbyML on my M1 Pro MBP with 32GB RAM. Count me impressed! So far the suggestions and chat feel as good, and as shitty, as my previous trial of Github Copilot; however, TabbyML is open-source, runs locally and does not require a 4$/mo subscription. 

It was a breeze to install and set up, which was perhaps the most impressive part. I started with the default code completion model `StarCoder-1B` and chat model `Qwen2-1.5B-Instruct` from the [brew quick start instructions](https://tabby.tabbyml.com/docs/quick-start/installation/apple/); however I quickly became curious if I could run more powerful models on my machine as well, since the [models page](https://tabby.tabbyml.com/docs/models/) suggested so.

This also made we wonder; **how big are all these models?** Not in terms of billions of parameters, but in **gigabytes of download and storage size**. I'm stuck with a 512GB SSD and can't just slap anything I want on it.

I couldn't easily find the answer; so I wrote a script to generate this table. Maybe it answers someone else's question too.
## The download size of TabbyML's suggested models
...as of August 19th, 2024.

| Model name              | Size in bytes |
| ----------------------- | ------------- |
| StarCoder-1B            | 1.4G          |
| StarCoder-3B            | 3.4G          |
| StarCoder-7B            | 8.1G          |
| StarCoder2-3B           | 3.3G          |
| StarCoder2-7B           | 7.7G          |
| CodeLlama-7B            | 7.2G          |
| CodeLlama-13B           | 13.9G         |
| Mistral-7B              | 7.7G          |
| DeepseekCoder-1.3B      | 1.5G          |
| DeepseekCoder-6.7B      | 7.2G          |
| CodeGemma-2B            | 2.7G          |
| CodeGemma-7B            | 9.1G          |
| CodeGemma-7B-Instruct   | 9.1G          |
| CodeQwen-7B             | 7.8G          |
| Qwen2-1.5B-Instruct     | 1.7G          |
| CodeQwen-7B-Chat        | 7.8G          |
| Codestral-22B           | 23.7G         |
| Nomic-Embed-Text        | 146.2M        |
| DeepSeek-Coder-V2-Lite  | 16.8G         |
| Jina-Embeddings-V2-Code | 172.9M        |
## The script to generate the above table

Here's the script I used to generate it. The included models and their download URLs can be found in `~/.tabby/models/TabbyML/models.json`. The script uses `curl` to get the `Content-Length` headers from the URLs, and formats the whole as Markdown table fragments which I could include here: 

```bash
#!/bin/bash

for JSON_OBJECT in $(cat ~/.tabby/models/TabbyML/models.json | jq -c '.[] | {name: .name, url: .urls[0]}');
do
  URL=$(jq --raw-output '.url' <<< "$JSON_OBJECT")

  # Get only the Content-Length header value of the final request after redirects.
  # See: https://stackoverflow.com/questions/3074288/get-final-url-after-curl-is-redirected#3077316
  BYTES=$(curl -sLI -o /dev/null -w '%header{Content-Length}' -I "$URL")

  # numfmt is part of gnu coreutils; available via brew in macOS
  FORMATTED=$(numfmt --to=si --format %.1f "$BYTES")

  #
  printf "| %s | %s |\n" "$(jq --raw-output '.name' <<< "$JSON_OBJECT")" "$FORMATTED"
done
```

Credit where it is due: this script came together with the help of `CodeGemma-7B-Instruct` (how meta), StackOverflow, [`tldr`](https://tldr.sh/) and good old man pages. Bash and the particulars of  `jq`, `numfmt` and `curl` are regrettably not yet committed to my muscle memory.