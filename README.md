# Prediction Model User Manual
**Predicting Postoperative Brain Injury After Cardiopulmonary Bypass (CPB)**
**Model Version:** 1.0

---

## 1. Overview
本手册提供了使用经过训练的**随机森林回归 (RFR)** 模型来预测体外循环 (CPB) 术后患者脑损伤严重程度的分步说明。损伤程度通过 **APACHE II** 评分进行量化。

* **模型原理**：该模型采用了通过随机森林回归递归特征消除 (RFE-RFR) 筛选出的前 20 个关键特征。
* **数据预处理**：由于随机森林模型对特征缩放具有不变性，因此在使用前**无需进行归一化或标准化**处理。

---

## 2. System Requirements
* **操作系统**：Windows / macOS / Linux
* **Python 版本**：3.8 或更高版本
* **依赖库**：请参考仓库中的 `requirements.txt` 文件

---

## 3. File Structure
确保您的本地目录包含以下必要文件：

```text
├── model/
│   └── rfr_model.pkl                 # 训练好的随机森林模型
├── data/
│   └── example_patient_data.csv      # 输入数据模板
├── scripts/
│   └── predict.py                    # 预测核心脚本
├── requirements.txt                   # Python 依赖清单
└── README.md                         # 项目说明文档

```

---

## 4. Input Data Format

输入数据必须为 CSV 格式，且包含以下 **20 个特征**。请务必严格遵守特征的**命名规范**和**排列顺序**：

| 特征名称 | 描述 | 示例值 |
| --- | --- | --- |
| **BSA** | 体表面积 (m) | 1.75 |
| **CBF/TIV** | 灌注密度指数 (PDI) | 0.05 |
| **rIPG** | 右下顶叶皮质体积 (cm) | 4.33 |
| **ltPuA** | 左侧枕前核体积 (cm) | 0.09 |
| **lIFGorb** | 左侧额下回眶部体积 (cm) | 1.92 |
| **rTPOmid** | 右颞极：颞中回体积 (cm) | 3.85 |
| **rCAU** | 右侧尾状核体积 (cm) | 3.52 |
| **rtPuA** | 右侧枕前核体积 (cm) | 0.07 |
| **rPCUN** | 右前楔叶体积 (cm) | 9.90 |
| **lCERCRU2** | 左侧小脑半球 Crus II 体积 (cm) | 6.70 |
| **rtLGN** | 右侧外侧膝状体体积 (cm) | 0.05 |
| **rtIL** | 右侧板内核体积 (cm) | 0.12 |
| **rCER10** | 右侧小脑半球第 X 叶体积 (cm) | 0.34 |
| **rPoCG** | 右中央后回体积 (cm) | 9.30 |
| **lHIP** | 左侧海马体体积 (cm) | 4.08 |
| **rTPOsup** | 右颞极：颞上回体积 (cm) | 4.70 |
| **Executive_control** | 执行控制网络评分 | 24.5 |
| **rtAV** | 右丘脑前腹核体积 (cm) | 0.06 |
| **lCUN** | 左楔叶体积 (cm) | 4.32 |
| **VER9** | 小脑蚓部第 IX 叶体积 (cm) | 0.55 |

> **注意**：所有大脑结构的体积特征必须统一使用 **cm³** 作为单位。

---

## 5. Running Predictions

### 第一步：安装依赖

在终端或命令行界面运行以下命令：

```bash
pip install -r requirements.txt

```

### 第二步：准备输入文件

创建一个 CSV 文件（例如 `my_patient_data.csv`），包含上述 20 个特征列。首行必须为特征名称，随后每一行代表一名患者的数据。

### 第三步：执行预测

运行预测脚本，并指定输入与输出路径：

```bash
python scripts/predict.py --input my_patient_data.csv --output predictions.csv

```

### 第四步：查看结果

脚本生成的 `predictions.csv` 将包含：

* **Predicted_APACHEII**: 模型为每位患者预测的术后评分。

---

## 6. Interpreting Output

* 预测值是一个连续的分数。
* **分值越高**：表示模型预测的术后脑损伤程度越严重。
* **临床声明**：本预测结果仅作为临床决策辅助工具，**不能取代专业医生的临床判断**。

---

## 7. Troubleshooting

| 常见问题 | 解决方案 |
| --- | --- |
| **"Module not found"** | 请重新运行第一步，确保所有依赖包已正确安装。 |
| **输入文件格式错误** | 请核对 CSV 列名是否与表格中的“特征名称”完全一致。 |
| **预测值异常** | 请检查输入的生理特征值（如体积、BSA）是否在合理范围内。 |
| **找不到模型文件** | 确保 `rfr_model.pkl` 文件存放在 `model/` 文件夹内。 |

---

## 8. Citation & Contact

如果您在研究中引用了此模型，请参考以下信息：

* **引用信息**: [作者, 年份, 期刊, DOI 等]
* **技术支持**: [zhangdongxqyy@163.c0m]
