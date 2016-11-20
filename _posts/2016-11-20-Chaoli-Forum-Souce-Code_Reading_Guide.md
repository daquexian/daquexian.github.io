---
layout: post

title: Chaoli Forum Source Code Reading Guide

date: 2016-11-15

categories: blog

tags: [Android, MVVM, Data Binding]

description: 

---

## **源码食用指南**

### 架构
使用了Data Binding的MVVM架构

### 包组织方式
项目中有8个包，分别是view, viewmodel, model, data, binding, network, meta, utils

* view

即MVVM中的View层，在MVVM中，View层应该负责和UI相关，和业务逻辑无关的操作，要注意的是和UI相关并不仅仅指改变某个控件的颜色这种简单的操作——实际上这些操作也基本被Data Binding接管了——切换到其它Activity、弹出Toast或者Dialog，也应该交给View来做，只是切换到哪个Activity，弹出的Toast内容是什么，就要让Viewmodel来指定。关于View和ViewModel的具体分工，我也疑惑了很久，直到找到[这篇文章](http://tech.vg.no/2015/07/17/android-databinding-goodbye-presenter-hello-viewmodel/)，我是先看到了它的中文版，疑惑于其中的ViewModel不应该持有任何Context的引用的说法，然后在谷歌上搜索，找到了它的首发地址，期待着会看到一些和中文版不同的更新，果然我在评论区里发现了作者对View和ViewModel职责的划分，这个app也基本是按照它区分View和ViewModel。

* viewmodel

即MVVM中的ViewModel层，处理和业务逻辑有关的功能，值得注意的是，在这个项目中，ViewModel既没有持有任何Context，也没有以任何形式持有View对象的引用，而是比较纯粹的数据驱动。

* model

即MVVM中的Model层，主要是一些POJO对象，用于JSON解析，或用于绑定在xml中。对一些情况来说，完全按照网页接口返回的JSON解析出的对象并不适合直接绑定于xml并显示在界面中，可能需要根据app当前的情况添加其它的数据，或对已有的数据进一步处理，因此在model包中一部分类是以Business开头的，表示这些类是由其他model进一步处理得到的，用于与界面中的元素绑定。

* data

一些有效范围在整个Application的数据，存放在了这个类中，比如自己的账户信息。

* binding

包含用于Data Binding框架的类，主要是各种BindingAdapter，其中有一个为了给RecyclerView绑定数据而实现的BindingAdapter，GitHub上有不少类似的第三方库，比如[binding-collection-adapter](https://github.com/evant/binding-collection-adapter)，但是不支持Support包中的DiffUtil，因此我自己重新造了一个轮子，代码在binding包下的RecyclerViewBA.java中，支持对不同的数据渲染不同的layout，对实现了DiffItem接口的类，在list更新时会应用DiffUtil实现插入、删除、移动的动画效果，有兴趣的同学可以参考一下，提一些建议:)。

* network

和网络有关的类，如对Retrofit、OkHttp的封装、Retrofit的service等，Android 5.0以下的系统默认不支持TLS协议，导致网络访问失败，修补的代码也在这个包里。

* meta

一些零零散散的内容，比如各种custom view，解析LaTeX和表情的代码。

* utils

各种工具类，主要是各种网络请求，比如发帖、回帖、登录、注销这些。

### 对MVVM架构的理解
在所有MV*架构中，View都是一个只负责UI的角色，虽然有一些文章提议MVVM中的Model层应该包含业务逻辑，但我的这个项目仍然将model作为存放数据的“哑类”。一篇介绍MVVM架构的文章，大部分篇幅应该都用于阐述View和ViewModel的关系，对它们之间的关系，网上也有各种不同的说法，总结出来，有这么几种：

1. 让ViewModel直接持有View的引用，需要时调用View暴露的各种方法

2. 让ViewModel持有View的Context的引用，来处理startActivity之类的需要Context的方法。

3. View实现特定的接口，将View作为接口的对象传递给ViewModel

4. 使用事件总线来在ViewModel和View之间传递消息。

第一种方法不符合MVVM中ViewModel应该对View一无所知的要求，也不符合松耦合的原则。第二种方法实用性不高——不是所有需要View层完成的事情都能用Context解决，比如更改menu。第三种方法不能适应复杂的需求——如果总共有三个需要暴露给ViewModel的方法，每个View可能只实现其中的某几个，那可能最多要实现七个不同的接口，或者在ViewModel里持有多个简单接口对象的引用——其实它们都指向同一个View。第四种方法是不错的，不过事件总线用起来有些麻烦。

我自己在项目中使用的办法是一种类似于事件总线的办法，利用Data Binding中的Observable...类，在View中使用它们的addOnPropertyChangedCallback方法为这些ViewModel中的变量添加监听器，在这些变量改变时，就可以执行监听器中的回调函数。这样，就把ViewModel和View解耦了，和事件总线一样都是使用了观察者模式。这样,ViewModel可以实现比较纯粹的数据驱动，对View保持一无所知，并且利于之后的维护。
