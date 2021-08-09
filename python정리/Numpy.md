# Numpy

## 1. Numpy Dimensional Array

```python
a = np.array([1,4,5,8],float)
a
```

## 2.  Array shape

```python
#Vector(1차원)
vector = np.array([1,4,5,8])
vector.shape
# 결과 : (4,)
```

- .shape : 차원과 크기 확인
- reshape : 행렬의 크기와 차원을 바꿀 수 있음
- flatten : 차원 상관없이 1차원으로 변환

## **3. Indexing & Slicing**

- Indexing : []와 ,를 사용하여 인덱싱

- Slicing

## 4. 생성(Creation)

- np.arange()
- ones, zeros, empty
- something like
- identity
- eye
- diag
- Random sampling