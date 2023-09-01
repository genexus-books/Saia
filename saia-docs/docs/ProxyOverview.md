---
sidebar_label: 'Proxy Overview'
sidebar_position: 2
---

## SAIA Proxy AI API

SAIA Proxy AI API stands as a transparent proxy that allows businesses to easily connect to various AI models across different providers, all through a single point of access. With the robust capabilities of SAIA Proxy AI, you don't need to juggle multiple SDKs or APIs; you can access any supported LLM with a single SDK.

**Seamless Access to LLMs**
- One SDK, Multiple Models: With the OpenAI SDK as your single interface, gain immediate access to a myriad of supported LLMs, including those from OpenAI, Azure, Amazon, and Google.
- Transparent Proxy: Operates seamlessly in the background, automatically routing your requests to the designated AI models without requiring any code changes on your end.

**Simplified API Requests**
- Streamlined Authentication: SAIA Proxy AI employs SAIA-specific APITokens, negating the need for individual API keys for each AI provider.
- Universal Request Formatting: Keep your API request format consistent, irrespective of the underlying AI model being accessed.

**Key Advantages**
- Cost and Performance Monitoring: Get insights into each model's performance and cost-efficiency without having to integrate multiple monitoring solutions.
- Future-Proof: As we continually add support for more LLMs, your integration remains intact, saving you future development time.

By leveraging the SAIA Proxy AI API, organizations can substantially simplify their AI model management and focus more on driving business outcomes. Our transparent proxy capabilities ensure that you can continue to scale and innovate without the complexities often associated with managing multiple AI providers.

### How to use it?

Incorporating the use of SAIA Http Proxy does not require programming-level changes. The modifications simply involve altering two elements present in each request:

1. The `BaseURL` used to invoke AI providers should now reference the SAIA component. The URL format to be used will be: 

    ```
    "https://api.saia.ai/proxy/{AIProviderName}/{hostRelativeProviderURL}"
    ```

2. The `Authorization` header present in each request should be replaced with the SAIA `APIToken` provided beforehand (without the need to specify the provider being invoked).

The request and response received from the call will be the same as if directly invoking the provider.

#### **Examples:**

1. If the original URL being invoked was:

    ```
    "https://api.openai.com/v1/chat/completions"
    ```

    To use SAIA Http Proxy, it should be invoked as:

    ```
    "https://api.saia.ai/proxy/openai/v1/chat/completions"
    ```

2. For the original URL:

    ```
    "https://api.replicate.com/v1/predictions"
    ```

    Using SAIA Http Proxy, it should be:

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

   **Using SAIA Proxy AI:**

    ```shell
    curl https://api.saia.ai/proxy/openai/v1/chat/completions \
      -H "Content-Type: application/json" \
      -H "Authorization: $SAIA_APITOKEN" \
      -d '{
        "model": "gpt-3.5-turbo",
        "messages": [{"role": "user", "content": "Hello!"}]
      }'
    ``` 

By following the above steps, you can easily integrate SAIA Http Proxy into your application and manage your AI models with ease.


### How to integrate SAIA with Third Party SDKs

#### cURL
```curl
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

