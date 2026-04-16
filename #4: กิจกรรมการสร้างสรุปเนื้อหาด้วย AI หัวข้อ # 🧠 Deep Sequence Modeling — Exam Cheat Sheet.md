# Deep Sequence Modeling — Exam Cheat Sheet

## 1) Structured summary

### A. What is sequence modeling?
Sequence modeling is learning from **ordered data** where earlier elements help interpret or predict later elements.

Typical sequence data:
- text / language
- speech / audio
- ECG and other medical signals
- stock prices / weather / time series
- biological sequences
- music
- video / motion

Typical tasks:
- **sequence → label**: sentiment classification
- **sequence → sequence**: machine translation
- **sequence generation**: next-word prediction, next-character prediction, music generation
- **single input → sequence**: image captioning

### B. Why ordinary feedforward networks are not enough
A standard feedforward network processes one input independently:

$$
\hat{y}^{(t)} = f(x^{(t)})
$$

Problem:
- it only sees the current input
- it ignores previous timesteps
- it cannot model temporal dependence or order

This is why sequence models are needed.

---

### C. Perceptron and feedforward foundation
A perceptron:
1. takes numerical inputs
2. multiplies by weights
3. sums them
4. applies a nonlinear activation
5. outputs a prediction

Core idea:

$$
z = \sum_{i=1}^{m} w_i x_i + b
$$

$$
\hat{y}=\phi(z)
$$

This is the basic building block for deeper neural networks.

---

### D. Recurrent Neural Networks (RNNs)
RNNs introduce a **hidden state** $h^{(t)}$ that carries information through time.

Core recurrence:

$$
h^{(t)} = f_W(x^{(t)}, h^{(t-1)})
$$

Output:

$$
\hat{y}^{(t)} = g(h^{(t)})
$$

Meaning:
- current prediction depends on current input **and** past memory
- same weights are reused at every timestep
- sequence is processed step by step

Important intuition:
- hidden state = memory
- recurrence = repeated update through time
- unrolling = showing the same RNN computation across many timesteps

---

### E. Training RNNs
Loss can be computed at each timestep and summed:

$$
\mathcal{L}_{total} = \sum_{t=1}^{T} \mathcal{L}^{(t)}
$$

Training uses **Backpropagation Through Time (BPTT)**:
- unroll RNN over timesteps
- compute losses
- backpropagate errors backward through time

Main training issues:
- **vanishing gradients**: gradients become too small
- **exploding gradients**: gradients become too large

Why this matters:
- long-term dependencies become hard to learn
- standard RNNs can be unstable

---

### F. LSTM idea
LSTM = **Long Short-Term Memory**

Why introduced:
- standard RNNs struggle with long-term dependencies
- LSTMs add internal control mechanisms to regulate information flow

What to remember for exams:
- LSTM is an improved recurrent architecture
- purpose: better memory handling and more stable learning over longer sequences

---

### G. Language representation: vocabulary, indexing, embeddings
Neural networks need numbers, not raw words.

Pipeline:
1. build a **vocabulary**
2. map each word to an **index**
3. convert index to a vector representation

#### One-hot encoding
A sparse vector with one 1 and the rest 0s:

$$
\mathbf{x}_i = [0,\dots,0,1,0,\dots,0]
$$

Use:
- represent identity of a token
- simple but sparse
- does **not** encode similarity between words

#### Learned embedding
Map index $i$ to a dense vector:

$$
i \rightarrow \mathbf{e}_i \in \mathbb{R}^{d}
$$

Use:
- lower-dimensional representation
- similar words can have similar vectors
- much better than one-hot for language modeling

---

### H. Why sequence modeling is difficult
Real sequences are hard because of:
- **variable length**
- **short-range dependencies**
- **long-range dependencies**
- **order sensitivity**

Very important:
changing order can change meaning completely.

---

### I. Limitations of RNNs
1. **Fixed-size hidden state bottleneck**
   - too much information must be compressed into one vector

2. **Sequential computation**
   - hard to parallelize
   - slower on long sequences

3. **Weak long-term memory**
   - especially with vanishing gradients

These limitations motivated newer architectures.

---

### J. Attention
Attention lets the model identify which parts of the input are most relevant to other parts.

Search analogy:
- **Query** = what I am looking for
- **Key** = descriptor used for matching
- **Value** = information to retrieve

Attention answers:
- what should I focus on?
- which tokens are most related?
- what information should be pulled out?

---

### K. Positional encoding / positional embedding
If we remove recurrence and process the whole sequence at once, we still need order information.

So we add **positional information** to token representations.

Purpose:
- tell the model where each token appears in the sequence
- preserve order without step-by-step recurrence

---

### L. Self-attention
From the position-aware input, compute:

$$
Q, K, V
$$

where:
- $Q$ = query matrix
- $K$ = key matrix
- $V$ = value matrix

Then compute similarity between queries and keys.

Core score idea:

$$
score(q,k)=q \cdot k
$$

Matrix form:

$$
QK^\top
$$

Then scale and apply softmax:

