Table of contents
=================

* [Overview](#gbrain-the-ai-brain-for-your-enterprise-applications)
* [Playground](#playground)
* [API Reference](#api-reference)
* [Backoffice](#backoffice)


# GBrain: the AI Brain for Your Enterprise Applications

Enterprise applications utilizing Large Language Models (LLMs) as a foundation are now a reality. These AI applications, designed to be integrated into production environments, require a set of essential non-functional and functional characteristics for business applications. 

![image](https://github.com/genexuslabs/GBrain/assets/33163715/bd34114d-e6cf-4354-a0ee-3eb4e4b25942)

## Non-functional Characteristics 
- Observability 
- Maintainability, future proofing 
- Security 
- Scalability
- Accountability

## Functional Characteristics 
- Search & Chat for private documentation 
- Intent-based navigation for existing user interfaces 
- Assistants for power autocomplete 
- Definition and chaining of assistants and actuators 
- Autonomous or semi-autonomous execution agents 

From a process perspective, it is also crucial to consider development elements such as versioning assistants, testing assistants, and deploying them. 

GBrain was designed with an architecture of multiple logical layers which can be accessed independently and incrementally.

So you can just start by just consuming some particular model, but then conceptualize those accesses as assistants and then as a use case.

![image](https://github.com/genexuslabs/GBrain/assets/33163715/aeff6b5a-8bec-45aa-a68a-937539a194df)

# What problems does GBrain solve with a unique time to market? 


+ Centralized data and cost observability 
+ Development of POCs (Proof Of Concept) or products that quickly “cognifying” business, reducing costs or time for a specific business area 
+ Time to market for AI solutions in a safe and scalable manner 
+ Canonical use cases for AI solutions: 
  - Intent-based navigation 
  - Search & Chat 
  - Autocomplete 
  - Business assistant-based processing conceptualization 

## Observability 

It is essential to understand the information lifecycle and be able to measure various business indicators regarding the use and costs of the AI models being utilized. 
Business applications need to understand which business domains are using which AI models. 

## Maintainability, Future Proofing 

AI models are evolving rapidly. Can businesses keep up with the pace? Can our interfaces fluctuate and test each model without breaking connection interfaces? How can we become independent of these decisions, often tied to cost, privacy, or other factors? 
GBrain allows for the independence of created interfaces from the accessed models, enabling agents to evolve with a business perspective, separate from the underlying AI models' evolution. 

## Security 

Businesses require access security, data security, data governance, and alerts for potential rule breaches. GBrain provides everything needed to achieve control over what happens with data within the company, ensuring data travels securely and adheres to business-imposed rules. 

## Scalability 

Performance should not degrade with business success. Having infrastructure designed for secure and scalable communication with models is essential when deploying products in production. Using GBrain ensures that if the business succeeds, scalability concerns are addressed. 

## Built-in Patterns Solved 

GBrain provides all the services and abstractions necessary for implementing emerging UX patterns. It offers the following services: 

- Search & Chat creation for any document set, essentially building a private ChatGPT for unstructured company documents 
- Assistant creation and chaining for various business objectives 
- Autocomplete assistants 
- Summarization assistants 

## Testing your assistants

GBrain allows you to version all your assistants, this is a great feature that allows you to evolve assistants without breaking things.
You need to be aware that programming with non-deterministic agents could be a challenge without help.

By using GBrain you can declare different versions of your assistants and start enabling different access to them depending on the consumer.

You can create a complete regression test suite with the new version of a new assistant before entering production. Actually, you can match your current staging planning in order to evolve your solution with confidence.

## Access to GBrain 

Access to GBrain's various layers is generally programmable using any programming language. Access is granted via access tokens per project or organization. 
In addition to programmatic interfaces, you can quickly create Playgrounds for demonstrating AI technology capabilities. 

# Playground

This playground allows you to try the built-in assistant. 

For example [here](https://apps-angular.genexus.com/Id5902578315db0d0929d5bab63eb6ff79/Chat_ChatPanel/Chat/ChatPanel-Level_Detail) you can test the Search and Chat assistant that has been ingested with documents about political history from Uruguay and also transit rules applied in Uruguay.


# API Reference

This section explain in detail the different API layers that are exposed to integrate in your applications.

## GBrain HTTP Proxy API

GBrain Http Proxy is an observability tool designed for businesses that interact with AI. It provides a unified view of models, enabling traceability of their usage. It is easy to use and can be integrated with multiple AI providers, allowing centralized access without the need for specific API keys for each provider.

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
  
  Using GBrain HTTP Proxy
  ```
  curl https://beta.pia.genexus.dev/api/openai/v1/chat/completions \
  -H "Content-Type: application/json" \
  -H "Authorization: $GBRAIN_APITOKEN" \
  -d '{
    "model": "gpt-3.5-turbo",
    "messages": [{"role": "user", "content": "Hello!"}]
  }'
  ```

# Backoffice
