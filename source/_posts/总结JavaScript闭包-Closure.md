---
title: 总结JavaScript闭包(Closure)
date: 2020-06-24 14:21:13
tags:
---
闭包（closure）是Javascript语言的一个难点，也是它的特色，很多高级应用都要依靠闭包实现。

根据MDN对闭包的讲解，[什么是闭包](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures)：
> 函数和对其周围状态（lexical environment，词法环境）的引用捆绑在一起构成闭包（closure）。也就是说，闭包可以让你从内部函数访问外部函数作用域。在 JavaScript 中，每当函数被创建，就会在函数生成时生成闭包。

第一次看到这里的时候，我还挺惊讶的，没想到每个函数的创建，实际上都有一个闭包也在生成，这也决定了，如果我被问到这方面的问题，绝对说不点子上、

然后把，我又想了一下，已经2020年了，针对这个问题网上资源众多，还是放弃了自己再重新写一篇博客，组织文章结构，组织语言，实在太麻烦了，放出几个我学习闭包的链接吧：

1. [什么是闭包？ - 知乎](https://www.zhihu.com/question/34210214?sort=created)
2. [阮一峰老师教学](https://javascript.ruanyifeng.com/grammar/function.html#toc22)
3. [廖雪峰老师教学](https://www.liaoxuefeng.com/wiki/1022910821149312/1023021250770016)

补充:
在**全栈工程师修炼指南**中，四火老师在**JavaScript面向对象**一文中也提到了闭包（Closure）的概念。他指出：
> 闭包简单说，就是引用了自由变量的函数。这里的关键是“自由变量”，其实这个自由变量，扮演的作用是为这个函数调用提供了一个“上下文”，而上下文的不同，将对入参相同的函数调用造成不同的影响，它包括：
+ 函数的行为不同，即函数调用改变其上下文中的其它变量，如例子中的 setName()
+ 函数的返回值不同，如例子中的 getName()

再补充：
闭包翻译自英文单词closure，这是个有些不好理解的词. 根据wiki百科：
> 在计算机科学中，**闭包**（英语：Closure），又称**词法闭包**（Lexical Closure）或**函数闭包**（function closures），是在支持头等函数的编程语言中实现词法绑定的一种技术。
闭包并不只是JavaScript独有的一个概念，实际上，这个概念最早应该是由Peter Landin提出，他将其定义为一种包含环境成分和控制成分的实体，用于在他的SECD机器上对表达式求值。在编程语言Scheme上，就有对闭包最早的实现

概念误区：

有些人会把JavaScript执行上下文，或者作用域(Scope, ES3规定的执行上下文的一部分)这个概念当作闭包。

实际上JavaScript中跟闭包对应的概念就是“函数”，可能是这个概念太过于普通，跟闭包看起来又没什么联系，所以大家才不自觉地把这个概念对应到了看起来更特别的“作用域”吧。

执行上下文：

相比普通函数，JavaScript函数的主要复杂性来自于它携带的“环境部分”。当然，发展到今天的JavaScript，它所定义的环境部分，已经比当初经典的定义复杂了很多。

闭包与JavaScript函数：

![闭包与JavaScript函数](https://static001.geekbang.org/resource/image/68/52/68f50c00d475a7d6d8c7eef6a91b2152.png)