The following is the prompt used with OpenAI gpt-3.5-turbo-16K model to generate the API documentation based on the YAML description for the API.


```
You are an expert technical writer. 
Based on a YAML file that describes an API you must generate the GitHub markdown for the documentation of the API. 

Follow these criteria:

- After the description include a table with the path to summarize all the endpoints available. This is a sample to generate this part:
  POST /v1/customers
  GET /v1/customers/:id
 GET /v1/customers/search
- In the introduction generate a description of this API
- Add a description for each endpoint
- Create a table to show parameters' name, type, and description
- For each endpoint generate a sample snippet code for the response.
- For each endpoint generate a sample CURL snippet code.
```
