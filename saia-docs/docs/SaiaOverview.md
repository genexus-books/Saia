---
sidebar_label: 'Overview'
sidebar_position: 1
slug: /
---
# SAIA: the Spine of AI  Applications

## What is SAIA?

SAIA is an enterprise platform designed to facilitate the implementation of AI-Assistants tailored to your specific needs and areas of expertise.

You can create AI-Assistants that can integrate and interact with your current operations, processes, systems and documents, creating new paths of innovation and productivity to explore.

One of the great benefits of using SAIA is the ability to select a Large Language Model (LLM) and later switch to another without changing your definitions. Thus, SAIA acts as a secure bridge, connecting enterprise applications to LLMs while providing a variety of tools and features to boost productivity and innovation.

Since SAIA is an intermediate layer, anyone who works with it is protected because the data will not be made public or used by the LLMs. 

To facilitate the integration of generative AI into your workflow, SAIA offers features that enable you to:
* Monitor access to LLMs through customized authentication and authorization protocols.
* Provide a Web interface with a look and feel that is familiar to Generative AI users branded and managed by your organization. 
* Supervise the costs and interactions associated with each Gen-AI Solution for streamlined analysis and control.
* Manage quotas per solution to keep your spending in check.

Further, for those looking to integrate AI capabilities into their custom corporate software, SAIA provides the ability to:
* Automatically generate APIs and version the assistants and prompts you create using our Prompt Editor. 
* Optimize your AI exploration by effortlessly switching between different LLMs.
* Reduce dependency between the AI applications you develop and their underlying LLMs.
* Leverage pre-built AI functions, like 'Chat with your Documents' and 'Chat with your Data,' to accelerate your AI adoption journey.


These AI applications, designed to be integrated into production environments, require a set of essential non-functional and functional characteristics for business applications. 


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

SAIA was designed with an architecture of multiple logical layers which can be accessed independently and incrementally.

So you can just start by just consuming some particular model, but then conceptualize those accesses as assistants and then as a use case.

![image](../assets/images/GBrain-FunctionalCharacteristics.png)

# What problems does SAIA solve with a unique time to market? 

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
SAIA allows for the independence of created interfaces from the accessed models, enabling agents to evolve with a business perspective, separate from the underlying AI models' evolution. 

## Security 

Businesses require access security, data security, data governance, and alerts for potential rule breaches. SAIA provides everything needed to achieve control over what happens with data within the company, ensuring data travels securely and adheres to business-imposed rules. 

## Scalability 

Performance should not degrade with business success. Having infrastructure designed for secure and scalable communication with models is essential when deploying products in production. Using SAIA ensures that if the business succeeds, scalability concerns are addressed. 

## Built-in Patterns Solved 

SAIA provides all the services and abstractions necessary for implementing emerging UX patterns. It offers the following services: 

- Chat with any document set, essentially building a private ChatGPT for unstructured company documents 
- Assistant creation and chaining for various business objectives 
- Autocomplete assistants 
- Summarization assistants
- Offer a private instance to chat with LLMs 

## Testing your assistants

SAIA allows you to version all your assistants, this is a great feature that allows you to evolve assistants without breaking things.
You need to be aware that programming with non-deterministic agents could be a challenge without help.

By using SAIA you can declare different versions of your assistants and start enabling different access to them depending on the consumer.

You can create a complete regression test suite with the new version of a new assistant before entering production. Actually, you can match your current staging planning in order to evolve your solution with confidence.

## Access to SAIA 

Access to SAIA's various layers is generally programmable using any programming language. Access is granted via access tokens per project or organization. 
In addition to programmatic interfaces, you can quickly create Playgrounds for demonstrating AI technology capabilities. 

If you already are using an integration with OpenAI API with minimal changes, simply update the base URL and Authorization header in your existing OpenAI SDK to setup the route request through [SAIA Proxy](SAIA Proxy AI.md)
