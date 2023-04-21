# design-pattern-notes
Design Pattern 筆記

# Creational Pattern
目的 <br>
- 抽出／抽象化 instantiation 的工序。令系統獨立於創造、構建物件（object）

要點 <br>
1. 隱藏（encapsulate）系統需要使用的 classes
    > encapsulate knowledge about which concrete classes the system uses
2. 隱藏創建 instances 嘅細節
    > hide how instances of these classes are created and put together

結果 <br>
> Consequently, the creational patterns give you a lot of flexibility in what gets created, who creates it, how it gets created, and when.

## Simple Factory
### 簡介／目的
*真．object 工廠*

- 根據輸入（parameter）建立相應的 object

版本：
<ol type="a">
    <li>static factory：在 class 中建立 static method</li>
    <li>simple factory：建立一個 factory class</li>
</ol>

### 使用時機
1. 決定建立哪個 object 牽涉特定邏輯，且
2. 經常需要以同樣邏輯建立 object

### 注意事項
- 此 pattern 違反了 *OCP* 的原則。因為新增 subclass 時，factory 都需要作出改動。

### UML
N/A

### 例子
1. static factory
    ```java
    public class File {
        public File static createFile(String type) {
            // if-else also ok
            switch (type) {
                case "PDF":
                    return new PdfFile();
                case "CSV":
                    return new CsvFile();
                case "JSON":
                    return new JsonFile();
            }
        }

        // ...
    }
    ```
2. simple factory
    ```java
    public class FileFactory {
        public File createFile(String type) {
            // if-else also ok
            switch (type) {
                case "PDF":
                    return new PdfFile();
                case "CSV":
                    return new CsvFile();
                case "JSON":
                    return new JsonFile();
            }
        }
    }
    ```
```java
File pdf = File.createFile("PDF");
```

## Factory Method
### 簡介／目的
*設置用作建立 object 的 function*
> Define an interface for creating an object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses.

版本：
<ol type="a">
    <li>定立規範（interface），再由 subclass 決定如何創造（implementation）object。</li>
    <li>在 base class 提供預設 implementation。在有需要時，在 subclass 重新定議（override）。</li>
</ol>

### 使用時機
1. 一系列的 function/logic 能接受不同 object 而達到不同結果
2. 需由 subclass 決定創造哪些 object

### 注意事項
N/A

