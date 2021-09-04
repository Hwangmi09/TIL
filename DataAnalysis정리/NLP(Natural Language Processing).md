# NLP(Natural Language Processing)

## 01. 텍스트 전처리(Text Processing)

### 1. 토큰화(Tokenization)

- 코퍼스(corpus) : 자연언어 연구를 위해 특정한 목적을 가지고 언어의 표본을 추출한 집합

- 토큰화 : 코퍼스(corpus)에서 토큰(token)이라 불리는 단위로 나누는 작업

  ```python
  import nltk
  nltk.download('punkt')
  nltk.download('stopwords') # 불용어
  ```

  <영어>

  1. 단어 토큰화

  ```python
  from nltk.tokenize import word_tokenize
  print(word_tokenize(text))
  
  from nltk.tokenize import WordPunctTokenizer
  print(WordPunctTokenizer().tokenize(text))
  
  from nltk.tokenize import TreebankWordTokenizer
  print(TreebankWordTokenizer().tokenize(text))
  
  from tensorflow.keras.preprocessing.text import text_to_word_sequence
  print(text_to_word_sequence(text))
  
  ```

  2. 문장 토큰화

  ```python
  from nltk.tokenize import sent_tokenize
  print(sent_tokenize(text))
  
  # 한글 문장 토큰화(KSS;Korean Sentence Splitter)
  !pip install kss
  import kss
  print(kss.split_sentence(text))
  ```

  

  3. 품사(POS) 태깅

  ```python
  nltk.download('averaged_perceptron_tagger')
  from nltk.tag import pos_tag
  pos_tag(word_tokenize(text))
  ```

  <한국어>

  1. 단어 토큰화

  ```python
  # KoNLPy 설치
  !pip install Konlpy
  
  from konlpy.tag import Okt
  okt = Okt()
  okt.morphs(text) # 형태소(의미를 가진 가장 작은 단위) 분석
  okt.nouns(text) # 명사 추출
  
  from konlpy.tag import Kkma
  kkma = Kkam()
  kkma.morphs(text) # 형태소 분석
  kkma.nouns(text) # 명사 추출
  ```
  
  2. 문장 토큰화
  
  ```python
  # KSS(Korean Sentence Splitter)
  !pip install kss
  
  import kss
  print(kss.split_sentences(text))
  ```
  
  3. 품사(POS) 태깅
  
  ```python
  okt.pos(text)
  kkma.pos(text)
  ```

### 2. 정제 및 정규화

- 길이가 1~2인 단어들을 정규 표현식을 이용하여 삭제 : 한국어는 이 과정을 하지 않음.

### 3. 어간 추출(Stemming) 및 표제어 추출(Lemmatization)

 1. 표제어 추출(Lemmatization)

    - 표제어 : 기본 사전형 단어, (am, are, is -> be)

    ```python
    nltk.download('wordnet')
    from nltk.stem import WordNetLemmatizer
    WordNetLemmatizer().lemmatize(word)
    WordNetLemmatizer().lemmatize(word, pos)
    ```

2. 어간 추출(Stemming)

   - 어간 : 단어의 의미를 담고 있는 단어의 핵심 부분

   ```python
   from nltk.stem import PorterStemmer
   PorterStemmer().stem(word)
   
   from nltk.stem import LancasterStemmer
   LancasterStemmer().stem(word)
   ```

### 4. 불용어(Stopwords)

- 불용어 : 문장에서는 자주 등장하지만 실제 의미 분석을 하는데는 거의 기여하는 바가 없는 단어

```python
from nltk.corpus import stopwords
stop_words = set(stopwords.words('english'))
stop_words.add() # 추가하고 싶은 단어 추가 가능
```

