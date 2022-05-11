---
title: Pytorch tutorials 03 | Transform
date: 2022-05-11 12:40:00 +0900
categories: [ML, pytorch]
tags: [ML, pytorch, tutorial]     # TAG names should always be lowercase
---

때때로 머신러닝 알고리즙 학습을 위해서는 데이터를 변형(transform)해서 데이터를 조작하고 학습에 적합하게 만들어야 한다.  

모든 TorchVision 데이터셋들은 변형 로직을 가진, 호출 가능한 객체(callable)를 받는 매개 변수 두개

1. 특징(feature)을 변경하기 위한 `transform`
2. 정답(label)을 변경하기 위한 `target_transform`

을 갖는다.  

FashionMNINST의 feature는 PIL Image 형식이고, label은 Interger이다. 학습을 하기 위해서는 Normarlize된 텐서 형태의 feature과 one-hot으로 encode된 텐서 형태의 label이 필요하다. 따라서, 이러한 transformation을 하기 위해 `ToTensor`와 `Lambda`를 사용한다.

```python
import torch
from torchvision import datasets
from torchvision.transforms import ToTensor, Lambda

ds = datasets.FashionMNIST(
    root = "data",
    train = True,
    download = True,
    transform = ToTensor(),
    target_transform = Lambda(lambda y: torch.zeros(10, dtype = torch.float).scatter_(0, torch.tensor(y), value = 1))
)
```

<br>

---

<br>

# ToTensor()

[ToTensor](https://pytorch.org/vision/stable/transforms.html#torchvision.transforms.ToTensor)는 PIL Image나 NumPy `ndarray`를 `FloatTensor`로 변환하고, 이미지의 픽셀의 크기(intensity) 값을 [0., 1.] 범위로 비례하여 조정한다.

# Lambda 변형(Transform)

Lambda 변형은 사용자 정의 람다(lambda) 함수를 적용한다.


<br>

---

<br>


# References

* <https://tutorials.pytorch.kr/beginner/basics/transforms_tutorial.html>
