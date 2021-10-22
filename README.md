# Text-Preprocess-in-Bahasa
Data is data crawling from the Twitter with query "Vaksin Covid-19".
!pip install nltk.
!pip install emoji
!pip install Sastrawi
!pip install regex

import nltk
nltk.download('punkt')

import pandas as pd
import re
import nltk
import emoji

pd.set_option('display.max_colwidth',500)

**2. Load Dataset**
dataraw = pd.read_excel('Hasil_Data_Crawling.xlsx')
data =pd.DataFrame(dataraw['text']) 
data

**2. Character Removing**
def preproses(tweets) :
    tweets = tweets.encode('ascii','ignore').decode('utf-8')
    tweets = re.sub(r'[^\x00-\x7f]',r'',tweets)
    tweets = re.sub(r'[_(){}[]]+','',tweets)
    tweets = re.sub(r'@[A-Za-z0-9]+','',tweets)
    tweets = re.sub(r'#[A-Za-z0-9]+','',tweets)
    tweets = re.sub(r'_[A-Za-z0-9]+','',tweets)
    tweets = re.sub(r'-[A-Za-z0-9]+','',tweets)
    tweets = re.sub(r':[A-Za-z0-9]+','',tweets)
    tweets = re.sub(r'https?:\/\/\S+','',tweets)
    tweets = re.sub(r'\d+','',tweets)
    tweets = re.sub(r'@+','',tweets)
    tweets = re.sub(r':+','',tweets)
    tweets = re.sub(r',+','',tweets)
    tweets = re.sub(r'_+','',tweets)
    tweets = tweets.lower()
    allemot = [str for str in tweets]
    listemot = [x for x in allemot if x in emoji.UNICODE_EMOJI]
    tweets = ' '.join([str for str in tweets.split()if not any (y in str for str in listemot)])
    return tweets
data['text']= data['text'].apply(preproses)
data

**3. Stopword Removal**
from Sastrawi.StopWordRemover.StopWordRemoverFactory import StopWordRemoverFactory
factory = StopWordRemoverFactory()
stopword = factory.create_stop_word_remover()

text_stopwords = []
for index,row in data.iterrows ():
    text_stopwords.append(stopword.remove(row['text']))  
    
data['text'] = text_stopwords
data

**4. Stemming**
from Sastrawi.Stemmer.StemmerFactory import StemmerFactory

factory= StemmerFactory()
Stemmer = factory.create_stemmer()
Stemming = []

for index,row in data.iterrows():
    Stemming.append(Stemmer.stem(row['text']))
    
data ['text']=Stemming
data

**5. Remove Dupicate**
def fungsi_ku(x):
  return list(dict.fromkeys(x))
daftar_ku = fungsi_ku(data['text'])
baru = pd.DataFrame(daftar_ku)
baru
