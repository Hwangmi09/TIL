# Web Crawling

## 01. BeautifulSoup 사용법

- Local HTML 파일 열기

  ```python
  from bs4 import BeautifulSoup
  with open('00_Example.html') as fp:
      soup = BeautifulSoup(fp, 'html.parser')
  soup
  >> <!DOCTYPE html>
  
      <html lang="en">
      <head>
      <meta charset="utf-8"/>
      <meta content="width=device-width, initial-scale=1.0" name="viewport"/>
      <title>Web Crawling Example</title>
      </head>
      <body>
      <div>
      <p>a</p><p>b</p><p>c</p>
      </div>
      <div class="ex_class sample">
      <p>1</p><p>2</p><p>3</p>
      </div>
      <div id="ex_id">
      <p>X</p><p>Y</p><p>Z</p>
      </div>
      <h1>This is a heading.</h1>
      <p>This is a paragraph.</p>
      <p>This is another paragraph.</p>
      <a class="a sample" href="www.naver.com">Naver</a>
      </body>
      </html>
  ```

- `soup.find()` - 해당 조건에 맞는 하나의 태그를 가져오는 method

  - 중복되는 경우 첫번째 태그를 가져온다.

  ```python
  first_div = soup.find('div')
  fist_div
  >>  <div>
      <p>a</p><p>b</p><p>c</p>
      </div>
  ```

- `soup.find_all()` - 해당 조건에 맞는 모든 태그들을 가져오는 method

  - 결과는 list로 반환

  ```python
  all_div = soup.find_all('div')
  all_div
  >>[<div>
   <p>a</p><p>b</p><p>c</p>
   </div>, <div class="ex_class sample">
   <p>1</p><p>2</p><p>3</p>
   </div>, <div id="ex_id">
   <p>X</p><p>Y</p><p>Z</p>
   </div>]
  ```

- `soup.select_one()` - CSS Selector로 하나만 찾는 method

  ```python
  # ''#id명'을 이용하여 id찾기
  ex_id_div = soup.select_one('#ex_id')
  ex_id_div
  >>  <div id="ex_id">
      <p>X</p><p>Y</p><p>Z</p>
      </div>
  ```

  ```python
  # '.class명'을 이용하여 class 찾기, 태그명.class도 가능
  ex_class_div = soup.select_one('.ex_class')
  ex_class_div
  >>  <div class="ex_class sample">
      <p>1</p><p>2</p><p>3</p>
      </div>
  ```

- `soup.select()` - CSS Selector로 모두를 찾는 method

  - 결과를 list로 반환

  ```python
  ex_id_divs = soup.select('#ex_id') # id
  ex_id_divs
  >> [<div id="ex_id">
   <p>X</p><p>Y</p><p>Z</p>
   </div>]
  ```

  ```python
  sample_divs = soup.select('.sample') # class
  sample_divs
  >> [<div class="ex_class sample">
   <p>1</p><p>2</p><p>3</p>
   </div>, <a class="a sample" href="www.naver.com">Naver</a>]
  ```

- `.get_text()`, `.text` 또는 `.string`을 이용하여 결과 가져오기

  ```python
  # <a class="a sample" href="www.naver.com">Naver</a>에서 Naver가져오기
  result = soup.select_one('.a.sample').get_text() # 여러개의 class명을 나열할 때는 .으로 구분, .text와 동일
  result
  >> 'Naver'
  ```

  ```python
  result = soup.select_one('.a.sample').string
  result
  >> 'Naver'
  ```

  - `.get_text()`와 `.string`의 차이
    - `get_text()`를 이용하면 **현재 태그를 포함하여 모든 하위 태그를 제거하고 유니코드 텍스트만 들어있는 문자열을 반환**한다. 따라서 하위태그의 텍스트까지 파싱하는 경우에 사용하는 것이 좋다. 또는 마지막 태그에 사용해야 한다.
    - `.string`은 **태그(tag)내 문자열을 반환**한다. 만약 태그 내 자식 태그가 둘 이상이면, 무엇을 반환해야 하는지 명확하지 않기 때문에 None을 반환한다. 단, 자식 태그가 하나이면서, 그 자식 태그가 `.stirng`값을 갖는다면 자식 태그의 문자열을 반환한다.

  ```python
  # 속성 값을 가져올 때(href 속성 가져오기)
  result = soup.select_one('.a.sample')['href']
  href
  >> 'www.naver.com'
  ```

- id = 'ex_id'인 div에서 p내용물 가져오는 방법

  ```python
  ex_id_div = soup.select('#ex_id > p') # soup.select_one('#ex_id').select('p') 또는 soup.select_one('#ex_id').find_all('p')도 가능
  for p in ex_id_div:
      print(p.string)
  >>  X
      Y
      Z
  ```

  - 자식 / 자손 태그 찾기
    - ' > ' : CSS 선택자에서 자식(부모보다 한 단계 아래에 있는 태크) 태그 표현 기호
    - ' ' : CSS 선택자에서 자손(조상보다 몇 단계든 아래에 있는 태그) 태그 표현 기호



## 02. Genie Chart

### 1. 인터넷 상에서 데이터 가져오기

```python
import requests
# Genie Top200
url = 'https://www.genie.co.kr/chart/top200'
req = requests.get(url)
html = req.text
html
>> The security policy of the connection request is blocked.
```

