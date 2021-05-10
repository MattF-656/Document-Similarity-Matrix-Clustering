# Document-Similarity-Matrix-Clustering
Document similarity comparison and clustering using similarity matricies and matrix clustering algorithms in the presence of noisy text data.

## Latent Dirichlet Allocation (LDA)
LDA is a generative Bag-of-Words model that uses probabilities to assign words to a topic that potentially insighted the use of that word originally. 
### Pro
- It is a learned model and therefore can assign unseen documents to a topic that it generated during training.
### Con
- This method does not generate good topics in the presence of noisy data. That is a set of many documents that have a proportionally small number of words that are used in a majority of the documents. This results in a large amount of overlap for the weighted keywords that the model determines important indicators of a topic when assigning documents.
- It is then difficult to distinguish the difference in meaning between the resulting topics.
- This is largely a result of the Bag-of-Words portion of the data preparation for the model. The word becomes disassociated from its original document and therefore in the presence of many of the same word it losses its context to a greater extent than when the occucerence of that word is proportionally less.

## Document Similarity Matrix Clustering (DSCM)
DSCM is a method of topic creation that considers all documents versus all the other documents in question and does not decompose the set of documents in question into a Bag-of-Words keeping proportionally high frequency words within the context of their original document.
### Pro
- As a result of the documents not being decomposed, in the presence of proptionally high frequency words, this method can still generate distinguishable topic groups of documents.
### Con
- DSCM is not a learned model in that its results can not be used to assign an unseen document to a previously created topic. It has to consider all documents in question everytime it needs to create topic clusters.

## What DSCM Does
- Creates a n x n similarity matrix using the desired document similarity scoring method
  - Jaccard
  - TF-IDF
  - BERT
  - USE
  - Word2Vec
- Optimizes the number of topics using an affinity matrix conversion of the similarity matrix, and then perform an eigen decomposition of that affinity matrix identifying the largest gap corresponding to the number of clusters by eigengap heuristic
- Performs spectral clustering on the original similarity matrix with the optimized best number of topics to assign documents to a group
- Topic assignments can then be mapped back to the original comment, grouped by cluster assignment, and then view the generated topic groupings
