---
title: 设计模式之Bridge模式
tags: 设计模式
categories: 编程思想
top: 1
abbrlink: 2af670e6
date: 2018-08-29 21:21:25
---

# Bridge 模式

## 存在之意图:
"将抽象与现实解耦，使它们可以独立地变化"。
----
> Bridge 模式是最难理解的模式，部分原因是它功能非常强大，适用于多种场合。而且，它还与常见的用继承来处理特殊情况的方式背道而驰。但是，它却是一个遵循设计模式社区两大原则的极好例子：“找出变化并封装之”和“优先使用对象聚集，而不是类继承”。


## 需求特点：
- 概念的抽象有变化；
- 这些概念的实现方式有变化。

**要点**：在需求定义期间，应该尽早而且经常地考虑变化。

<!-- more -->

## UML类图如下：
- 问题：一个抽象类的派生类必须使用多个实现，但它不能出现类数量爆炸性增长。
- 解决方案：为所有实现定义一个接口，供抽象类的所有派生类使用。
- 参与者与协作者：``Abstraction``为要实现的对象定义接口，``Implementor``为具体的实现类定义接口。``Abstraction``的派生类使用``Implementor``的派生类，却无需知道自己具体使用哪一个``ConcreteImplementor``。
- 效果：实现与使用实现的对象解耦，提供了可扩展性，客户对象无需操心实现问题。

![image](http://on-img.com/chart_image/5b6eab38e4b0f8477dadbdeb.png)

## 代码示例：
*假设目前有这么几个类:``GoogleTV`` 和 ``AppleTV``,都能实现换台，与开关电视等基本功能，但是并没有一个统一的接口，用户其实也不care你用的是哪个TV，用户只是单纯想看芒果台而已。现在使用Bridge模式实现这个需求。*

```
public interface TV{
    public void powerON();
    public void powerOff();
    public void changeChannel(int channel);
}

public class GoogleTV implements TV{
    public void powerOn(){}
    public void powerOff(){}
    public void changeChannel(int channel){}
}
public class AppleTV implements TV{
    public void powerOn(){}
    public void powerOff(){}
    public void changeChannel(int channel){}
}

public abstract class TVRemoteControl{
    private TV tv;
    
    public void setTV(TV tv){
        this.tv = tv; 
    }
    
    public TV getTV(){
        return this.tv;
    }
    
    public void powerOn(){
        implementor.powerOn();
    } 
    
    public void powerOff(){
        implementor.powerOff();
    }
    
    public void setChannel(int channel){
        implementor.changeChannel(channel);
    }
}

public class ConcreteTVRemoteControl extends TVRemoteControl{
    private int currentChannel;
    
    public void nextChannel(){
        currentChannel++;
        setChannel(currentChannel);
    }
    
    public void prevChannel(){
        currentChannel++;
        setChannel(currentChannel);
    }
}

public class BridgeTest { 
    public static void main(String[] args) {
    TVRemoteControl tvBridge = new TVRemoteControl();
    /*使用google TV */ 
    GoogleTV googleTV  = new GoogleTV(); 
    tvBridge.setTV(googleTV);
    tvBridge.setChannel(3);
    /*使用 Apple TV */ 
    GoogleTV googleTV  = new GoogleTV(); 
    tvBridge.setTV(googleTV);
    tvBridge.setChannel(3); 
    }
}

```
**可结合 ``Strategy``、 ``Adapter`` 模式来看。**


















