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

## Factory Method
### 目的
先範創造方法，再由 subclass 決定如何創造（implementation）object（將 instantiation 的責任交至 subclass） 

### 使用時機
1. 在程式執行前，系統並不知道需要創造哪些 object
2. 希望由 subclass 決定創造哪些 object

### UML


### 例子
```java
// product
interface Student {
    public void study();
}

// concret product
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

// creator
abstract class Parent {
    abstract Student createStudent();

    public void yell() {
        Student student = this.createStudent();
        System.out.println("Go study!");
        student.study();
    }
}

// concret creator 
abstract class SmartParent extends Parent{
    abstract Student createStudent() {
        return new SmartStudent();
    };
}

abstract class LazyParent extends Parent{
    abstract Student createStudent() {
        return new LazyParent();
    };
}

```
```java
Parent smartP = new SmartParent();
smartP.yell();
```

## Abstract Factory
### 目的
- 規範化建立一系列相關、同屬的 object 的方法
- 使用方毋須知道「產品」的建立細節
> Provide an interface for creating families of related or dependent objects without specifying their concrete classes. 

### 使用時機
1. 系統毋須知道「產品」的建立方法、詳細
2. 同屬「產品」須同時使用
3. 使用者毋須知道建立「產品」的 implementation

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
### 目的
將建立／創造複雜的 object 抽離，使得相同的「工序」能建立擁有不同設置／參數的 object

### 使用時機
1. 建立 object 的邏輯（logic, algorithm）應獨立於 object 本身的建構方式（construct）及其參數（parameters）
2. 可建立擁有不同設置／參數的 object


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

    public HumanBuilder {
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

    public HumanBuilder {
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