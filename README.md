# Document-Similarity-Matrix-Clustering
Document similarity comparison and clustering using similarity matricies and matrix clustering algorithms in the presence of noisy text data.

## Latent Dirichlet Allocation (LDA)
LDA is a generative bag-of-words model that uses probabilities to assign words to a topic that potentially insighted the use of that word. 
### Pro
- It is a learned model, therefore it can assign unseen documents to a topic that it generated during training.
### Con
- This method does not generate desirable topics in the presence of noisy data. That is, a set of many documents that have a proportionally small number of words that make up a majority of the documents. This results in a large amount of overlap in the weighted keywords that the model determined are important indicators for assigning documents to a topic.
- It is then difficult to distinguish the difference in meaning between the resulting topics.
- This is largely a result of the bag-of-words portion of the data preparation for LDA. The word becomes disassociated from its original document and within the presence of a group of documents with a small number of high frequency words, the models ability to measure which topic the word originated from is lessend to an undesirable extent.

## Document Similarity Matrix Clustering (DSMC)
DSMC is a method of topic creation that considers all documents versus all the other documents in question and does not decompose the set of documents into a bag-of-words, keeping high frequency words within the context of their original document.
### Pro
- As a result of the documents not being decomposed, this method can still generate meaningfully different groups of smiliar documents in the presence of proptionally low, high frequency words.
### Con
- DSMC is not a learned model, its results can not be used to assign an unseen document to a previously created document group. It has to consider all documents in question everytime it needs to create similiar document groups.

## How DSMC Creates Similiar Document Groups
- Creates a n x n similarity matrix using the desired document similarity scoring method.
  - Jaccard
  - TF-IDF
  - BERT
  - USE
  - Word2Vec
- Optimizes the number of topics using an affinity matrix conversion of the similarity matrix, and then performs an eigen decomposition of that affinity matrix identifying the largest gap corresponding to the number of clusters by eigengap heuristic.
- Applies spectral clustering to the original similarity matrix with the optimized best number of topics to assign documents to a cluster.
- Cluster assignments are then mapped back to the original comment, grouped by cluster assignment, and then viewed as the generated document groups.

## Potential Areas for Improvement
- Yet to implement a pre-trained model for calculating similarity scores.
  - Latest version of DSMC has used Jaccard and TF-IDF.
  - While TF-IDF yields better results than Jaccard, it appears there is always 2-3 document groups that contain a majority of the documents and are mostly longer in length than the documents assigned to the other groups.
    - This is because the TF-IDF and Jaccard methods of calculating similarity are influenced by differences in lengths of documents hurting the accuracy of their scores between documents of noteable difference in length.
  - It is likely the pre-trained models can help with this.
- Refine optimization for the number of clusters.
  - Current choice of k for the affinty matrix is arbitrary and this choice effects the eigen values when determining k for spectral clustering.
    - Need to create way to systematically choose k for the affinity matrix
      - Potentially a gridsearch type solution, but this then creates the need to determine the best number of k from the eigen values therefore the quality of the clusters themselves.
  - Lower thresholds for determing cluster contents.
    - Overly specific clusters are being formed potentially as a result of k for the number of clusters being set too high by the optimization.
      - May contribute to a few large clusters as the similarities of the small groups is so high the algorithm has nowhere else to put them and these 2-3 clusters become 'uncategorizable' or 'catch-all' clusters.
      - Through, the use of pre-trained models may help to create more generalized representations of the documents regardless of length and remove this side effect of the clustering.