$$
A = softmax(scaled(QK^\top))
$$

This gives attention weights.

Finally use those weights on values:

$$
Output = AV
$$

Meaning:
- tokens attend to other tokens
- the model extracts features based on relevance relationships inside the sequence

---

### M. Transformer
Transformer = sequence model built on **attention**, not recurrence.

Main advantages over RNNs:
- handles full sequence at once
- easier to parallelize
- can model rich token-to-token relationships
- foundation of modern large language models

Important fact:
- the **T** in **GPT** stands for **Transformer**

---

### N. Multi-head attention
Transformers usually use multiple attention heads.

Why:
- each head can capture different relationships
- increases model capacity
- can sometimes improve interpretability

---

### O. Applications
- next-word prediction
- large language models
- machine translation
- music generation
- biological sequence modeling
- image processing via **Vision Transformers**

---

## 2) Key formulas — highlight + when to use

### 1. Perceptron

$$
z = \sum_{i=1}^{m} w_i x_i + b
$$

$$
\hat{y}=\phi(z)
$$

**Use when:** describing the basic neuron / perceptron / feedforward building block.

**Remember:** weighted sum + activation.

---

### 2. Naive feedforward timestep processing

$$
\hat{y}^{(t)} = f(x^{(t)})
$$

**Use when:** explaining why plain feedforward networks fail for sequence modeling.

**Remember:** no memory, no past context.

---

### 3. RNN hidden-state update

$$
h^{(t)} = f_W(x^{(t)}, h^{(t-1)})
$$

**Use when:** explaining recurrence, memory, or how RNNs process sequences.

**Remember:** current state depends on current input + previous hidden state.

---

### 4. RNN output

$$
\hat{y}^{(t)} = g(h^{(t)})
$$

**Use when:** explaining how RNN produces predictions after updating memory.

---

### 5. Total sequence loss

$$
\mathcal{L}_{total} = \sum_{t=1}^{T} \mathcal{L}^{(t)}
$$

**Use when:** discussing training of RNNs over sequences.

**Remember:** loss is computed across all timesteps.

---

### 6. One-hot encoding

$$
\mathbf{x}_i = [0,\dots,0,1,0,\dots,0]
$$

**Use when:** explaining basic token representation.

**Remember:** represents identity only, not semantic similarity.

---

### 7. Learned embedding

$$
i \rightarrow \mathbf{e}_i \in \mathbb{R}^{d}
$$

**Use when:** explaining embeddings in NLP.

**Remember:** dense vector, lower dimension, semantic structure possible.

---

### 8. Query-key similarity

$$
score(q,k)=q \cdot k
$$

**Use when:** explaining attention similarity.

**Remember:** higher dot product -> stronger relevance.

---

### 9. Attention weights

$$
A = softmax(scaled(QK^\top))
$$

**Use when:** explaining self-attention computation.

**Remember:** converts similarity scores into normalized relevance weights.

---

### 10. Attention output

$$
Output = AV
$$

**Use when:** explaining how attention extracts useful features.

**Remember:** weighted combination of value vectors.

---

## 3) Quick examples

### Example 1: Ball trajectory
Given only the current ball position, predicting the next location is hard.  
Given past positions too, prediction becomes easier.

**Lesson:** sequence history matters.

---

### Example 2: Sentiment classification
Input: a sentence  
Output: positive or negative label

**Task type:** sequence -> label

---

### Example 3: Machine translation
Input: English sentence  
Output: sentence in another language

**Task type:** sequence -> sequence

---

### Example 4: Next-word prediction
Input: “This morning I took my cat for a walk”  
Task: predict the next word

**Why important:** this is the core training idea behind language modeling.

---

### Example 5: RNN word prediction
Input fragment: “I love recurrent neural”  
Process words one by one, update hidden state each step, then predict next word.

---

### Example 6: One-hot vs embedding
Word: “cat”

- One-hot: a sparse vector with 1 only at cat’s index
- Embedding: a dense learned vector for “cat”

**Exam point:** embeddings are more expressive than one-hot encoding.

---

### Example 7: Attention search analogy
Query: “deep learning”  
Keys: descriptors of many videos  
Value: actual content of the most relevant video

**Lesson:** attention is like searching for what matters most.

---

### Example 8: Word relations in self-attention
Sentence fragment: “he tossed the tennis ball to serve”

Possible strong relations:
- toss <-> ball
- tennis <-> ball

**Lesson:** attention identifies which words are related.

---

### Example 9: Music generation
Train an RNN on note sequences.  
Given previous notes, predict the next note.

**Same idea as:** next-word prediction, but for music.

---

### Example 10: Vision Transformer
Even images can be processed with Transformer-style attention models.

**Lesson:** attention is not only for language.

---

## 4) Exam tips

### What is definitely exam-worth knowing
Know these cold:
- what sequence modeling is
- why feedforward networks are insufficient
- what hidden state means in an RNN
- recurrence formula
- why RNNs struggle with long sequences
- vanishing vs exploding gradients
- why LSTM was introduced
- one-hot vs embedding
- query / key / value
- dot product similarity
- softmax attention weights
- why Transformers are better than plain RNNs for many tasks

