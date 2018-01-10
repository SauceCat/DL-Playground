
## Represent word as a vector
To represent a word by a vector with certain dimension.  

## Notebooks
- [Explore text preprocessing](https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/text_preprocess.ipynb)
- [Explore text preprocessing for Chinese](https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/jieba_test.ipynb)
- [Gensim word2vec tutorial](https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/word2vec.ipynb)
- [Gensim fastText tutorial](https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/FastText_Tutorial.ipynb)
- [Load pretrained vectors in Gensim](https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/load_pretrained_vectors.ipynb)

## Packages
- nltk
- Gensim
- Jieba (for Chinese)
<br><br>

## one-hot vector  
Represent every word as an (V, 1) vector, with all 0s and one 1 at the index of that word
in the sorted english language and V is the size of the vocabulary.  
- represent each word as a completely independent entity
- does not provide any notion of similarity between words
    
  <br><img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/one-hot.png" width=60%/>
  <br><br>
  
## SVD based methods
Generate an co-occurrence matrix, X. Apply SVD on X, get X = USV^T. 
Select the first k columns of U to get a k-dimensional word vectors.  
- two ways to construct **word co-occurrence matrix**
  - **Word-Document Matrix**  
  Loop over billions of documents and for each time word i appears in document j, we add one to entry Xij.  
  Shape of X: (V, M), M scales with the number of documents.  
  - **Window based Co-occurrence Matrix**   
  Count the number of times each word appears inside a window of a particular size
  around the word of interest. The matrix X becomes affinity matrix.  
  Shape of X: (V, V)  

- Applying **SVD** to the cooccurrence matrix  
  <br><img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/SVD.png" width=60%/> 
  <img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/reduct_SVD.png" width=60%/><br>  
  
- **Conclusions**  
SVD based methods do not scale well for big matrices (always sparse) and it is hard to incorporate new words or documents.
However, count-based method make an efficient use of the statistics.  
<br><br>

## Iteration Based Methods - Word2vec
Iteration-based methods capture cooccurrence of words one at a time instead of capturing all cooccurence 
counts directly like in SVD method. Word2vec is actually a software package includes 2 algorithms (CBOW, Skip-gram) 
and 2 training methods (negative sampling and hierarchical softmax).  

### continuous bag-of-words (CBOW)
Predicts a center word from the surroudning context in terms of word vectors. V is the input word matrix,
U is the output word matrix. For each word we want to learn both the input and output vectors.  
<br><img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/CBOW.png" width=30% /><br> 
- Generate one hot word vectors for the input context with size **2m**:   
**(x(c-m),...,x(c-1),x(c+1),...,x(c+m) with shape (V, 1))**  
- Get the embedded word vectors for the context: (**n** is the embedded dimension)  
**(v(c-m)=Vx(c-m),v(c-m+1)=Vx(c-m+1),...,v(c+m)=Vx(c+m) with shape (n, 1))**  
- Average all context word vectors, get **v(hat)**.  
- Generate a score vector **z=Uv(hat)(with shape(V, 1))**.  
- Turn the scores into probabilities using **softmax** function.  
- Calculate the cross entropy **H(y(hat), y)** between the predicted probability distribution and 
the one hot vector of the true center word.  
<br><img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/CBOW_obj.png" width=60% /><br> 
    
### Skip-gram (SG)  
Predicts the context words from a center word. V is the input word matrix,
U is the output word matrix. For each word we want to learn both the input and output vectors.  
<br><img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/SG.png" width=30% /><br> 
- Generate one hot word vector for the center word **x, with shape(V, 1)**   
- Get the embedded word vector for the center word: **v(c)=Vx with shape(n, 1)**   
- Generate a score vector **z=Uv(c)(with shape(V, 1))**.  
- Turn the scores into probabilities using **softmax** function.  
- Calculate the cross entropy **H(y(hat), y)** between the predicted probability distribution and 
the one hot vector of the true context words.  
<br><img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/SG_obj.png" width=60% />

### Training methods
- **Negative Sampling**  
For softmax function, the summation over the whole vocabulary(V) is computationally huge. 
A simple idea is to approximate it by sampling the negative samples.  
We can build a new objective function that tries to maximize the probability of a word 
and context being in the corpus data if it indeed is, and maximize the probability of a word 
and context not being in the corpus data if it indeed is not. We take a simple maximum 
likelihood approach of these two probabilities.  
    <br><img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/NS_01.png" width=60% /><br>  
Maximizing the likelihood is the same as minimizing the negative log likelihood. 
    <br><img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/NS_02.png" width=60% /><br>  
We can generate the negative half of cost by randomly sampling this negative from the word bank.
Regarding the sampling function **P(W)**, what seems to work best is the Unigram Model raised to the power of 3/4.  
    <br><img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/NS_03.png" width=30% /><br> 
    - Negative Sampling objective function for **CBOW**  
        <br><img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/NS_CBOW.png" width=40% /><br>  
    - Negative Sampling objective function for **Skip-gram**  
        <br><img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/NS_SG.png" width=45% />
    
