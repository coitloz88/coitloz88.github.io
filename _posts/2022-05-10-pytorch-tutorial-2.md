---
title: Pytorch tutorials 02 | Dataset과 DataLoader
date: 2022-05-10 13:55:00 +0900
categories: [ML, pytorch]
tags: [ML, pytorch, tutorial]     # TAG names should always be lowercase
---

데이터셋 코드는 더 나은 가독성과 모듈성을 위해 모델 학습 코드로부터 분리하는 것이 좋다.  
Pytorch에서는 `torch.utils.data.DataLoader`와 `torch.utils.data.Dataset`의 두 가지 데이터 기본 요소를 통해 미리 준비된(pre-loaded) 데이터 셋은 물론 가지고 있는 데이터를 사용할 수도 있다.  
`Dataset`은 샘플과 정답(label)을 저장하고, `DataLoader`는 `Dataset`을 샘플에 쉽게 접근할 수 있도록 순회 가능한 객체(iterable)로 감싼다.

<br>

---

<br>

# 데이터셋 불러오기

*TorchVision*에서 [FashionMNIST] 데이터셋을 불러오는 예제를 살펴보자.

* `root`: 학습/테스트 데이터가 저장되는 경로
* `train`: 학습용 또는 테스트 데이터셋 여부를 지정
* `download=True`: `root`에 데이터가 없는 경우 인터넷에서 다운로드
* `transform`과 `target_transform`: feature과 label transform을 지정

```python
import torch
from torch.utils.data import Dataset
from torchvision import datasets
from torchvision.transforms import ToTensor
import matplotlib.pyplot as plt

training_data = datasets.FashionMNIST(
    root = "data",
    train = True,
    download = True,
    transform = ToTensor()
)

test_data = datasets.FashionMNIST(
    root = "data",
    train = False,
    download = True,
    transform = ToTensor()
)
```

<br>

---

<br>

# 데이터 셋을 순회하고 시각화하기

`Dataset`에 List처럼 직접 접근(index)할 수 있다.

* 예: `training_data[index]`

```python
labels_map = {
    0: "T-Shirt",
    1: "Trouser",
    2: "Pullover",
    3: "Dress",
    4: "Coat",
    5: "Sandal",
    6: "Shirt",
    7: "Sneaker",
    8: "Bag",
    9: "Ankle Boot",
}

figure = plt.figure(figsize=(8,8))
cols, rows = 3, 3

for i in range(1, cols * rows + 1):
    sample_idx = torch.randint(len(training_data), size=(1,)).item()
    img, label = training_data[sample_idx]
    figure.add_subplot(rows, cols, i)
    plt.title(labels_map[label])
    plt.axis("off")
    plt.imshow(img.squeeze(), cmap = "gray")

plt.show()
```

<br>

---

<br>


# 파일에서 사용자 정의 데이터셋 만들기

사용자 정의 Dataset 클래스는 반드시 3개의 함수를 구현해야 한다.

1. `__init__`
    Dataset 객체가 생성(instantiate)될 때 한 번만 실행된다.

2. `__len__`
    데이터셋의 샘플 개수를 반환한다.

3. `__getitem__`
    주어진 인덱스 `idx`에 해당하는 샘플을 데이터셋에서 불러오고 반환한다. 인덱스를 기반으로, 디스크에서 이미지의 위치를 식별하고, `read_image`를 사용하여 이미지를 텐서로 변환하고, `self.img_labels`의 csv 데이터로부터 해당하는 정답(label)을 가져오고, 해당되는 경우 변형(transform) 함수들을 호출한 뒤, 텐서 이미지와 라벨을 Python 사전(dict)형으로 반환한다.


```python
import os
import pandas as pd
from torchvision.io import read_image

class CustomImageDataset(Dataset):
    def __init__(self, annotations_file, img_dir, transform=None, target_transform=None):
        self.img_labels = pd.read_csv(annotations_file, names = ['file_name', 'label'])
        self.img_dir = img_dir
        self.transform = transform
        self.target_transform = target_transform
    
    def __len__(self):
        return len(self.img_labels)

    def __getitem__(self, idx):
        img_path = os.path.join(self.img_dir, self.img_labels.iloc[idx, 0])
        image = read_image(image_path)
        label = self.img_labels.iloc[idx, 1]
        if self.transform:
            image = self.transform(image)
        if self.target_transform:
            label = self.target_transform(label)
        return image, label
```


<br>

---

<br>


# DataLoader로 학습용 데이터 준비하기

`Dataset`은 데이터셋의 feature을 가져오고, 하나의 샘플에 label을 지정하는 일을 한 번에 수행한다.  
모델을 학습할 때, 일반적으로 샘플들을 minibatch로 전달하고, 매 epoch마다 데이터를 다시 섞어서 overfit을 막는다. 또한, Python의 `multiprocessing`을 사용하여 데이터 검색 속도를 높인다.  

`DataLoader`는 간단한 API로, 이러한 복잡한 과정을 추상화한 순회 가능한 객체(iterable)이다.

```python
from torch.utils.data import DataLoader

train_dataloader = DataLoader(training_data, batch_size = 64, shuffle = True)
test_dataloader = DataLoader(test_data, batch_size = 64, shuffle = True)
```

<br>

---

<br>

# DataLoader를 통해 순회하기(iterate)

`DataLoader`에 데이터 셋을 불러오고 나서는, 필요에 따라 데이터 셋을 순회(iterate)할 수 있다. 

```python
# 이미지와 정답(label)을 표시한다.
train_features, train_label = next(iter(train_dataloader))
print(f"Feature batch shape: {train_features.size()}")
print(f"Labels batch shape: {train_labels.size()}")

img = train_features[0].squeeze()
label = train_labels[0]

plt.imshow(img, cmap = "gray")
plt.show()

print(f"Label: {label}")
```


<br>

---

<br>

# References

* <https://tutorials.pytorch.kr/beginner/basics/data_tutorial.html>
* <https://wikidocs.net/32829>
* <https://gooopy.tistory.com/68>