### Best way to explain RNN in one sentence
“An RNN processes a sequence one timestep at a time, updating a hidden state that carries memory from earlier timesteps.”

### Best way to explain attention in one sentence
“Attention lets the model compare parts of the sequence to each other and focus on the most relevant information.”

### Best way to compare RNN and Transformer
- **RNN:** sequential, recurrent, memory in hidden state
- **Transformer:** non-recurrent, attention-based, processes sequence more efficiently in parallel

### If a short-answer question asks “why embeddings?”
Answer:
- neural networks need numbers
- one-hot is sparse and weak semantically
- embeddings are dense and can capture similarity

### If asked “why positional encoding?”
Answer:
- Transformers process all tokens together
- without positional information, order would be lost

### If asked “why attention?”
Answer:
- to identify which elements in the sequence matter most to each other
- to avoid the bottlenecks of pure recurrence
- to better handle long-range dependencies

### If asked “why are Transformers powerful?”
Answer:
- they model token-to-token relationships directly
- they parallelize better than RNNs
- they are the foundation of modern LLMs

---

## 5) Common mistakes

### Mistake 1
**Saying an RNN sees the whole sequence at once.**  
Wrong. Standard RNNs process one timestep at a time.

### Mistake 2
**Saying one-hot encoding captures word meaning.**  
Wrong. One-hot only identifies the token; it does not represent similarity.

### Mistake 3
**Confusing hidden state with output.**  
Hidden state = internal memory.  
Output = prediction produced from hidden state.

### Mistake 4
**Forgetting that RNN weights are shared across time.**  
Same RNN parameters are reused at each timestep.

### Mistake 5
**Thinking BPTT is a completely different idea from backpropagation.**  
It is backpropagation extended across time in an unrolled recurrent network.

### Mistake 6
**Mixing up exploding and vanishing gradients.**
- exploding = too large
- vanishing = too small

### Mistake 7
**Saying attention removes the need for order information.**  
Wrong. Without recurrence, Transformers still need positional information.

### Mistake 8
**Mixing up Q, K, V.**
- Query = what is looking
- Key = what is being matched against
- Value = information to retrieve

### Mistake 9
**Thinking attention weights are raw dot products.**  
Not final ones. Dot products are scaled and then normalized by softmax.

### Mistake 10
**Saying Transformer = only for text.**  
Wrong. Also used in biology and computer vision.

---

## 6) High-yield comparison table

| Topic | Feedforward NN | RNN | Transformer |
|---|---|---|---|
| Handles order naturally? | No | Yes | Yes, with positional encoding |
| Processes sequentially? | No | Yes | No, can process whole sequence |
| Keeps memory of past? | No | Yes, hidden state | Yes, via attention relationships |
| Easy to parallelize? | Yes | No | More than RNN |
| Long-range dependencies | Weak | Often difficult | Stronger in practice |
| Core mechanism | weighted layers | recurrence | self-attention |

---

## 7) Minimal answer templates for exam writing

### “What is sequence modeling?”
Sequence modeling is learning from ordered data where earlier elements influence later predictions, such as in text, speech, ECG, music, and other time-dependent data.

### “What is an RNN?”
An RNN is a neural network for sequential data that updates a hidden state over time so the current output can depend on both current input and past context.

### “Why are RNNs hard to train?”
Because gradients propagated through many timesteps can vanish or explode, making long-term dependencies difficult to learn.

### “Why use embeddings?”
Because words must be converted to numerical vectors, and learned embeddings provide dense representations that can encode similarity between words.

### “What is attention?”
Attention is a mechanism that measures relevance between elements of an input and uses those relevance weights to extract the most important information.

### “What is a Transformer?”
A Transformer is a sequence model built around self-attention rather than recurrence, enabling efficient parallel processing and strong performance on long-range dependencies.

---

## 8) Completeness check

### Are any key concepts missing?
This cheat sheet includes the major concepts from the lecture:
- sequence modeling motivation
- perceptron / feedforward basis
- RNN recurrence and hidden state
- sequence loss and BPTT
- vanishing / exploding gradients
- LSTM motivation
- vocabulary / one-hot / embeddings
- variable length and long-range dependencies
- RNN limitations
- attention intuition
- query / key / value
- positional encoding
- self-attention equations
- Transformer and multi-head attention
- major applications

### Is it sufficient for exam use?
Yes for a lecture-level open-book exam on this specific topic, especially for:
- concept questions
- compare/contrast questions
- formula interpretation
- short explanation questions

What it does **not** include in depth:
- exact LSTM gate equations
- exact attention scaling constant
- detailed derivations
- implementation details in TensorFlow/PyTorch

If the exam is derivation-heavy, those would need to be added.  
If the exam is concept-focused, this sheet should be sufficient.

### Final improvement notes
For open-book exam use, the most important things to locate quickly are:
- the RNN recurrence equation
- the sequence loss equation
- the attention equations
- the RNN vs Transformer comparison
- the list of common mistakes