- **Hierarchical Softmax**  
Hierarchical softmax uses a binary tree (like a binary Huffman tree, which assigns frequent
words shorter paths in the tree) to represent all words in the vocabulary.
Each leaf of the tree is a word, and there is a unique path from root to leaf.
In this model, **there is no output representation for words.**
Instead, each node of the graph (except the root and the leaves) is associated to a vector 
that the model is going to learn.  
    <br><img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/HS.png" width=50%/><br>   
Let **L(w)** be the number of nodes in the path from the root to the leaf **w**.
For instance, **L(w2)** in the above figure is 3.
Let’s write **n(w, i)** as the **i-th** node on this path with associated vector **vn(w,i)**.
So **n(w, 1)** is the root, while **n(w, L(w))** is the father of **w**.
Now for each inner node **n**, we arbitrarily choose one of its
children and call it **ch(n)** (e.g. always the left node). Then, we can
compute the probability as  
    <br><img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/HS_PROB.png" width=60%/><br>   
For example, taking w2 in the above figure, we must take two left edges and then a right 
edge to reach w2 from the root, so  
    <br><img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/HS_example.png" width=60%/><br>   
To train the model, our goal is still to minimize the negative log likelihood **-log P(w|wi)**.
But instead of updating output vectors per word, we update the vectors of the nodes in the binary 
tree that are in the path from root to leaf node.
<br><br>

## fastText
Same as word2vec, but enrich the vocabulary with word ngrams.  
For the fastText method provided by Gensim, hyperparameters for training the model follow the same pattern as Word2Vec, but with three additional parameters: 
- min_n: min length of char ngrams (Default 3)
- max_n: max length of char ngrams (Default 6)
- bucket: number of buckets used for hashing ngrams (Default 2000000)  

Parameters min_n and max_n control the lengths of character ngrams that each word is broken down into while training and looking up embeddings. If max_n is set to 0, or to be lesser than min_n, no character ngrams are used, and the model effectively reduces to Word2Vec.
<br><br>

## Global Vectors for Word Representation (GloVe)

### Comparison with previous models
- **count-based and matrix factorization models (e.g. LSA, HAL):** effectively leverage global statistical information, sub-optimal vector space structure.
- **shallow window-based models (e.g. the skip-gram and the CBOW models):** capture complex linguistic patterns beyond word similarity, but fail to make use of the global co-occurrence statistics.
- **GloVe:** trains on global word-word co-occurrence counts and thus makes efficient use of statistics, shows state-of-the-art performance.

### Methods
- **Co-occurrence Matrix:** 
  - X: word-word co-occurrence matrix
  - Xij: number of times word j occur in the context of word i
  - Xi: the number of times any word k appears in the context of word i
  - Pij = P(wj|wi) = Xij/Xi : the probability of j appearing in the context of word i
  
- **Least Squares Objective**
  - the probability of word j appears in the context of word i:   
  <img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/glove01.png" width=35%>   
  
  - the implied global cross-entropy loss can be calculated as:   
  <img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/glove02.png" width=35%>    
  
  - group together the same values for i and j:   
  <img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/glove03.png" width=30%>   
  
  - cross-entropy loss requires the expensive summation over the entire vocabulary. Instead, we use a least square objective in which the normalization factors in P and Q are discarded:   
  <img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/glove04.png" width=30%>     
  
  - -Xij often takes on very large values and makes the optimization difficult:   
  <img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/glove05.png" width=35%>   
  
  - we can also adjust the weighting factor Xij:  
  <img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/glove06.png" width=35%>      

- **Conclusion**    
GloVe model efficiently leverages global statistical information by by training only on the nonzero elements in a wordword
co-occurrence matrix, and produces a vector space with meaningful sub-structure.
<br><br>

## Evaluation of Word Vectors
### Intrinsic Evaluation Example: Word Vector Analogies: a : b : : c : ?
<img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/eval01.png" width=35%>    

Ideally, we want **xb - xa = xd - xc** (For instance, queen – king = actress – actor). This implies
that we want xb - xa + xc = xd. Thus we identify the vector xd which maximizes the normalized dot-product between the two word vectors.   
  
**Semantic testing example:**   
<br><img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/eval02.png" width=50%>     

**Syntax testing example:**   
<br><img src="https://github.com/SauceCat/NLP-Playground/blob/master/word_embedding/images/eval03.png" width=35%> 

### Intrinsic Evaluation Example: Correlation Evaluation
Another simple way to evaluate the quality of word vectors is by asking humans to assess the similarity between two words on a fixed scale (say 0-10) and then comparing this with the cosine similarity between the corresponding word vectors. This has been done on various datasets that contain human judgement survey data.   
