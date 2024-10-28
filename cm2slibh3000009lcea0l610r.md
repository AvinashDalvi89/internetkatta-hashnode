---
title: "Building Resilient GenAI pipeline with Open-source AI Gateway"
seoTitle: "PortKey AI Gateway: Simplifying Multi-Model Management for Generative "
seoDescription: "Portkey's open-source AI Gateway streamlines workflows, enhances reliability, supports multiple models, and includes failover mechanisms"
datePublished: Mon Oct 28 2024 05:45:54 GMT+0000 (Coordinated Universal Time)
cuid: cm2slibh3000009lcea0l610r
slug: building-resilient-genai-pipeline-with-open-source-ai-gateway
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1730094321106/40a77699-f7ba-485a-b3b4-409c6360f62c.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1730094189638/2d5dfbb6-9353-4ff5-b811-69c71773c752.png
tags: ai, gateway, opensource, model, genai, multimodel-ai, portkey

---

Hello Devs,

Generative AI is rapidly gaining traction, with companies eager to integrate it into their workflows and drive product innovation. As its popularity soars, numerous LLM models and Generative AI companies are emerging. However, this surge in interest brings its own set of challenges. Companies face difficulties in managing large models on their infrastructure, caching results, handling credentials, and dealing with new-age queries (prompts). Most critically, they struggle with managing a diverse array of AI models, each with its unique structure and communication format. This complexity makes it challenging to establish failover mechanisms and efficiently switch between models, leading to significant time and resource investments.

## Introducing Portkey open-source AI Gateway: 

[Portkey AI Gateway](http://portkey.ai) offers an open-source AI Gateway that simplifies managing generative AI workflows. This powerful tool supports working with multiple large language models (LLMs) from various providers, along with different data formats (multimodal).  By acting as a central hub, Portkey streamlines communication and simplifies integration, improving your application's reliability, cost-effectiveness, and accuracy. 

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXcVHdrGL_aWpuuE7-e612P3_CVhah5-EOjX490wwZT5UmJuUxBfjLohrtg3gaNf8vl6LFX6OeoVPQSqbciSsSC9zx0PNh_VPMqNokjw27GrT27Zkzf8bUhCpj2_qifmbPUcljGWkTUnb5w9bg2ukIqKBLc?key=_l91YjqQx1jxUIzcPo4ceQ align="left")

## Getting Started with PortKey AI Gateway

Getting started with PortKey AI Gateway involves very simple steps.

