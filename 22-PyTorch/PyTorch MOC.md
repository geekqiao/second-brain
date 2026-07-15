---
tags:
  - subject/pytorch
  - moc
aliases:
  - PyTorch
  - torch
  - 深度学习
  - PyTorch索引
created: 2026-07-15
status: 🟢 已完成
---

# PyTorch MOC

> [!info] 重点科目 · [[🗺️ 知识地图 MOC]] · [[Home]]

## 📖 概述

**PyTorch** 是当前最主流的**深度学习框架**——用「动态计算图 + 张量 + 自动求导」让定义、训练神经网络变得直观灵活。从研究原型到工业部署（TorchServe），它都是事实标准。经典 ML（表格数据）用 [[sklearn MOC]]，神经网络（图像、文本、序列）用 PyTorch。

> 学 PyTorch 的主线是四件套：**张量（Tensor）→ 自动求导（Autograd）→ 模型（nn.Module）→ 训练循环**。这四块通了，搭任何网络都是排列组合；剩下的只是调结构（卷积、Transformer）和调参。

## 📚 学习路径

1. [[张量基础]] — Tensor 创建、形状操作、dtype、CPU/GPU 迁移
2. [[自动求导 autograd]] — 计算图、requires_grad、backward
3. [[构建模型 nn.Module]] — 层、前向传播、参数
4. [[训练循环]] — loss、optimizer、forward/backward/step、保存加载
5. [[数据加载 Dataset & DataLoader]] — 自定义数据集、transform、批处理

## 🧠 核心速查表

| 需求 | 写法 |
| --- | --- |
| 建张量 | `torch.tensor([1,2])` / `torch.zeros(3,4)` |
| 迁到 GPU | `x = x.to('cuda')` |
| 自动求导 | `x.requires_grad_(True)` → `loss.backward()` → `x.grad` |
| 定义模型 | `class M(nn.Module):` + `forward()` |
| 常用层 | `nn.Linear`, `nn.Conv2d`, `nn.ReLU`, `nn.Dropout` |
| 损失函数 | `nn.MSELoss()`, `nn.CrossEntropyLoss()` |
| 优化器 | `torch.optim.Adam(model.parameters(), lr=1e-3)` |
| 训练三步 | `optimizer.zero_grad()` → `loss.backward()` → `optimizer.step()` |
| 关闭梯度 | `with torch.no_grad():`（评估/推理） |
| 保存/加载 | `torch.save(model.state_dict(), p)` / `model.load_state_dict(...)` |

## 📈 进阶方向

- **网络结构**：CNN（图像）、RNN/LSTM（序列）、Transformer（NLP 主流）。
- **生态**：TorchVision（视觉）、HuggingFace Transformers（NLP）、PyTorch Lightning（简化训练）。
- **部署**：TorchScript / ONNX 导出、TorchServe、量化。
- **GPU/分布式**：多卡训练（DDP）、混合精度（`torch.cuda.amp`）。

---

*🔗 返回 [[🗺️ 知识地图 MOC]] · [[Home]]*
