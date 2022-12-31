---
title: sklearnNote
date: 2020-08-27 09:59:49
tags:
categories:
description:
photos:

---

# 整体介绍

模型公共部署，提供python传统机器学习模型（XGBoost,Sklearn模型）部署和计算服务。

具体为：业务方将需要部署的模型文件（XGB方式打包、Sklearn方式打包）交由引擎部署，业务方通过请求报文发起请求服务到引擎，引擎送入模型计算并以报文方式返回计算结果。

交易码：A0942A406

# 接口规范

请求报文格式：（新增申请件编号字段 *todo*）

1. 申请件编号
2. 调用模型文件名称
3. 请求信息json串

返回报文格式：（AplId中的A改为小写 *todo*）

1. 申请件编号
2. 服务响应状态代码（01 success 00 fail)
3. 返回信息json串

```json
//请求json信息
{
    //"traceId": "", //交易流水号 VMD中加入
    "aplId": "", //申请件编号
    "bsnName": "",  //业务名称 模型放置在该目录下
    "modType": "", //对应机器学习模型类型：XGB/LR/SKL_COMMOD
    "modName": "", //模型文件名称
    "packPKLAppro": "", //模型KPL的封装方式 1 SKL_JOBLIB 2 SKL_PICKLE 3 XGB
    "aplParams": {
        "":"",
        "":""
    }				//模型的输入参数
}

//返回的json信息
//统一格式
//正确返回
{
    "pre_results": "[0.06245352]", //模型的返回结果
    "aplId": "test001", //申请件ID
    "sysRespCode": "201", //返回状态码 成功201 失败 500
    "rspInf": "success" //返回状态具体描述
}
//错误返回
{
    "aplId": "null", //申请件ID
    "sysRespCode": "500", //返回状态码
    "rspInf": "Exception('请求信息解析出错')" //返回状态具体描述
}
//LR模型返回（特殊）
{
    "pre_labels": "0", 
    "pre_probas": "0.879682,0.120308,0.000011", 
    "aplId": "test001", 
    "sysRespCode": "201", 
    "rspInf": "success"
}
```

# 流程

* 请求解析为模型输入格式
* 下载模型
* 模型预测
* 编辑返回结果并返回

# 测试

开启flask调试模式

1. XGB模型和LR模型正确请求测试 **done**
2. 动态新增模型是否可动态加载 **done**
3. 请求信息有错误下处理方法：缺失请求信息，请求信息格式，内容错误 **done**
4. 请求信息为空 请求信息不是post请求 **done**

# Questions

1. python LR 逻辑回归 预测变量中有缺失值如何处理? 引擎不处理
2. **vscode 组织python工程的方法之后学习（包含导包、调试），目前先临时测试下自己写的代码，然后放到linux上面去测试，先完成开发**
3. 

# 模型描述唯一标识

添加模型类别 modTarget：回归 or 分类(***不需要***，因为SKL的预测函数都是model.predict())

模型类型modType：LR、XGB、SKL_COMMOD（SKL提供的模型方法）

模型打包方式modPKLAppro：  SKL_PICKLE、SKL_JOBLIB、 XGB

# 注意事项

json.dump():转化dict->str

json.dumps():转化dict->str并存入文件

# LR模型

```python
#json请求 转化为 array-like
"""
{
"aplParams":"N1,N2,N3,...,Nn"
}
array([[N1,N2,N3,...,Nn]])

"""
```



```python
#封装为PKL文件
1. pickle
>>> from sklearn import svm
>>> from sklearn import datasets
>>> clf = svm.SVC()
>>> iris = datasets.load_iris()
>>> X, y = iris.data, iris.target
>>> clf.fit(X, y)  
SVC(C=1.0, cache_size=200, class_weight=None, coef0=0.0,
    decision_function_shape=None, degree=3, gamma='auto', kernel='rbf',
    max_iter=-1, probability=False, random_state=None, shrinking=True,
    tol=0.001, verbose=False)

>>> import pickle
>>> s = pickle.dumps(clf)
>>> clf2 = pickle.loads(s)

>>> clf2.predict(X[0:1])
array([0])
>>> y[0]

2. joblib
>>> from sklearn.externals import joblib
>>> joblib.dump(clf, 'filename.pkl') 
>>> clf = joblib.load('filename.pkl')


#进行预测以及输出结果
>>> from sklearn.datasets import load_iris
>>> from sklearn.linear_model import LogisticRegression
>>> X, y = load_iris(return_X_y=True)
>>> clf = LogisticRegression(random_state=0).fit(X, y)
>>> clf.predict(X[:2, :])
array([0, 0])
>>> clf.predict_proba(X[:2, :])
array([[9.8...e-01, 1.8...e-02, 1.4...e-08],
       [9.7...e-01, 2.8...e-02, ...e-08]])
>>> clf.score(X, y)
0.97...
```

#  XGB模型

https://cloud.tencent.com/developer/article/1387686

```python
# 请求解析为模型输入格式
# X_test类型可以是二维List，也可以是numpy的数组 array([[]])
dtest = DMatrix(X_test)
ans = model.predict(dtest)
# 下载模型\模型预测
	# 使用XGB.save_model()封装的PKL文件
# save model
bst.save_model('xgb.model')
# load model and data in
bst2 = xgb.Booster(model_file='xgb.model')
dtest2 = xgb.DMatrix('dtest.buffer')
preds2 = bst2.predict(dtest2)
	# 使用SKL封装的PKL文件
# alternatively, you can pickle the booster
pks = pickle.dumps(bst2)
# load model and data in
bst3 = pickle.loads(pks)
preds3 = bst3.predict(dtest2)
# 编辑返回结果并返回
pre_output["results"] = str(preds3)
```



