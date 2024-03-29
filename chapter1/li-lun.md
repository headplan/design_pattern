# 封装 , 抽象 , 继承 , 多态分别可以解决哪些编程问题 ?

#### 封装\(Encapsulation\)

**封装**也叫信息隐藏或者数据访问保护 . 类通过暴露有限的访问接口 , 授权外部仅能通过类提供的方式\(或者叫函数\)来访问内部信息或者数据 .

下面是一个简化的虚拟钱包代码实现 :

```java
public class Wallet {
    private String id; // 钱包的唯一编号
    private long createTime; // 钱包的创建时间
    private BigDecimal balance; // 钱包中的余额
    private long balanceLastModifiedTime; // 上次钱包余额变更的时间

    // 初始化
    public Wallet() {
        this.id = IdGenerator.getInstance().generate();
        this.createTime = System.currentTimeMillis();
        this.balance = BigDecimal.ZERO;
        this.balanceLastModifiedTime = System.currentTimeMillis();
    }

    // get获取属性
    public String getId() { return this.id; }
    public long getCreateTime() { return this.createTime; }
    public BigDecimal getBalance() { return this.balance; }
    public long getBalanceLastModifiedTime() { return this.balanceLastModifiedTime; }

    public void increaseBalance(BigDecimal increasedAmount) {
        if (increasedAmount.compareTo(BigDecimal.ZERO) < 0) {
          throw new InvalidAmountException("...");
        }
        this.balance.add(increasedAmount);
        this.balanceLastModifiedTime = System.currentTimeMillis();
    }

    public void decreaseBalance(BigDecimal decreasedAmount) {
        if (decreasedAmount.compareTo(BigDecimal.ZERO) < 0) {
          throw new InvalidAmountException("...");
        }
        if (decreasedAmount.compareTo(this.balance) > 0) {
          throw new InsufficientAmountException("...");
        }
        this.balance.subtract(decreasedAmount);
        this.balanceLastModifiedTime = System.currentTimeMillis();
    }
}
```

Wallet类主要有四个属性 . 参照封装特性 , 对钱包的这四个属性的访问方式进行了限制 . 只允许调用通过下面六个方法来访问或者修改钱包里的数据 .

* String getId\(\)
* long getCreateTime\(\)
* BigDecimal getBalance\(\)
* long getBalanceLastModifiedTime\(\)
* void increaseBalance\(BigDecimal increasedAmount\)
* void decreaseBalance\(BigDecimal decreasedAmount\)

从业务的角度来说 , id , createTime在创建钱包的时候就确定好了 , 之后不应该再被改动 . 所以在Wallet类中没有暴露任何修改这两个属性的方法 set都没有 . 这两个属性的初始化设置 , 对于Wallet类的调用者来说 , 应该也是透明的 . 这里在构造函数内将其初始化设置好了 , 而不是通过构造函数的参数来外部赋值 .

对于balance属性 , 也就是钱包中的余额 . 只能增减 , 不存在重新设置 . 所以 , 这里只暴露了两个方法 , increaseBalance\(\)和decreaseBalance\(\) , 也没有暴露set方法 . balanceLastModifiedTime这个属性完全跟balance属性的修改操作绑定在一起 . 只有在balance修改的时候 , 它才会被修改 . 所以 , balanceLastModifiedTime的修改操作完全封装在了increaseBalance\(\)和decreaseBalance\(\)方法中 , 不对外暴露修改这个属性的方法和业务细节 , 也保证了balance和balanceLastModifiedTime的一致性 .

封装特性需要语言本身的语法支持 , 也就是**访问权限控制** .

#### 总结

封装也叫做信息隐藏或者数据访问保护 . 类通过暴露有限的访问接口 , 授权外部仅能通过类提供的方式来访问内部信息或者数据 .

它需要编程语言提供权限访问控制语法来支持 , 例如Java中的private , protected , public关键字 .

封装特性存在的意义 , 一方面是保护数据不被随意修改 , 提高代码的可维护性 ; 另一方面是仅暴露有限的必要接口 , 提高类的易用性 .

#### 抽象\(Abstraction\)

封装主要讲的是如何隐藏信息 , 保护数据 , 而抽象讲的是如何隐藏方法的具体实现 , 让调用者只需要关心方法提供了哪些功能 , 并不需要知道这些功能是如何实现的 . 在面向对象编程中 , 常借助编程语言提供的接口类\(interface\)或者抽象类\(abstract\)这两种语法机制 , 来实现抽象这一特性 .

