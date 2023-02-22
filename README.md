
## CS321 - Software Engineering
### Isabella Feng
**Project 2:  Python2Vec**
*GitHub Repository: https://github.com/isabella-feng/CS421-project2*

### Abstract
In this project, I applied the Gensim Word2Vec model to source code of multiple python libraries to create Python2Vec, which is a machine learning structure that generate word embeddings based on Python code.

### Results
First off all, I downloaded a few Python repositories in an automated way using Python. Then, I crawled through the repositories looking for Python files, and aggregated the Python files found into a single text file. After counting the number of lines and words and tokenized to words, I put the training set to the Gensim Word2Vec model. 

Exploring the model, I discover that the Python2Vec model can accurately detect and perdict similar syntax. For example, it is able to find the closest words to "if" as elif, when, while, whether, etc. It also can find the closest words to terms, for example, it finds closest words for "numpy" as py, api, scipy, etc. To an extent it understands similar meanings of words pretty well, for example, it finds similar words to "number" as amount, part, ratio, presence, combination, total, numbers, etc.

It can also find similarity between words. For example, "numpy" and "math" are loosely similar with a similarity of 0.21401104. "true" and "false" are both booleans and are usually used in similar settings, so they have high similarity. "big_endian" and "define" are competely unrelated so they have very low similarity.

It can also find the words that doesn't match in a group of words. For example, among the words 'if', 'for', 'while' and 'string', 'string' is the only one that's not a syntax word. Python2Vec can successfully detect that. In another example, among the words 'string', 'integer', 'boolean', 'define', 'define' is the only one that's not a data type. Python2Vec can successfully detect that as well.

In analysis of the similarity between identifiers, we're able to have interesting discoveries. For one, the pair with highest similarity is 'ValueError' and 'TypeError', as both of them are errors and usually appear in similar settings. For another, common variable names, such as 'x' and 'y', also have high similarities, as they're usually just names and can be interchangeable. 

On the other hand, it seems like random variable names that have unrelated meanings, such as 'plt' & 'obj', 'res' & 'fig', have very low similarity with negative values.

Here below is the Heatmap showing the similarity between each pair of words. The yellower the color, the higher the similarity. 
![Fig 1. Similarity of pairs of identifiers](https://github.com/isabella-feng/CS421-project2/blob/main/Similarity_of_pairs_of_identifiers.png?raw=true "Fig 1. Similarity of pairs of identifiers")
<p style="text-align: center;">*Fig 1. Similarity of pairs of identifiers*</p>

Furthermore, I enhanced the way to flag identifiers as similar/dissimilar. I categorize the similarities into 3 categories: Similar pairs are the ones with similarity > 0.5; Not-so-similar pairs are the ones with 0 < similarity < 0.5; and Dissimilar pairs are the ones with similarity < 0. In this grayscale Heatmap below, Similar pairs are white, Dissimilar pairs are black, and Not-so-similar pairs are gray.
![Fig 2. Similarity Categorization](https://github.com/isabella-feng/CS421-project2/blob/main/Similarity_Categorization.png?raw=true "Fig 2. Similarity Categorization")
<p style="text-align: center;">*Fig 2. Similarity Categorization*</p>


### Discussions

Generally, my results are essentially similar to the ones presented in the original paper. Note that the *regress*' performance dip in slope error, which was brought to attention in the paper, is seen in my results. 

It is worth noting that the performance of *compare* is fair and is certainly not as bad as it was described in the paper. I deem that it is because we are using the simulated data instead of a real data, especially data from children. Therefore, since *compare* is bad with behaviors such as regression, it doesn't perform quite bad on simulated data with controlled variables. 

For two regression errors, the accuracy of all algorithms declines as opposed to some algorithms being invariant as shown in the paper. I suppose it may be due to the extent of regression error created, or the way that accuracy was calculated. 

### Extensions
- Create a version of the Warp algorithm that detects regressions and adapts to them.
Since my results for between-line regression errors do not show much variance among algorithms, I am creating this adapted version of Warp towards within-line regression. 
I use an algorithm to detect whether the collection of fixations has significant amount of within-line regression errors. If it has, *attach* is used instead of *warp*. 
The algorithm goes like this: 1) randomly select a fixation 2) check its next 10 fixations, and check whether there are more than 2 backward in x value, which means that there are regressions instead of return sweeps. Lastly, repeat these two steps for 10 times, and if 3 or more of them exhibit such regression behavior, use *attach*. Else use *warp*.
The result of *adapted_warp*, comparing to other algorithms after running on within-line regression errors for 100 times, is shown below. 
![Fig 6. adapted_warp on within-line regression error](https://github.com/isabella-feng/CS421-project1/blob/main/graphs/adatped_warp_comparison.png?raw=true "Fig 6. adapted_warp on within-line regression error")
My *adapted_warp* algorithm has a mean accuracy of  0.80, whereas *warp* has only 0.76. *adapted_warp* is superior than *warp* because it takes the best of *attach* method when encountering regressions. 

### Reference
The code for 10 algorithms is acquired from J# on W. Carr's GitHub https://github.com/jwcarr/drift.







