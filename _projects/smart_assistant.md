---
permalink: /projects/smart-assistant/
---

# Knowledge-as-a-Service: How to use large language models to access unstructured information
More and more, the hospitality industry is expected to deliver exceptional customer experiences with fewer resources and high competition.
Hotels typically maintain information across dozens of systems and documents, making it challenging for staff to quickly access critical details during guest interactions.
At the same time, guests increasingly expect instant, frictionless service and high availability of support.
Together with US-based hospi-tech company [LosjiTech](losjitech.com), we developed an AI-powered solution that makes knowledge instantly accessible and automates the processing of guest requests into actionable items.

![nlp-modules]({{ site.url }}{{ site.baseurl }}/assets/images/losjitech-setup.png)
_The operational infrastructure of LosjiTech's platform_
## Summary
We built modules to support the following workflows:
- A guest has a question about how to engage with the property they have rented, such as how they can roll down the sun blinds <>. Rather than having the guest weed through a knowledge base or depend on staff, they are empowered through conversational access with their knowledge base
- A guest has no access to hot water. They discuss the issue with a service chatbot, which compiles a service report through natural conversation, including metadata such as check-in date and room location. The report satisfies the structured information requirements on a service ticket and both the guest and the service staff get instant confirmation of the problem.

## Knowledge retrieval
The solution combines traditional approaches in natural language processing (NLP) with the power of large language models (LLMs), in a hybrid approach dubbed _retrieval-augmented generation_ (RAG).

![nlp-modules]({{ site.url }}{{ site.baseurl }}/assets/images/nlp-modules.png)

First, we index the documents in a hotel's knowledge base into a so-called _vector database_, where each document gets assigned an _embedding_, a long vector of numbers. When two embeddings are close to each other, it means that the documents they represent likely contain similar information. 

When a guests requests information, we compute the embedding of his request. This allows us to identify the documents that might contain the information the guest is looking for.
We take the most related documents from the vector database and present them to the LLM, which then searches through these documents to find the exact piece of information the guest is looking for and formulate it as an answer to their question.

<iframe width="560" height="315" src="https://www.youtube.com/embed/aobSwZyj5dM" frameborder="0" allowfullscreen></iframe>
## Automated service tickets


Using the same technology, but somewhat in reverse, we are able to convert a request in natural language into a structured ticket. LLMs are suitable to convert information from one format into another, and tools like [LangChain](https://www.langchain.com/) or [LlamaIndex](https://docs.llamaindex.ai/en/stable/) have built-in support to choose which procedures to run based on contextual conversations.
By mapping the different fields a service ticket requires to a concise description, the chatbot can infer what information it can directly take from a request, as well as what is still needed
<iframe width="560" height="315" src="https://www.youtube.com/embed/ryFum4i2M10" frameborder="0" allowfullscreen></iframe>

## Advantages
Apart from the efficiency and repetitive work that is saved, this also gives more insight of what guests are generally struggling with, and requires clarification.
In addition, it allows for seamless adaption to the preferred channel; if a guest desired a voice interface, the only extra effort required is integration with e.g. the [Whisper API](https://openai.com/index/whisper/).
In addition, the knowledge assistant can be helpful to the issue handler as well; imagine that a guest doesn't have access to hot water, because the tank requires a start. the knowledge assistent is able to link the information in the ticket to the information in the knowledge base, coming up with helpful suggestions.

This system has been deployed and is in commercial use by LosjiTech.