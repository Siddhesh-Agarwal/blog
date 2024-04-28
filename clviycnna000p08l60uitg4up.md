---
title: "RAG Model using Langchain.py and ChromaDB"
seoTitle: "RAG Model with Langchain & ChromaDB"
seoDescription: "Create a Retrieval Augmented Generation (RAG) Model with Langchain.py and ChromaDB for custom data."
datePublished: Sun Apr 28 2024 03:09:47 GMT+0000 (Coordinated Universal Time)
cuid: clviycnna000p08l60uitg4up
slug: rag-model-using-langchainpy-and-chromadb
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1714199115386/5716207c-c837-47de-b7ea-999c09926695.png
tags: python, langchain, chromadb, rag

---

Today, I will discuss creating a Retrieval Augmented Generation (RAG) Model on your custom data using Python, LangChain, and ChromaDB (or any VectorStore you choose).

## What is a RAG Model?

To put it in simple words, a RAG Model is a Large Language Model that is connected to a "Retrieval Agent". A retrieval agent is an agent that fetches documents from a "VectorStore" using the [Approximate Nearest Neighbor](https://en.wikipedia.org/wiki/Nearest_neighbor_search#Approximation_methods) (ANN) algorithm. A VectorStore can be thought to be similar to a [Knowledge Base](https://en.wikipedia.org/wiki/Knowledge_base). VectorStore or Vector Database is a special type of Database that stores information as high-order vectors. These vectors are based on the "text embeddings" of the information. Since LLMs do not understand "Language", sentences are tokenized (essentially, tokenization is the process of breaking down sentences into smaller units called "tokens". A token may or may not be equivalent to a word) and converted to their vector equivalent. Each LLM has a unique text embedding.

## What is the use of it?

There are 2 major problems while using Large Language Models:

1. **Training Date Cutoff**: LLMs do not have information after their training cutoff date, so they cannot answer questions about the latest events.
    
2. **Hallucinations**: LLMs tend to "imagine" certain information to answer questions. This may be because the relevant information is missing or is getting lost due to the context size of the model. This results in the model spitting out factually Inconsistent information.
    

We can solve both of these problems using RAG Models.

> **READ MORE**: [\[2311.13878\] Minimizing Factual Inconsistency and Hallucination in Large Language Models (](https://arxiv.org/abs/2311.13878)[arxiv.org](http://arxiv.org)[)](https://arxiv.org/abs/2311.13878)

We can create custom Chatbots that answer questions based on our custom data. For example, a car manufacturer could make a chatbot for its website that answers basic questions!

---

# Building a RAG Model

We can build our RAG Model in 4 simple steps.

## Step 1: Selecting LLM and VectorStore

While this step is the simplest, it is also the most crucial in the process. You can select [any VectorStore](https://python.langchain.com/docs/integrations/vectorstores/) and [any LLMs](https://python.langchain.com/docs/integrations/llms/). For simplicity, let's choose ChromaDB and the GPT 3.5-Turbo Model (requires an OpenAI API key).

## **Step 2: Data Extraction and Document Loading**

First, we need to extract our data. It could be in the form of a text file, a PDF, or even a webpage. To extract the data, we should use [Langchain's document loaders](https://python.langchain.com/docs/integrations/document_loaders/). For instance, if we are developing a Chatbot trained on the [Django documentation](https://django.readthedocs.io/en/stable/), we should start by using a document loader to fetch the documents from the website:

```python
from langchain_community.document_loaders.recursive_url_loader import RecursiveUrlLoader
from bs4 import BeautifulSoup

loader = RecursiveUrlLoader(
    url="https://django.readthedocs.io/en/stable/",
    max_depth=3,
    extractor=lambda x: BeautifulSoup(x).text,
)
```

Now that we have created a loader, we load the documents:

```python
docs = loader.load()
```

While this gets the job done, there is an issue with `loader.load()`. The problem is that it produces documents with inconsistent sizes (one page might be too large while another might be too small, causing variations in document size) and there is no set maximum size for a document (this could lead to the LLM missing context even if the right document is recognized due to large context size). This problem can be resolved by utilizing the `load_and_split()` method of the `loader`:

```python
from langchain.text_splitter import RecursiveCharacterTextSplitter

text_splitter = RecursiveCharacterTextSplitter()
docs = loader.load_and_split(text_splitter=text_splitter)
```

The idea behind a "text splitter" is to split the documents into small uniformly sized documents.

## Step 3: Creating Embeddings and Storage

Now we have to add the documents to a Vector Store. But first, we need to initialize the VectorStore:

```python
from langchain.vectorstores.chroma import Chroma
from langchain_openai.embeddings import OpenAIEmbeddings
import chromadb

client = chromadb.PersistentClient()
embeddings = OpenAIEmbeddings(model="text-embedding-3-large")
db = Chroma(
    client=client,
    collection_name="django",
    embedding_function=embeddings,
)
```

Now, we add the documents to the store:

```python
db.add_documents(docs)
```

You will face the `RateLimitError` if you have enough data and are working on the free tier/tier-1. To prevent the rate limiting, we can add a small amount of documents after small intervals:

```python
import time

# change these variables to experiment
batch_size = 10
sleep_time = 1
for i in range(0, len(docs), batch_size):
    db.add_documents(docs[i : i + batch_size])
    time.sleep(sleep_time)
```

After this step is done, you have successfully embedded all the data in a VectorStore

## Step 4: Connecting Retriever to LLM

Now comes the nice part, we create a retriever instance, an LLM instance and a Prompt template and join all 3 to form an LLM chain:

```python
from langchain.prompts.chat import ChatPromptTemplate
from langchain_core.output_parsers.string import StrOutputParser
from langchain_core.runnables.passthrough import RunnablePassthrough
from langchain_openai.llms import OpenAI

retriever = db.as_retriever()
model = OpenAI(temperature=0)
template = """Answer the question based only on the following context:
{context}

Question: {question}
"""
prompt = ChatPromptTemplate.from_template(template)

chain = (
    {"context": retriever, "question": RunnablePassthrough()}
    | prompt
    | model
    | StrOutputParser()
)

query = "What are the features of Django?"
chain.stream(query)
```

Congrats, you have made your very own RAG Model that answers questions from the Django documentation!!!

---

# Deployment

The RAG Model can be deployed using Streamlit.

> To learn more about streamlit, check out my previous article on [Streamlit for ML deployment](https://blog.siddhesh.tech/deploy-ml-models-with-streamlit)