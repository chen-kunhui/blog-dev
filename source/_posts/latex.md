---
title: LaTex  笔记
tags:
  - markdown
date: 2018-10-04 16:00:01
categories:
---


> https://katex.org/docs/supported.html

> http://www.mohu.org/info/symbols/symbols.htm

# 基础语发
## 行内式
  - `\(...\)`。如`\( y=x^2 \)` 渲染的结果为 \( y = x^2 \)
  - `$...$`。如`$ y=x^2 $` 渲染的结果为 $ y=x^2 $

## 块模式

```
$$
y=x^2
$$
```
渲染结果如下
$$
y=x^2
$$

---

```
\[
y=x^2
\]
```
渲染结果如下
\[
y=x^2
\]

---

\```math
x^2
\```
渲染结果如下
```math
y=x^2
```

# 数学符号表示

| 语法 | 结果 |
| - | - |
| `$ x^2 $` | $ x^2 $ |
| `$ x^{a+1} $` | $ x^{a+1} $ |
| `$ x_{1} $` | $ x_{1} $ |
| `$ \sqrt9 $` | $ \sqrt9 $ |
|`$ \sqrt[3]9 $` | $ \sqrt[3]9 $ |
| `$ \overline{m} $` | $ \overline{m} $ |
| `$ \underline{m} $` | $ \underline{m} $ |
| `$ \cdots $` | $ \cdots $ |
| `$ \underbrace{a+b+\cdots+n}_{25} $` | $ \underbrace{a+b+\cdots+n}_{25} $ |
| `$ \vec a $` | $ \vec a $ |
| `$ \overrightarrow{AB} $` | $ \overrightarrow{AB} $ |
| `$ \frac{1}{2}$` | $ \frac{1}{2}$ |
| `$ \sum_{i=1}^{n} $` | $ \sum_{i=1}^{n} $ |
|`$\displaystyle\sum_{i=1}^n$`|$\displaystyle\sum_{i=1}^n$|
| `$ \int_{0}^{n} $` | $ \int_{0}^{n} $ |
| `$\prod_{\epsilon}$` | $\prod_{\epsilon}$ |

## 小写希腊字母

| 语法 | 结果 |
| - | - |
| `$\alpha$` | $\alpha$ |
| `$\beta$` | $\beta$ |
| `$\theta$`|$\theta$ |
| `$\delta$`|$\delta$ |
| `$\lambda$`|$\lambda$|
|`$\pi$`| $\pi$ |

## 二元关系符

|语法|结果|语法| 结果 |语法|结果|
|-|-|-|-|-|-|
|`$<$`|$<$|`$\subset$`|$\subset$|`$\vdash$`|$\vdash$|
|`>`|$>$|`$\supset$`|$\supset$|`$\dashv$`|$\dashv$|
|`=`|$=$|`$\approx$`|$\approx$|`$\models$`|$\models$|
|`$\leq or \le$`|$\leq or \le$|`$\subseteq$`|$\subseteq$|`$\mid$`|$\mid$|
|`$\geq or \ge$`|$\geq or \ge$|`$\supseteq$`|$\supseteq$|`$\parallel$`|$\parallel$|
|`$\doteq$`|$\doteq$|`$\cong$`|$\cong$|`$\perp$`|$\perp$|
|`$\ll$`|$\ll$|`$\sqsubset$`|$\sqsubset$|`$\smile$`|$\smile$|
|`$\gg$`|$\gg$|`$\sqsupset$`|$\sqsupset$|`$\frown$`|$\frown$|
|`$\equiv$`|$\equiv$|`$\Join$`|$\Join$|`$\asymp$`|$\asymp$|
|`$\prec$`|$\prec$|`$\sqsubseteq$`|$\sqsubseteq$|`$:$`|$:$|
|`$\succ$`|$\succ$|`$\sqsupseteq$`|$\sqsupseteq$|`$\notin$`|$\notin$|
|`$\sim$`|$\sim$|`$\bowtie$`|$\bowtie$|`$\neq or \ne$`|$\neq or \ne$|
|`$\preceq$`|$\preceq$|`$\in$`|$\in$|
|`$\succeq$`|$\succeq$|`$\ni or \owns$`|$\ni or \owns$|
|`$\simeq$`|$\simeq$|`$\propto$`|$\propto$|

