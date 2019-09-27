

实现功能：
1. score自定义
2. cv方法自定义
3. 输出最优超参数
4. 显示较为美观
5. 模型中多个超参数一样好的，并列输出

## 回归
```python
import numpy as np
import pandas as pd

from sklearn import linear_model, tree, naive_bayes, neighbors, svm, neural_network
from sklearn import ensemble

from sklearn import model_selection
from sklearn import datasets
from sklearn import metrics

X, y = datasets. \
    make_classification(n_samples=1000,
                        n_features=10,
                        n_informative=2,
                        n_redundant=3,  # 用 n_informative 线性组合出这么多个特征
                        n_repeated=3,  # 用 n_informative+n_redundant 线性组合出这么多个特征
                        n_classes=2,
                        flip_y=0.1
                        )

# 定义 CV 方法
cv = model_selection.KFold(n_splits=5, shuffle=True, random_state=0)
# 定义最优的标准
scoring = 'accuracy'

all_models = [
    ['LogisticRegression', linear_model.LogisticRegression(), dict()],
    ['DecisionTreeClassifier', tree.DecisionTreeClassifier(), dict()],
    ['GaussianNB', naive_bayes.GaussianNB(), dict()],
    # ['MultinomialNB', naive_bayes.MultinomialNB(),dict(],
    ['BernoulliNB', naive_bayes.BernoulliNB(), dict()],
    ['KNeighborsClassifier', neighbors.KNeighborsClassifier(), dict()],
    ['LinearSVC', svm.LinearSVC(), dict()],
    ['SVC', svm.SVC(), dict()],
    ['MLPClassifier', neural_network.MLPClassifier(), dict()],
]

for models in all_models:
    model_name = models[0]
    model = models[1]
    param_grid = models[2]

    gscv = model_selection.GridSearchCV(estimator=model,
                                        scoring=scoring,  # 打分
                                        param_grid=param_grid,
                                        cv=cv,  # cv 方法
                                        return_train_score=True,  # 默认不返回 train 的score
                                        refit=False,  # 默认为 True, 用最好的模型+全量数据再次训练，用 gscv.best_estimator_ 获取最好模型
                                        n_jobs=-1)

    gscv.fit(X, y)
    cv_results_ = gscv.cv_results_
    idx_best_test_score = (cv_results_['rank_test_score'] == 1)
    models.extend([gscv.best_score_,
                   cv_results_['mean_test_score'][idx_best_test_score],
                   cv_results_['std_test_score'][idx_best_test_score],
                   cv_results_['mean_train_score'][idx_best_test_score],
                   cv_results_['std_train_score'][idx_best_test_score],
                   np.array(cv_results_['params'])[idx_best_test_score],
                   cv_results_])
    print(models[0], gscv.best_score_)


#%% 输出
df = pd.DataFrame(all_models, columns=['model_name', 'model', 'param_grid', 'best_score_',
                                       'mean_test_score', 'std_test_score', 'mean_train_score', 'std_train_score',
                                       'best_params', 'cv_results_'])
df.sort_values('best_score_', ascending=False, inplace=True)
df_describe = df.loc[:, ['model_name', 'best_score_',
                         'mean_test_score', 'std_test_score', 'mean_train_score', 'std_train_score',
                         'best_params']]
df_describe

```

用最好的模型重新全量训练
```python
best_model = df.iloc[0,:].model
best_model.fit(X, y)
```

## 回归
