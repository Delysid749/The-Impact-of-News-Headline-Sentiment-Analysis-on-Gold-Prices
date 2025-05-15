# 黄金价格与新闻情感分析

## 项目简介

本项目通过 **`Gold_Price_Analysis.ipynb`** Notebook，系统性地研究了 2000‑2025 年黄金期货（代码 `GC=F`）价格与华尔街日报新闻标题情绪之间的关联。Notebook 覆盖了数据获取、清洗、探索性数据分析（EDA）、情感打分、特征工程、时间序列可视化、预测模型构建与评估的完整流程。

## 数据集来源

- **名称**：Precious Metals: Data & News (2000‑Present)
- **链接**：Kaggle 公开数据集（数据列举金/银/铂/钯的每日 OHLCV 及对应新闻标题）。
- **使用范围**：本 Notebook 仅使用黄金相关行，并额外基于 *FinBERT* 对新闻标题生成情感极性得分，保存为 `gold_data_with_finbert_sentiment.csv`。

## 文件与目录结构

```text
├── Gold_Price_Analysis.ipynb   # 主分析 Notebook
├── gold_data_with_finbert_sentiment.csv  # 预处理后的数据集
├── yearly_wordclouds_grid.png  # 结果示例图（自动生成）
├── README.md                  # 当前文件
└── requirements.txt           # 可选：依赖清单（建议提供）
```

## 环境依赖

> **建议使用 Python ≥ 3.9，并在虚拟环境中运行**。

核心依赖包（Notebook 中显式导入）：

- pandas, numpy, matplotlib, seaborn
- scikit‑learn, xgboost, statsmodels, scipy
- nltk, wordcloud
- torch, transformers（FinBERT 推断所需）

快速安装示例：

```bash
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
pip install -U pip wheel
pip install pandas numpy matplotlib seaborn scikit-learn xgboost statsmodels scipy nltk wordcloud torch transformers
```

> **NLTK 资源下载**：首次运行 Notebook 时，会自动调用 `nltk.download()` 下载必要的分词与停用词语料。

## 快速上手

1. **克隆或下载** 本仓库。

2. **放置数据集**：确保 `gold_data_with_finbert_sentiment.csv` 位于仓库根目录，或修改 Notebook 中 `pd.read_csv()` 路径。

3. **激活虚拟环境 & 安装依赖**（见上节）。

4. **启动 Jupyter**：

   ```bash
   jupyter notebook Gold_Price_Analysis.ipynb
   ```

5. 按顺序执行各代码单元，或直接运行 `Run All`。

## Notebook 内容概览

| 序号 | 模块               | 关键操作                                                     |
| ---- | ------------------ | ------------------------------------------------------------ |
| 1    | 初始设置与数据导入 | 环境准备、数据加载、缺失值检查、类型转换                     |
| 2    | EDA                | 描述性统计、时间序列趋势图、价格分布、成交量季节性分析       |
| 3    | 情感分析           | 使用 **FinBERT** 对新闻标题打分，生成 *avg_polarity*，并可视化年度词云 |
| 4    | 特征工程           | 构造滞后收益率、计算对数收益、标准化特征等                   |
| 5    | 建模与评估         | 线性回归、Logistic 回归、XGBoost 回归/分类，TimeSeriesSplit 交叉验证，性能指标（MSE、MAE、R²、ROC‑AUC、混淆矩阵） |
| 6    | 结果解读           | 比较情感得分与黄金价格涨跌的 Pearson 相关、滞后/领先效应结论 |
| 7    | 结论与下一步工作   | 研究局限、可改进方向（多资产、多语种情感、宏观因子）         |



## 主要结果

- 2008 及 2020 年经济冲击期出现显著价格波动与负面情绪同步。
- Pearson 相关系数在 **0.28～0.35**（取决于滞后 0‑1 天），说明情感对短期价格具有弱到中度解释力。
- XGBoost 分类器在预测“次日上涨/下跌”时，平均 AUC ≈ **0.63**，优于基线模型。
- 年度词云揭示：危机年份关键词集中在 *recession*、*uncertainty* 等负面词汇。

