# 공공API 및 지도 시각화

## 01. Folium 지도 시각화

- `folium`을 `import`하여 지도를 시각화

```shell
# (참고) local에서 하는 경우에 folium 설치
C:\Users\username>conda activate base
(base) C:\Users\username>conda install -c conda-forge folium
```

```python
import folium
map = folium.Map(location=[37.541, 126.984], zoom_start=12) # 서울시 중심점
map
```

- `folium.Map.save()`를 통해 지도를 html파일로 저장

```python
map.save('seoulmap.html')
```

- `tiles=`option 또는 `folium.TileLayer().add_to()`를 이용하여 지도의 이미지를 바꿀 수 있다.

```python
# 방법1
map = folium.Map(location, tiles='Stamen Toner', zoom_start=12)
```

```python
# 방법2
map = folium.Map(location=[37.541, 126.984], zoom_start=11)
folium.TileLayer('cartodbpositron').add_to(map)
```

tiles 종류는 Stamen Terrain, Stamen Toner 등 매우 다양하다.



### Markers

- `folium.Marker().add_to(folium.Map())`을 이용하여 지도에 마커를 표시할 수 있다.

```python
folium.Marker(
	location=[37.566317, 126.977829], # 필수 입력, [위도, 경도]
    popup='서울 시청, popup', # 마커를 누르면 볼 수 있다.
    tooltip='서울특별시청, tooltip', # 마커위에 커서를 올리면 볼 수 있다.
    icon=folium.Icon(color='red', icon='info-sign') # folium.Icon()을 이용하여 마커의 디자인을 변경할 수 있다.
).add_to(map)
```

2개 이상의 마커를 지도위에 표시하려면 앞의 내용을 여러번 사용하여 나타낸다.

`popup=folium.Popup('문자열', max_width=)`를 통해 팝업창의 넓이를 조절할 수 있다.



### CircleMarker

`folium.Circle().add_to(folium.Map())`과 `folium.CircleMarker().add_to(folium.Map())`을 이용하여 지도에 원모양의 마커를 표시할 수 있다.

```python
# folium.Circle()
folium.Circle(
	radius=200,	# 반지름
    location=[37.566317, 126.977829],
    popup='서울 시청, popup',
    tooltip='서울특별시청, tooltip',
    color='crimson', # 원주 색
    fill=True,
    fillcolor='red' # 원 내부 색
).add_to(map)

# folium.CircleMarker()
folium.CircleMarker(
	radius=50,
    location=[37.577317, 126.988829], 
    popup='popup',
    tooltip='tooltip',
    color='#3186cc',
    fill=False
).add_to(map)
```



### 로컬 파일을 Colab에 upload하여 지도 시각화

- 로컬 파일을 upload하는 코드

  ```python
  from google.colab import files
  uploaded = files.upload() # 파일 upload
  filename = list(uploaded.keys())[0] # 파일명
  ```

- `toilet_seoul.csv`파일을 upload하여 '서울시 공공화장실 안내' 지도 시각화

  ```python
  import pandas as pd
  df = pd.read.csv(filename)
  
  map = folium.Map(location=[37.541, 126.984], zoom_start=11)
  for i in df.index[:100]:
          folium.Circle(
          radius=200,
          location=[df.위도[i], df.경도[i]], 
          popup=folium.Popup(f'{df.구명[i]} {df.법정동명[i]}', max_width=200), # 팝업 넓이 조절
          tooltip=df.고유번호[i],
          color="#3186cc",
          fill=True
      ).add_to(map)
  title = '<h3 align="center" style="font-size:20px">서울시 공공화장실 안내</h3>'
  map.get_root().html.add_child(folium.Element(title)) # 지도 제목 표시
  
  map
  ```

  

## 02. 도로명주소 API

- 건물명, 명소로부터 도로명 주소 구하기

### [도로명주소 API](https://www.juso.go.kr/addrlink/devAddrLinkRequestWrite.do?returnFn=write&cntcMenu=URL)에서 승인키 받기

