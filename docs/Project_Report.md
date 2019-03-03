# PROJECT Report 

**Focus:** Unsupervised Learning and NLP

------

> **Problem Statement:** 
>
> - What should a data scientist's LinkedIn summary look like?

  

### Project Description

LinkedIn is the number one social platform that is used by professionals to network with each other. This plaftorm also provides individuals a way to showcase their work experiences, abilities, and expertise.  These information are typically used by job recruiters as a measure of fitness for a given job position. To appeal to job recruiters, how should one write this 1-2 paragraphs of LinkedIn summary? In particular, how should a scientist in academia (transitioning into data science or related field) write his/her bio in a way that appeals to recruiters?   

**Objectives:**

- [x] Collect text data by webscraping or using API and use NLP to analyze it 

- [x] Use unsupervised learning (dimensionality reduction, topic modeling) to provide insights into the dataset

- [x] Create a recommender system that can be used to suggest other related articles

  

### Methodology: data acquisition, pre-processing, and modeling

- Due to recent legal issues surrounding [scraping LinkedIn data](https://techcrunch.com/2016/08/15/linkedin-sues-scrapers/), I decided to use summaries of scientists that are archived on wikipedia. These articles were collected using [wikipediaapi](https://pypi.org/project/Wikipedia-API/), which collects one title at a time. I created a (for-loop) function to iterate this process over a list of titles, generated from [PETSCAN](https://petscan.wmflabs.org/). To collect this list, articles with **Category: Scientists** (at depth:2) were requested, and PETSCAN returned ~15,000 articles under this category. Unfortunately, a bunch of _"related_" articles were also included in this acquisition, even though the articles do not correspond to _"real"_ scientists. For example, 'Data' - a character from Star Trek - was also included in this group.  

- Preliminary cleaning was done on this dataset, which includes removing duplicates and dropping unrelated titles. This process reduced the total number of articles to  ~13,000 (a more extensive cleaning would be needed to remove all _'related'_ articles). 

- To convert article summaries into document-to-term matrix, I used the TF-IDF vectorizer, removed (english) stopwords & punctuations, and included tri-grams.  This process resulted in a matrix with ~13,000 rows and ~1.2 million features (words).

- I used Non-negative Matrix Factorization (NMF) to reduce the matrix dimension and to model the appropriate topics for these documents. Nine topics seemed reasonable, as 9 components seem to capture 80% of the explained variance ratio in a TruncatedSVD model (I ran prior to topic modeling). Also, the collection of top keywords appearing in the each topic seem to convey distinct ideas from another. 

- Cosine similarity was used to power the recommender system for a given query, i.e., a LinkedIn bio. Specifically, cosine similarities were calculated between a given query vector and the rows of the matrix (wiki articles). The top 10 result was then considered to be the most related to a given query (LinkedIn bio).    

  

### Result of topic modeling and recommendation of similar scientists    

- Topic modeling resulted in 9 distinct topics, in which the top 8 keywords in one NMF component seem to uniquely describe a particular topic  (**Figure 1**).  Based on the top keywords, topics were assigned as scientists that relate to:
  1. Academia
  2. Comic books (*)
  3. Indian
  4. Fictional characters (*)
  5. European
  6. Russian
  7. Computer (data scientists included)
  8. TV characters (*)
  9. Physics 
- Topics with asterisks (**Topic-2**, **-4**, and **-8** are not real scientists (or persons). These were unintentionally collected, as they must have some relations (on some level) with articles categorized under `Category:scientists`  (as described above). **Topic 5** (European) includes repeated keywords of months e.g., april, march, february, etc., corresponding to people's date of births in the wikipedia summaries. (Additional cleaning in the future could fix this issue). 

![Figure1](./Figures/wordclouds.png)

**Figure 1**. Top 8 keywords appearing in each of the nine modeled topics. Clouds created manually on powerpoint, to avoid redundant and repetitive words. Variation in font sizes were added for visual effects, but it does not reflect the word's frequency in the documents.        

- The distribution of topics in the dataset is illustrated using t-SNE representation (**Figure 2**). Each article is assigned 1 topic, based on its highest topic probability. Distances in the hyperdimensional space is also reflected in this 2D projection, i.e., points that are close to each other correspond to similar documents. 

  ![Figure2](./Figures/nmf_tsne.png)

  **Figure 2.** t-SNE representation of various articles about scientists in different domains, archived in Wikipedia. Each point corresponds to an article (i.e., scientist) and the assigned color reflects its highest probability for a given topic.  

- Data scientists are included in the computer scientist group. In particular, Hadley Wickham's description in wikipedia is considered by the model as a computer scientist (gray), with high similarity with other scientists in academia (blue) (**Figure 3**). As it turns out Hadley Wickham - a famous data scientist - is also an adjunct professor in Auckland, New Zealand. Hence, Wickham's position in the t-SNE is positioned near the academia group.    

  Z![Figure3](./Figures/computersScientists.png)

  **Figure 3**. t-SNE representation of wikipedia articles about scientists related to academia (blue) and to computer science (gray). Each point correspond to an article (i.e., scientist) and the assigned color reflects its highest probability for a given topic. 

- The 10 articles that are most similar to Hadley Wickham is shown in **Table 1**. Based on cosine similarity, articles that are closely related to Hadley Wickham describe computer (data) scientists, as well as others in academia. These similarities are consistent with the close proximity of the corresponding points in the t-SNE representation (**Figure 3**).

![Figure3](./Figures/Hadley.png)

**Table 1**. List of top 10 most related scientists to Hadley Wickham, based on wikipedia summaries. Each row corresponds to an observation, i.e., NMF-transformed vector of a scientist/article. Values in each column represent elements in the vector, expressed in percentages. For each observation, the topic assigned in t-SNE (Figure 3) reflects the highest percentage among the 9 topics.  

  



> **Problem Solution:**
>
> - What should a data scientist's LinkedIn summary look like? Based on Wiki summaries, most data scientists descriptions look like computer scientists' summaries.       



### Future work

- *More cleaning and collecting quality data targeting the data science field* . At the current stage, the data is still messy! There's quite a handful of _"fake"_ scientists, which are  'fictional', 'comic', and 'TV' characters that appear in the topics. It would be nice if LinkedIn data is freely available. The number of data scientists in wikipedia is also limited. Hence, the model coulndt distinguish a data scientist vs. a computer scientist. It would be interesting to combine this text exploration with a supervised learning project that classifies whether a person categorized as a computer/data scientist (on LinkedIn) actually works as a data scientist.    

- *Automated text generation*. In a distant future, it would be exciting to create a deep learning model that generates a full description about someone, given specific attribute about him/her as the _seed_.                    

  

### Data Source and toolsets

**Data sources:**

- [Wikipedia](https://en.wikipedia.org/wiki/Main_Page)

**Tools:**

- Data acquisition: `PetScan`, `wikipediaapi`
- NLP and data analysis: `Pandas`, `seaborn`, `nltk`
- Unsupervised learning: `Scikit-learn`, `TruncatedSVD`,   `nmf`, `kmeans`
- Data visualization: `t-SNE` and `U-MAP` 
- Model deployment: `D3`, `Flask`, hosted on `Heroku` (under development)

**Attribution:**

- I stumbled upon t-SNE as I was looking at this [Kaggle](https://www.kaggle.com/xdsarkar/nips-papers-visualized-with-nmf-and-t-sne) kernel.

  

------



