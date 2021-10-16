### 基本

1. 有两个处理数据 `torch.utils.data.DataLoader` , `Dataset`  . 后者存放数据和标签，前者吧一个iterable包在set中

2. 使用TorchText, TorchVision, TorchAudio 这些基本库

3. ```python
   import torch
   from torch import nn
   from torch.utils.data import DataLoader
   from torchvision import datasets
   from torchvision.transforms import ToTensor, Lambda, Compose
   import matplotlib.pyplot as plt
   
   traning_data = datasets.FashionMNIST(
   	root="data",
       train=True,
       download=True,
       transform=ToTensor(),
   )
   
   test_data = datasets.FashionMNIST(
   	root="data",
       train=False,
       download=True,
       transform=ToTensor(),
   )
   
   batch_size=64
   # create data loaders
   train_dataloader = DataLoader(training_data, batch_size=batch_size)
   test_dataloader = DataLoader(test_data, batch_size=batch_size)
   
   for X, y in test_dataloader:
       print("shape of X[N,C,H,W]", X.shape)
   ```

4. 

