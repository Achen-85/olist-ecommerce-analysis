# Olist 巴西电商销售数据分析项目

**Olist Brazilian E-commerce Sales Analysis**

> 基于 Olist 巴西电商公开数据，使用 Python
> 完成数据清洗、多表关联、销售表现、商品品类、地区市场与客户行为分析，并通过
> Matplotlib 输出可视化结果。

## 项目简介

本项目以 Olist
巴西电商平台公开数据为分析对象，围绕电商业务中的核心问题展开：

-   平台已送达订单的商品销售表现如何？
-   哪些商品品类贡献了更多商品销售额？
-   销售主要集中在哪些客户所在州？
-   客户购买频次和复购情况如何？
-   如何识别累计消费金额较高的客户群体？

项目主要使用 Pandas 进行数据处理和统计分析，使用 Matplotlib
进行图表展示。分析过程、代码和运行结果集中在 Jupyter Notebook
中，便于查看与复现。

## 项目内容

  -------------------------------------------------------------------------------------------------------------
  模块                                分析内容
  ----------------------------------- -------------------------------------------------------------------------
  数据读取与质量检查                  读取 CSV 数据，检查数据规模、字段类型、缺失值、重复记录和订单状态

  数据表合并                          以商品明细表为基础，关联订单、商品、客户及品类英文翻译数据

  数据清洗                            转换日期和金额字段，筛选已送达订单，构建销售分析数据集

  销售核心指标                        商品销售额、已送达订单量、客户数、平均订单商品金额、月度趋势及同比/环比

  商品品类分析                        品类销售额、订单量、Top 10 品类、品类销售额占比

  地区销售分析                        按客户所在州统计销售额、订单量、客户数和客单价，并比较主要市场

  客户行为分析                        新老客户、复购率、购买频次、基于累计消费金额的客户分层

  数据可视化与业务建议                使用 Matplotlib
                                      绘制销售趋势、品类占比、地区销售及客户分析图表，并总结业务建议
  -------------------------------------------------------------------------------------------------------------

## 技术栈

-   **Python**
-   **Pandas**：数据读取、清洗、合并、分组聚合和指标计算
-   **NumPy**：数值计算
-   **Matplotlib**：数据可视化
-   **Jupyter Notebook**：分析过程记录与展示
-   **Git / GitHub**：项目版本管理与作品集展示

## 数据来源

数据集：**Brazilian E-Commerce Public Dataset by Olist**

Kaggle 页面：
https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce

数据包含订单、商品明细、商品信息、客户信息和商品品类翻译等 CSV
文件。请从 Kaggle 数据集页面下载并按下方目录结构放置。

## 项目目录

``` text
olist-ecommerce-analysis/
├── data/
│   └── raw/                         # 本地存放 Kaggle 原始 CSV
├── notebooks/
│   └── sales_analysis.ipynb         # 核心分析 Notebook
├── images/
│   ├── monthly_sales_trend.png      # 月度销售趋势图
│   ├── category_sales_top10.png     # Top 10 品类销售额
│   ├── category_sales_share.png     # 商品品类销售占比
│   ├── region_sales_top10.png       # Top 10 州销售额
│   ├── region_orders_top10.png      # Top 10 州订单量
│   ├── region_aov_top10.png         # 符合订单量门槛的州客单价排名
│   ├── customer_purchase_frequency.png
│   └── customer_value_segmentation.png
├── README.md
└── .gitignore
```

图表文件会在 Notebook
对应代码单元格运行后生成。若某张图尚未生成，可先运行 Notebook 再补充到
`images/` 文件夹。

## 环境配置与运行

### 1. 克隆仓库

``` bash
git clone https://github.com/Achen-85/olist-ecommerce-analysis.git
cd olist-ecommerce-analysis
```

### 2. 创建并激活虚拟环境（可选但推荐）

Windows：

``` bash
python -m venv .venv
.venv\Scripts\activate
```

macOS / Linux：

``` bash
python3 -m venv .venv
source .venv/bin/activate
```

### 3. 安装依赖

``` bash
pip install pandas numpy matplotlib jupyter
```

### 4. 下载并放置数据

从 Kaggle 下载 Olist 数据集，将本项目 Notebook 所需 CSV 文件放入
`data/raw/`。至少需要：

-   `olist_orders_dataset.csv`
-   `olist_order_items_dataset.csv`
-   `olist_products_dataset.csv`
-   `olist_customers_dataset.csv`
-   `product_category_name_translation.csv`

### 5. 启动 Jupyter Notebook

``` bash
jupyter notebook
```

打开 `notebooks/sales_analysis.ipynb`，按顺序运行单元格。Notebook
中使用相对路径 `../data/raw/` 读取数据，并将图表保存到 `../images/`。

## 核心指标口径与注意事项

为避免对分析结果产生误解，本项目采用以下统计口径：

1.  **分析订单范围**：核心销售与客户行为分析以
    `order_status == "delivered"` 的已送达订单为基础。
2.  **商品销售额**：使用商品明细表中的 `price` 求和，不包含运费
    `freight_value`；本项目未计算商品成本、利润或净收入。
3.  **订单量**：使用 `order_id`
    去重统计，不能直接使用商品明细行数替代订单量。
4.  **客户数**：使用 `customer_unique_id`
    识别客户，避免将同一客户的不同订单误认为不同客户。
5.  **商品明细数量**：商品明细表一行代表一条订单商品明细记录。数据集中没有原始商品购买数量字段，因此不能将明细行数直接解释为实际商品件数。
6.  **新客户定义**：指在本数据集已送达订单范围内首次出现的客户，不代表客户在平台完整生命周期中的首次购买。
7.  **复购客户定义**：在统计范围内至少有两笔不同已送达订单的客户。
8.  **高价值客户定义**：按客户累计商品消费金额的第 90
    百分位数进行分层；该指标反映消费金额，不等同于利润贡献，且分位数并列时客户占比可能不恰好为
    10%。
9.  **地区维度**：使用客户所在州 `customer_state`，并非卖家所在地。

## 主要输出

运行 Notebook 后，可以查看：

-   销售核心指标与月度销售汇总表
-   商品品类销售额排名及占比
-   州级销售额、订单量和客单价排名
-   新老客户、复购率、购买频次和客户价值分层结果
-   保存至 `images/` 的分析图表

所有具体数值以 Notebook 在当前数据文件上实际运行的输出为准。本 README
不预填未经本次运行验证的统计结果。

## 项目局限与后续优化方向

-   当前销售额未扣除成本、退款、折扣或其他费用，因此不能代表利润。
-   客户首次购买和复购判断受公开数据集时间范围及已送达订单筛选条件限制。
-   可进一步整合支付、评价、物流时效和卖家数据，解释销售与客户体验差异。
-   可继续使用 SQL 复现关键指标，并使用 Power BI 制作交互式业务看板。

## English Summary

This project analyzes the Olist Brazilian e-commerce public dataset
using Python. It covers data quality checks, table joins,
delivered-order sales metrics, product category performance,
customer-state analysis, customer repurchase behavior, customer purchase
frequency, and customer value segmentation. Matplotlib is used to create
visualizations, while the Jupyter Notebook documents the analysis
workflow and results.

## License and attribution

The dataset is provided by Olist through the Kaggle dataset page linked
above. Please review the dataset page and its terms before reuse. This
repository contains an independent analysis project and is not an
official Olist product.
