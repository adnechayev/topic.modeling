<h1>Topic Modeling with Artist Lyrics</h1>

<h2>Description</h2>
This project utilizes BERTopic, a topic modeling technique leveraging transformer embedding models, UMAP dimensionality reduction, HDBSCAN clustering, and c-TF-IDF cluster tagging. We take
a look at pop artist Khalid's lyrics in order to create sensible topic clusters, which we then run through an OpenAI API call to assign descriptions to the created topics.
<br />


<h2>Languages Used</h2>

- <b>Python</b> 

<h2>Environments Used </h2>

- <b>Google Colab</b>

<h2>Project walk-through:</h2>

<p align="center">
First, we perform some preliminary data exploration, such as looking at the breakdown of songs per album, total song count, missing values, and a distribution plot showcasing the average character count across the songs: 
<br/>
<br/>
<img src="https://i.imgur.com/iWjQ8oj.png" height="50%" width="50%" alt="albums"/> <img src="https://i.imgur.com/DXQ7La6.png" height="50%" width="50%" alt="number of songs"/> <img src="https://i.imgur.com/Eudsuwz.png" height="50%" width="50%" alt="dist plot"/>
<br />
Looking at the song counts, it looks like we have some albums containing only 1 song. It's safe to say we can consider these as singles.
<br /> 
<br />
Looking into the 'NaN' values, we can determine that these songs are also either singles or remixes.
<br/>
<br/>
<img src="https://i.imgur.com/NvVFAkT.png" height="65%" width="65%" alt="albums"/>
<br />
<br />
We can remedy this by creating two functions to group all singles and remixes into a single album labeled "Singles":
<br/>
<br/>
<img src="https://i.imgur.com/hbQQXpE.png" height="45%" width="45%" alt="albums"/><img src="https://i.imgur.com/KvYhI4g.png" height="50%" width="50%" alt="albums"/>
<br />
<br />
We also explore the word frequencies across all the songs using a bar chart and WordCloud. This will give us a better idea of the overarching theme to expect from Khalid's songs:
<br/>
<br/>
<img src="https://i.imgur.com/QX19huj.png" height="50%" width="50%" alt="bar_chart"/><img src="https://i.imgur.com/x3Eg7ZU.png" height="50%" width="50%" alt="word cloud"/>
We can infer the theme from the most common lyrics to revolve around the experience of love and the associated emotions and challenges. It touches on the idea of knowing someone or something deeply, the innocence and passion of youth, the intensity of feelings, the passage of time, moments of foolishness or naivety, and the significance of night as a setting for introspection or romantic encounters. 
<br />
<br />
Now for the fun part - <b>building our model!</b>
<br />
We want to look at several parameters when constructing our model. For the purpose of removing stopwords, we'll call on a vectorizer model:
<br />
<br/>
<img src="https://i.imgur.com/s7fjLlR.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<br />
<br />
In order for the BERTopic model to work with our list of lyrics, we need to transform them into vector embeddings. We'll essentially convert the string text into its numerical representation - in other words we're translating human "meaning" into machine "meaning". In our case, we're using the HuggingFace sentence transformer model 'all-mpnet-base-v2'  <br/>
<br/>
<img src="https://i.imgur.com/EAw2iOS.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<br />
<br />
Our vector embedding model yields a dense multi-dimensional vector space. For the purpose of topic clustering and extraction, we want to reduce the dimensionality of our vector representations. We do this via UMAP (Uniform Manifold Approximation and Production), which transforms our vectors into 2 or 3 dimensions. UMAP excels at this task because we can control how well local or global structures are preserved via the 'n-neighbors' parameter. Increasing the parameter creates larger clusters, however since we are dealing with a smaller dataset, we opt to set the value 'n-neighbors' to 2. We also use the default distance computing metric 'cosine' since our data has been vectorized in high dimensions.
<br/>
<br/>
<img src="https://i.imgur.com/WBJbK6O.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<br />
<br />
Now that we have our low-dimension vectors, we want to cluster them to eventually extract meaningful topics. To this end, we employ HDBSCAN - an hierarchical density-based clustering technique. The hierarchical aspect entails that the clustering will look for a logical "sequence" when considering different points together. This is something can later visualize to see how our BERTopic model determined the hierarchy. HDBSCAN being density-based means that it can cluster points together based on their density (how close they are to each other). This also means it can handle irregular shapes and outliers. Our parameters include the minimum cluster size, which we set to 2 due to the fact that any higher and we hardly get any topics generated, and any lower yields too many, possibly diluting any significant meaning per topic. All other parameters are set to default.   
<br/>
<br/>
<img src="https://i.imgur.com/sacf4ZK.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<br/>
<br/>
For topic extraction, we work with a technique called c-TF-IDF, a modified version of TF-IDF, for BERTopic modeling. While the traditional TF-IDF technique looks at the most relevent documents given a term, c-TF-IDF looks at the most relevent terms within a document instead. We add the parameter BM-25, a class based weighting measure that works better with smaller datasets.
<br/>
<br/>
<img src="https://i.imgur.com/xioKfde.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<br />
<br />
With all our parameters set, we can now run the model! Upon completion, we can look at the various topics generated (keep in mind that row '-1' is denoted to outliers):
<br/>
<br/>
<img src="https://i.imgur.com/BY7dJmy.png" height="65%" width="65%" alt="Disk Sanitization Steps"/>
<br/>
<br/>
We're also able to take a look at several visualizations where we can glean more information about the topics and how they might relate to each other. This series of barcharts shows the relevency score of term per topic:
<br/>
<br/>
<img src="https://i.imgur.com/lSvkOyB.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
<br/>
<br/>
We can take a look at a heatmap highlighting the similarity between the different topics:
<br/>
<br/>
<img src="https://i.imgur.com/eQBUOf1.png" height="75%" width="75%" alt="Disk Sanitization Steps"/>
<br/>
It's interesting to note here that most topics, excluding 4 and 7, have a high similarity score with each other. When we call on OpenAI API to generate descriptions for the topics, we can expect to see this similarity to present itself in analogous descriptions.
<br/>
<br/>
However before we do that, it's worthwhile to see how our BERTopic model determined hierarchy when producing topics:
<br/>
<br/>
<img src="https://i.imgur.com/pcXuEDf.png" height="70%" width="70%" alt="Disk Sanitization Steps"/>
<br/>
Reading it from right to left, we can see the storyline of how our model took certain terms and divided them into different branches, eventually leading to the creation of different topics. By taking a look at the first two nodes, we can see the most dominant terms:
<br/>
<br/>
<img src="https://i.imgur.com/iLtGu1k.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<br/>
Node 1
<br/>
<br/>
<img src="https://i.imgur.com/EZVasuK.png" height="50%" width="50%" alt="Disk Sanitization Steps"/>
<br/>
Node 2
<br/>
<br/>
<br/>
<br/>
We have all these terms grouped into topics, but how can we interpret them? Is there some theme we can glean from each topic? By calling on OpenAI API via a representation model, we can create an apt topic description for each of our generated topics. Representation models are alternative methods for fine-tuning topic representations. Rather than the default list of words we get via BERTopic's Bag-of-Words representation, we can call on different models to potentially produce different results for comparison. While most other models also give us a list of words, the OpenAI representation uses one of its LLM models (gpt-3-turbo in our case) to create an appropriate description for all the topics:
<br/>
<br/>
<img src="https://i.imgur.com/qxrxCqu.png" height="30%" width="30%" alt="Disk Sanitization Steps"/>
<br/>
<br/>
To finalize this project, I was curious to see a more detailed description a given topic, so I ran an OpenAI API call on a selected topic to see what ChatGPT could come up with, and compare it with the representation model:
<br/>
<br/>
<img src="https://i.imgur.com/KkbUQQJ.png" height="120%" width="120%" alt="Disk Sanitization Steps"/>
</p>
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
