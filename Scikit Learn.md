
### 文档就是最好的笔记，多读文档   

学习 sklearn 的最重要的方法就是查文档，看需要什么参数，参数含义  

FN：False Negative，预测错了，y_true 是 Positive，预测成了 Negative。   
FP：False Positive，预测错了，y_true 是 Negative，预测成了 Positive。   

结果的含义，读文档。    

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

support 的含义：support 是 true_label 里，这个 pred_label 的数据数量    

The support is the number of occurrences of each class in y_true.    

参数含义读文档：[precision_recall_fscore_support](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_recall_fscore_support.html#sklearn.metrics.precision_recall_fscore_support)


打印 precision、recall 和 f1 值。   

```python 
classification_report(y_true, y_pred)
```

classification_report 保存成 Excel   

```python 
import numpy as np
import pandas as pd
from sklearn.metrics import classification_report


def read_result_excel():
    df = pd.read_excel("./data/san_ji_label/result/concat_result.xlsx")
    return df


def get_label_list():
    y_true_list = []
    y_pred_list = []
    df = read_result_excel()
    for _, row in df.iterrows():
        y_true_list.append(row["问题分类3"])
        y_pred_list.append(row["日报问题分类3"])
    return np.array(y_true_list), np.array(y_pred_list)


def generate_report():
    y_true, y_pred = get_label_list()
    report = classification_report(y_true, y_pred, digits=20, output_dict=True)
    print(report)
    return report


def get_df_list(report):
    del report["accuracy"]
    df_list = []
    for key, value in report.items():
        d = {"label": key}
        d.update(value)
        df_list.append(d)
    return df_list


def write_to_excel():
    report = generate_report()
    df_list = get_df_list(report)
    df = pd.DataFrame(df_list)
    df.to_excel("./data/san_ji_label/result/classification_report.xlsx", index=False)
    # 手动复制 accuracy 结果


write_to_excel()
```  

