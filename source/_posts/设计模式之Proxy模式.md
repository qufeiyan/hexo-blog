---
title: 设计模式之Proxy模式
tags: 设计模式
categories: 编程思想
top: 1
abbrlink: ae91a8e6
date: 2018-08-29 16:19:39
---

# Proxy 模式
## 意图
> 为其他对象提供一种代理以控制对这个对象的访问。
## 动机 
> 对一个对象进行访问控制的一个原因是为了只有在我们确实需要这个对象时才对它进行创建和初始化。
## 应用场景
> 代理模式主要用于当我们需要一个简单对象来表示一个复杂对象的情形。如果创建对象的开销很大，那么可以==推迟其创建==，并使用一个简单对象来代理其功能直到必须立即创建的时候。这个简单对象就可以称为复杂对象的代理。

<!-- more -->

----
UML类图如下：
![proxy](http://on-img.com/chart_image/5b72ec3ee4b08d3622adcc83.png)

## 参与者与协作者：
- [ ]  ``抽象角色``（``Subject``）：它是一个由真实角色实现的接口，并能够表征真实角色的功能。该接口同时也必须由代理角色实现，这样代理角色能够用于所有真实角色所能使用的地方。
- [ ]  ``代理角色``（``Proxy``）：它维护了能够允许代理角色访问真实角色的引用。代理角色与真实角色都实现了同一个接口，这样代理角色就能代替真实角色，实现对真实角色的访问控制并负责数据的创建和删除。根据代理的类型，其还能负责其他的职责。
- [ ]  ``真实角色``（``RealSubject``）：这是代理角色所代表的真实对象。

## 代码示例:
假如我们想要在电子邮件中附加一张图片。而添加一张照片并发送是一件耗费大量资源的操作。

这时我们使用代理模式，让我们能够在确实需要加载图片时才对它进行创建和初始化。


1. *首先，我们创建一个能够被真实对象和代理对象实现的通用接口*
```
public interface Image {
   void showImage();
}
```
2. *建立一个真实图片类实现上述接口*
```
public class RealImage implements Image {
 
   private URL url;
 
   public RealImage(URL url){
      this.url = url;
      loadImage(url);
   }
 
   @Override
   public void showImage() {
      System.out.println("Displaying " + url.getFile());
   }
 
   private void loadImage(URL url){
      System.out.println("Loading " + url.getFile());
   }
}

```
3. *编写代理类，提供对真实图片类的访问。*
```
public class ProxyImage implements Image{
 
   private RealImage realImage;
   private URL url;
 
   public ProxyImage(URL url){
      this.url = url;
   }
 
   @Override
   public void showImage() {
      if(realImage == null){
         realImage = new RealImage(url);
      }
      realImage.showImage();
   }
} 
```
4. *测试*
```
public class ProxyTest {
   
   public static void main(String[] args) {
      Image image = new ProxyImage(new URL("test.jpeg"));
 
      // 加载图片
      image.showImage(); 
      System.out.println("");
      // 图像加载过，则不需再次加载
      image.showImage();  
   }
}
```














