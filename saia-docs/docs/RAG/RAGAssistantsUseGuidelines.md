---
sidebar_label: 'Use Guidelines'
sidebar_position: 3
---
# RAG Assistants Use Guidelines

This document is intended to provide guidelines on how to ask questions to get information that was ingested in a particular 
RAG Assistant. 

## General Considerations

### Use Natural Language 
Type your question naturally just like you would ask a colleague for information.

It doesn't matter if you make spelling mistakes or use different synonyms. It is important to convey your intention to the 
assistant correctly.

**Do:** What's the correct reset procedure for the FFS security system?
**Don't:** reset FFS

### Avoid Jargon
Even though the assistant may understand technical terms, simpler language can often provide more accurate results.

**Do:** How can I increase traffic to my website?
**Don't:** What are the best SEO practices for SERP dominance in Q4?

### Provide Context
You can add a bit of context or background to your query to help the assistant understand the scope of your request.

**Do:** I'm working on a 2014 Ford Ranger and need to know how to replace the brake pads. What are the steps?
**Don't:** How do I change brake pads?

### Use Clear and Concise Language
Be specific when you know what you need and clearly articulate what you're looking for to receive more accurate and relevant 
information.

When possible, avoid ambiguity by phrasing your questions or requests in a way that minimizes ambiguity. This helps the 
assistant provide precise responses.

**Do:** What is the process for Uruguayan citizens to apply for a tourist visa to Japan?
**Don't:** How do I travel to Japan?

### Ask One Question at a Time
Consider breaking it down into simpler parts for better comprehension if you have a complex query.

**Do:** What is the average temperature in Paris in June? 
What are some popular tourist attractions there?

**Don't:** What is the weather like in Paris and what should I do during my visit?

### Iterate and Refine your Questions
If the initial response is different from what you need, consider refining your question based on the assistant's response. 
You can ask follow-up questions to complement what you are looking for.

At what time does the museum close?
**The museum closes at 5 PM.** 

Could you also tell me what days of the week is it open?
**From Tuesday to Sunday**

### Provide Alternative Phrasings
Experiment with different ways of phrasing your query, including synonyms and variations, to explore the assistant's 
understanding.

What's the best way to increase productivity at work?
**If it doesn’t understand the question, the assistant might provide general tips on productivity rather than specific 
workplace strategies.**

Can you suggest specific methods for improving team productivity in an office setting?
**This rephrasing clarifies that the focus is on team productivity in a professional environment, not individual productivity 
tips.**

I'm looking for techniques to enhance collaborative efficiency among employees in a corporate office. What are some proven 
strategies?

## Questions that the Assistant Can't Answer

* Avoid asking for information related to dates. For example, “show me the latest documents” or “what was the contract
  between March and April?”.
*	Comparisons or contrasting information are not expected to get good results; all these use cases may require different
  techniques for interaction.
*	Information that is not part of the documents that were used to give context to the RAG Assistant.
*	It can't summarize or count items.
*	It is not a search tool based on keywords; for this reason, use the general recommendations in the previous section.
* Texts in images are not processed.
