

## 关键思路

#### 1. 数据采集和清洗

​	利用提供的dmzo_nomal.csv和xssed.csv文件，观察正常payload和恶意payload之间的词法特征区别。其中xssed.csv的payloads存在<br/换行标签，但xss会使用换行标签构造payload，并非由于爬虫造成的，不需要将其去除。

​	在未合并两种数据之前，先增加是否xss的标签并赋值1/0，再使用panda的concat进行外连接。

#### 2. 特征选取

​	观察两种数据后，发现能够利用的特征基本为字符特征，选取了payload长度、%个数、是否包含大写字母、是否含有xss常用标签、是否含有事件触发、是否含有敏感单词（伪协议、字符编码等）、大小写字母转换频率、标签是否闭合、敏感字符个数等9个字符特征。

​	由于dmzo_nomal.csv文件中的数据都为小写，但是实际上正常的payload可以有大写字母出现，恶意payload会利用字母大写来避免过滤，所以大小写字母转换频率本可以作为一个较好的特征，但是由于数据的问题，在这两个特征上正常和恶意的payload区别明显，与实际差别大，虽然精确率高但实际效果欠缺。

#### 3. 数据分割

​	设置样本占比，并分割样本特征集和样本结果。

#### 4. 特征评分

​	使用sklearn.feature_selection.SelectKBest 给特征打分

#### 5. 模型搭建

​	使用SVM（支持向量机）模型和随机森林模型，构建SVM分类器并进行模型训练。

#### 6. 模型评估

​	可使用classification_report进行评估，也分别进行模型精确率和召回率的评估。

#### 7. 模型运用

​	输入测试payload，进行特征提取并转化为指定格式，进行是否恶意预测。