```java
public interface IPictureStorage {
    void savePicture(Picture picture);
    Image getPicture(String pictureId);
    void deletePicture(String pictureId);
    void modifyMetaInfo(String pictureId, PictureMetaInfo metaInfo);
}

public class PictureStorage implements IPictureStorage {
    @Override
    public void savePicture(Picture picture) { ... }
    @Override
    public Image getPicture(String pictureId) { ... }
    @Override
    public void deletePicture(String pictureId) { ... }
    @Override
    public void modifyMetaInfo(String pictureId, PictureMetaInfo metaInfo) { ... }
}
```

上面的代码中 , 利用Java中的interface接口语法来实现抽象特性 . 调用者在使用图片存储功能的时候 , 只需要了解IPictureStorage这个接口类暴露了哪些方法就可以了 , 不需要知道具体的实现逻辑 .

抽象这个特性是非常容易实现的 , 并不需要非得依靠接口类或者抽象类这写特殊的语法机制 . 因为类的方法就是通过编程语言中的函数机制实现的 . 通过函数包裹具体的实现逻辑 , 这本身就是一种抽象 .

抽象是一个非常通用的设计思想 , 并不需要特殊的语法机制 , 只需要提供函数这一非常基础的语法机制 , 就可以实现抽象特性 , 所以有时候不被看做是面向对象编程的特性 .

很多设计原则都体现了抽象这种设计思想 , 比如基于接口而非实现编程 , 开闭原则 , 代码解耦等 . 一个简单的例子 , 定义类的方法时 , 也有抽象思想 , getAliyunPictureUrl\(\)命名为getPictureUrl\(\) , 如果以后存储的图片不在Aliyun上了 , 这个方法的命名也不需要修改 .

#### 总结

封装主要讲如何隐藏信息 , 保护数据 , 那抽象就是讲如何隐藏方法的具体实现 , 让使用者只需要关心方法提供了哪些功能 , 不需要知道这些功能是如何实现的 . 抽象可以通过接口类或者抽象类来实现 , 但也并不需要特殊的语法机制来支持 . 抽象存在的意义 , 一方面是提高代码的可扩展性 , 维护性 , 修改实现不需要改变定义 , 减少代码的改动范围 ; 另一方面 , 它也是处理复杂系统的有效手段 , 能有效的过滤掉不必要关注的信息 .

#### 继承\(Inheritance\)

继承对于常见的面向对象编程语言来说并不陌生 , 是用来表示类之间的is-a关系 . 从继承关系上来讲 , 继承可以分为两种模式 , 单继承和多继承 . 单继承表示一个子类只继承一个父类 , 多继承表示一个子类可以继承多个父类 .

为了实现继承这个特性 , 编程语言需要提供特殊的语法机制来支持 , 比如Java使用extends关键字来实现继承 , C++使用冒号\(class B : public A\) , Python使用paraentheses\(\) , Ruby使用&lt; . 不过 , 有些编程语言只支持单继承 , 不支持多继承 , 比如Java , PHP , C\# , Ruby等 , 而有些编程语言即支持单继承 , 也支持多继承 , 比如C++ , Python , Perl等 .

通过继承来关联两个类 , 反映真实世界中的关系 , 非常符合人类的认知 , 但过渡使用继承 , 继承层次过深过复杂 , 就会导致代码可读性 , 可维护性变差 . 子类和父类高度耦合 , 修改父类的代码 , 会直接影响到子类 .

所以 , 继承特性也是有争议的 , 也有人剔除继承是一种反模式 , 提倡多用组合少用继承 .

#### 总结

继承是用来表示类之间的is-a关系 , 分为两种模式 : 单继承和多继承 . 单继承表示一个子类只继承一个父类 , 多继承表示一个子类可以继承多个父类 . 为了实现继承这个特性 , 编程语言需要提供特殊的语法机制来支持 . 继承主要是用来解决代码复用的问题 .

#### 多态\(Polymorphism\)

多态是指 , 子类可以替换父类 , 在实际的代码运行过程中 , 调用子类的方法实现 .

