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
First, we perform some preliminary data exploration, such as looking at the breakdown of songs per album, total song count, missing values, and a distribution plot showcasing the average character count across the songs: <br/>
<img src="https://i.imgur.com/iWjQ8oj.png" height="50%" width="50%" alt="albums"/> <img src="https://i.imgur.com/DXQ7La6.png" height="50%" width="50%" alt="number of songs"/> <img src="https://i.imgur.com/Eudsuwz.png" height="50%" width="50%" alt="dist plot"/>
<br />
Looking at the song counts, it looks like we have some albums containing only 1 song. It's safe to say we can consider these as singles.
<br /> 
<br />
Looking into the 'NaN' values, we can determine that these songs are also either singles or remixes.
<img src="https://i.imgur.com/NvVFAkT.png" height="65%" width="65%" alt="albums"/>
<br />
<br />
We can remedy this by creating two functions to group all singles and remixes into a single album labeled "Singles":
<img src="https://i.imgur.com/hbQQXpE.png" height="50%" width="45%" alt="albums"/><img src="https://i.imgur.com/KvYhI4g.png" height="50%" width="50%" alt="albums"/>
<br />
<br />
We also explore the word frequencies across all the songs using a bar chart and WordCloud. This will give us a better idea of the overarching theme to expect from Khalid's songs:  <br/>
<img src="https://i.imgur.com/QX19huj.png" height="50%" width="50%" alt="bar_chart"/><img src="https://i.imgur.com/x3Eg7ZU.png" height="50%" width="50%" alt="word cloud"/>
We can infer the theme from the most common lyrics to revolve around the experience of love and the associated emotions and challenges. It touches on the idea of knowing someone or something deeply, the innocence and passion of youth, the intensity of feelings, the passage of time, moments of foolishness or naivety, and the significance of night as a setting for introspection or romantic encounters. 
<br />
<br />
Now for the fun part - building our model!  <br/>
<img src="https://i.imgur.com/nCIbXbg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Confirm your selection:  <br/>
<img src="https://i.imgur.com/cdFHBiU.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Wait for process to complete (may take some time):  <br/>
<img src="https://i.imgur.com/JL945Ga.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Sanitization complete:  <br/>
<img src="https://i.imgur.com/K71yaM2.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
Observe the wiped disk:  <br/>
<img src="https://i.imgur.com/AeZkvFQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
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
