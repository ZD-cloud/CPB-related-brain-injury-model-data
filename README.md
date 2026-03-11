# [cite_start]Prediction Model User Manual [cite: 1]
[cite_start]**Predicting Postoperative Brain Injury After Cardiopulmonary Bypass (CPB)** [cite: 1]
[cite_start]**Model Version:** 1.0 [cite: 1]

---

## 1. Overview
* [cite_start]本手册提供了使用经过训练的随机森林回归 (RFR) 模型预测体外循环 (CPB) 患者术后脑损伤严重程度（由 APACHE II 评分量化）的操作说明 [cite: 1]。
* [cite_start]该模型使用通过随机森林回归递归特征消除 (RFE-RFR) 选择的前 20 个特征 [cite: 2]。
* [cite_start]由于随机森林对特征缩放具有不变性，因此不需要额外的数据预处理（如归一化）[cite: 3]。

---

## [cite_start]2. System Requirements [cite: 4]
* [cite_start]**Operating System:** Windows / macOS / Linux [cite: 4]。
* [cite_start]**Python Version:** 3.8 或更高版本 [cite: 4]。
* [cite_start]**Required Python Packages:** 请参阅仓库中的 `requirements.txt` [cite: 4]。

---

## [cite_start]3. File Structure [cite: 4]
请确保已从仓库下载以下文件：
* [cite_start]`model/rfr_model.pkl`: 训练好的随机森林模型 [cite: 4]。
* [cite_start]`data/example_patient_data.csv`: 示例输入数据模板 [cite: 4]。
* [cite_start]`scripts/predict.py`: 预测脚本 [cite: 4]。
* [cite_start]`requirements.txt`: Python 依赖项 [cite: 5]。
* [cite_start]`README.md`: 复现说明 [cite: 5]。

---

## [cite_start]4. Input Data Format [cite: 5]
[cite_start]模型要求输入数据为 CSV 文件，并包含以下 20 个选定特征，且必须严格遵守下表的顺序和命名规范 [cite: 5, 6]：

| 特征名称 | 描述 | 示例值 |
| :--- | :--- | :--- |
| **BSA** | 体表面积 (m) | [cite_start]1.75 [cite: 6, 7] |
| **CBF/TIV** | 灌注密度指数 (PDI) | [cite_start]0.05 [cite: 7] |
| **rIPG** | 右下顶叶皮质体积 (cm) | [cite_start]4.33 [cite: 7] |
| **ltPuA** | 左侧枕前核体积 (cm) | [cite_start]0.09 [cite: 7, 8] |
| **lIFGorb** | 左侧额下回眶部体积 (cm) | [cite_start]1.92 [cite: 8] |
| **rTPOmid** | 右颞极：颞中回体积 (cm) | [cite_start]3.85 [cite: 8] |
| **rCAU** | 右侧尾状核体积 (cm) | [cite_start]3.52 [cite: 8, 9] |
| **rtPuA** | 右侧枕前核体积 (cm) | [cite_start]0.07 [cite: 9] |
| **rPCUN** | 右前楔叶体积 (cm) | [cite_start]9.90 [cite: 9, 10] |
| **lCERCRU2** | 左侧小脑半球 Crus II 体积 (cm) | [cite_start]6.70 [cite: 10] |
| **rtLGN** | 右侧外侧膝状体体积 (cm) | [cite_start]0.05 [cite: 10] |
| **rtIL** | 右侧板内核体积 (cm) | [cite_start]0.12 [cite: 10, 11] |
| **rCER10** | 右侧小脑半球第 X 叶体积 (cm) | [cite_start]0.34 [cite: 11] |
| **rPoCG** | 右中央后回体积 (cm) | [cite_start]9.30 [cite: 11, 12] |
| **lHIP** | 左侧海马体体积 (cm) | [cite_start]4.08 [cite: 12] |
| **rTPOsup** | 右颞极：颞上回体积 (cm) | [cite_start]4.70 [cite: 12] |
| **Executive_control** | 执行控制网络评分 | [cite_start]24.5 [cite: 12, 13] |
| **rtAV** | 右丘脑前腹核体积 (cm) | [cite_start]0.06 [cite: 13] |
| **lCUN** | 左楔叶体积 (cm) | [cite_start]4.32 [cite: 13] |
| **VER9** | 小脑蚓部第 IX 叶体积 (cm) | [cite_start]0.55 [cite: 14] |

> [cite_start]**注意：** 所有体积特征应使用统一的单位 (cm) [cite: 15]。

---

## [cite_start]5. Running Predictions [cite: 16]
* **Step 1: 安装依赖** - 在终端运行 `pip install -r requirements.txt` [cite: 16]。
* [cite_start]**Step 2: 准备输入文件** - 创建包含 20 个特征和标题行的 CSV 文件 [cite: 16]。
* [cite_start]**Step 3: 运行预测脚本** - 执行 `python scripts/predict.py --input my_patient_data.csv --output predictions.csv` [cite: 16]。
* **Step 4: 查看结果** - 脚本生成的 `predictions.csv` 将包含每位患者的 `Predicted_APACHEII` 评分 [cite: 16]。

---

## 6. Interpreting Output [cite: 17]
* [cite_start]预测值是一个连续评分 [cite: 17]。
* [cite_start]较高的分数表示预测的脑损伤更严重 [cite: 17]。
* 此预测旨在辅助临床决策，不能取代临床判断 [cite: 16, 17]。

---

## 7. Troubleshooting [cite: 18]
* [cite_start]**"Module not found" 错误：** 确保已安装所有依赖项 [cite: 18]。
* [cite_start]**输入文件格式错误：** 检查 CSV 是否包含精确的 20 个特征且标题匹配 [cite: 18]。
* **预测值过于极端：** 验证特征值是否在合理的生理范围内 [cite: 18]。
* [cite_start]**找不到模型文件：** 确保 `rfr_model.pkl` 位于 `model/` 目录中 [cite: 18]。

---

## [cite_start]8. Citation & Contact [cite: 19]
* [cite_start]**引用：** 如果您在研究中使用此模型，请引用相关论文 [cite: 19]。
* **联系方式：** 如有技术问题，请联系 [zhangdongxqyy@163.c0m] [cite: 19]。