```java
public class DynamicArray {
    private static final int DEFAULT_CAPACITY = 10;
    protected int size = 0;
    protected int capacity = DEFAULT_CAPACITY;
    protected Integer[] elements = new Integer[DEFAULT_CAPACITY];

    public int size() { return this.size; }
    public Integer get(int index) { return elements[index]; }

    public void add(Integer e) {
        ensureCapacity();
        elements[size++] = e;
    }

    protected void ensureCapacity() {
        //... 如果数组满了就扩容... 代码省略...
    }
}

public class SortedDynamicArray extends DynamicArray {
    @Override
    public void add(Integer e) {
        ensureCapacity();
        int i;
        for (i = size-1; i>=0; --i) { // 保证数组中的数据有序
            if (elements[i] > e) {
                elements[i+1] = elements[i];
            } else {
                break;
            }
        }
        elements[i+1] = e;
        ++size;
    }
}

public class Example {
    public static void test(DynamicArray dynamicArray) {
        dynamicArray.add(5);
        dynamicArray.add(1);
        dynamicArray.add(3);
        for (int i = 0; i < dynamicArray.size(); ++i) {
            System.out.println(dynamicArray.get(i));
        }
    }

    public static void main(String args[]) {
        DynamicArray dynamicArray = new SortedDynamicArray();
        test(dynamicArray); // 打印结果:1,3,5
    }
}
```

多态这种特性也需要编程语言提供特殊的语法机制来实现 . 上面的代码用了三个语法机制来实现多态 :

* 第一个语法机制是编程语言要支持父类对象可以引用类对象 , 也就是可以将SortedDynamicArray传递给DynamicArray . 
* 第二个语法机制是编程语言要支持继承 , 也就是SortedDynamicArray继承了DynamicArray , 才能将SortedDyamicArray传递给DynamicArray . 
* 第三个语法机制是编程语言要支持子类可以重写\(override\)父类中的方法 , 也就是SortedDyamicArray重写了DynamicArray中的add\(\)方法 . 

这三种语法机制配合在一起 , 实现了test\(\)方法中子类替换父类的add方法\(\) , 也就是实现了多态特性 .

对于多态特性的实现方式 , 除了利用"继承加方法重写"这种实现方式之外 , 还有其他两种比较常见的实现方式 , **一个是利用接口类语法** , **另一个是利用duck-typing语法** . 不过有的语言不支持 , 比如C++就不支持接口类的语法 , 而duck-typing只有一些动态语言才支持 .

#### 接口类实现多态特性

```java
public interface Iterator {
    String hasNext();
    String next();
    String remove();
}

public class Array implements Iterator {
    private String[] data;

    public String hasNext() { ... }
    public String next() { ... }
    public String remove() { ... }
}

public class LinkedList implements Iterator {
    private LinkedListNode head;

    public String hasNext() { ... }
    public String next() { ... }
    public String remove() { ... }
}

public class Demo {
    private static void print(Iterator iterator) {
        while (iterator.hasNext()) {
            System.out.println(iterator.next());
        }
    }

    public static void main(String[] args) {
        Iterator arrayIterator = new Array();
        print(arrayIterator);

        Iterator linkedListIterator = new LinkedList();
        print(linkedListIterator);
    }
}
```

Iterator是一个接口类 , 定义了一个可以遍历集合数据的迭代器 . Array和LinkedList都实现了接口类 , 通过传递不同类型的实现类到print\(Iterator iterator\)函数中 , 支持动态的调用不同的next\(\) , hasNext\(\)实现 .

#### Duck-typing来实现多态特性

```py
class Logger:
    def record(self):
        print("I write a log into file.")

class DB:
    def record(self):
        print("I insert data into db.")

def test(recorder):
    recorder.record()

def demo():
    logger = Logger()
    db = DB()
    test(logger)
    test(db)
```

上面的代码可以发现 , duck-typing实现多态的方式非常灵活 . Logger和DB两个类没有任何关系 , 既不是继承关系 , 也不是接口和实现的关系 , 但是只要它们都有定义了record\(\)方法 , 就可以被传递到test\(\)方法中 , 在实际运行的时候 , 执行对应的record\(\)方法 .

也就是说 , 只要两个类具有相同的方法 , 就可以实现多态 , 并不要求两个类之间有任何关系 , 这就是所谓的**duck-typing** , 是一些动态语言所特有的语法机制 . 而像Java这样的静态语言 , 通过继承实现多态特性 , 必须要求两个类之间有继承关系 , 通过接口实现多态特性 , 类必须实现对应的接口 . 

#### 总结

多态是指子类可以替换父类 , 在实际的代码运行过程中 , 调用子类的方法实现 . 多态这种特性也需要编程语言提供特殊的语法机制来实现 , 比如继承 , 接口类 , duck-typing . 多态特性能提高代码的可扩展性和复用性 , 是很多设计模式 , 设计原则 , 编程技巧的代码实现基础 , 比如策略模式 , 基于接口而非实现编程 , 依赖倒置原则 , 里式替换原则 , 利用多态去掉冗长的if-else语句等等 . 



