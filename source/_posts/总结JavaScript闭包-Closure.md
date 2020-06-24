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
