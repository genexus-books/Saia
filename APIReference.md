# API Reference

This section explain in detail the different API layers that are exposed to integrate in your applications.

## GBrain Proxy AI API

GBrain Proxy AI is an observability tool designed for businesses that interact with AI. It provides a unified view of models, enabling traceability of their usage. It is easy to use and can be integrated with multiple AI providers, allowing centralized access without the need for specific API keys for each provider.

### What is it used for?

With minimal changes to the traditional invocation of each provider's APIs, this component eliminates the need for specific API keys for each provider, streamlining the usage through a GBrain-specific APIToken. These APITokens are linked at the organization and project levels, allowing, among other things, the tracking of usage costs at those levels.

Some of its features include:

- Request logging: GBrain Http Proxy generates logs of all requests sent to AI providers, including the inputs and outputs.
- Request latency: The time consumed in responses is measured, providing valuable information for understanding the performance of the used models and identifying issues early on.
- Cost monitoring: The tool records the usage costs of each AI model.

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
