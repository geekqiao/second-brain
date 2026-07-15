---
tags:
  - subject/pytorch
  - data
aliases:
  - Dataset
  - DataLoader
  - 数据加载
  - transform
created: 2026-07-15
status: 🟢 已完成
---

# 数据加载 Dataset & DataLoader

> [!info] 所属：[[PyTorch MOC]] · 上一篇 [[训练循环]]

## 核心概念

训练要喂数据，但数据往往很大（图片、文本）、格式各异。PyTorch 用 **`Dataset`（定义每条数据怎么取）+ `DataLoader`（负责批量、打乱、并行加载）** 的组合，把杂乱数据变成训练循环里 `for xb, yb in loader` 的干净迭代。这二者解耦，灵活且高效。

### Dataset（数据集）
继承 `torch.utils.data.Dataset`，实现两个方法：
- `__len__(self)`：数据集大小（有多少条）。
- `__getitem__(self, idx)`：取第 `idx` 条数据（返回 `(x, y)`）。

在这里做**单条数据的处理**：读图片、解析文本、归一化、增强。

### DataLoader（数据加载器）
把 Dataset 包起来，自动提供：
- **batch**：按 `batch_size` 打包。
- **shuffle**：每 epoch 打乱顺序。
- **多进程加载**：`num_workers` 并行读数据，不阻塞训练。
- **自动迭代**：`for xb, yb in loader:`。

### Transform（数据变换/增强）
对单条数据做的变换，常链式组合：
- 图像：`Resize` → `ToTensor`（转 [0,1] 张量，CHW 排列）→ `Normalize`。
- 增强：`RandomFlip`、`RandomCrop`（训练时加，让模型见过更多变化，抗过拟合）。
- TorchVision 提供 `transforms.Compose([...])` 串联。

### 数据流全景
原始文件 → `Dataset.__getitem__`（读 + transform）→ `DataLoader`（批 + 打乱 + 并行）→ 训练循环。

> [!tip] 内存友好
> Dataset 每次 `__getitem__` 才**即时读取**一条，所以即便数据集几百 GB 也不会一次塞进内存——这是处理大数据的关键。

## 代码示例

```python
import torch
from torch.utils.data import Dataset, DataLoader

# 1) 自定义 Dataset
class TabularDataset(Dataset):
    def __init__(self, X, y, transform=None):
        self.X, self.y, self.transform = X, y, transform

    def __len__(self):
        return len(self.X)

    def __getitem__(self, idx):
        x, label = self.X[idx], self.y[idx]
        if self.transform:
            x = self.transform(x)        # 单条变换
        return x, label

X = torch.randn(1000, 20); y = torch.randint(0, 3, (1000,))
train_ds = TabularDataset(X, y)

# 2) DataLoader：批处理 + 打乱 + 并行
train_loader = DataLoader(
    train_ds, batch_size=32, shuffle=True, num_workers=0,  # Windows 用 0
    drop_last=False,
)
for xb, yb in train_loader:               # xb: [32,20]  yb: [32]
    print(xb.shape, yb.shape); break

# 3) 图像数据集 + transform（用 TorchVision）
from torchvision import datasets, transforms
train_tf = transforms.Compose([
    transforms.Resize((128, 128)),
    transforms.RandomHorizontalFlip(),    # 训练增强
    transforms.ToTensor(),                # → [0,1] 张量，C×H×W
    transforms.Normalize(mean=[.5,.5,.5], std=[.5,.5,.5]),
])
# image_folder = datasets.ImageFolder('data/train', transform=train_tf)
# img_loader = DataLoader(image_folder, batch_size=64, shuffle=True, num_workers=4)

# 4) 划分训练/验证集
from torch.utils.data import random_split
full_ds = TabularDataset(X, y)
train_part, val_part = random_split(full_ds, [800, 200])
train_loader = DataLoader(train_part, batch_size=32, shuffle=True)
val_loader   = DataLoader(val_part, batch_size=64, shuffle=False)  # 评估不打乱
```

## ⚠️ 易错点 / 面试要点

- **Dataset 只需实现 `__len__` + `__getitem__`**：这是协议（[[装饰器迭代器与生成器|鸭子类型]]）。DataLoader 靠这两个方法工作，不管你内部数据是文件、数组还是网络流。
- **训练 shuffle、评估不 shuffle**：训练打乱防模型记顺序；评估打乱无意义，且保持顺序便于对结果。
- **`num_workers` 多进程加载**：能显著加速（数据读取不阻塞 GPU），但 Windows 下可能报错（需 `if __name__=='__main__'` 保护），调试时先设 0。
- **图像维度顺序**：PyTorch 图像是 **C×H×W**（通道在前），而 PIL/NumPy 是 H×W×C。`ToTensor` 会自动转换，别手动拼错维度。
- **数据增强只在训练用**：`RandomFlip` 等增强让模型抗过拟合，但**验证/测试不能增强**（要评估真实表现），用单独的纯 Resize+Normalize 的 transform。
- **大数据别全读进内存**：在 `__init__` 只存路径/索引，在 `__getitem__` 即时读文件，否则内存爆。
- **`drop_last`**：训练时数据不能被 batch_size 整除，最后一个不完整 batch 可丢弃（影响 BatchNorm 统计）；评估时一般保留。

## 相关链接

- [[PyTorch MOC]] · [[训练循环]]
- 概念相通：Python 迭代器协议 [[装饰器迭代器与生成器]]
- 数据预处理思路：[[数据预处理]]（sklearn）
