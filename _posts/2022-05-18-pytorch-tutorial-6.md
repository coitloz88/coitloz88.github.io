---
title: Pytorch tutorials 06 | 모델 매개변수 최적화하기
date: 2022-05-18 16:00:00 +0900
categories: [ML, pytorch]
tags: [ML, pytorch, tutorial]     # TAG names should always be lowercase
---

이때까지의 과정을 통해 모델과 데이터를 준비했다. 이제는 데이터에 매개변수를 최적화하여 모델을 학습하고, 검증(평가)하고, 테스트할 차례이다.  

모델을 학습하는 과정은 다음과 같은 반복적인 과정(epoch)을 거친다.  

1. 모델은 출력을 추측한다.
2. 추측과 실제 정답 사이의 오류(loss)를 계산한다.
3. 매개변수에 대한 오류의 도함수를 수집하고, Gradient descent를 사용하여 이러한 매개변수를 최적화(optimize)한다.  

이론과 관련하여 자세한 사항은 따로 Back propagation을 정리해놓은 [영상](https://www.youtube.com/watch?v=tIeHLnjs5U8)을 참조해보자.

# Pre-requisite code

이전 장을 거치며 작성된 코드이다.

```python
import torch
from torch import nn
from torch.utils.data import DataLoader
from torchvision import datasets
from torchvision.transforms import ToTensor, Lambda

training_data = datasets.FashionMNIST(
    root="data",
    train=True,
    download=True,
    transform=ToTensor()
)

test_data = datasets.FashionMNIST(
    root="data",
    train=False,
    download=True,
    transform=ToTensor()
)

train_dataloader = DataLoader(training_data, batch_size=64)
test_dataloader = DataLoader(test_data, batch_size=64)

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

model = NeuralNetwork()
```

# Hyperparameter

Hyperparameter는 모델 최적화 과정을 제어할 수 있는 조절 가능한 매개변수이다. 이는 모델 학습과 수렴율(convergence rate)에 영향을 미친다.  

학습할 때에는 다음과 같은 하이퍼파라미터를 정의한다.  

* Epoch 수: 데이터셋을 반복하는 횟수
* batch size: 매개 변수가 갱신되기 전 신경망을 통해 전파된 데이터 샘플의 개수
* learning rate: 각 batch / epoch에서 모델의 매개변수를 조절하는 비율

```python
learning_rate = 1e-3
batch_size = 64
epochs = 5
```

# Optimization Loop

Optimization 단계의 각 iteration을 **Epoch**이라고 부르는데, 하나의 Epoch은 두 가지로 loop로 구성된다.
* Train loop: 학습용 데이터 셋을 반복하고 최적의 매개변수로 수렴
* Validation / test loop: 모델 성능이 개선되고 있는지 확인하기 위해 테스트 데이터 셋을 반복

## Loss function

Loss function, 손실 함수는 획득(계산)한 결과와 실제 값 사이의 틀린 정도(degree of dissimilarity)를 측정하며, training 중에 이 값을 최소화하려고 한다.

```python
loss_fn = nn.CrossEntropy()
```

## Optimizer
각 train loop에서 모델의 error을 줄이기 위해 모델 매개변수를 조정한다. **최적화 알고리즘**은 이 과정이 수행되는 방식을 정의한다. (여기에서는 Stochastic Gradient Descent를 사용함)

```python
optimizer = torch.optim.SGD(model.parameters(), lr = learning_rate)
```


<br>

---

<br>

<!--
# Image Source

* <https://tutorials.pytorch.kr/_images/comp-graph.png>

-->

# References

* <https://tutorials.pytorch.kr/beginner/basics/optimization_tutorial.html>