1. `API 종류` : 도로명주소 API,` API유형` :  검색 API, `서비스망` : 인터넷망, `서비스 용도` : 개발
2. `roadapikey.txt`에 승인키 저장하기
3. `.gitignore`에 ` roadapikey.txt` 추가하기



### 도로명 주소 구하기

```python
# roadapikey.txt 업로드
uploaded = files.upload()
filename = list(uploaded.keys())[0]
```

```python
# 승인키 읽어오기 (화면에 노출되지 않도록 주의하기!)
with open(filename) as f:
    api_key = f.read()
```

```python
import requests
from urllib.parse import quote
```

- 승인 키에 의한 주소 검색요청

  -option 필수 입력 요청 변수, &로 연결

  - confmKey : 신청 시 발급받은 승인키
  - currentPage : 현재 페이지 번호(n>0), default=1
  - countPerPage : 페이지당 출력할 결과 Row수(0<=n<=100), default=10
  - keyword : 주소 검색

```python
bldg = '서울특별시청' # 주소를 검색하고 싶은 건물명
road_url = 'https://www.juso.go.kr/addrlink/addrLinkApi.do'
option = f'confmKey={api_key}&currentPage=1&countPerPage=10&keyword={quote(bldf)}' # quote() : base64 incoding
url = f'{road_url}?{option}&resultType=json'

result = requests.get(url).json() # HTTP Method get()을 사용하여 리소스를 조회
```

- 받은 주소 정보로 DataFrame 만들기(for문 이용)

```python
bldgs = ['종로구청', '노원구청', '송파구청', '마포구청', '양천구청']
addr_list = []

for bldg in bldgs:
    option = f'confmKey={api_key}&currentPage=1&countPerPage=10&keyword={quote(bldf)}'
    url = f'{road_url}?{option}&resultType=json'
    result = requests.get(url).json() 
    addr= result['results']['juso'][0]['roadAddr']
    addr_list.append(addr)
    
addr_list
>>['서울특별시 종로구 종로1길 36(수송동)',
 '서울특별시 노원구 노해로 437(상계동)',
 '서울특별시 송파구 올림픽로 326(신천동)',
 '서울특별시 마포구 월드컵로 지하190(성산동)',
 '서울특별시 양천구 목동동로 105(신정동)']
```

```python
df = pd.DataFrame({
	'공공기관' : bldgs,
    '도로명주소' : addr_list
})
df
>>	공공기관	도로명주소
0	종로구청	서울특별시 종로구 종로1길 36(수송동)
1	노원구청	서울특별시 노원구 노해로 437(상계동)
2	송파구청	서울특별시 송파구 올림픽로 326(신천동)
3	마포구청	서울특별시 마포구 월드컵로 지하190(성산동)
4	양천구청	서울특별시 양천구 목동동로 105(신정동)
```

```python
df.to_csv('공공기관.csv', index=False) # df을 csv파일로 만들기
```



## 03. 카카오 로컬 API

- 도로명 주소 --> 위도, 경도

### [카카오](https://developers.kakao.com/product/map) 승인키 받기 (REST API 키 사용)

1. 제품 > 지도/로컬 > API사용하기 > 시작하기
2. `kakaoapikey.txt`에 REST API 키 저장하기 
3.  `.gitignore`에 ` kakaoapikey.txt` 추가하기



### 위도, 경도 구하기

```python
# kakaoapikey.txt 업로드 
uploaded = files.upload()
filename = list(uploaded.keys())[0]
```

```python
# 승인키 읽어오기 (화면에 노출되지 않도록 주의하기!)
with open(filename) as f:
    api_key = f.read()
```

- 승인 키에 의한 위도, 경도 검색요청

  ```python
  addr = '서울특별시 중구 세종대로 110(태평로1가)'
  search_url = 'https://dapi.kakao.com/v2/local/search/address.json'
  url = f'{search_url}?query={quote(addr)}'
  # "Authorization: KakaoAK {REST_API_KEY}"
  result = requests.get(url, headers={"Authorization": f'KakaoAK {api_key}'}).json()
  result
  ```

