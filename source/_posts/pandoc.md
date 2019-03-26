---
title: md转html
categories: 
- 工具
tags:
- pandoc 
comments: true
---

### md 转html
```
pandoc -s -S --toc -c github2.css a.md  -o a.html   
```
### math.tex 转html
 math.tex文件
    ```
    % Pandoc math demos
    $a^2 + b^2 = c^2$
    $v(t) = v_0 + \frac{1}{2}at^2$
    $\gamma = \frac{1}{\sqrt{1 - v^2/c^2}}$
    $\exists x \forall y (Rxy \equiv Ryx)$
    $p \wedge q \models p$
    $\Box\diamond p\equiv\diamond p$
    $\int_{0}^{1} x dx = \left[ \frac{1}{2}x^2 \right]_{0}^{1} = \frac{1}{2}$
    $e^x = \sum_{n=0}^\infty \frac{x^n}{n!} = \lim_{n\rightarrow\infty} (1+x/n)^n$
    ```
    ```
    pandoc math.tex -s --mathml  -o mathMathML.html
    ```
    
### 命令行参数
- -s –standalone转换输出文档时会自动加上合适的header和footer(例如standalone HTML, LaTeX, RTF)
- -S 
- --toc 
- –data-dir=DIRECTORY指定用户数据目录，设定之后会在该目录下搜索pandoc数据文件。如果没有指定该选项，则会使用默认的用户数据目录:$HOME/.pandoc可通过pandoc --version命令查看
- -c URL, –css=URL链接到CSS样式表。该选项能够使用多次来引入多个文件，所指定的文件能够以指定的顺序依次引入
- -mathml 参数强制 Pandoc 将 LaTeX 中的数学公式转换成 MathML

