---
title: "Type I error & Type II error 与 false positives & false negatives"
---

第一型错误对应伪阳性，第二型错误对应伪阴性。

# Type I error & Type II error

第一型及第二型错误

> In [statistical hypothesis testing](https://en.wikipedia.org/wiki/Statistical_hypothesis_testing), a **type I error** is the incorrect rejection of a true [null hypothesis](https://en.wikipedia.org/wiki/Null_hypothesis) (a "false positive"), while a **type II error** is incorrectly retaining a false null hypothesis (a "false negative").
> -- https://en.wikipedia.org/wiki/Type_I_and_type_II_errors

# False positives & false negatives

伪阳性与伪阴性

> In [medical statistics](https://en.wikipedia.org/wiki/Medical_statistics), **false positives and false negatives** are concepts analogous to [_type I and type II errors_](https://en.wikipedia.org/wiki/Type_I_and_type_II_errors) in [statistical hypothesis testing](https://en.wikipedia.org/wiki/Statistical_hypothesis_testing), **where a positive result corresponds to rejecting the **[**null hypothesis**](https://en.wikipedia.org/wiki/Null_hypothesis)**, and a negative result corresponds to not rejecting the null hypothesis.**

> A **false positive error**, or in short a **false positive**, commonly called a “**false alarm**”, is a result that indicates a given condition exists, when it does not.

> A **false negative error**, or in short a **false negative**, is a test result that indicates that a condition does not hold, while in fact it does. 

> -- https://en.wikipedia.org/wiki/False_positives_and_false_negatives

注意加粗的半句，positive result 对应 rejecting the null hypothesis，negative result 对应 not rejecting the null hypothesis，也就是说，拒绝零假设相当于医学统计中的阳性。

一个瞎说的例子，比如一个统计假设验证中的零假设是没病，拒绝该零假设就是说有病，对应医学统计里检查结果也是有病，是阳性。

没病看成有病，伪阳性，有病看成没病，伪阴性。

放回统计假设验证，零假设是没病，没病看成有病是拒绝假设，第一型错误，正好对应到伪阳性；有病看成没病是**没有**拒绝假设，第二型错误，正好对应伪阴性。

是阴性还是阳性取决于检测结果，而非真实情况。

# Rate

几个比率的定义，

> * True positive rate (or sensitivity): TPR = TP / (TP + FN)
> * False positive rate: FPR = FP / (FP + TN)
> * True negative rate (or specificity): TNR = TN / (FP + TN)
> -- https://stats.stackexchange.com/a/61838

记法：占名字所对应的真实情况的比例。

比如，伪阳性率，伪阳性的定义是没病看成了有病，真实情况是没病，所以就是没病看成有病的和所有没病的人的比例，而所有没病的人里面，除了看成有病的，剩下的就是看成没病的（确实也没病，真阳性），也就是第二条公式中的 FP / (FP + TN)。

再比如，真阳性，真阳性的定义是有病也确实看成有病，真实情况是有病，所以就是有病看成有病的和所有有病的人的比例，而所有有病的人的里面，除了看成有病的，剩下的就是看成没病的了（伪阴性），也就是第一条公式中的 TP / (TP + FN)。

# False positive paradox

伪阳性悖论

> The **false positive paradox** is a [statistical](https://en.wikipedia.org/wiki/Statistics) result where [false positive](https://en.wikipedia.org/wiki/False_positive) tests are more probable than true positive tests, occurring when the overall population has a low incidence of a condition and the incidence rate is lower than the false positive rate.
> -- https://en.wikipedia.org/wiki/False_positive_paradox

首先，这是一种伪阳性比真阳性更可能发生的统计结果，出现在某个情况发生的可能性很低，低于伪阳性率（没病的人里面，有多少人被看成了有病）。

举个例子，如果对于每种疾病，10000 个人里有 1 个人得病，但是一项检测的伪阳性率是 1%，也就是说，对于这 10000 个人里的 9999 个正常人，其中仍然会有 99.99 个人会并错误的判断为有病，也就是伪阳性，对于这 10000 个人，其中的伪阳性（99.99 人）比真阳性（1 人）更可能发生。

如果一个算法能够识别恐怖分子，有 0.001% 的伪阳性率，可以说得上准确率相当高了，但是实际上恐怖分子在现实中的比例比 0.001% 还要低，比如说是 0.0000001% 吧，那这样的算法如果直接用来识别恐怖分子，就会把很多普通人也错误地判断为恐怖分子，这样的误判会远多于准确识别出的恐怖分子。

<!--
#someday
[靈敏度和特異度](https://zh.wikipedia.org/wiki/%E9%9D%88%E6%95%8F%E5%BA%A6%E5%92%8C%E7%89%B9%E7%95%B0%E5%BA%A6)
[ROC 曲线](https://zh.wikipedia.org/wiki/ROC%E6%9B%B2%E7%BA%BF)
-->
