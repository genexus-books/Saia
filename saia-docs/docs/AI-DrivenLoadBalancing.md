---
sidebar_label: 'AI-Driven Load Balancing'
sidebar_position: 20
---
# AI-Driven Load Balancing

Learn how GeneXus Enterprise AI automatically manages the Load Balancing process when you work with generative AI providers, efficiently addressing the limits imposed by LLM platforms. This ensures optimal performance, distributes the workload evenly, avoids overloading individual servers, and provides continuous availability. In this way, your application or service will run smoothly and efficiently, with no need for you to worry about the limits imposed by LLM providers.

## What is Load Balancing?

Load Balancing is a technique that divides the workload equally among several servers or resources of an infrastructure. 
This is achieved in an intelligent and automated way, ensuring that no server is overloaded while others remain idle.

## The MaxTokens and RPM Challenge

Language model service providers, such as GPT, set limits on Requests Per Minute (RPM) based on the MaxTokens parameter. 
To understand how they are related, consider the following scenario:

**MaxTokens and their Meaning:** MaxTokens indicates the maximum number of text tokens that a language model can generate 
in a single response. These tokens can be words or parts of words in natural languages, and contribute to the computational
cost and time required to generate a response.

**Calculated RPM Limit:** Language model service providers, such as AzureOpenAI, calculate the Tokens Per Minute (TPM) 
limit based on the MaxTokens value provided in a request. This means that, if MaxTokens is high, the RPM limit will be 
lower, and vice versa.

The problem arises when requests are made with different MaxTokens values. A higher MaxTokens value reduces the RPM limit, 
which in turn can affect the processing capacity. 

For example, if there is a limit of 240,000 TPM and you set MaxTokens to 4,000, you get a limit of 60 RPM. This means that
no more than 60 requests per minute can be made to AzureOpenAI with that configuration.

## MaxTokens and RPM in Load Balancing

The link between Load Balancing and the MaxTokens and RPM parameters is crucial to ensure optimal performance and efficient
request handling. 

When using Load Balancing in an environment with requests that have different MaxTokens values, it is critical to 
understand how this affects overall performance. If the Load Balancer does not properly handle requests with different 
MaxTokens, it could lead to an imbalance in load distribution and performance may suffer.

To address this connection, multiple Deployments are automatically used to increase capacity and distribute the load 
evenly. The Load Balancer automatically redirects requests to these deployments, ensuring a balanced workload.

## How Load Balancing Works in Practice

**Multiple Deployments:** Deployments are separate instances of the language model service that operate independently. 
By having several deployments available, the capacity to process requests is increased.

The number of deployments is automatically determined, generating N + 1 deployments. That is, if you already have N 
deployments running, an additional deployment will be automatically generated.

**Request Redirection:** The Load Balancer receives incoming requests and decides to which deployment to redirect each 
request. This process is done automatically based on the current load of each deployment and the available capacities.

**Load Balancing:** The idea is to distribute the requests evenly among the different deployments. In this way, no 
deployments are overloaded, and capacity is used efficiently.

## Advantages of MaxTokens/RPM Load Balancing

**Capacity Optimization:** Load balancing allows taking full advantage of the processing capacity, even when MaxTokens is 
set to low values to increase RPM. 

**Availability Guarantee:** By distributing the load evenly, service availability is improved by avoiding overloading a 
single deployment.

**Consistent Performance:** Users experience more consistent performance, since the service dynamically adapts to demand.
