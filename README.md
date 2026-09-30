> **🗄️ 项目已完成** · 最近提交：2025-12-24（约 9 个月前）
>
> 课程期末作业，已结课，功能完整、代码保留作参考；**不再接受功能迭代与 Bug 修复**。

<div align="center">

# 🎬 电影评论情感分析系统

**基于 BERT 的中文三分类情感分析 —— 从数据处理到 Web 演示的完整流水线**

《期末大作业》课程项目 · 代码与结果保留作参考

<sub>数据集：798 条影评语料 · 三分类（正面 / 负面 / 中性）</sub>

</div>

---

## 📚 项目简介

基于预训练语言模型（**BERT**）的中文电影评论情感分类系统，对影评文本做**三分类**情感判断：**正面 / 负面 / 中性**。

项目覆盖一条完整的机器学习流水线：

```
数据准备  →  模型微调  →  评估  →  交互演示
   │            │          │         │
   ▼            ▼          ▼         ▼
语料清洗     BERT 微调   准确率/     命令行
标签映射     超参可调   混淆矩阵     + 网页
数据集划分              错误分析     双形态演示
```

> **同源说明**：本仓库与 [`Campus-Text-Sentiment-Classification-System`](https://github.com/Fish-under-sea/Campus-Text-Sentiment-Classification-System) 共用同一套代码骨架（文件结构完全一致），区别仅在于**替换了语料与模型产物**。

## ✨ 主要特性

- **完整工作流** —— 数据准备 → 模型训练 → 评估 → 交互演示，一条命令跑通
- **优化数据处理** —— 智能数据分割、数据质量检查
- **详细评估报告** —— 准确率、分类报告、混淆矩阵、错误案例分析
- **双形态演示** —— 命令行交互 + Flask 网页版
- **配置灵活** —— 支持命令行参数与配置文件双通道覆盖
- **多种模型可选** —— `bert` / `qwen` / `simple` 三种模式

## ⚠️ 关于评估结果的重要说明

> **本节是本 README 最需要你留意的地方。仓库内的两份结果文件相互矛盾，请不要把「100% 准确率」当作本项目的真实性能。**

### 仓库中的两份结果

**① `results/evaluation_results.json`（评估脚本产出）**

| 指标 | 数值 |
|------|------|
| 准确率 | **100.00%**（121 样本，错误数 **0**） |
| 分类别 F1 | 正面 1.0000 / 负面 1.0000 / 中性 1.0000 |

**② `results/training_results.json`（训练过程产出）**

| 指标 | 数值 |
|------|------|
| 最佳验证准确率 | **66.67%** |
| 训练样本数 | **16** |
| 验证样本数 | **3** |
| 测试样本数 | **4** |
| 训练轮数 / 批大小 / 学习率 | 3 / 4 / 2e-05 |
| 训练准确率曲线 | 0.375 → 0.6875 → 0.875 |
| 验证准确率曲线 | 0.333 → 0.667 → 0.667 |

### 为什么这两个数字不可信

1. **样本量对不上**：训练结果记录的是 **16 / 3 / 4** 个样本，而数据集统计文件（`dataset_stats.json`）显示的是 **558 / 119 / 121**。两者不是同一次运行，属于**旧配置残留**。
2. **测试集太小**：即使按 121 样本计，**0 错误**在 NLP 三分类任务上几乎不可能是真实泛化能力，通常指向**数据泄漏**（测试样本出现在训练集中）或评估集与训练集重叠。
3. **两者互相矛盾**：同一模型不可能一边是 66.67% 验证准确率、一边是 100% 测试准确率。

**结论**：本项目作为**课程作业的工程骨架**是完整的、可运行的；但仓库内的评估数字**只应视为流水线跑通的证明，不构成性能结论**。若需真实指标，请按下方步骤重新训练与评估。

## 🗂️ 数据集

| 项 | 数值 |
|----|------|
| 有效样本 | **798** |
| 划分方式 | 随机划分（random），比例 **7 : 1.5 : 1.5** |
| 训练集 | 558（正面 215 / 负面 212 / 中性 131） |
| 验证集 | 119（正面 43 / 负面 38 / 中性 38） |
| 测试集 | 121（正面 52 / 负面 33 / 中性 36） |
| 标签映射 | `positive=0` / `negative=1` / `neutral=2`（见 `datasets/labels.json`） |

**类别分布**：正面 310 / 负面 283 / 中性 205 —— 相比同源的校园语料版本，本数据集**三类别分布更均衡**（最多与最少之比约 1.5:1）。

## 🛠️ 技术栈

| 层 | 实现 |
|----|------|
| 语言 | Python（推荐 3.10） |
| 深度学习 | PyTorch + Transformers（`bert-base-chinese`） |
| 数据处理 | pandas · numpy |
| NLP 模型 | `models/bert-base-chinese/`（预训练） → `models/finetuned/`（微调后） |
| Web 演示 | Flask（`app/app.py` + `app/templates/index.html`） |
| 环境管理 | conda |
| 模型分发 | Git LFS（`git lfs pull`） |

## 🚀 运行说明

### 环境配置

```bash
# 创建并激活环境
conda create -n movies
conda activate movies

# 安装依赖（使用清华源加速）
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple -r req.txt

# 拉取实际模型文件（仓库使用 Git LFS 存储模型）
git lfs pull
```

### 代码运行

```bash
cd 'D:\你的文件目录\电影评论情感分析{期末大作业}\'

python .\main.py --all      # 运行完整训练流程
python .\main.py --demo     # 命令行交互式演示
python .\app\app.py         # 网页版演示
```

### 全部可用参数（实测自 `main.py` argparse）

| 参数 | 作用 |
|------|------|
| `--prepare-data` | 数据准备与清洗 |
| `--train` | 训练（通用入口） |
| `--train-bert` | 显式训练 BERT 模型 |
| `--train-simple` | 训练轻量级模型 |
| `--evaluate` | 模型评估，产出报告 |
| `--demo` | 命令行交互演示 |
| `--all` | 完整流水线（准备→训练→评估） |
| `--list-config` | 打印当前配置 |
| `--fix-env` | 环境修复 |
| `--setup-model` | 下载/配置 BERT 模型 |
| `--pretrained-model <路径>` | 覆盖预训练模型路径 |
| `--finetuned-model <路径>` | 覆盖微调模型路径 |
| `--device {cpu,cuda}` | 指定计算设备 |
| `--batch-size <N>` | 批大小 |
| `--num-epochs <N>` | 训练轮数 |
| `--max-length <N>` | 最大序列长度 |
| `--model-type {bert,qwen,simple}` | 选择模型类型 |

> 想拿到**可信指标**，建议用更大的 `--batch-size` 与更多 `--num-epochs` 重跑 `--train-bert` + `--evaluate`，并确认测试集与训练集无重叠。

## 📁 项目结构

```text
电影评论情感分析系统/
│
├── 📂 app/                         # Web 应用
│   ├── 📂 static/                  # 静态资源（CSS, JS, 图片）
│   ├── 📂 templates/index.html     # 主页面
│   ├── 📂 configs/model_config.py  # 模型配置
│   └── app.py                      # Flask 应用主文件
│
├── 📂 cache/                       # huggingface / transformers 缓存
├── 📂 datasets/
│   ├── 📂 processed/               # train.csv / val.csv / test.csv / dataset_stats.json
│   ├── 📂 raw/                     # 原始影评语料
│   └── labels.json                 # 标签定义
│
├── 📂 models/
│   ├── 📂 bert-base-chinese/       # BERT 预训练模型
│   ├── 📂 finetuned/               # 微调后的模型
│   └── 📂 tokenizer/               # Tokenizer 文件
│
├── 📂 notebooks/                   # 数据下载与处理
├── 📂 results/                     # training_results.json / evaluation_results.json
├── 📂 scripts/
│   ├── 📂 src/                     # setup_bert_model / fix_environment / fix_issues / fix_pytorch
│   ├── data_preprocess.py          # 数据预处理
│   ├── train_bert.py               # BERT 训练
│   └── evaluate_cpu.py             # CPU 环境下评估
│
├── main.py                         # 主程序入口
└── req.txt                         # 依赖列表
```

## 📌 说明

- 本项目为**课程期末作业**，已结课归档，**不提供后续维护**。
- 评估数字存在矛盾，已在「关于评估结果的重要说明」一节明确标注 —— 请勿引用为性能结论。
- 语料为公开影评数据，不含个人敏感信息。

<sub>课程期末作业 · 2025 年秋季学期 · 代码与结果保留作参考</sub>