1. Open this [https://github.com/Portkey-AI/gateway](https://github.com/Portkey-AI/gateway) and follow instructions or run the below command in your terminal. Make sure your NodeJs is installed in your machine. 
    
2. Hit [http://localhost:8787](http://localhost:8787) in the browser. 
    

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdPS7O5cTXCLsPSQjm4sTqlHRPSBQl70OF_zqxoUH5jXLJs9WWxp78UlaOy8RJTcKSiroMlUYUAeYRUziWJUYDmXJkiTT_3JPxqKUtbY7hA15f2XIWw7st2tZGRqBNRfexPtSrUb3XmcbcaHxR-M-FB9bA?key=_l91YjqQx1jxUIzcPo4ceQ align="left")

3. That’s it! This server is up, the next step is to test out a Large Language Model (LLM). You can pick up any LLM from the [list](https://github.com/Portkey-AI/gateway?tab=readme-ov-file#supported-providers.) 
    

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdNWPo6ChtrhMXsZ0DMHam_yvsLVGp0n0LeB_rB2BqMprGOb9CXcGHP5ODq4X-Vq9a8Yy8M_JXl9YNSvwowrNQJ6tDcAqiuJFdA_0ygOT3SPXTeq8ZX7cWf1VIL-XLXgHSV8_GMnQU_TEWr55HT3cejGOw-?key=_l91YjqQx1jxUIzcPo4ceQ align="left")

In this blog, I will explain how to use Google PaLM and Gemini model as an example to use with PortKey gateway. 

## Google PaLM and Gemini with Portkey AI Gateway

If you already know how to get the API key then skip these steps. 

1. Sign up using google account here [https://aistudio.google.com](https://aistudio.google.com). And click on “Create new key” on the left navigation menu.  
    
2. You can choose any AI model from [here](https://ai.google.dev/tutorials/rest_quickstart) 
    

To call PortKey Gateway you need two things: 

* Your AI model API key or secret key ( Google AI Studio API key ) 
    
* Model name which you would like to test. 
    

PortKey currently supports AI model API hardcoded versions like Google gemini version supported is v1beta and Google Palm supported is v1beta3. A feature request is under process. So you need to find a model which is supported under those API versions, otherwise it will throw an error “Model doesn’t support under API version”. 

![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdHVgLG2f_kQgLu7P7FThBMPELxUZJ9_gFiiTpejZtrv8uU1LEQB03p0vajFyCagYmHOx9wvsB-RBX8_FYSX64fKDVUkGlpHp1BxhhtTMRM-r5JQnY8t-HqoMxwKUvbIqktja8Kx-ahzFWR8GAW8ZUZ7cYB?key=_l91YjqQx1jxUIzcPo4ceQ align="left")

As per this reference document [https://ai.google.dev/tutorials/rest\_quickstart](https://ai.google.dev/tutorials/rest_quickstart) Google gemini model supported for `generateContent` is `gemini-pro`. We will test our `/generateContent` AI API model. This standard example is to call a CURL request to PortKey Gateway. 

```bash
curl '127.0.0.1:8787/v1/chat/completions' \
  -H 'x-portkey-provider: openai' \
  -H "Authorization: Bearer $OPENAI_KEY" \
  -H 'Content-Type: application/json' \
  -d '{"messages": [{"role": "user","content": "Say this is test."}], "max_tokens": 20, "model": "gpt-4"}'
```

Here is an API request for Google Gemini AI model : Replace **$GOOGLE\_KEY** with your actual key.   

```bash
curl --location '127.0.0.1:8787/v1/chat/completions' \
--header 'x-portkey-provider: google' \
--header 'Authorization: Bearer $GOOGLE_KEY' \
--header 'Content-Type: application/json' \
--data '{
    "messages": [
        {
            "role": "user",
            "content": "Write a story about a magic backpack."
        }
    ],
    "model": "gemini-pro"
}'
```

Exactly similarly you can use other AI models like OpenAI, Ollama etc with just model name and AI API key. 

Now you have understood how to use this PortKey Gateway API with different AI models. Lets checkout  how you can developed resilient GenAI pipeline with PortKey AI Gateway 

I will cover major features of PortKey support to make your GenAI pipeline resilient. 

### Build failover mechanism with PortKey AI Gateway 

As we saw early in blog fallback is one feature of PortKey gateway which supports adding a list of AI Model API if incase any one of them or primary API fails. In this list you can add numbers of AI model AI. 

In the same REST call under header against `x-portkey-config`  you can add these params to add a list of LLM models API along their API keys so that if the primary key fails it will take the second one. Here's a quick example of a config to fallback to Anthropic's claude-v1 if Gemini’s “gemini-pro” fails.

```json
{
  "strategy": {
      "mode": "fallback",
  },
  "targets": [
    {
      "virtual_key": "google-virtual-key",
    },
    {
      "virtual_key": "anthropic-virtual-key",
      "override_params": {
          "model": "claude-1"
      }
    }
  ]
}
```

Here is example CURL request to PortKey AI Gateway with config params  : 

```bash
curl --location '127.0.0.1:8787/v1/chat/completions' \
--header 'x-portkey-provider: google' \
--header 'Authorization: Bearer $GOOGLE_KEY' \
--header 'Content-Type: application/json' \
--header 'x-portkey-config: {"strategy":{"mode":"fallback"},"targets":[{"provider":"google","api_key":"$GOOGLE_KEY"},{"provider":"openai","api_key":"sk-***"}]}' \
--data '{
    "messages": [
        {
            "role": "user",
            "content": "Write a story about a magic backpack."
        }
    ],
    "model": "gemini-pro"
}'
```

This way you can have a fallback mechanism in your AI pipeline. In this config you set on which status code of failing API should have fallback by just adding under “strategy”

```json
"strategy": {
    "mode": "fallback",
    "on_status_codes": [ 429 ]
  }
```

**Note** : You need to ensure while using this fallback mechanism that the LLMs in your fallback list are compatible with your use case. Not all LLMs offer the same capabilities.

Similarly you can have more capabilities to make sure your GenAI pipeline is resilient and stable. Like adding auto retry mechanism, caching and load balancing. Same can be configured under “config” parameters. More details given [here](https://docs.portkey.ai/docs/product/ai-gateway-streamline-llm-integrations).

This way PortKey AI gateway simplifies managing generative AI workflows by offering multi-model support and easy integration. This translates to improved app performance through features like automatic failover and efficient model switching.

So give it a try with PortKey AI gateway and make your GenAI pipeline resilient. Let us know if you have any query. 

## References 

* More details about feature and how to use : [https://docs.Portkey AI Gateway/docs/product/ai-gateway-streamline-llm-integrations](https://docs.portkey.ai/docs/product/ai-gateway-streamline-llm-integrations)
    
* [https://github.com/Portkey-AI/gateway/blob/27a6ebbc9ddd972ef1a716176fc17d0b2671c366/src/providers/google/api.ts#L4C22-L4C70](https://github.com/Portkey-AI/gateway/blob/27a6ebbc9ddd972ef1a716176fc17d0b2671c366/src/providers/google/api.ts#L4C22-L4C70)
    
* [https://github.com/Portkey-AI/gateway/blob/27a6ebbc9ddd972ef1a716176fc17d0b2671c366/src/providers/palm/api.ts#L4C64-L4C71](https://github.com/Portkey-AI/gateway/blob/27a6ebbc9ddd972ef1a716176fc17d0b2671c366/src/providers/palm/api.ts#L4C64-L4C71)
    
* PortKey Config : https://docs.Portkey AI Gateway/docs/api-reference/config-object
    
* [https://docs.portkey.ai/docs/product/ai-gateway-streamline-llm-integrations/fallbacks](https://docs.portkey.ai/docs/product/ai-gateway-streamline-llm-integrations/fallbacks)
    
* How to add JSON object in header Postman API:  https://community.postman.com/t/get-custom-header-value-as-object-json-stringify/16172