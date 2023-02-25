# design-pattern-notes
Design Pattern 筆記

# Creational Pattern
目的 <br>
- 抽象化 instantiation 的工序。令系統獨立於創造、構建物件（object）

要點 <br>
1. 隱藏（encapsulate）系統需要使用的 classes
    > encapsulate knowledge about which concrete classes the system uses
2. 隱藏創建 instances 嘅細節
    > hide how instances of these classes are created and put together

結果 <br>
> Consequently, the creational patterns give you a lot of flexibility in what gets created, who creates it, how it gets created, and when.

## Factory Method
### 目的
先範創造方法，再由 subclass 決定創造 object 的詳細方法 (implementation)

### 使用時機
1. 系統並不知道需要創造哪些 object
2. 系統希望由 subclass 決定創造哪些 objectA


### UML


### 例子
```java

 
```

## Abstract Factory
### 目的
- 規範化創造一系列相關、同屬的 object 的方法
- 使用方毋須知道「產品」的創造者／細節
> Provide an interface for creating families of related or dependent objects without specifying their concrete classes. 

### 使用時機
1. 系統毋須知道「產品」的創造方法、詳細
2. 同屬「產品」須同時使用
3. 使用者毋須知道創造「產品」的 implementation

### UML
![圖片](https://user-images.githubusercontent.com/71750702/221368481-fdf64fcd-786c-4a8e-a32c-f080c9560f93.png)

### 例子
```java
interface CarFactory {
  public Car createWheel();
  public Engine createEngine();
}

class JapanCarFactory implements CarFactory {
   public Car createTier() {
      return new JapanWheel();
   }
   
   public Engin createEngine() {
      return new JapanEngine();
   };
}

class USACarFactory implements CarFactory {
   public Car createWheel() {
      return new USAWheel();
   }
   
   public Engin createEngine() {
      return new USAEngine();
   };
} 
 
```
