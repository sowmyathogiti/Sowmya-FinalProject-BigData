# FinalProject-BigData
Author : [Sowmya Thogiti](https://github.com/sowmyathogiti)   
Data source : [The legend of sleepy hollow by Washington Irving](https://www.gutenberg.org/ebooks/41)   
Motive: Process text using Databricks Community Edition and PySpark.
## Languages and Tools:
Python language
Pyspark, Databricks Notebook, Pandas, MatPlotLib, Seaborn Language
## Key words:
- Resilient Distributed Dataset (RDD) 
- DataFrame 
- SparkContext(sc) 
- Collect()
## Begin with databrick:
Use below commands once the cluster is active by creating a new Notebook from databrick by selecting python as a language.  
1. Data Collection:
* First we need to import library to fetch data from any server request into a string
* To fetch data, we need t retrieve it into a folder
* Then move the data to distributed file system from source
```
import urllib.request
StringInURL = "https://raw.githubusercontent.com/sowmyathogiti/Sowmya-FinalProject-BigData/main/TheLegendOfSleepyHollow.txt"
urllib.request.urlretrieve(StringInURL, "/tmp/Hollow.txt")
dbutils.fs.mv("file:/tmp/Hollow.txt", "dbfs:/data/Hollow.txt")
```
2. Data cleaning:
* We need to use below commands for data cleaning
* 
```
inHollowRDD = HollowRawRDD.flatMap(lambda line : line.lower().strip().split(" "))
import re
cleanHollowRDD = inHollowRDD.map(lambda alpha : re.sub( r'[^a-zA-Z]' , '' , alpha))
```
3. Data filtering: 
* We can use library from pyspark for filtering as a remover
* Use filter method by providing lambda expression
```
from pyspark.ml.feature import StopWordsRemover
remover = StopWordsRemover()
stopwords = remover.getStopWords()
cleanHollowRDD.filter(lambda w : w not in stopwords)
```
4. Map to intermediate key-value pairs:
* Use map method by lambda expression for words above count by five times.
* collect all the reduced values by identifying uniquely with key and print it using below commands
```
IKVHollowRDD = cleanHollowRDD.map(lambda word : (word, 5))
resultsRDD  = IKVHollowRDD.reduceByKey(lambda res, value: res + value )
results = resultsRDD.collect()
```
5. Data visualization:
* Get list of words by below command 
```
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from collections import Counter
Sampletext = "A gay castles in a cloud that a summer sky cloud"
word_list = Sampletext.lower().split()
```
* Call the counter most_common function to get list of tuples
```
tuples = Counter(word_list).most_common()
print(tuples)
```
* Create graph plot using below commands by maplotlib
```
plt.figure(figsize=(10,5))
sns.barplot(xlabel, ylabel, data=df, palette="Blues_d").set_title(title)
```
## Output of graph:
![](https://github.com/sowmyathogiti/Sowmya-FinalProject-BigData/blob/main/graph.JPG)
## Image creation of word cloud using python:
The input to the program will be a paragraph copied from any website of your choice. With the paragraph as input, we can pre-process it and send it to the wordcloud package.
Use below commands:
```
pip install wordcloud #installing the wordcloud
pip install nltk
import nltk
nltk.download('popular') 
import matplotlib.pyplot as plt

from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
from wordcloud import WordCloud

class WordCloudGeneration:
    def preprocessing(self, data):
        # convert all words to lowercase
        data = [item.lower() for item in data]
        # load the stop_words of english
        stop_words = set(stopwords.words('english'))
        # concatenate all the data with spaces.
        paragraph = ' '.join(data)
        # tokenize the paragraph using the inbuilt tokenizer
        word_tokens = word_tokenize(paragraph) 
        # filter words present in stopwords list 
        preprocessed_data = ' '.join([word for word in word_tokens if not word in stop_words])
        print("\n Preprocessed Data: " ,preprocessed_data)
        return preprocessed_data

    def create_word_cloud(self, final_data):
        # initiate WordCloud object with parameters width, height, maximum font size and background color
        # call the generate method of WordCloud class to generate an image
        wordcloud = WordCloud(width=1600, height=800, max_font_size=200, background_color="black").generate(final_data)
        # plt the image generated by WordCloud class
        plt.figure(figsize=(12,10))
        plt.imshow(wordcloud)
        plt.axis("off")
        plt.show()

wordcloud_generator = WordCloudGeneration()
# you may uncomment the following line to use custom input
# input_text = input("Enter the text here: ")
import urllib.request
url = "https://raw.githubusercontent.com/sowmyathogiti/Sowmya-FinalProject-BigData/main/TheLegendOfSleepyHollow.txt"
request = urllib.request.Request(url)
response = urllib.request.urlopen(request)
input_text = response.read().decode('utf-8')
input_text = input_text.split('.')
clean_data = wordcloud_generator.preprocessing(input_text)
wordcloud_generator.create_word_cloud(clean_data)
```
Output:
![](https://github.com/sowmyathogiti/Sowmya-FinalProject-BigData/blob/main/wordCount.JPG)
## References:
* [Kaggle](https://www.kaggle.com/mchirico/quick-look-seaborn-wordcloud)
* [Word cloud section](https://www.section.io/engineering-education/word-cloud/)
* [PySpark](https://www.edureka.co/blog/spark-with-python-pyspark)
* [Databricks](https://community.cloud.databricks.com/)
