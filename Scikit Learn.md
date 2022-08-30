
### 文档就是最好的笔记，多读文档   

学习 sklearn 的最重要的方法就是查文档，看需要什么参数，参数含义  

FN：False Negative，预测错了，y_true 是 Positive，预测成了 Negative。   
FP：False Positive，预测错了，y_true 是 Negative，预测成了 Positive。


### [multilabel_confusion_matrix](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.multilabel_confusion_matrix.html#sklearn.metrics.multilabel_confusion_matrix)

多类别混淆矩阵   

第一个例子：   

```python 
import numpy as np
import pandas as pd
from sklearn.metrics import multilabel_confusion_matrix, classification_report


def read_result_excel():
    df = pd.read_excel("./data/san_ji_label/result/result_红旗.xlsx")
    return df


def get_label_list():
    y_true_list = []
    y_pred_list = []
    df = read_result_excel()
    for _, row in df.iterrows():
        y_true_list.append(row["问题分类3"])
        y_pred_list.append(row["日报问题分类3"])
    return np.array(y_true_list), np.array(y_pred_list)


y_true, y_pred = get_label_list()

mcm = multilabel_confusion_matrix(y_true, y_pred)
print(mcm)
print(mcm.sum(axis=0))

report = classification_report(y_true, y_pred)
print(report)
```  

第二个例子：   

```python 
import pandas as pd
import numpy as np
from sklearn.metrics import multilabel_confusion_matrix, classification_report


origin_df = pd.read_excel('green_data.xlsx')
result_df = pd.read_excel('green_data_or_result_1.xlsx')
origin_df = origin_df.drop(origin_df.columns[:10], axis=1)
result_df = result_df.drop(result_df.columns[:10], axis=1)


def get_label_list(df):
    y_list = []
    for row in df.iterrows():
        row = row[1]
        one_row = []
        for col in df.columns:
            one_row.append(row[col])
        y_list.append(one_row)
    return np.array(y_list)


y_true = get_label_list(origin_df)
y_pred = get_label_list(result_df)


mcm = multilabel_confusion_matrix(y_true, y_pred)
mcm.sum(axis=0)

report = classification_report(y_true, y_pred)
```


### [classification_report](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.classification_report.html#sklearn-metrics-classification-report)

打印 precision、recall 和 f1 值。   

```python 
classification_report(y_true, y_pred)
```


