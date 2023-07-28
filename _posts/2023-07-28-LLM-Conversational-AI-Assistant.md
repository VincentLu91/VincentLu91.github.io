# Conversational AI Assistant: How LLMs can answer questions around banking products

## Introduction

Since ChatGPT came out, LLM models have proliferated over a span of 8 months. Established organizations are trying to reposition themselves as AI companies with rapid launches of new products. Startups are popping up left and right claiming to build generative AI (or "GenAI" as some like to call it) products for their use cases. It's no surprise that LLMs are the catalyst behind society's mainstream adoption of AI going forward.

This also presents massive opportunities for anyone looking to improve processes at their organizations, with mine being one of them. Recently, I've been thinking of use cases that could be addressed to improve customer service, and one such use case would be to help prospective customers to understand a brochure about investing services, offered by a big bank.

I've since then developed a chatbot that is capable of providing relevant information. The deployed Streamlit app is given [here](https://pwcai-82m4lkzqptu.streamlit.app/). Corresponding repository [here](https://github.com/VincentLu91/PrivateWealthClientAI).

## Implementation

I didn't set out to build a chatbot per se, but rather an information retrieval system that generates answers by way of semantic search.

In any knowledge base system, a searcher would enter a query that would be used to scan the entire corpus in question to return an answer that is meaningful and relevant to the query. Context matters, because any word could represent multiple meanings but a phrase (or set of words in a certain order) would represent a different meaning. This is called a vector embedding. It is usually high dimensional with each dimension capturing the structure of corpuses, contextual meanings, and phrasing patterns.

There are models that could do this. OpenAI's text-embedding-ada-002 computes embeddings that could be used for search and other language processing tasks. There are also open source pre-trained models offered by BLOOM and StableLM that enable conversion from text to vector embeddings. I chose SentenceTransformer's all-MiniLM-L6-v2 model for its ease of use, speed, model size, and that it requires no payment to get started.

Next, I searched for public facing pdf brochures in the financial industry, since most customers often need to make informed decisions on where and how to entrust their money, and came across one that advertises products I know nothing about but may help inform prospects of the types of investment products available.

I saved the PDF into a local directory and loaded it for further text processing before passing it to SentenceTransformer's embedding API to create embedding for storing into a Pinecone vector database. This database is used for indexing and semantic searching later on. Other metadata will also be stored there - such as citations - as the vector DB may return other article links to users for further reading after indexing takes place.

With a vector database established, I designed the frontend application in Streamlit with the chat interface, complete with a conversation chain to keep track of all queries and responses. The interface allows the user to enter their query, in which I refined it using OpenAIs' text-davinci-003 model. By refining the query, I would have an appropriately phrased prompt which can then be converted into an embedding for indexing through the vector database to find the most relevant matching responses. Once a match is found, I returned its corresponding text in the form of a response in the chatbot engine.

Finally, I uploaded the codebase to Github and deployed the application using Streamlit Community Cloud service. I saved the API keys in their portal to avoid exposing them in the source code.

## Limitations and Challenges

At this point, the LLM is capable of understanding one piece of document. The dataset I sourced provides information on two investing products, however the provider of the brochure offers several others. As such, if a prospective customer wants to learn about other products that are not included in the dataset, the LLM will not be able to provide such information.

Because LLMs are a new technology, there are a handful of solutions that could potentially allow ingesting multiple documents for training. One such solution is privateGPT. It is also developed with LangChain and SentenceTransformers, and it allows for question and answering without an internet connection.

In their current state, LLMs are not entirely perfect either, especially with prompting. Occasionally I had to change the wording to get the answer I wanted. Here's an example I tried with the fraud prevention brochure, where I asked "how do deal with phishing" but I had to change the prompt later to "what do we do when we get a phishing message?"

![Screenshot 2023-07-21 at 4 31 25 PM](https://github.com/VincentLu91/VincentLu91.github.io/assets/3411100/ef420547-b2f7-48f8-99b5-aae4761078cd)

![Screenshot 2023-07-21 at 4 31 35 PM](https://github.com/VincentLu91/VincentLu91.github.io/assets/3411100/07aaf539-7253-42fa-ae21-75e10b686467)


## Some Ideas for Improvement

Having built the POC, there are a few ideas where it could be improved. One of which would be to explore privateGPT for ingesting multiple documents. This would be hugely useful as one document may not be enough for users to find pertinent information of a wide array of investment offerings.

As described in the previous section, the outputs may not always be contextually perfect. There may be opportunities to fine tune different hyperparameters to improve the quality of information retrieval. A related direction I could go with the POC is prompt engineering. Thanks to the `openai` API, I could experiment creatively with how I want the LLM to generate a response. This comes in different forms, such as variability of a response (temperature), and the type of instruction given to the model (prompt).

Finally, I would love to explore other popular LLMs. Earlier, I mentioned that I chose open source models that are free, easy to use, and fast (all-MiniLM-L6-v2), but other models including LLaMA and Vicuna are achieving SOTA performance closer to GPT-4. GPT-4 is currently a top performing model as it has allowed for 8-32k tokens (huge input text) which allows for less splitting for easier text processing. Also it could return links to other websites, emulating human-to-human customer service. As such, this would be the current ideal model to use for creating vector embeddings.


## Conclusion

As LLMs continue to improve in performance, companies are looking to integrate them into their offerings and internal workflows. A recent example is Shopify's Sidekick, which was released on July 12. Sidekick was created to help facilitate eCommerce store development and management for merchants.

Banks are also looking to start implementing LLMs into their products and internal workflows. With huge amounts of data and corpus of information, they could deliver a promising value proposition to new and existing offers to all stakeholders. An example is [Morgan Stanley](https://www.forbes.com/sites/tomdavenport/2023/03/20/how-morgan-stanley-is-training-gpt-to-help-financial-advisors/?sh=14fa25523fc3), which is currently implementing their own generative AI product to help financial advisors improve knowledge management.

On the technical front, LLMs are finally able to generate contextually relevant responses by way of storing embeddings in vector databases. This allows for semantic search by finding matches between an embedded query and the data sourced from a piece of document.

To improve the chatbot POC, there are avenues to explore. They include ingesting multiple relevant documents to provide a complete, comprehensive conversational experience to end users, fine-tuning and prompt engineering, and experimenting with other SOTA LLMs.
