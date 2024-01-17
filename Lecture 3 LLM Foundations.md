### Lecture 3: LLM Foundations

Youtube link: https://www.youtube.com/watch?v=MyFrMFab6bo&list=PL1T8fO7ArWleyIqOy37OVXsP4hFXymdOZ&index=2&t=17s

- Software 1.0: Traditional algorithmic programmic
- Software 2.0: Bunch of parameters that we don't have much visibility. Supervised/unsupervised/reinforcement learning
    - Inputs and outputs are always just numbers. We may see a "photograph" of Lincoln. But machine sees a matrix and outputs another number, a vector that defines Lincoln.
    - Why is it hard? 
        - Infinite variety of inputs can all mean the same thing. I love this movie. vs This movie is amazing.
        - Meaningful differences can be tiny. 
        - Structure of the world is complex. 
    - Dominant approach nowadays is neural nets (deep learning)

- Pre training: Large model with a lot of data. 
- Fine tuning: Fast training on a little data. 
- Before 2020, each task had its own NN architecture. nowadays, all transformers. 
- 2017: Attention is all you need: it is set to work on translation, but later into other NLP tasks.
    - Transformer decoder: 
        - Task is to complete text: "It's a blue" -> "sundress" 
        - Inputs: a sequence of N tokens: [It's, a, blue]
        - Output: Probability distribution over the next token
        - Inference: Sample the next token from the distribution, append to inputs, run throught the model again, sample, append ...
        - Inputs: vectors of numbers. First turn into sequence of tokens. 
            - [<SOS>, It, 's, a, blue, sund, ress, <EOS>]
        - Turn into vocabulary ID: [0, 1026, 338, 257, 4171, .. ,1]
        - Each ID can be represented by a one-hot vector.
        - Input Embedding:
            - One hot vectors are poor representations of words or tokens. 
            - Distance between cat and kitten is same as between cat and tractor. 
            - Solution: Learn an embedding matrix. 30k by 512: embedding matrix for 30k vocabulary. 
        - Attention (Ignore Masked)
            - For a given token in the output sequence, only one or a few tokens in the input sequence are most important. Introduced in 2015 for translation tasks. 
            - Basic self attention:
                - Input: sequence of vectors, x1,x2,x3..
                - Output: sequence of vectors, each one a weighted sum of input sequence. 
                - Every input used 3 ways: query, key, value
                - Problem: There is no learning. 
                - Solution: Project inputs into query, key, value roles. 
            - Multi head attention: We can allow different ways of transforming into queries. 
            - Masking attention: Actual outputs: probability distribions. 
                - All output probabilities are computed at the same time. But you should only see one at a time, future tokens should not be visible. 
                - Conceptual: 
                    - token comes in
                    - gets augmented, with previously seen tokes that seem relevant. 
                    - this happens in several ways simultaneously (multiple head)
            - Positional encoding: Attention is totally position-invariant, ie. [this, movie, is, great] is the same as [movie, this, great, is]
                - So let's add position-encoding vectors to embedding vectors. 
            - Add & Norm:
                - output = module (input) + input
                - Allows gradient to flow from the loss function all the way to the first layer. 
                - This is possible because we do not change the dimension of output. 
            - Norm:
                - As the inputs flow through network, means and std's get blown out, so it is reset. 
            - Feed forward layer: Standard multi layer perceptron
                - "You start at word level, and upgrade to more semantic meanining"
            - This gets repeated multiple times. People generally scale all these together. 
                - GPT3: 175B parameters: Mostly the feedforward layer. 
                - GPT3 small: embedding and attention is larger. 
                
        The transformer is great because it is general-purpose differentiable computer. It is simultaneously:
        1- Expressive (in the forward pass)
        2- Optimizable (via backpropagation + gradient descent)
        3- Efficient (high parallelism compute graph)

        In-context learning and Induction Heads from Anthropic #type/paper2read

        Question: Should you be able to code a transformer? 
            - It's fun and not difficult. 
            - Andrej Karpathy has GPT2 implemented with <400 lines.


BERT (2019): 
    - Encoder only. no attention masking
    - 110M parameters.
    - 15% of all words masked out. Trained on task to predict the masked word. 
    - Was great, now dated. 

T5: Text to Text Transfer Transformer (2020):
    - Input and output are both text strings. 
    - Encoder - Decoder architecture
    - 11B parameters
    - Still a good choice for fine-tuning.
    - Training data C4: collosal clean crawled corpus. 800Gb, 160B tokens. 
    - Removed the pages with code. 

GPT / GPT2 (2019) Generative Pre-Trained Transformer:
    - decoder only.
    - Common Crawl has major data quality. 
    - scraped all links from reddit. 

    Byte pair encoding: 
        - How does GPT tokenize: Some words map to one word some not. Old school NLP: out-of-vocab words would be replaced by a special token vs UTF-8 bytes. Use middle ground. 

GPT-3 (2020):
    - 100x larger than GPT-2. 175B params. 
    - Exibited zero-shot and few-shot learning.
    - It only saw a token once? really? 

GPT-4 (2023): We do not know the technical details. 

The bitter lesson: Stack more layers -> Get better performance. 

What is the relationship between model size and dataset size? 
    - Training compute optimal llms (deep mind paper). Chinchilla. 
    - If I have a fixed buget, more data with smaller model, larger model with smaller data? 
    - Most LLMs are undertrained. They trained 70B model, and beats the performance of 280B, by using 4x fewer params and 4x more data. 

LLaMa (2023): Chinchilla optimal LLM. At least 1T tokens. Open source but non-commercial. 
    - it has github, wiki, arxiv, stackexchange. 

T5 and GPT-3 specifically removed code. Most recent models have %5 code. 
    - Emprically, it improved performance on non-code tasks. 
    - There is an open source data Stack. 

Instruction Tuning: 
    - few shot: model sees few examples of the task. 
    - zero shot: I shouldnt provide examples, just tell the instruction. 
    From few shot to zero shot: There is very little text on the internet of this Q&A form. 
    - Fine tune a smaller high quality dataset. Get contractors to label data. 


Alignment Tax: Instruction tuning: Zero shot ability increases but few shot ability suffers. Confidence becomes less calibrated. Because you teach different things it gets confused. 

Stanford Alpaca: Used GPT3 to create a labeled dataset, and fine tuned Llama. 

