# 大数据分析大作业

此仓库用于存储研究生期间大数据分析课程的课程大作业，分别使用两种方法实现命名实体识别，并进行对比。

**一.利用LSI算法，求词项与文档各自的3维表示**

**解：**将词项文本矩阵进行奇异值分解进行潜在语义分析：

<img src=".\图片资源\图1.png" alt="img" style="zoom:80%;" />

得到话题空间<img src=".\图片资源\图2.png" alt="img" style="zoom:80%;" />，以及文本在话题空间的表示<img src=".\图片资源\图3.png" alt="img" style="zoom:80%;" />

<img src=".\图片资源\图4.jpg" alt="img" style="zoom:80%;" /> 

代码：

```python
import numpy as np

from numpy import linalg as la

from numpy import mat

if __name__=="__main__":

  X = mat([[1, 0, 1, 0, 0, 0], [0, 1, 0, 0, 0, 0], [1, 1, 0, 0, 0, 0], [1, 0, 0, 1, 1, 0], [0, 0, 0, 1, 0, 1]])

  U, S, T = la.svd(X) #SVD降维

  print(U[:,0:3].round(1))

  print(T[0:3,:].round(1))
```

 

二. **分别使用LSTM+CRF和BERT+LSTM+CRF方法进行命名实体识别，数据集为conll-2003，最后将两者结果进行对比。**

**总体实验思路：**将此命名实体识别(Named Entity Recognition,NER)问题看作一个序列词性标注(Part-Of-Speech tagging, POS tagging)问题，可以用词性标注的方法来做；

 

\1. **方法一：**词性标注问题可以用长短期记忆神经网络(LSTM)和条件随机场(CRF)来实现，通过LSTM训练出标注序列，使用CRF对模型增加约束,使用向前向后算法计算出损失值Loss，最后使用维特比(Viterbi)算法解码出标记序列,以达到命名实体识别的目的。

 

\2. **方法一在数据集conll-2003的实验效果：**

<img src=".\图片资源\图5.jpg" alt="img" style="zoom: 80%;" /> 

\3. **方法二：**引入BERT对语料进行预训练，之后使用方法一中相同的方法进行命名实体识别，实现算法优化。

\4. **方法二在数据集conll-2003的实验效果：**

<img src=".\图片资源\图6.jpg" alt="img" style="zoom:80%;" /> 

\5. **实验结果分析：**LSTM+CRF的方法进行命名实体识别虽然在训练集上准确率较高，但在测试集上表现并不是很好，在加入BERT预训练优化后，相比LSTM+CRF，BERT+LSTM+CRF的训练集准确率到达了98%，比前者提高了4%，总体召回率提升了20%，无论训练集还是测试集准确率、召回率和F1值都得到了很大的提升。

\6. **结论：**BERT果然名不虚传！

\7. **实验代码：**

1. BERT_LSTM_CRF_CONLL2003
2. LSTM_CRF_CONLL2003
