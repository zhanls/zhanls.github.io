---
title: GitLab分支管理规范
date: 2020-06-29 09:18:26
tags:
---
# 版本号规范

规范的概要如下：

版本格式：主版本号.次版本号.修订号，版本号递增规则如下：

主版本号：当你做了不兼容的功能修改，最小为0

次版本号：当你做了向下兼容的功能性新增，最小为0

修订号：当你做了向下兼容的问题修正，最小为0

版本编译信息可以加到“主版本号.次版本号.修订号”的后面，作为四位版本号（可选）。

# 分支命名规范

通常每个应用的代码将包括 master、develop、release、hotfix、feature分支，release、hotfix 、feature分支的命名规则分别为：release/*，hotfix/*、feature/*。

## 主要分支（保护分支）

**master分支：**

master分支的所有操作都是受保护的，团队中应该指定相关负责人对master分支进行管理。 master分支上存放的应该是随时可供在生产环境中部署的代码（Production Ready state）。当开发活动告一段落，产生了一份新的可供部署的代码时，master分支上的代码会被更新。同时，每一次更新，都有对应的版本号标签（TAG）。

**develop分支：**

develop分支也是受保护的，开发人员不具备权限直接将代码合并到deveolp，团队中应该指定相关人员进行code review，决定是否接受开发人员合并代码的请求。 主库除了master分支外，至少还要有一个活动分支，即develop分支，平时日常的开发都基于活动分支develop开发。

## 辅助分支

**feature分支：**

feature分支用于开发一项新的功能，feature分支从develop分支创建，feature分支最终需要合并回develop分支或者被废弃，合并回develop分支后feature分支可删除。

**release分支：**

release分支用于开发完成后将代码进行发布。当本次迭代的所有功能完成开发，并且合并到develop分支后，从develop分支创建release分支（创建动作可由模块负责人来操作）进行后续的提测和发布工作。release分支上可以做缺陷修复工作，但当前迭代之外的需求不能混到release分支中，通过在release分支上进行缺陷修复工作，可以将develop分支空闲出来进行后续迭代的开发。

本次迭代的测试环境，预发布环境，生产环境部署都是从release分支进行产物构建的。发布到生产环境后，release分支需要合并到develop分支和master分支，并且master分支需要打上这一次迭代上线的版本号（比如1.2.0），合并完成之后release分支可删除。

**hotfix分支：**

hotfix用于线上BUG的紧急修复（需要立即修复的BUG）。hotfix分支从master分支创建，hotfix类似于release分支，可用于测试，预发布，生产环境产物的构建。上线完成之后，hotfix分支需要合并到develop分支和master分支，合并后master分支需要打tag（比如1.2.1），合并完成之后，该分支可删除。

# Gitflow示意图
![Gitflow](http://doc.iot.chinamobile.com/download/attachments/1212629/image2019-10-25_18-1-11.png?version=1&modificationDate=1573459394000&api=v2)