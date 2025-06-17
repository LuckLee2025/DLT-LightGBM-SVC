大乐透数据处理与分析系统

## 1. dlt_data_processor.py - 数据获取与预处理

### 主要功能
这个脚本负责从多个来源获取大乐透历史开奖数据，并将其整合到一个标准化的CSV文件中。

### 核心逻辑流程
1. **数据获取**：
   - 从网络文本文件获取数据（`fetch_data_from_txt`）- 主要数据源，包含日期信息
   - 从网页抓取数据（`fetch_latest_data`）- 备用数据源，不包含日期信息

2. **数据解析**：
   - 解析文本数据（`parse_txt_data`）- 将原始文本转换为结构化数据
   - 网页数据解析 - 使用BeautifulSoup提取表格数据

3. **数据整合与更新**：
   - 将新获取的数据与现有CSV文件合并（`update_csv_with_txt_data`和`update_csv_with_latest_data`）
   - 优先使用txt文件中的日期信息
   - 使用外连接（outer join）确保保留所有期号的数据

4. **错误处理**：
   - 多种编码尝试（utf-8, gbk, latin-1）
   - 代理连接错误处理
   - 数据格式验证

### 技术特点
- 使用requests库进行网络请求
- 使用BeautifulSoup进行HTML解析
- 使用pandas进行数据处理和CSV操作
- 实现了日志系统和进度显示
- 使用上下文管理器（SuppressOutput）控制输出

## 2. dlt_analyzer.py - 数据分析与预测

### 主要功能
这个脚本负责分析大乐透历史数据，识别模式，训练机器学习模型，并生成下一期的推荐号码组合。

### 核心逻辑流程
1. **数据加载与预处理**：
   - 加载CSV数据（`load_data`）
   - 数据清理和结构化（`clean_and_structure`）
   - 特征工程（`feature_engineer`）- 创建前区和、跨度、奇偶计数等特征

2. **历史统计分析**：
   - 频率和遗漏分析（`analyze_frequency_omission`）
   - 模式分析（`analyze_patterns`）- 分析前区奇偶比、区域分布、后区特征等
   - 关联规则挖掘（`analyze_associations`）- 使用Apriori算法

3. **机器学习模型训练**：
   - 创建滞后特征（`create_lagged_features`）
   - 训练多种模型（`train_prediction_models`）：
     - LightGBM分类器
     - 逻辑回归
     - 支持向量机（SVC）
   - 预测下一期号码概率（`predict_next_draw_probabilities`）

4. **号码评分与组合生成**：
   - 计算综合得分（`calculate_scores`）- 结合频率、遗漏和ML预测概率
   - 生成推荐组合（`generate_combinations`）- 基于得分和历史模式

5. **回测与验证**：
   - 历史数据回测（`backtest`）- 评估预测方法的有效性
   - 结果分析与统计

6. **结果输出**：
   - 生成分析报告
   - 推荐单式组合
   - 推荐前区7+后区7复式投注

### 技术特点
- 使用多种机器学习算法（LightGBM, 逻辑回归, SVC）
- 实现了特征工程和滞后特征创建
- 使用关联规则挖掘（Apriori算法）
- 实现了回测系统评估预测效果
- 使用matplotlib和seaborn进行可视化
- 采用模块化设计，功能分离清晰

## 系统整体效果

这两个脚本共同构成了一个完整的大乐透数据分析与预测系统：

1. **数据流向**：
   - `dlt_data_processor.py` 负责获取和预处理数据，生成标准化CSV
   - `dlt_analyzer.py` 读取CSV，进行分析和预测

2. **系统优势**：
   - **数据获取的健壮性**：多数据源，多编码支持，错误处理
   - **分析的全面性**：结合统计分析和机器学习
   - **预测的多样性**：提供单式和复式推荐
   - **可验证性**：通过回测评估预测效果

3. **实际应用效果**：
   - 系统能够基于历史数据识别模式
   - 使用机器学习预测单个号码的出现概率
   - 生成综合评分较高的号码组合
   - 通过回测评估预测方法的有效性
   - 提供详细的分析报告和可视化结果

## 潜在改进空间

1. **数据获取**：
   - 增加更多数据源的支持
   - 实现自动定时更新

2. **机器学习**：
   - 尝试更多模型和集成方法
   - 实现特征重要性分析
   - 添加超参数优化

3. **用户体验**：
   - 开发图形界面
   - 实现Web应用或移动应用
   - 添加更多可视化和交互式分析

## 项目同步到 GitHub 仓库指南

以下步骤演示了如何将本地项目同步到位于 `https://github.com/LJQ-HUB-cmyk/DLT-LightGBM-SVC` 的 GitHub 远程仓库。若已在其他电脑执行过一次，可直接执行 *步骤 5* 及之后操作。

1. 安装并配置 Git（仅首次使用需要）。
   ```bash
   # Windows 可从 https://git-scm.com 下载并安装
   git config --global user.name  "Your Name"
   git config --global user.email "youremail@example.com"
   ```
2. 打开终端（PowerShell / CMD / Git Bash），切换到本项目根目录。
   ```bash
   cd "E:/Desktop/测试/DLT+LightGBM+逻辑回归+SVC"
   ```
3. 初始化 Git 仓库（首次执行）。
   ```bash
   git init
   ```
4. 将 GitHub 仓库添加为远程并命名为 *origin*（首次执行）。
   ```bash
   # HTTPS 方式
   git remote add origin https://github.com/LJQ-HUB-cmyk/DLT-LightGBM-SVC.git
   # 或者使用 SSH 方式（需先配置 SSH Key）
   # git remote add origin git@github.com:LJQ-HUB-cmyk/DLT-LightGBM-SVC.git
   ```
5. 将项目文件加入版本控制并提交。
   ```bash
   git add .
   git commit -m "Initial commit: import project files"
   ```
6. 如果远程仓库已存在 `README.md` 等初始化文件，建议先拉取再推送，避免历史冲突：
   ```bash
   git pull --rebase origin main  # 或 master，取决于远程默认分支名称
   ```
7. 将本地 `main`（或 `master`）分支推送到 GitHub，并建立跟踪关系。
   ```bash
   git branch -M main             # 可统一改用 main 为主分支
   git push -u origin main        # 首次推送
   ```
8. 之后如有代码修改，只需执行：
   ```bash
   git add <changed_files>
   git commit -m "feat: <your message>"
   git push
   ```

> ⚠️ 如果推送时报错 `rejected - non-fast-forward`，说明 GitHub 上已有新的提交，需要先 `git pull --rebase origin main` 合并后再推送。

完成上述步骤后，即可在浏览器打开仓库地址查看已同步的代码。此后建议在每次开发完成后提交并推送，以保持本地与远程仓库同步。


查看更改状态

git status
添加更改的文件

git add 更改的文件
# 或添加所有更改
git add .
提交更改

git commit -m "更新说明"
推送到GitHub

git push
常见问题解决
如果遇到分支名称问题（如master与main）：

git branch -M main  # 将当前分支重命名为main
如果需要强制推送（谨慎使用）：

git push -f origin main
如果需要从GitHub拉取最新更改：

git pull origin main