- 공공기관.csv파일에 위도, 경도 추가하기

  ```python
  # 공공기관.csv 파일 upload
  filename = list(uploaded.keys())[0]
  ```

  ```python
  # 공공기관.csv read
  df = pd.read_csv(filename)
  df
  >>공공기관	도로명주소
  0	종로구청	서울특별시 종로구 종로1길 36(수송동)
  1	노원구청	서울특별시 노원구 노해로 437(상계동)
  2	송파구청	서울특별시 송파구 올림픽로 326(신천동)
  3	마포구청	서울특별시 마포구 월드컵로 지하190(성산동)
  4	양천구청	서울특별시 양천구 목동동로 105(신정동)
  ```

  ```python
  lat_list, lng_list = [], []
  
  for addr in df.도로명주소:
      url = f'{search_url}?query={quote(addr)}'
      result = requests.get(url, headers={"Authorization": f'KakaoAK {api_key}'}).json()
      lat = float(result['documents'][0]['y'])
      lng = float(result['documents'][0]['x'])
      lat_list.append(lat_list)
      lng_list.append(lng_list)
      
  df['위도'] = lat_list
  df['경도'] = lng_list
  df 
  >>	공공기관	도로명주소							위도	경도
  0	종로구청	서울특별시 종로구 종로1길 36(수송동)	37.573	126.978
  1	노원구청	서울특별시 노원구 노해로 437(상계동)	37.654	127.056
  2	송파구청	서울특별시 송파구 올림픽로 326(신천동)	37.514	127.105
  3	마포구청	서울특별시 마포구 월드컵로 지하190(성산동)	37.563	126.903
  4	양천구청	서울특별시 양천구 목동동로 105(신정동)	37.517	126.866
  ```

- 지도위에 공공기관 표시하기

  ```python
  map = folium.Map(location=[df.위도.mean(), df.경도.mean()], zoom_start=12)
  
  for i in df.index:
          folium.Circle(
          radius=300,
          location=[df.위도[i], df.경도[i]], 
          popup=folium.Popup(f'{df.도로명주소[i]}', max_width=200),
          tooltip=df.공공기관[i],
          color='#3186cc',
          fill=True,
          fillcolor ='#3186cc'
      ).add_to(map)
  title = '<h3 align="center" style="font-size:20px">서울시 구청 안내</h3>'
  map.get_root().html.add_child(folium.Element(title))
  map
  ```




## 04. 단계 구분도(Choropleth)

```python
# 'older_population.csv'파일 업로드
uploaded = files.upload()
filename = list(uploaded.keys())[0]
```

```python
import pandas as pd
df = pd.read_csv(filename)
```

```python
# 'seoul-dong.geojson '파일 업로드
uploaded = files.upload()
filename = list(uploaded.keys())[0]
```

```python
import json
with open(filename) as json_file: # json파일 읽어오기
    geo_data = json.load(json_file)
```

- 업로드한 데이터 파일로 '서울시 동별 노령 인구수'지도 시각화

  - `folium.Choropleth().add_to(folium.Map())`이용하여 단계구분도 그리기

  ```python
  center = [37.541, 126.986]
  map = folium.Map(location=center, zoom_start=11)
  
  folium.Choropleth(
      geo_data=geo_data, # geo_data=filename도 가능, 지도 데이터 파일 경로 또는 데이터
      data=df,     # 시각화하고자 하는 데이터 프레임
      columns=('동', '인구'), # (지도 데이터와 매핑할 값, 시각화하고자 하는 변수)
      key_on='feature.properties.동', # 데이터 파일과 매핑할 값
      fill_color='BuPu', # Color Map
      legend_name='노령 인구수' # Color 범주 이름
  ).add_to(map)
  title = '<h3 align="center" style="font-size:20px">서울시 동별 노령인구</h3>'
  map.get_root().html.add_child(folium.Element(title))
  map
  ```

  