### UML
![圖片](https://user-images.githubusercontent.com/71750702/227728426-d5fdcb7f-387e-473f-9932-2c330e21ee1a.png)

### 例子
```java
interface Student {
    public void study();
}

class SmartStudent implements Student{
    public void study() {
        System.out.println("I don't need to study.");
    }
}

class LazyStudent implements Student{
    public void study() {
        System.out.println("I don't want to study.");
    }
}

abstract class Parent {
    /** factory method */
    protected abstract Student createStudent();

    public void yell() {
        Student student = this.createStudent();
        System.out.println("Go study!");
        student.study();
    }
}

abstract class SmartParent extends Parent{
    protected abstract Student createStudent() {
        return new SmartStudent();
    };
}

abstract class LazyParent extends Parent{
    protected abstract Student createStudent() {
        return new LazyParent();
    };
}

```
```java
Parent smartP = new SmartParent();
smartP.yell();
```

## Abstract Factory
### 簡介／目的
*為同一系列、相關的 object 定立規範*
> Provide an interface for creating families of related or dependent objects without specifying their concrete classes.

- 將互相關連、依賴的 class/object 組合成一個 class
- decopule from concrete classes 

### 使用時機
1. 同屬 object 須同時使用（dependency）
2. 系統（class/function）有需要使用同特定 object 來執行其功能

### 注意事項
N/A

### UML
![圖片](https://user-images.githubusercontent.com/71750702/221368481-fdf64fcd-786c-4a8e-a32c-f080c9560f93.png)

### 例子
```java
interface CarComponentFactory {
  public Wheel createWheel();
  public Engine createEngine();
}

class JpCarComponentFactory implements CarComponentFactory {
   public Wheel createWheel() {
      return new JapanWheel();
   }
   
   public Engine createEngine() {
      return new JapanEngine();
   };
}

class UsaCarComponentFactory implements CarComponentFactory {
   public Wheel createWheel() {
      return new USAWheel();
   }
   
   public Engine createEngine() {
      return new USAEngine();
   };
} 
 
```
```java
CarComponentFactory factory = new JpCarComponentFactory();
Car car = Car(factory.createWheel(), factory.createEngine());
```

## Builder
### 簡介／目的
*將建立 object 分拆成不同步驟*
> Separate the construction of a complex object from its representation so that the sam construction process can create different representations

- 透過將建立／創造複雜的 object 抽出成不同步驟（function），使得相同的「工序」能建立擁有不同設置／參數的 object

版本：
<ol type="a">
    <li>被建立的 object 以不同的參數（paremters）進行 construction</li>
    <li>被建立的 object 以 builder object 進行 construction</li>
</ol>

### 使用時機
1. 建立 object 的邏輯複雜，例如有大量參數（parameter）
2. 需方便地建立擁有不同設置／參數的 object

### 注意事項
N/A

### UML
![圖片](https://user-images.githubusercontent.com/71750702/226191654-ffe8916b-fc06-45a0-b8af-35c1cc561d38.png)

### 例子

1. Construct with parameters
    ```java
    class Human {

        private string name;
        private int age;

        public Human(string name, int age) {
            this.name = name;
            this.age = age;
        }
    }

    class HumanBuilder {
        private string name;
        private int age;

        // optional, assign default value for fields
        public HumanBuilder() {
            this.name = "John Doe";
            this.age = 30;
        }

        public Human(HumanBuilder builder) {
            this.name = builder.name;
            this.age = builder.age;
        }

        public HumanBuilder setName(string name) {
            this.name = name;
            return this;
        }

        public HumanBuilder setAge(int age) {
            this.age = age;
            return this;
        }

        public Human getHuman() {
            return Human(this.name, this.age);
        }
    }
    ```

2. Construct with builder
    ```java
    class Human {
        private string name;
        private int age;

        public Human(HumanBuilder builder) {
            this.name = builder.name;
            this.age = builder.age;
        }
    }

    class HumanBuilder {
        public string name;
        public int age;

        // optional, assign default value for fields
        public HumanBuilder() {
            this.name = "John Doe";
            this.age = 30;
        }

        public HumanBuilder setName(string name) {
            this.name = name;
            return this;
        }

        public HumanBuilder setAge(int age) {
            this.age = age;
            return this;
        }

        public Human getHuman() {
            return Human(this);
        }
    }
    ```
```java
Human human = HumanBuilder()
                .setName("Peter")
                .setAge(20)
                .getHuman()
```

## Prototype
### 簡介／目的
*用作被複製的 object*
> Specify the kinds of objects to create using a prototypical instance, and create new objects by copying this prototype

- 需要 subclassing 的機會較 *Factory Method* 少，因為 object 可從 prototype field 中複雜而不是交由 subclass 負責建立。
- prototype 可隨時被切換

### 使用時機
1. 一個 class 只有少量設置（construct）的組合（combination），而
2. 複製比重新建立更有效率

### 注意事項
- 須留意 *shallow copy* 及 *deep copy* 問題
- 可建立一個 prototype manager 來管理不同的 prototype
  - 例如以 `Library` class 來儲存不同 `Book` 的 prototype
- 一般而言，`clone()` 不應接受任何參數。如有需要，應考慮以 prototype 提供的 method 來改變其參數或新增一個 `initialize` 的 method 來接受新的參數設定。

### UML
![圖片](https://user-images.githubusercontent.com/71750702/227728407-5e94b049-6d70-4d15-86a8-ea8459620a3a.png)

### 例子

```java
abstract class Book implements Cloneable {
    public abstract String getName();
    public abstract void read();

    @override
    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}

class EnglishBook extends Book {
    public String getName() {
        return "ABC";
    }

    public void read() {
        System.out.println("This book is about...");
    }
}

class ChineseBook extends Book {
    public String getName() {
        return "甲乙丙";
    }

    public void read() {
        System.out.println("這本書是...");
    }
}
```
```java
Book prototype = new EnglishBook();
Book book1 = (Book) prototype.clone();
Book book2 = (Book) prototype.clone();
// book1, book2 is the same
```

## Singleton
### 簡介／目的
*全域（global）、唯一的 object*
> Ensure a class only has one instance, and provide a global point of access to it.

- 可確保資料（field, state）的統一性
- 等同全域變量（global variable）

### 使用時機
1. class 只能有一個 instance 存在

### 注意事項
- 在極端情況下（例如 multi-threading），`singleton` 有機會被建立多於一個 instance。［取決於 language design］
  - 可以 [Double Check Locking](https://en.wikipedia.org/wiki/Double-checked_locking) 來解決。

<p style="color: red;">
    此 pattern 一般被視作為 anti-pattern，因為 global state 難以 debug。
</p>

### UML
![圖片](https://user-images.githubusercontent.com/71750702/227728383-75fd3ca0-e581-4e2e-b8a9-1236e2689911.png)

### 例子

```java
public class Home {
    private static Home instance;

    private Home() {}

    public Home getInstance() {
        if (Home.instance == null) {
            Home.instance = new Home();
        }
        return Home.instance
    }
}

```
```java
Home myHome = Home.getInstance();
```

## Structural Pattern

目的 <br>
- 著眼於 class 及 object 如何互相組合，構成更完整的結構
> Structural patters are concerned with how classes and objects to form larger structure.

要點 <br>
1. 利用 inheritance 將 interface 或 implementation 組合在一起 
2. 透過不同組合方法來實現新的功能

結果 <br>
> The added flexibility of object composition comes from the ability to change the composition at run-time, which is impossible with static class composition.

## Adapter
### 簡介／目的
*object 偽裝者*
> Convert the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.

- 裝一個 class 的 interface 轉換成另一個 class 的 interface，令原不同相容的 class/interface 能夠被運用

版本：
<ol type="a">
    <li>Class adapter：利用繼承（inheritance）或實作（implemente）來 override parent 的 method/function</li>
    <li>Object adapter：利用 composition，再在相應 method/function invoke adaptee 的 function</li>
</ol>

### 使用時機
1. 需使用的 class 的 interface 並不相容於代碼
2. 使要使用有多於一個 interface，而 subclassing 並不可行

### 注意事項
Class adapter
 - 繼承（inheritance）的方式會導致 adapter 只能 adapt 單一個 class。但此方法能 override parent 的 implementation
 - 某些 language 只容許單一繼承（single inheritance）

Object adapter
 -  如果需要 override adaptee，需先建立一個 inherit adaptee 的 class，再在 adapter 接受該 subclass

### UML
N/A

### 例子

1. Class adapter
    ```java
    /** 兩腳插座 */
    class ATypeOutlet {
        public void plugAType() {
            System.out.println("plugged");
        }
    }

    /** 三腳插座 */
    class GTypeOutlet {
        public void plugGType() {
            System.out.println("plugged");
        }
    }

    class UkAppliance {
        public void powerOn(GTypeOutlet outlet) {
            outlet.plugGType();
        }
    }

    /** 三腳轉兩腳的轉插 */
    class GTypetoATypeAdapter extends ATypeOutlet {

        @Override
        public void plugAType() {
            this.plugGType();
        }
    }
    ```
    ```java
    UkAppliance appliance = new UkAppliance();
    appliance.powerOn(new GTypetoATypeAdapter());
    ```

2. Object adapter
    ```java
    /** 兩腳插座 */
    interface ATypeOutlet {
        public void plugAType();
    }

    /** 三腳插座 */
    interface GTypeOutlet {
        public void plugGType();
    }

    class JapanPowerOutlet implements GTypeOutlet {
        @Override
        public void plugAType() {
            System.out.println("plugged");
        }
    }

    class BritishPowerOutlet implements GTypeOutlet {
        @Override
        public void plugGOutlet() {
            System.out.println("plugged");
        }
    }

    class UkAppliance {
        public void powerOn(GTypeOutlet outlet) {
            outlet.plugGType();
        }
    }

    /** 英式（三腳）轉日式（兩腳）的轉插 */
    class UktoJpAdapter implements GTypeOutlet {
        private JapanPowerOutlet outlet;

        UktoJpAdapter(JapanPowerOutlet outlet) {
            this.outlet = outlet;
        }

        @Override
        public void plugGType() {
            this.plugAType();
        }
    }
    ```
    ```java
    UkAppliance appliance = new UkAppliance();
    Outlet adapter = new UktoJpAdapter(new JapanPowerOutlet());
    appliance.powerOn(adapter);
    ```

## Bridge
### 簡介／目的
*TBC*
> Decouple an abstraction from its implementation so that the two can vary independently

- 將 abstraction 跟 implementation 的 class hierarchy 分離。
- 以 composition 代替 inheritance
  - 一般情況下，一個 abstraction 會以繼承（inheritance）的方式建立不同的 concrete implementation。但這會導致 abstraction 跟 implementation 之間的 coupling，並不 flexible。

### 使用時機
- implementation 可在 runtime 決定／改變
- abstraction 或 implementation 的任何改定都不應影響對方／recompile
- > proliferation of classes

### 注意事項


### UML
N/A

### 例子

```java


```
```java

```