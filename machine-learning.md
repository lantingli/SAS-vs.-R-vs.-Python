机器学习算法分为监督式学习，半监督式学习，非监督式学习以及增强式学习。

监督式学习：  
工作原理：此类算法有一个目标/输出变量（依赖变量），该值是通过一组预测因子（独立变量）推测出来的。通过这些预测因子，我们生成方程式得出输入与期望输出的对应关系。整个训练过程会持续到模型能通过训练数据达到一定的精确度。这类监督式学习算法有：回归算法，决策树，随机森林，邻近算法（KNN），逻辑回归算法 SVM， 人工神经网络， 朴素贝叶斯。

非监督式学习：  
工作原理：此类算法，没有目标或预期输出变量。而是用于聚集不同的组别，其被广泛的应用于根据不同设定对客户人群进行分类。这类非监督式算法有：关联规则（Apriori algorithm），K均值聚类算法。

增强式学习：  
运用这类算法，机器被训练成为能都做出特定的决策。机器被暴露于一个环境，它在其中通过不断的尝试与错误训练自己。这种机器学习通过过去的经验来试图捕捉最优解来做出最准确的商业决策。这类增强式算法有：马可夫决策过程。

模型前数据处理：

1. 查看数据结构：  
   str\(traindata\)

2. 查看异常值以及异常值处理  
   a. 查看  
   summary\(traindata\)  
   boxplot\(traindata$v6\)  
   which.max\(traindata$v6\)

b. 拿掉异常值  
ch &lt;- lm\(v6 ~., traindata\[-116,\]

c. 再通过预测补上  
w\[116,6\] = predict\(ch, traindata\(116, -6\]\)

1. 缺失值处理
   a. 查看缺失值模式
   library\(mice\)
   library\(VIM\)
   md.pattern\(traindata, prop = F, numbers = T\)
   matrixplot\(traindata\)
   b. 填补缺失值
   随机森林插补法
   library\(missForest\)
   mis.model &lt;- missForest\(traindata\)
   c. 中位数替补法
   library\(caret\)
   imputation\_m &lt;- preProcess\(traindata, method = "medianImpute"\)
   traindata &lt;- predict\(imputation\_m, traindata\)
   str\(traindata\)

### 1. 线性回归

多元线性回归之前先看下相关性，如果相关性差，改用其它机器学习方法

R：  
    cor\(states\)  
    library\(car\)  
       scatterplotMartix \(states, spread = FALSE, main = "Scatter Plot Matrix"\)  
       scatterplotMatrix\(\)     cor\(states\)

常规方法：  
    fit &lt;- lm\(weight ~height, data = woman\)  
    par\(mfow= c\(2,2\)\)  
    plot\(fit\)

共生成四幅图  
1） 残差vs. 拟合值   
2） Normal QQ \(残差的正态图，验证残差是否正态分布\)  
3） scale-location 4\) residuals vs. leverage

图形解读：  
线性： 若因变量与自变量线性相关，那么残差值与预测值就没有任何关联。换句话说，除了白噪声，模型应该包含数据中所有的系统方差，  
正态性: 当预测变量值固定时，因变量成正态分布，则残差值也应该是一个均值为0的正态图，正态QQ 图是在正态分布对应的值下，标准化残差的概率图。若满足正态假设，那么图上的点应该落在呈45度的直线上，若不是如此，那么就威凡了正态性的假设

同方差性：若满足不变方差假设，那么在位置尺度图中，水平线周围的点应该随机分布  
残差与杠杆图： 提供了可能关注的单个观测值的信息，从图中可以鉴别出离群值，高杠杆点和强影响点。

### 2. 逻辑回归

### 类型

逻辑回归属于概率型非线性回归，分为二分类和多分类的回归模型。利用Logistic 函数将因变量的取值范围控制在0 和1 之间，表示取值为1的概率。   
Logsitic 回归模型中的因变量只有1-0（是和否，发生和不发生）两种取值。假设在p个独立变量x1, x2, ...xn作用下，记Y取1的概率是p = P（y =1\|x\),取0的概率是1-p, 取1和0的概率之比为p/1-p,称为事件的优势比（odds），对odds 取对数自然系的Logistic变换Logit\(p\) = ln\(p/1-p\)

### 3. Decision Trees

