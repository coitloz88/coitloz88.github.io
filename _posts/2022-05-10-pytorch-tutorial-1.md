---
title: Pytorch tutorials 01 | Tensor
date: 2022-05-10 12:50:00 +0900
categories: [ML, pytorch]
tags: [ML, pytorch, tutorial]     # TAG names should always be lowercase
---

모든 코드는 colab에서 실행했다.


텐서(tensor)란, 배열(array)이나 행렬(matrix)와 비슷한 특수한 자료 구조이다. Pytorch에서는 텐서를 사용하여 모델의 input과 output, 그리고 매개 변수 등을 encode한다.

```python
import torch
import numpy as np
```

<br>

---

<br>

# 텐서 초기화

텐서는 여러가지 방법으로 초기화할 수 있다.

## 1. 데이터로부터 직접(directly) 생성

데이터로부터 직접 텐서를 생성할 수 있으며, 자료형은 자동으로 유추된다.

```python
data = [[1,2],[3,4]]
x_data = torch.tensor(data)
```


## 2. 다른 텐서로부터 생성

명시적으로 override하지 않는 경우, 인자로 주어진 텐서의 속성(shape, datatype)을 유지한다. 텐서의 속성에 관해서는 아래에서 자세히 살펴볼 수 있다.

```python
x_ones = torch.ones_like(x_data) # x_data의 텐서 속성을 유지
x_rand = torch.rand_like(x_Data, dtype = torch.float) # x_data의 텐서 속성을 재정의
```


## 3. Random한 값 혹은 Constant 값을 사용

`shape`은 텐서의 dimension을 나타내는 tuple로, 출력 텐서의 차원을 결정한다.

```python
shape = (2,3,)
rand_tensor = torch.rand(shape) # random한 value를 요소로 갖는 텐서 생성
ones_tensor = torch.ones(shape) # 상수 '1'을 요소로 갖는 텐서 생성
zeros_tensor = torch.zeros(shape) # 상수 '0'을 요소로 갖는 텐서 생성
```

<br>

---

<br>

# 텐서의 속성(Attribute)

텐서의 속성은 다음 두 가지로 구성된다.

* 모양: `tensor.shape`
* 자료형: `tensor.datatype`
* 어느 장치에 저장되어 있는지: `tensor.device`

<br>

---

<br>

# 텐서 연산(Operation)

Transposing, Indexing, Slicing, 그 외 여러 수학 계산, 선형 대수, Random sampling 등 다양한 텐서 연산을 [공식 docs](https://pytorch.org/docs/stable/torch.html)에서 확인할 수 있다.

* GPU로 텐서 이동

    기본적으로 텐서는 CPU에 생성되지만, 명시적으로 GPU에 이동시킬 수 있다.

    ```python
    # GPU가 존재하는 경우, 텐서를 이동한다
    if torch.cuda.is_available():
        tensor = tensor.to("cuda")
    ```

* NumPy식의 표준 Indexing과 Slicing

    ```python
    tensor = torch.rand(4,4)
    print(f"First row: {tensor[0]}")
    print(f"First Column: {tensor[:,0]}")
    print(f"Last Column: {tensor[...,-1]}")

    tensor[:,1] = 0
    print(tensor)
    ```

* 텐서 합치기
    주어진 차원에 따라 일련의 텐서를 연결할 수 있다.

    ```python
    t1 = torch.cat([tensor, tensor, tensor], dim = 1)
    ```

* 산술 연산(Arithmetic operations)

    ```python
    # 두 텐서 간의 matrix multiplication을 계산하는 3가지 방법
    y1 = tensor @ tensor.T
    y2 = tensor.matmul(tensor.T)
    
    y3 = torch.rand_like(tensor)
    torch.matmul(tensor, tensor.T, out = y3)

    # 두 텐서 간의 element-wise product를 계산하는 3가지 방법
    z1 = tensor * tensor
    z2 = tensor.mul(tensor)

    z3 = torch.rand_like(tensor)
    torch.mul(tensor, tensor, out = z3)
    ```

* Single-element 텐서

    텐서의 모든 값을 하나로 aggregate하여 요소가 하나인 텐서의 경우, 이를 Python 숫자 값으로 변경할 수 있다.

    ```python
    agg = tensor.sum()
    agg_item = agg.item()
    ```

* In-place 연산

    연산 결과를 피연산자(operand)에 저장하는 연산을 In-place 연산이라고 부르며, 해당 연산은 메서드에 `_` 접미사를 갖는다.

    ```python
    tensor.add_(5)
    ```

    > In-place 연산은 도함수(derivative) 계산에 문제를 일으킬 수도 있으므로 사용을 권장하지 않는다.
    {: .prompt-tip }

<br>

---

<br>

# NumPy 변환(Bridge)

CPU 상의 텐서와 NumPy 배열은 메모리 공간을 공유하므로, 하나를 변경하면 다른 하나도 변경된다.

<br>

---

<br>

# References

* <https://tutorials.pytorch.kr/beginner/basics/tensorqs_tutorial.html>
