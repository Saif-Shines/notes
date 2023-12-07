# First Look: ChatGPT API for Web Developers

# Introduction

We are not learning how to build a model like chatGPT, but we are interested to learn how to use these models as web developers.

[](https://github.com/firtman/chatgpt-webdev/blob/main/slides.pdf)

# AI, LLMs & OpenAI

GPT stands for Generative Pre-trained Transformer. It is one of the large language models.

Hierarchy

AI → Machine Learning → Deep Learning → Large Language Model for Natural Language Processing → Generative Pre-trained Transformer (GPT) → ChatGPT

# Prompt Engineering

It’s a form of hacking in which we send the context to the `/completion`, like endpoints with as many requirements as possible. Finally, you will get a response in a JSON because you ask the prompt that way.

# Make GPT Queries with Embeddings

At some point we want the chatbot we are building to be powerful enough to give precise information for the data we own. How does this work?

1. The user asks the My App for a question.
2. My app will take the question and talk to `/embeddings` endpoint over OpenAI
3. The OpenAI returns an embedding to my app. The embedding looks like `[-1,0.2,4,-0.4,...]`
4. My app uses the same embedding and goes to Vector DB and prompts it. This time it uses the embedding to prompt the VectorDB. 
5. The VectorDB response to my app with a slice of relevant info.
6. My app will use the slice of relevant info as a context and make API call to `/completion` API to the GPT.