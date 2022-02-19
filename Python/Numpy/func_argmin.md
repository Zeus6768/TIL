# Numpy Array와 Python List의 비교

## numpy.argmin


```python
import numpy as np

ar1 = np.array([1, 2, 3, 4])  # [1, 2, 3, 4]
ar2 = np.arange(4, 0, -1)     # [4, 3, 2, 1]

result = np.argmin(ar1)
print(result)

result = np.argmin(ar2)
print(result)
```

    0
    3
    

위 코드 결과와 같이 `argmin()` 함수는 array에서 가장 작은 값의 인덱스를 반환합니다. 

## (1) Numpy Array

2차원의 ndarray를 만들고, `argmin()` 함수에 axis 값을 바꿔가며 계산하면 다음과 같습니다. 


```python
array = np.array([[5, 6, 8], [5, 7, 1], [2, 8, 4]])
result1 = np.argmin(array, axis=0)
result2 = np.argmin(array, axis=1)
print(result1)
print(result2)
```

    [2 0 1]
    [0 2 0]
    

2차원 array는 `axis=0`일 때 행 방향으로 같은 인덱스의 값끼리 비교합니다. 

즉, 각각의 행 `[5, 5, 2], [6, 7, 8], [8, 1, 4]`에서 최솟값의 인덱스인 `[0, 2, 0]`을 반환합니다. 

`axis=1`일 때는 `[5, 6, 8], [5, 7, 1], [2, 8, 4]`에서 열 방향으로 비교해서, `[2, 1, 1]`을 반환합니다.

<br> 

## (2) Python List

numpy 라이브러리를 이용하지 않고 python에서 numpy의 `argmin()`와 동일한 함수를 만들면 다음과 같습니다. <br>
<br>
입력값이 2차원 리스트임을 전제로 작성한 함수이므로 2차원 리스트를 입력할 때만 정상적으로 작동합니다.<br>
<br>
또한 axis를 입력하는 방법을 구체화하지 않았고 axis에 0이 입력되는 경우, 1이 입력되는 경우로만 분기했으며 입력값이 잘못된 예외에 대해서는 크게 신경쓰지 않았습니다.<br>
<br>


```python
def argmin(array, axis):
    col = len(array[0])
    result_array = []
    if axis == 0:
        for i in range(col):
            temp = []
            for a in array:
                temp.append(a[i])
            result_array.append(temp.index(min(temp)))
        return result_array
    elif axis == 1:
        for a in array:
            result_array.append(a.index(min(a)))
        return result_array
    else:
        return "error"
```


```python
array = [[5, 6, 8], [5, 7, 1], [2, 8, 4]]
result1 = argmin(array, axis=0)
result2 = argmin(array, axis=1)
print(result1)
print(result2)
```

    [2, 0, 1]
    [0, 2, 0]
    

### 핵심 코드

```python
# code
리스트.index(min(리스트))

# example
>>> list1 = [1, 2, 3]
>>> list1.index(min(list1))    
0    # 최솟값 1의 인덱스인 0를 반환합니다.
```

위 코드가 함수의 핵심입니다. 리스트 최솟값의 인덱스를 반환합니다. 
<br><br>

### 함수

함수를 위에서부터 차례대로 살펴보겠습니다. 
<br><br>

```python
def argmin(array, axis):
    col = len(array[0])
    result_array = []
```


함수를 사용할 때 배열과 axis를 입력받습니다. 

그리고 2차원 리스트를 행렬처럼 생각할 때 열에 해당하는 축의 길이를 첫번째 원소(행)의 길이(열)를 이용해 미리 입력했습니다. 

예를 들어 위에서 작성했던 `array` 리스트를 입력값으로 하는 함수의 `col` 값은 3이 됩니다. 

이는 뒤에서 반복문 사용을 편리하게 하기 위함입니다. 

<br>

```python
if axis == 0:
    for i in range(col):
        temp = []
        for a in array:
            temp.append(a[i])
        result_array.append(temp.index(min(temp)))
    return result_array
```


axis가 0인 경우, 각 행의 같은 인덱스를 가진 값들을 비교합니다. 

모든 행을 순회하며 같은 인덱스의 값을 임시 리스트 `temp`에 모은 뒤, `temp`의 최소값의 인덱스를 결과 리스트에 저장합니다.

해당 명령을 열의 개수만큼 반복하면 원하는 결과를 얻을 수 있습니다. 

<br>

```python
elif axis == 1:
    for a in array:
        result_array.append(a.index(max(a)))
    return result_array
```


axis가 1인 경우, 모든 행을 순회하며 각 행의 최솟값의 인덱스를 결과 리스트에 저장하여 반환합니다. 

<br>

```python
else:
    return "error"
```


axis에 0이나 1이 아닌 값을 입력할 경우 오류 메시지를 반환합니다. 
<br><br>

## (3) 실행 시간 비교 (%timeit)

 numpy의 함수와 직접 만든 함수의 작동 시간을 비교합니다. 

우선 임의의 2차원 배열과 리스트를 다음과 같이 만들었습니다. 

10 * 5 행렬과 같은 모양입니다. 


```python
import random as r
list1 = [[r.randrange(1, 10) for i in range(5)] for j in range(10)]
array1 = np.array(list1)
print(list1)
print(array1)
```

    [[3, 8, 9, 3, 9], [8, 9, 8, 1, 9], [5, 3, 1, 7, 4], [2, 3, 7, 1, 2], [1, 4, 4, 6, 8], [8, 6, 7, 1, 9], [8, 8, 3, 3, 8], [5, 5, 6, 6, 1], [9, 5, 3, 2, 7], [1, 3, 4, 9, 2]]
    [[3 8 9 3 9]
     [8 9 8 1 9]
     [5 3 1 7 4]
     [2 3 7 1 2]
     [1 4 4 6 8]
     [8 6 7 1 9]
     [8 8 3 3 8]
     [5 5 6 6 1]
     [9 5 3 2 7]
     [1 3 4 9 2]]
    

<br>

먼저 `axis=0`일 때의 작동 시간 비교입니다. 입력값이 많을 경우 numpy의 argmin 함수가 더 빠른 것을 볼 수 있습니다. 


```python
%timeit np.argmin(array1, axis=0)
```

    1.57 µs ± 9.51 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)
    


```python
%timeit argmin(list1, axis=0)
```

    5.03 µs ± 16.7 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
    

<br>

`axis=1`일 때의 작동 시간 비교입니다. 마찬가지로 numpy의 함수가 더 빠르게 동작합니다. 


```python
%timeit np.argmin(array1, axis=1)
```

    1.16 µs ± 4.58 ns per loop (mean ± std. dev. of 7 runs, 1000000 loops each)
    


```python
%timeit argmin(list1, axis=1)
```

    2.97 µs ± 18.9 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
    
