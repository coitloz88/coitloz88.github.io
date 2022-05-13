---
title: Pytorch tutorials 05 | TORCH.AUTOGRAD를 사용한 자동 미분
date: 2022-05-14 00:11:00 +0900
categories: [ML, pytorch]
tags: [ML, pytorch, tutorial]     # TAG names should always be lowercase
---

신경망을 학습할 때 가장 자주 사용되는 알고리즘인 Back Propagation에서, 매개변수(weight과 bias)는 주어진 매개변수에 대한 Error function의 Gradient에 따라 조정된다.  

이를 위해 Pytorch에 내장되어 있는 자동 미분 엔진 `torch.autograd`를 사용한다.  

Input `x`, Weight `w`, Bias `b`, 그리고 Error function이 있는 간단한 Single Layer Neural Network를 Pytorch에서 정의해보자.

```python
import torch

x = torch.ones(5) # input tensor
y = torch.zeros(3) # expected output
w = torch.randn(5, 3, requires_grad = True)
b = torch.randn(3, requries_grad = True)
z = torch.matmul(x, w) + b
loss = torch.nn.functional.binary_cross_entropy_with_logits(z, y)
```

위의 코드는 다음의 Computational graph를 정의한다.

![Computational graph](https://tutorials.pytorch.kr/_images/comp-graph.png){: width="80%"}{: .center}

이때, `w`와 `b`가 최적화를 해야하는 **매개변수**이다.

<br>

---

<br>

# Gradient 계산하기

Neural network에서 매개변수의 가중치를 최소화하기 위해서는 매개벼수에 대한 Error function의 도함수(derivative)를 계산해야 한다. 

즉, 다시 말해 어떤 각각의 고정값 `x`와 `y`에 대해 $\frac{\partial loss}{\partial w}$와 $\frac{\partial loss}{\partial b}$를 계산해야 한다.  

이러한 도함수 계산을 위해, `loss.backward()`를 호출한 뒤, `w.grad`와 `b.grad`에서 값을 가져온다.

```python
loss.backward()
print(w.grad)
print(b.grad)
```

# Gradient 추적 멈추기

기본적으로 `requires_grad = True`로 설정된 모든 텐서는 연산 기록이 추적되며 Gradient 계산도 지원된다. 그러나 Forward propagation 연산만 필요한 경우, 이러한 추적이나 계산이 필요 없을 수도 있기 때문에 연산 코드를 `torch.no_grad()`로 둘러싸서 추적을 멈출 수 있다.

```python
with torch.no_grad()
    z = torch.matmul(x, w) + b
print(z.requires_grad)
```

> Gradient 추적을 왜 멈춰야 할까?  
    - Neural network의 일부 매개변수를 frozen parameter로 표시한다.  
    - Forward propagation 단계만 수행할 때 연산 속도가 향상된다.
{: .prompt-tip }

<br>

---

<br>


# Computational Graph에 대한 추가 정보

autograd는 데이터(tensor)와 실행된 모든 연산의 기록을 `Function` 객체로 구성된 Directed Acyclic Graph에 저장한다. 이 그래프의 leaf들은 input tensor이고, root는 output tensor이다. 그래프를 root부터 leaf까지 추적하면 chain rule에 따라 gradient를 자동으로 계산할 수 있다.  

* Forward propagation 단계에서, `autograd`의 작업:
    - 요청된 연산을 수행하여 결과 텐서를 계산
    - DAG에 연산의 Gradient function을 유지

* Back propagation 단계는 DAG의 root에서 `.backward()`가 호출될 때 시작되며, 이 때 `autograd`의 작업:
    - 각 `.grad_fn`으로부터 gradient를 계산
    - 각 텐서의 `.grad` 속성에 계산 결과를 축적
    - chain rule을 사용하여, 모든 leaf 텐서들까지 전파



<br>

---

<br>


# Image Source

* <https://tutorials.pytorch.kr/_images/comp-graph.png>

# References

* <https://tutorials.pytorch.kr/beginner/basics/autogradqs_tutorial.html>