决策树是一种树状分类结构模型。它是一种通过对变量值拆分建立分类规则，又利用树形图分割形成概念路径的数据分析技术。

决策树的基本思想由两个关键步骤组成：  
第一步对特征空间按变量对分类效果影响大小进行变量和变量值选择；  
第二步用选出的变量和变量值对数据区域进行矩阵划分，在不同的划分区间进行效果和模型复杂性比较，从而确定最合适的划分，分类结果由最终划分区域的优势类确定。

决策树主要用于分类，也可以用于回归，与分类的主要差异在于选择变量的标准不是分类的效果，而是预测误差。

当决策树的输出变量（因变量）是分类变量时，叫分类树，而当决策树的输出变量为连续变量时，但叶节点数据是有穷的，因此输出的值也是在这个叶节点上的观测值的平均。回归树不用嘉定经典回归中的诸如独立性，正态性，线性或者光滑性等，无论自变量是数量变量还是定性变量都同样适用。

决策树的优点和缺点  
和经典回归不同，决策树不需要对总体进行分布的假设。而且，决策树对于预测很容易解释，这是其优点。此外，决策树很容易计算，但有必要设定不使其过分生长的停止规则或者修剪方法  
决策树的一个缺点是每次分叉只和前一次分叉有关，而且并不考虑对以后的影响。因此，每个节点都依赖于前面的节点，如果一开始的划分不同，结果也可能很不一样。

决策树是以后要介绍的更加强大的各种组合算法的基础之一，因此理解决策树是很有必要的 。

树的构建

我们从理论上概述决策树的构建过程，这一过程包括如下四个步骤  
1）决策树的生成  
2）生成树的剪枝\(防止过拟合）  
预剪枝和后剪枝  
预剪枝：在构建决策树之前先设置好生长停止的一系列准则  
后剪枝：树生长好之后剪去不具有代表性的枝和叶

3）生成规则  
4）模型性能和预测

常用算法  
ID3 算法是最有影响力的决策树之一，ID3算法的某些弱点被改善之后得到了C4.5算法， C5.0则进一步改进了C4.5算法， 使其综合性能大幅度提高。

CART（分类回归树），顾名思义，是既可以建立分类树，也可以建立回归树的算法， CART算法只支持二分类

条件推理决策树算法是以自变量为目标变量的相关性为分裂度量指标

无树叶和枝时：  
oldentroy &lt;- -\(9/14 _ log2\(9/14\)\)-\(5/14_log2\(5/14\)\)

计算 信息熵  
myentroy &lt;- function\(x,y\){  
x&lt;- w$outlook; y&lt;- w$play  
t&lt;- table\(x,y\)  
m&lt;- matrix\(t, 3,2, dimnames = list\(rownames\(t\), colnames\(t\)\)\)  
freq &lt;- -rowSums\(\(m/rowSums\(m\)\)_ log2\(m/rpwSi,s\(m\)\)\)  
entroy &lt;- sum\(rowSums\(m\)_ freq/dim\(w\)\[1\], na.rm = T\)  
return\(entroy\)  
}

计算outlook  
myentroy1 &lt;- myentroy\(x= w\[, 1\], y = w\[, 5\]\)  
计算outlook 信息增益  
d1&lt;- oldentroy - myentroy1

C5.0 决策树算法  
函数包C50

例子

xaa&lt;- read.table\("xaa.dat"\)  
xab&lt;- read.table\("xab.dat“）

libraray\(C50\)  
mod &lt;- C5.0\(xaa$V19~., data = xaa\)

mod  
summary\(mod\)

pred &lt;- predict\(mod, xab, type = "class"\)  
a= table\(xab$V19, pred\)  
b = paste0\(round\(\(sum\(a\) - sum\(diag\(a\)\)\)/sum\(a\), 4\) \* 100, "%"\)\)

CART算法  
rpart函数  
在rpart包中针对CART决策树算法提供的函数主要用于建模型的rpart函数以及用于剪枝的prune函数

library\(rpart\)  
b = rpart\(xaa$v19~., data = xaa\)\)  
plot\(b\); text\(b\)  
libraray\(rattle\)  
fancyRpartplot\(b\)

### 2. Ensebles of decision trees

### 3. support vector machines

### 4. neural networks

### 5. naive bayes

### 6. Cluster analysis

### 7. Association analysis

### 8. multivariate adaptive regression splines

### 9. evaluating models



