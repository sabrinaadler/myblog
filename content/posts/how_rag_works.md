---
title: How Retrieval Augmented Generation Works
date: 2025-07-16
author: Sabrina Adler
---

# How Retrieval Augmented Generation Works

## Introduction

Natural Language Processing (NLP) is the process of taking a natural language input and then processing this input into something that a computer can understand. Large Language Models, or LLMs, are a subfield of NLP that uses artificial intelligence to interpret and generate natural language; some popular examples include ChatGPT and Google Bard.

The disadvantage of LLMs is that they are trained on outdated information that might not be specific enough for a particular use case. Additionally, many can only accept up to a few thousand words, so it is impossible to pass in large documents. That is where Retrieval Augmented Generation comes in. 

## Definition and Process

Retrieval Augmented Generation (RAG) is a process that improves the outputs of large language models using context retrieved from a vector database. There are four main steps: first, it takes the documents (PDFs, text files, word documents) and embeds them into a vector; then, it takes these embeddings and stores them in a vector database; next, when the user asks a question, it finds chunks of text in the database that are similar to the question; finally, it uses this text to give the LLM extra context to answer the question. 

## Embedding and Vectorization

The user will select documents that are relevant to their use case. For example, if you want to ask an LLM about your car, you may upload the user manual. The next step is to chunk the document into chunks of text that can be embedded; typically, these chunk sizes are a sentence to a paragraph in length. These chunks of text are then passed to the embedding model.

An embedding model is a tiny LLM trained on how relevant words are to each other. Words or concepts that are more strongly related have closer vectors to each other. For example, the words “queen” and “chess” would have vectors that are very close to each other, but the word “lime” would have a very different vector from those two since they are not closely related. To understand how embeddings work better, the first five minutes of this [video](https://www.youtube.com/watch?v=viZrOnJclY0&list=PLblh5JKOoLUIxGDQs4LFFD--41Vzf-ME1&index=18) explain how embeddings work in a very simplified way. 

From the embedding model, it returns vectors of all of the chunks of text from the document that the user passed in. These vectors are then passed into a vector database, which stores the chunk number, the embedded vector, and the text in the chunk. Now, the document is entirely parsed and stored in a vector database so that it will be easier to pull the most relevant chunks. 

## Making Requests to the Vector Database

This step aims to get the chunks of text from the documents that the user passed in most relevant to the question. This step is important because the goal of a RAG pipeline is to supply the LLM with extra context to improve the answer. From the previous step, there is now a vector database that stores all of the chunks of text that have been embedded into vectors. Now, the user can query these documents. 

The user can now enter a question, for example, “How many miles can I drive before changing my oil?”. The embedding model will then embed this question into a vector. It needs to embed the question into a vector because a mathematical calculation will occur between the vector of the question and all the other vectors of the chunks of text. From the embedding model, the closer two vectors are to each other, the more similar a meaning they have to each other. So, the point of the calculation is to find the vector of text with the smallest distance from the question, since the chunk of text will have the most similar meaning to the question.

Typically, the most common type of calculation is a semantic cosine similarity search which will return a value between negative one and one: one being the most similar and the negative one being the most opposite. Below is an example of the calculator it will perform, and the summations are to account for the number of dimensions that the vectors are in, typically anywhere from one hundred to one thousand. 

This calculation will occur between all of the vectors, and only the text chunks with the vectors that have the highest similarity to the vector of the question will be returned. Continuing with the previous example, a chunk of text about the oil will have a higher cosine similarity score than a chunk of text about the windshield wipers; this means, the chunk of text about oil will get returned. Ideally, this calculation will get the chunks of text that will be the most relevant and useful for answering the question. 

## Passing Context to the LLM

The final step of the RAG pipeline is to pass the question alongside the retrieved context to an LLM to generate a final response. There are many different templates to format this, but typically they are something like: “You are a helpful assistant given the following context: {insert context retrieved}; Please answer this question only using the context provided: {insert user question}”. 

## Conclusion

The RAG pipeline embeds the documents and stores them in a vector database. Then, the pipeline uses a cosine similarity search to find the most relevant chunks to the question. Finally, it sends the extra context and question to the LLM and gets a final answer. Retrieval Augmented Generation is a useful tool in artificial intelligence. It can make large language models better for a variety of use cases. 
