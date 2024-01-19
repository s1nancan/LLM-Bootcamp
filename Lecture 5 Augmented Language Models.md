Lecure 5: Augmented Language Models

Youtube link: https://www.youtube.com/watch?v=YdeuQhlHmCA&list=PL1T8fO7ArWleyIqOy37OVXsP4hFXymdOZ&index=4

There is a ton that LLM's dont know. What (base) language models are good at? 

- Language understanding
- Instruction following
- Basic reasoning
- code understandong

What they need help with? 
- Up to date knowledge
- Knowledge of your dataset
- More challenging reasoning
- Interacting with the world

#### LLMs are for general reasoning, not specific knowledge. 

- LLM is the brain, you need to give the tools to it. They are "smart" but dont know anything. To solve problems, need trainig, calculator, other tools. 

- A baseline: using the context window, basically giving more information. But it only fits a limited amount of information. Sota GPT4 has 32k tokens. it is size of a college thesis. 256k is a novel. 65M tokens, 500mb of unicode text data, for comparison elastic search node can store 50gb. 

``` Context windows are great but you are not gonna fit everything for a while ```

So instead of training larger models, we use augmented language models. 

3 ways: 
- retrieval: augment with bigger corpus
- chains: augment with more LLM calls
- tools: Augment with outside sources

This is an extremely large/complex topic. These lecture/notes are just a summary. 

##### Retrieval Augmentation

Why: We want our model to have access to user data. 
    - Approach 1: Put into the context. 

Context building is information retrieval. "search", ie. putting the right data into context. 

B - Traditional information retrieval: 
    - Information retrieval basics:    
        - Query
        - Object, ie. document
        - relevance: measure if object satisfies the information need
        - ranking: Ordering relevant results. 

    - Search via inverted indexes. Finding documents that has the words in it. 

    Ranking & Relevance: via boolean search. e.g. return docs that has word X and Y and Z. then rank via BM25 with 3 factors: 
        - Term Frequency (TF)
        - Inverse document frequency (IDF)
        - Field length: few words in the sentece for the word of interest. 

!! Search engines are more than inverted indices
- Not just store data, but ingest and process documents. Need to be able handle transactions, should be able to scale, rank and relevance order. 

Limitations of sparse traditional search: 
- Only models simple word frequencies. 
- No semantic, correlation. 

C. AI-Powered Information Retrieval via Embeddings: 
Search and AI make each other better. Better representation of data <> Better information in the context. 

What is an embedding: Abstract, dense, compact, fixed size, learned representation of data. 

Sparse: 
If a certain words in the document we flag it. contains "cofee", "tea" "laptop".
Dense" 
It's a vector that has a meaning about the word. 

What are they not? 
- Not last learned layer. 
- Doesnt single type of input.
- Dont have to be from neural net. 
- Dont have to be comparable in vector space. 

::: These 4 above are not clear to me? :::

Overall, its a compact and universal represantation of data. 

Embeddings to know: 
    - Word2Vec: Try to predict the center word from the surrounding context. 
    - Sentence transformers
    - CLIP : text & image / find similar images given text vice versa. 
    - !!!! OpenAI embeddings is the good fast cheap. text-embedding-ada-002. Near SoTA. 
    -SoTA: Instructor: every text input is embedded together with instructions explaining the use case (e.g., task and domain descriptions). Unlike encoders from prior work that are more specialized, INSTRUCTOR is a single embedder that can generate text embeddings tailored to different downstream tasks and domains, without any further training [https://arxiv.org/abs/2212.09741]

    - For now if retrieval quality is important, you cant't escape training your own!

----------------
##### Similarity Search: 

Embed your corpus, store embeddings as an array, embed the query, compute dot product with the array. 

When do you need more than that? 
    - If you have <100k vectors, you wont notice the difference in speed with simple numpy code vs vector database. 
    - Approximate nearest neighbors (ANN)
        - Embedding indexes are data strcuctures that let us perform approximate search.
        - Some algorithms: Flat, LSH, HNSW, IVF: it partitions/divide space so you only search "proximity" 

    - FAISS: Facebook AI Similarity Search
    - Hnswlib
    - nmbslib
    - annoy

Do you need to understand all of this? 
    - Not at the first. 
    - when prototying doesnt matter. 
    - when productionizing, the important choice is not the index by the "InformationRetrieval system" its part of. 
    - if you mush choose, FAISS + HNSW is a reasonable start. 

Limitations of ANN indexes: 
    - They are just a data structure, they dont offer histing, not store dat, dont let you combine sparse + dense retrieval, not scalable. 

Embedding Datbases: Do you need an embedding database or just a database? 
- Elasticsearch, postgress, redis run NN/ANN. 
- You are probably using one of those. 
- Won't work for the most complicated queries of highest scale, but will probably work for you. 

Dream of how this would work:

- Dump bunch of data. Run a query. Get the most relevant data back. 

Challenges: 
    - Scale, reliability
    - Document splitting and embedding management. 
    - Query language.
    - Search algorithm. 

Suggestion: Pinecone for embedding database. 


Stopped at 39:00. to be continued. 