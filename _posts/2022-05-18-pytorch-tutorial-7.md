---
title: Pytorch tutorials 07 | 모델 저장하고 불러오기
date: 2022-05-30 13:50:00 +0900
categories: [ML, pytorch]
tags: [ML, pytorch, tutorial]     # TAG names should always be lowercase
---

마지막으로 모델의 예측을 저장, 로드 및 실행하여 모델의 state를 유지하는 방법에 관해 알아보려고 한다.

```python
import torch
import torchvision.models as models
```

# Saving and Loading Model Weights

Pytorch의 모델은 학습한 매개변수를 `state_dict`라고 불리는 internal state dictionary에 저장한다. 이 상태 값들은 `torch.save`를 사용하여 저장(persist)할 수 있다.

```python
model = models.vgg16(pretrained = True)
torch.save(model.state_dict(), 'model_weights.pth')
```

모델 weights를 불러오기 위해서는, 먼저 동일한 모델의 인스턴스(instance)를 생성한 다음에 `load_state_dict()` 메소드를 사용하여 매개변수들을 불러온다.

```python
model = models.vgg16()
model.load_state_dict(torch.load('model_weights.pth'))
model.eval()
```

> Inference를 하기 전에, `model.eval()` 메소드를 호출하여 dropout과 batch normalization을 evaluation mode로 설정해야 한다.
{: .prompt-tip }

<br>

# Saving and Loading Models with Shapes

모델의 weights를 불러올 때, 신경망의 구조를 정의하기 위해 모델 클래스를 먼저 생성해야 한다. 이 클래스의 구조를 모델과 함께 저장하려면 `model` (not `model.state_dict()`)을 저장 함수에 전달해야 한다.

```python
torch.save(model, 'model.pth')
```

그러면 다음과 같이 모델을 불러올 수 있다.
```python
model = torch.load('model.pth')
```

> 이러한 접근 방식은 Python pickle 모듈을 사용하여 모델을 직렬화한다.
{: .prompt-tip }



<br>

---

<br>

<!--
# Image Source

* <https://tutorials.pytorch.kr/_images/comp-graph.png>

-->

# References

* <https://tutorials.pytorch.kr/beginner/basics/saveloadrun_tutorial.html>