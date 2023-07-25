## Saia Proxy AI API

Saia Proxy AI is a powerful observability tool designed for businesses that leverage artificial intelligence. It allows organizations to gain a unified view of all their AI models, ensuring full traceability of their usage and providing valuable metrics to improve AI performance over time. 

With minimal adjustments to existing API requests, this tool eliminates the need for individual API keys for all providers. Instead, it utilizes Saia-specific APITokens that are managed at the organization and project levels, providing seamless integration with multiple AI providers and simple centralized access.

Some of the key features of Saia Proxy AI include:

- Request logging: Saia Proxy AI generates comprehensive logs detailing all requests sent to AI providers, including both inputs and outputs.
- Request latency: The response time for each request is measured, providing valuable insights for understanding performance and identifying potential issues as early as possible.
- Cost monitoring: With Saia Proxy AI organizations can easily track usage costs at an organization and project level, so that they can monitor and optimize their AI workloads.

By leveraging the powerful features provided by Saia Proxy AI, organizations can dramatically improve their AI workflows, optimize costs, and ultimately drive greater business success.

### How to use it?

Incorporating the use of Saia Http Proxy does not require programming-level changes. The modifications simply involve altering two elements present in each request:

1. The `BaseURL` used to invoke AI providers should now reference the Saia component. The URL format to be used will be: 

    ```
    "https://api.beta.saia.ai/proxy/{AIProviderName}/{hostRelativeProviderURL}"
    ```

2. The `Authorization` header present in each request should be replaced with the Saia `APIToken` provided beforehand (without the need to specify the provider being invoked).

The request and response received from the call will be the same as if directly invoking the provider.

#### **Examples:**

1. If the original URL being invoked was:

    ```
    "https://api.openai.com/v1/chat/completions"
    ```

    To use Saia Http Proxy, it should be invoked as:

    ```
    "https://api.beta.saia.ai/proxy/openai/v1/chat/completions"
    ```

2. For the original URL:

    ```
    "https://api.replicate.com/v1/predictions"
    ```

    Using Saia Http Proxy, it should be:

    ```
    "https://api.beta.saia.ai/proxy/replicate/v1/predictions"
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

   **Using Saia Proxy AI:**

    ```shell
    curl https://api.beta.saia.ai/proxy/openai/v1/chat/completions \
      -H "Content-Type: application/json" \
      -H "Authorization: $SAIA_APITOKEN" \
      -d '{
        "model": "gpt-3.5-turbo",
        "messages": [{"role": "user", "content": "Hello!"}]
      }'
    ``` 

By following the above steps, you can easily integrate Saia Http Proxy into your application and manage your AI models with ease.


### How to integrate Saia with Third Party SDKs

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
openai.api_base = "https://api.beta.saia.ai/proxy/openai/v1"

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
  basePath: "https://api.beta.saia.ai/proxy/openai/v1",  
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

