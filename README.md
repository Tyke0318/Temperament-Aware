### Fashion Style Recommendation via Temperament-Aware Latent Regression Using VAE and Semantic Vector Spaces

# Fashion-Product Datasets

### The structure of the dataset:

`Fashion-Product` Dataset:

```
- Details.zip (Product details) 
- reviews_1.zip (Buyer's Review 1) 
- reviews_2.zip (Buyer's Review 2)
```

Due to the restriction of the file size in Github, we divided the `reviews` dataset into two separate parts. You should combine `Buyer's Reviews` in a single file named `reviews`after you downloaded and unzipped them.

The dataset conducted product searches in the Fashion Section of Amazon using 15 keywords respectively, and the results are as follows: (Style type + number of searched products)

```
- athleisure		(1,028)
- bohemian_fashion	(1,050)
- casual			(1,035)
- college			(1,076)
- cute				(1,040)
- formal			(1,036)
- gothic			(1,056)
- luxury			(1,287)
- minimalist		(1,081)
- punk				(1,149)
- retro				(1,339)
- romantic			(1,014)
- streetwear		(2,034)
- workwear			(1,029)
- Y2K_Aesthetic		(1,116)
- total:			(17,387)
```

### Load the Dataset (Python)

```
# 读取多个文件夹中的 CSV 文件并提取文本
def load_data(folder_paths, column_names):
    """
    Args:
        folder_paths: 包含多个文件夹路径的列表（例如 ["reviews", "details"]）
        column_names: 对应每个文件夹中要提取的列名（例如 ["评论详情", "详情"]）
    Returns:
        texts: 合并后的文本列表
        labels: 对应的风格标签列表
    """
    texts = []
    labels = []
    

    # 遍历每个文件夹路径及其对应的列名
    for folder_base, column in zip(folder_paths, column_names):
        for folder in os.listdir(folder_base):
            folder_full = os.path.join(folder_base, folder)
            if os.path.isdir(folder_full):
                for file in os.listdir(folder_full):
                    if file.endswith('.csv'):
                        file_path = os.path.join(folder_full, file)
                        try:
                            df = pd.read_csv(file_path)
                            if column in df.columns:

                                # 提取指定列的文本并合并到总数据中
                                for text in df[column].dropna():
                                    texts.append(text)
                                    labels.append(folder)  # 标签为风格名称
                        except Exception as e:
                            print(f"加载文件 {file_path} 错误：{e}")
    return texts, labels

# 定义两个数据集的路径和对应的列名
folder_paths = [
    "C:/Users/Administrator/Desktop/Fashion-Product_Dataset/Fashion-Product_Dataset/reviews",  # 原评论数据
    "C:/Users/Administrator/Desktop/Fashion-Product_Dataset/Fashion-Product_Dataset/details"   # 新增商品详情数据
]
column_names = ["评论详情", "详情"]  # 分别对应两个文件夹中的列名

# 加载数据（合并两个数据集）
texts, labels = load_data(folder_paths, column_names)
```

By Yichen Tan, 2025.05.14