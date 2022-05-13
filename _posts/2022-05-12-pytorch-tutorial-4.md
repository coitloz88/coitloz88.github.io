---
title: Pytorch tutorials 04 | 신경망 모델 구성하기
date: 2022-05-12 09:40:00 +0900
categories: [ML, pytorch]
tags: [ML, pytorch, tutorial]     # TAG names should always be lowercase
---

신경망은 데이터 연산을 수행하는 layer / module로 구성되어있다.  
[torch.nn](https://pytorch.org/docs/stable/nn.html) 네임스페이스는 신경망을 구성하는데 필요한 모든 구성 요소를 제공한다.  

<br>

FashionMNIST 데이터셋의 이미지를 분류하는 신경망을 구성해보자.

```python
import os
import torch
from torch import nn
from torch.utils.data import DataLoader
from torchvision import datasets, transforms
```

<br>

---

<br>


# 학습을 위한 장치 얻기

가능한 경우 GPU와 같은 하드웨어 가속기에서 모델을 학습하자.

```python
device = "cuda" is torch.cuda.is_available() else "cpu"
print(f"Using {device} device")
```

# 클래스 정의하기

신경망 모델을 `nn.Module`의 하위클래스로 정의하고, `__init__`에서 신경망 계층을 초기화한다. `nn.Module`을 상송받은 모든 클래스는 `forward` 메소드에 입력 데이터에 대한 연산들을 구현한다.

```python
class NeuralNetwork(nn.Module):
    def __init__(self):
        super(NeuralNetwork, self).__init__()
        self.flatten = nn.Flatten()
        self.linear_relu_stack = nn.Sequential(
            nn.Linear(28*28, 512),
            nn.ReLU(),
            nn.Linear(512, 512),
            nn.ReLU(),
            nn.Linear(512, 10),
            )

    def forward(self, x):
        x = self.flatten(x)
        logits = self.linear_relu_stack(x)
        return logits
```

`NeuralNetwork`의 instance를 생성하고 이를 `device`로 이동한 뒤, structure을 출력한다.

```python
model = NeuralNetwork().to(device)
print(model)
```

모델을 사용하기 위해 입력 데이터를 전달한다. 이는 일부 백그라운드 연산과 함께 모델의 `forward`를 실행한다.

> `model.forward()`를 직접 호출하지 말자.
{: .prompt-tip }

모델에 입력을 호출하면 각 class에 대한 raw 예측 값이 있는 10차원 텐서가 반환된다. raw 예측 값을 `nn.Softmax` 모듈의 인스턴스에 통과시켜 예측 확률을 얻을 수 있다.

```python
X = torch.rand(1, 28, 28, device = device)
logits = model(X)
pred_probab = nn.Softmax(dim = 1)(logits)
y_pred = pred_probab.argmax(1)
print(f"Predicted class: {y_pred}")
```
<br>

---

<br>


# Layer

FashionMNIST 모델의 layer을 28*28 크기의 이미지 3개로 구성된 미니배치 예제를 통해 살펴보자.

```python
input_image = torch.rand(3, 28, 28)
```

## nn.Flatten

`nn.Flatten` layer를 초기화하여 각 28*28의 2D 이미지를 784 픽셀을 갖는 연속된 배열로 반환한다.

```python
flatten = nn.Flatten()
flat_image = flatten(input_image)
```

## nn.Linear

선형 계층은 저장된 weight과 bias를 사용하여 입력에 linear transformation을 적용하는 모듈이다.

```python
layer1 = nn.Linear(in_features = 28*28, out_features = 20)
hidden1 = layer1(flat_image)
```

## nn.ReLU

non-linear activation은 모델의 입력과 출력 사이에 복잡한 mapping을 만든다. non-linear activation은 선형 변환 후 적용되어 nonlinearity를 도입하며, 신경망이 다양한 현상을 학습할 수 있도록 돕는다.

```python
hidden1 = nn.ReLU()(hidden1)
```

## nn.Sequential

`nn.Sequential`은 순서를 갖는 모듈의 컨테이너로, 데이터는 정의된 순서로 모든 모듈들을 통해 전달된다. sequential container를 사용하여 일부 신경망을 빠르게 만들 수 있다.

```python
seq_modules = nn.Sequential(
    flatten,
    layer1,
    nn.ReLU(),
    nn.Linear(20, 10)
)
input_image = torch.rand(3,28,28)
logits = seq_modules(input_image)
```


## nn.Softmax

신경망의 마지막 선형 계층은 `nn.Softmax` 모듈에 전달될 ([-infty, infty] 범위의 raw value인) logits 를 반환한다. 이때, logits는 모델의 각 class에 대한 예측 확률을 나타내도록 [0, 1] 범위로 조정된다.

```python
softmax = nn.Softmax(dim = 1)
pred_probab = softmax(logits)
```

# 모델 매개변수

신경망 내부의 많은 계층은 parameterize된다. 즉, 학습 중에 최적화되는 weight, bias와 연관지어진다. `nn.Module`를 상속하면 모델 객체 내부의 모든 필드를 자동으로 추적할 수 있게 되며, 모델의 `parameters()` 및 `named_parameters()` 메소드로 모든 매개변수에 접근할 수 있다.


<br>

---

<br>


# References

* <https://tutorials.pytorch.kr/beginner/basics/buildmodel_tutorial.html>