```python
# Chrome User-Agent
# requests를 보낼 때, 브라우저가 접속한 것이라고 header 정보를 수정해서 보내는 방법
header = {'User-Agent':
         'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/51.0.2704.103 Safari/537.36'}
req = requests.get(url, headers=header)
html = req.text
```

```python
soup = Beautifulsoup(html, 'html.parser') # 구문 분석
```



### 2. 찾으려고 하는 데이터의 태그 찾기

- 웹 페이지에서 `F12`를 누르고 `개발자 도구`에서 마우스 커서를 천천히 내리면서 찾으려고 하는 데이터 영역에 표시가 되는 부분을 찾는다.
- HTML 안에 찾으려는 부분에서 마우스 우클릭을 하여 Copy > Copy selector의 도움을 받을 수 있다.

```python
# <table class="list-wrap">에서 1~50위 차트에 해당하는 태그 가져오기
trs = soup.select('.list-wrap > tbody > tr')
len(trs)
>> 50
```



### 3. 여러개의 데이터 중 하나를 선택해서 원하는 정보 추출

- rank, title, artist, album 찾기

- rank

  ```python
  tr = trs[0]
  rank = int(tr.select_one('.number').text.split('\n')[0])
  rank
  >> 1
  ```

- title

  ```python
  title = tr.select_one('.info > .title').string.strip()
  title
  >> 'Queendom'
  ```

- artist

  ```python
  artist = tr.select_one('.info > .artist').string
  artist
  >> 'Red Velvet (레드벨벳)'
  ```

- album

  ```python
  album = tr.select_one('.info > .albumtitle').string
  album
  >> 'Queendom - The 6th Mini Album'
  ```



### 4. 한 페이지에 있는 모든 데이터를 반복문으로 가져오기

```python
import pandas as pd
rank_list, title_list, artist_list, album_list = [], [], [], []

for tr in trs:
    rank = int(tr.select_one('.number').text.split('\n')[0])
    title = tr.select_one('.info > .title').string.strip()
    artist = tr.select_one('.info > .artist').string
    album = tr.select_one('.info > .albumtitle').string
    rank_list.append(rank)
    title_list.append(title)
    artist_list.append(artist)
    album_list.append(album)
    
df = pd.DataFrame({
    '순위' : rank_list, '곡명' : title_list,
    '가수' : artist_list, '앨범' : album_list
})

df.tail()
>>
```

|      | 순위 | 곡명                                              | 가수        | 앨범                                   |
| ---- | ---- | ------------------------------------------------- | ----------- | -------------------------------------- |
| 45   | 46   | 미워요                                            | 임영웅      | 미워요 & 소나기                        |
| 46   | 47   | Timeless                                          | SG워너비    | Wanna Be＋                             |
| 47   | 48   | 사이렌 Remix (Feat. UNEDUCATED KID & Paul Blanco) | 호미들      | 사이렌 Remix                           |
| 48   | 49   | At My Worst                                       | Pink Sweat$ | The Prelude                            |
| 49   | 50   | 내 손을 잡아                                      | 아이유 (IU) | 최고의 사랑 OST Part.4 (MBC 수목드라마 |



### 5. 모든 페이지 데이터 가져오기

-각 페이지 url 형식 = 'https://www.genie.co.kr/chart/top200?ditc=D&ymd=20210818&hh=01&rtm=Y&pg='

```python
sub = 'https://www.genie.co.kr/chart/top200?ditc=D&ymd=20210818&hh=01&rtm=Y&pg='
rank_list, title_list, artist_list, album_list = [], [], [], []
for page in range(1, 5):
    req = requests.get(f'{sub}{page}', headers=header)
    soup = BeautifulSoup(req.text, 'html.parser')
    trs = soup.select('.list-wrap > tbody > tr')
    for tr in trs:
        rank = int(tr.select_one('.number').text.split('\n')[0])
        title = tr.select_one('.info > .title').string.strip()
        artist = tr.select_one('.info > .artist').string
        album = tr.select_one('.info > .albumtitle').string
        rank_list.append(rank)
        title_list.append(title)
        artist_list.append(artist)
        album_list.append(album)
    
df = pd.DataFrame({
    '순위' : rank_list, '곡명' : title_list,
    '가수' : artist_list, '앨범' : album_list
})

df.tail()
>>
```

|      | 순위 | 곡명              | 가수        | 앨범                                    |
| ---- | ---- | ----------------- | ----------- | --------------------------------------- |
| 195  | 196  | Love poem         | 아이유 (IU) | Love poem                               |
| 196  | 197  | 진또배기          | 이찬원      | 내일은 미스터트롯 예선곡 베스트         |
| 197  | 198  | Paris In The Rain | Lauv        | I met you when I was 18. (the playlist) |
| 198  | 199  | 찐이야            | 영탁        | 내일은 미스터트롯 결승전 베스트         |
| 199  | 200  | On The Ground     | 로제 (ROSE) | R                                       |



## 요약정리

1. 인터넷 상에서 데이터 가져오기

   req = requests.get(url[, headers=])

   html = req.text

   soup = BeautifulSoup(html, 'html.parser')

2. 찾으려고 하는 데이터의 태그 찾기

   trs =  soup.select_one() / soup.select() / soup.find() / soup.find_all()



## 참고

- `.get_text()`와 `.string`의 차이 : https://hogni.tistory.com/21

- `User-Agent` : https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent