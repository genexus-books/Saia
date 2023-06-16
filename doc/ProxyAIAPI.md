## GBrain Proxy AI API

GBrain Proxy AI is a powerful observability tool designed for businesses that leverage artificial intelligence. It allows organizations to gain a unified view of all their AI models, ensuring full traceability of their usage and providing valuable metrics to improve AI performance over time. 

With minimal adjustments to existing API requests, this tool eliminates the need for individual API keys for all providers. Instead, it utilizes GBrain-specific APITokens that are managed at the organization and project levels, providing seamless integration with multiple AI providers and simple centralized access.

Some of the key features of GBrain Proxy AI include:

- Request logging: GBrain Proxy AI generates comprehensive logs detailing all requests sent to AI providers, including both inputs and outputs.
- Request latency: The response time for each request is measured, providing valuable insights for understanding performance and identifying potential issues as early as possible.
- Cost monitoring: With GBrain Proxy AI organizations can easily track usage costs at an organization and project level, so that they can monitor and optimize their AI workloads.

By leveraging the powerful features provided by GBrain Proxy AI, organizations can dramatically improve their AI workflows, optimize costs, and ultimately drive greater business success

### How to use it?

Incorporating the use of GBrain Http Proxy does not require programming-level changes. The modifications simply involve altering two elements present in each request:

The BaseURL used to invoke AI providers should now reference the GBrain component. The URL format to be used will be: "https://beta.pia.genexus.dev/api/{AIProviderName}/{hostRelativeProviderURL}"

The Authorization header present in each request should be replaced with the GBrain APIToken provided beforehand (without the need to specify the provider being invoked).

The response received from the call will be the same as if directly invoking the provider.

Example 1: If the original URL being invoked was:

"https://api.openai.com/v1/chat/completions"

To use GBrain Http Proxy, it should be invoked as:

"https://beta.pia.genexus.dev/api/openai/v1/chat/completions"

Example 2: For the original URL:

"https://api.replicate.com/v1/predictions"

Using GBrain Http Proxy, it would be:

"https://beta.pia.genexus.dev/api/replicate/v1/predictions"

Example 3: cURL specification for OpenAI (traditional):
```
curl https://api.openai.com/v1/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $OPENAI_API_KEY" \
  -d '{
    "model": "gpt-3.5-turbo",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
  ```
  
  Using GBrain Proxy AI
  ```
  curl https://beta.pia.genexus.dev/api/openai/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: $GBRAIN_APITOKEN" \
  -d '{
    "model": "gpt-3.5-turbo",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
  ```
