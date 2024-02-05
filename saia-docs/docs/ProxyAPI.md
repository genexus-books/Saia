---
sidebar_label: 'Proxy API'
sidebar_position: 2
---

# GeneXus Enterprise AI Proxy

GeneXus Enterprise AI Proxy API stands as a transparent proxy for businesses to easily connect to various AI models across different providers, all through a single point of access. With the robust capabilities of GeneXus Enterprise AI Proxy, there is no need to juggle multiple SDKs or APIs because you can access any supported LLM with a single SDK.

**Seamless Access to LLMs**
- One SDK, Multiple Models: with the OpenAI SDK as your single interface, gain immediate access to a myriad of supported LLMs, including those from OpenAI, Azure, Amazon, and Google.
- Transparent Proxy: operates seamlessly in the background, automatically routing your requests to the designated AI models, and without the need for code changes at your end.

**Simplified API Requests**
- Streamlined Authentication: GeneXus Enterprise AI Proxy applies SAIA-specific APITokens eliminating the requirement of individual API keys for each AI provider.
- Universal Request Formatting: keep your API request format consistent, irrespective of the underlying AI model that is accessed.

**Key Advantages**
- Cost and Performance Monitoring: get insights into each model's performance and cost-efficiency without having to integrate multiple monitoring solutions.
- Future-Proof: as we continually add support for more LLMs, your integration remains intact, saving you future development time.

By leveraging the GeneXus Enterprise AI Proxy API, organizations substantially simplify their AI model management with the possibility to focus on business outcomes. Our transparent proxy capabilities enable you to continue scaling and innovating, free from the complex management of multiple AI providers.

### How to use it

To integrate the use of GeneXus Enterprise AI Http Proxy no changes are required at the programming level. Modifications simply imply alterations to two elements in each request:

1. The `BaseURL` used to invoke AI providers must reference the GeneXus Enterprise AI component. The URL format to be used is: 

    ```
    "https://api.saia.ai/proxy/{AIProviderName}/{hostRelativeProviderURL}"
    ```

2. The `Authorization` header present in each request must be replaced with the GeneXus Enterprise AI `APIToken` provided beforehand (without the need to specify the provider invoked).

The request and response received from the call will be the same as when the provider is invoked directly.

#### **Examples:**

1. If the original URL invoked was:

    ```
    "https://api.openai.com/v1/chat/completions"
    ```

    To use GeneXus Enterprise AI Http Proxy, it should be invoked as:

    ```
    "https://api.saia.ai/proxy/openai/v1/chat/completions"
    ```

2. For the original URL:

    ```
    "https://api.replicate.com/v1/predictions"
    ```

    Using GeneXus Enterprise AI Http Proxy, it should be:

    ```
    "https://api.saia.ai/proxy/replicate/v1/predictions"
    ```

3. **cURL specification for OpenAI (traditional):**

    ```shell
    curl https://api.openai.com/v1/completions \
      -H "Content-Type: application/json" \
      -H "Authorization: Bearer $OPENAI_API_KEY" \
      -d '{
        "model": "gpt-3.5-turbo",
        "messages": [{"role": "user", "content": "Hello!"}]
      }'
    ```

   **Using GeneXus Enterprise AI Proxy:**

    ```shell
    curl https://api.saia.ai/proxy/openai/v1/chat/completions \
      -H "Content-Type: application/json" \
      -H "Authorization: $SAIA_APITOKEN" \
      -d '{
        "model": "gpt-3.5-turbo",
        "messages": [{"role": "user", "content": "Hello!"}]
      }'
    ``` 

By following the above steps, you can easily integrate GeneXus Enterprise AI Http Proxy into your application and manage your AI models.


### How to integrate GeneXus Enterprise AI with Third Party SDKs

#### cURL
```shell
curl https://api.openai.com/v1/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -d '{
    "model": "gpt-3.5-turbo",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
```
    
#### OpenAI SDK for Python
```python
import openai
openai.api_key = "$SAIA_APITOKEN"
openai.api_base = "https://api.saia.ai/proxy/openai/v1"

# create a chat completion
chat_completion = openai.ChatCompletion.create(model="gpt-3.5-turbo", messages=[{"role": "user", "content": "Hello world"}])

# print the chat completion
print(chat_completion.choices[0].message.content)
```

#### OpenAI SDK for Typescript
```typescript
const { Configuration, OpenAIApi } = require("openai");

const configuration = new Configuration({
  apiKey: SAIA_APITOKEN,
  basePath: "https://api.saia.ai/proxy/openai/v1",  
});
const openai = new OpenAIApi(configuration);

async function main() {
  const chatCompletion = await openai.createChatCompletion({
    model: "gpt-3.5-turbo",
    messages: [{role: "user", content: "Hello world"}],
  });
  
  console.log(chatCompletion.data.choices[0].message);
}

main();
```
#### Langchain
```typescript
 const model = new OpenAI({
    temperature: options?.llm?.temperature || DefaultLLM.TEMPERATURE, // increase temperature to get more creative answers
    verbose: options?.llm?.verbose || DefaultLLM.VERBOSE,
    cache: options?.llm?.cache || DefaultLLM.CACHE,
    modelName: options?.llm?.modelName || DefaultLLM.MODEL_NAME,
  },
   {
      basePath: process.env.SAIA_API_BASE_URL,
      apiKey: process.env.SAIA_API_KEY
    }
  );
```

### How to call DALL-E-2 and DALL-E-3 to generate images

#### DALL-E-2 - cURL ​

```shell
curl --location 'https://api.saia.ai/proxy/openai/v1/images/generations' \
-H 'Content-Type: application/json' \
-H 'Authorization: $SAIA_APITOKEN" \
-d '{
    "model": "dall-e-2", 
    "prompt": "a halloween pumpkin",
    "size": "1024x1024"
}'
```
#### DALL-E-3 - cURL ​

```shell
curl --location 'https://api.saia.ai/proxy/openai/v1/images/generations' \
-H 'Content-Type: application/json' \
-H 'Authorization: $SAIA_APITOKEN" \
-d'{
    "model": "dall-e-3",
    "quality": "hd", 
    "prompt": "a halloween pumpkin",
    "size": "1792x1024" 
}'
```
There are no specificities in the generation of requests to the GeneXus Enterprise AI Proxy, as compared to the direct requests to OpenAI. Therefore, it might prove useful to query the official documents of OpenAI for obtaining further details: [Image generation](https://platform.openai.com/docs/guides/images/usage).

### How to call GPT-4 to interpret images

#### cURL

```shell
curl https://api.saia.ai/proxy/openai/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: $SAIA_APITOKEN" \
  -d '{
    "model": "gpt-4-vision-preview",
    "messages": [
      {
        "role": "user",
        "content": [
          {
            "type": "text",
            "text": "Describe the content of this image."
          },
          {
            "type": "image_url",
            "image_url": {
              "url": "IMAGE_URL"
            }
          }
        ]
      }
    ],
    "max_tokens": 300
  }'
```
Make sure to replace "IMAGE_URL" with the URL of the image you want to process.

There are no specificities as to image interpretation using gpt-4 in the GeneXus Enterprise AI Proxy API, as compared to doing it directly in OpenAI. Therefore, it might prove useful to query the OpenAI documents available at [Vision](https://platform.openai.com/docs/guides/vision). 
