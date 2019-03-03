# Finding Similar Scientists on Wikipedia Articles
Project-4 at METIS data science bootcamp

**Focus**: Unsupervised learning and Natural Language Processing

  

## Project Description

> **Problem Statement:** 
>
> - What should a data scientist's LinkedIn summary look like? 

  

### Project Description

LinkedIn is the number one social platform that is used by professionals to network with each other. This plaftorm also provides individuals a way to showcase their work experiences, abilities, and expertise.  These information are typically used by job recruiters as a measure of fitness for a given job position. To appeal to job recruiters, how should one write this 1-2 paragraphs of LinkedIn summary? In particular, how should a scientist in academia (transitioning into data science or related field) write his/her bio in a way that appeals to recruiters in industry?   

**Objectives:**

- [x] Collect text data by webscraping or using API and use NLP to analyze it 
- [x] Use unsupervised learning (dimensionality reduction, topic modeling) to provide insights into the dataset
- [x] Create a recommender system that can be used to suggest other related articles



**Code, notebooks, and documents**

- [Project_Report.md](./docs/Project_Report.md), [Project_Presentation.pptx](./docs/Project4_Presentation.pptx), or [PDF](./docs/Project4_Presentation.pdf) - project report on markdown and powerpoint (or pdf) formats 
- [Step1_DataAcquisition.ipynb](./notebooks/Step1_DataAcquisition.ipynb) - notebook on importing dataset from Wikipedia 
- [Step2_Cleaning.ipynb](./notebooks/Step2_Cleaning.ipynb) - notebook on preliminary cleaning of the dataset
- [Step3_TopicModeling.ipynb](./notebooks/Step3_TopicModeling.ipynb) - notebook on _topic modeling_ using using Non-negative Matrix Factorization 
- [Step4_ClusteringViz.ipynb](./notebooks/Step4_RandomForest_Explainer.ipynb) - notebook on visualization of hyperdimensional document-to-term matrix using t-SNE and kMeans clustering using UMAP