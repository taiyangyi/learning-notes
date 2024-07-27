### 1. 前言概述

> Array + List = 数组 + 列表 = ArrayList

ArrayList类实现了List接口，它的数据结构是基于数组实现的，在我们实际的开发中，ArrayList是我们最常用一种集合之一，本文主要从基本原理、优缺点、常用的核心方法原理、数组扩容以及元素拷贝等方面学习，希望对大家有所帮助。

### 2. 创建方式


> 方式1:List<数据类型> 变量名 = new ArrayList<数据类型>();

这种方式是最常见的方式。<数据类型>表示采用泛型方式，这里会强制限定存储在集合中的元素类型。当然，也可以不去定义，默认元素类型是  **Object** ，即集合中可以存储任意类型的数据。

```java
// 泛型创建
List<String> list = new ArrayList<String>();
list.add("小邰");
list.add("胖邰");
list.add("老邰");

// 非泛型创建
List list2 = new ArrayList();
list.add("小邰");
list.add(1);
list.add(1.1);
```
> 使用匿名内部类的方式、

这种方式使用匿名内部类的方式来初始化创建
- 外层括号{}:建立ArrayList的匿名子类
- 内层括号{}:建立匿名子类的构造模块

```java
    List<String> list = new ArrayList<String>() {
        {
            list.add("1111");
            list.add("2222");
            list.add("3333");
        }
    };
```

> 使用Arrays.asList来初始化

通过Arrays.asList传递给ArrayList构造函数的方式进行初始化


```java
ArrayList<String> list = new ArrayList<String>(Arrays.asList("小邰","胖邰","老邰"));
```

### 基本原理

ArrayList底层是基于数组实现的。我们知道，数组一旦被初始化，长度固定且不可变。这就导致了ArrayList中的一个重要特性：扩容。数组元素类型为Object类型来实现的。具体看下面源码：

> 初始化无参集合

```java
  public static void main(String[] args) {
    // 未指定初始化大小
    List<String> list = new ArrayList<String>();
    // 指定初始化大小
    List<String> stringList = new ArrayList<String>(10);
  }
```

#### 2.1 未指定初始化大小的ArrayList源码步骤拆解

> 点击ArrayList，进入源码层，此时我们看到，当未指定初始化大小时，给elementData指向DEFAULTCAPACITY_EMPTY_ELEMENTDATA

```java
    /**
     * 空构造集合并且默认的大小为10
     * Constructs an empty list with an initial capacity of ten.
     */
    public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }
```

> DEFAULTCAPACITY_EMPTY_ELEMENTDATA是Object类型的空数组

```java
    /**
     * Shared empty array instance used for default sized empty instances. We
     * distinguish this from EMPTY_ELEMENTDATA to know how much to inflate when
     * first element is added.
     */
    private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
```

> DEFAULT_CAPACITY 默认的空间大小为10个

```java
   /**
     * Default initial capacity.
     */
    private static final int DEFAULT_CAPACITY = 10;
```

#### 2.2 指定初始化大小的ArrayList源码步骤拆解
> 指定elementData数组的大小，不允许初始化大小小于0，否则抛出异常
```java
    /**
     * Constructs an empty list with the specified initial capacity.
     *
     * @param  initialCapacity  the initial capacity of the list
     * @throws IllegalArgumentException if the specified initial capacity
     *         is negative
     */
    public ArrayList(int initialCapacity) {
        // 当 initialCapacity > 0 时，初始化对应大小的数组
        if (initialCapacity > 0) {
            // 初始化元素数组
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            // 当初始容量为0，指向EMPTY_ELEMENTDATA
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            // 当初始容量小于0，抛出非法异常
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    }
```
> 当初始容量为0时的构造函数初始化，指向EMPTY_ELEMENTDATA空数组对象实例
```java
    /**
     * Shared empty array instance used for empty instances.
     */
    private static final Object[] EMPTY_ELEMENTDATA = {};
```

### 3.优缺点分析

#### 3.1 缺点是什么?

> 缺点一

首先，数组的长度是固定的，java的数组都是定长数组。比如：数组长度大小为50，此时不停的往ArrayList里面插入这个数据，当元素数量>50，此时就会发生一个数组的扩容的现象，就会生成一个更大的数组，把以前的数组拷贝到新的数组里面去，这个**数组扩容+元素拷贝**的过程，相对来说会慢一些。所以，不要频繁的往ArrayList里面去插入数据，导致他频繁的数组扩容，避免扩容的时候较差的性能影响了系统的运行。

> 缺点二

如果往数组的中间位置插入一个元素，导致的结果是新增元素后面的元素需要集体往后面挪动一位。所以说，如果往ArrayList中间插入一个元素，会导致他后面的大量的元素挪动一个位置，势必导致性能比较差。

```java
    public static void main(String[] args) {
        List<String> list = new ArrayList<String>();
        // 0
        list.add("中国");
        // 1
        list.add("俄罗斯");
        // 2
        list.add("加拿大");
        // 此时inde为2的值就为英国   
        // 3的位置加拿大
        list.add(2,"英国");
        System.out.println(list.get(2));

    }
```

```java
英国
加拿大
Process finished with exit code 0
```

#### 3.2 优点是什么？

ArrayList基于数组来实现，非常适合随机读，你可以随机的去读数组中的某个元素。

比如：

```java
// 相当于获取索引为2的值也就是获取第3个元素
list.get(2)
```

因为基于底层对数组的实现来快速的随机读取到某个元素，直接可以通过内存地址来定位某个元素。

> 这里就有一个疑问，为什么数组查询快？

- 数组是存储同一类型值的集合，一旦被创建，就不能改变它的长度
- 声明数组的内存空间是连续的，数组变量名指向连续空间的首地址

由此我们会得出：

- 连续的内存地址，元素的内存地址作为整个数组对象的内存地址
- 数组中的元素都是同一类型，占用空间大小一样，通过下标就可以计算出被查找的元素

#### 3.3 场景总结

> 合适场景

- 按顺序插入一些数据

- 不会频繁的插入、移动、数组扩容元素
- 插入数据后，主要用于遍历、通过索引随机读取某个元素

### 4. 常用的核心方法原理

#### 4.1 add(E e)插入方法源码

每次往ArrayList中插入数据的时候，人家都会判断当前数组的元素是否被塞满，如果塞满的话，此时就会扩容这个数组，然后将老数组中的元素拷贝到新数组中去，确保说数组一定是可以承受足够多的元素的。

```java
    /**
     * Appends the specified element to the end of this list.
     *
     * @param e element to be appended to this list
     * @return <tt>true</tt> (as specified by {@link Collection#add})
     */
    public boolean add(E e) {
        // 判断是否扩容
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        elementData[size++] = e;
        return true;
    }
```

> ensureCapacityInternal(int minCapacity)  对构造方法初始化的数组进行处理

```java
    private void ensureCapacityInternal(int minCapacity) {
        ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
    }
```

> calculateCapacity(Object[] elementData, int minCapacity) 计算空间容量

```java
    private static int calculateCapacity(Object[] elementData, int minCapacity) {
        // 判断元素数组是否为空数组
        if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            // 两者取最大
            return Math.max(DEFAULT_CAPACITY, minCapacity);
        }
        return minCapacity;
    }
```

> ensureExplicitCapacity(int minCapacity) 检查是否需要扩容，确保elemenData数组有合适的大小

```java
// ArrayList成员变量
protected transient int modCount = 0;    

	private void ensureExplicitCapacity(int minCapacity) {
        // 修改次数的计数器
        modCount++;
        // overflow-conscious code
        // // 如果需要的空间大小 > 当前数组的长度，则进行扩容
        if (minCapacity - elementData.length > 0)
            grow(minCapacity);
    }
```

>grow(int minCapacity) 对数组进行扩容  

```java
    /**
     * Increases the capacity to ensure that it can hold at least the
     * number of elements specified by the minimum capacity argument.
     *
     * @param minCapacity the desired minimum capacity
     */
    private void grow(int minCapacity) {
        // overflow-conscious code
        // 老容量
        int oldCapacity = elementData.length;
        // 带符号右移等价于除以2，因此每一次扩容以后，数组的长度为原来的1.5倍。
        // 比如：二进制0001=1 >> 1   ->   0011=3    3/2 = 1.5
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        // 新容量小于参数指定容量，修改新容量
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        // 新容量大于最大容量
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            // 指定新容量
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        // 搞一个新数组，Arrays.copyOf()工具方法，完成老数组到新数组的一个拷贝
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
```

> hugeCapacity(int minCapacity) 最大的容量

```java
 @Native public static final int   MAX_VALUE = 0x7fffffff;   
 private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;

  private static int hugeCapacity(int minCapacity) {
        if (minCapacity < 0) // overflow
            // 抛异常
            throw new OutOfMemoryError();
        // 需要的最小容量 > 数组最大的长度，则取Integer的最大值，否则取数组最大长度
        return (minCapacity > MAX_ARRAY_SIZE) ?
            Integer.MAX_VALUE :
            MAX_ARRAY_SIZE;
    }
```

#### 4.2 add(int index, E element)插入方法源码

> 指定位置插入元素,并且后面的元素统一向后移一位

```java
    /**
     * Inserts the specified element at the specified position in this
     * list. Shifts the element currently at that position (if any) and
     * any subsequent elements to the right (adds one to their indices).
     *
     * @param index index at which the specified element is to be inserted
     * @param element element to be inserted
     * @throws IndexOutOfBoundsException {@inheritDoc}
     */
    public void add(int index, E element) {
        // 检查索引index是否越界
        rangeCheckForAdd(index);
				// 判断是否需要扩容，并根据需要扩容
        ensureCapacityInternal(size + 1);  // Increments modCount!!
        // 将index及之后的元素向后移动1个位置,调用System.arraycopy方法进行数组赋值
        System.arraycopy(elementData, index, elementData, index + 1,
                         size - index);
        // 对index位置赋值
        elementData[index] = element;
        // size自增
        size++;
    }
```

> rangeCheckForAdd(index)  检查索引index是否越界，如果index小于0或大于当前已存储的大小，就抛出IndexOutOBoundsException的异常

```java
    /**
     * A version of rangeCheck used by add and addAll.
     */
    private void rangeCheckForAdd(int index) {
        if (index > size || index < 0)
            throw new IndexOutOfBoundsException(outOfBoundsMsg(index));
    }
```

#### 4.3 set(int index, E element)修改索引对应的元素

> 代码示例

```java
    public static void main(String[] args) {
        List<String> list = new ArrayList<String>();
        // 0
        list.add("中国");
        // 1
        list.add("俄罗斯");
        System.out.println(list);
        list.set(1,"美国");
        System.out.println(list);
    }
```

```java
[中国, 俄罗斯]
[中国, 美国]

Process finished with exit code 0
```
> 源码赏析

```java
    /**
     * Replaces the element at the specified position in this list with
     * the specified element.
     *
     * @param index index of the element to replace
     * @param element element to be stored at the specified position
     * @return the element previously at the specified position
     * @throws IndexOutOfBoundsException {@inheritDoc}
     */
    public E set(int index, E element) {
        // 1. 判断索引是否越界
        rangeCheck(index);
        // 2. 缓存原有数据
        E oldValue = elementData(index);
        // 3. 用新值替换原值
        elementData[index] = element;
        // 4. 返回原有数值
        return oldValue;
    }

    E elementData(int index) {
        return (E) elementData[index];
    }
```

#### 4.4 get(int index) 获取某一索引对应的值

> 代码示例

```java
    public static void main(String[] args) {
        List<String> list = new ArrayList<String>();
        // 0
        list.add("中国");
        // 1
        list.add("俄罗斯");
        String s = list.get(0);
        System.out.println(s);
    }
```

```java
中国

Process finished with exit code 0
```

> 源码赏析

```java
    /**
     * Returns the element at the specified position in this list.
     *
     * @param  index index of the element to return
     * @return the element at the specified position in this list
     * @throws IndexOutOfBoundsException {@inheritDoc}
     */
    public E get(int index) {
        // 1. 判断索引是否越界
        rangeCheck(index);
        // 2. element函数返回具体的元素
        return elementData(index);
    }
```

#### 4.5 remove(int index) 删除某一索引对应的值

> 代码示例

```java
    public static void main(String[] args) {
        List<String> list = new ArrayList<String>();
        // 0
        list.add("中国");
        // 1
        list.add("俄罗斯");
        System.out.println(list);
        list.remove(2);
        System.out.println(list);
    }
```

```java
[中国, 俄罗斯]
[中国]

Process finished with exit code 0
```

> 源码赏析

```java
    /**
     * Removes the element at the specified position in this list.
     * Shifts any subsequent elements to the left (subtracts one from their
     * indices).
     *
     * @param index the index of the element to be removed
     * @return the element that was removed from the list
     * @throws IndexOutOfBoundsException {@inheritDoc}
     */
    public E remove(int index) {
        // 1. 判断索引是否越界
        rangeCheck(index);
        // 2. 修改次数的计数器
        modCount++;
        // 3. 缓存原有数据
        E oldValue = elementData(index);
				// 4. 计算前移的元素数量
        int numMoved = size - index - 1;
        if (numMoved > 0)
            System.arraycopy(elementData, index+1, elementData, index,
                             numMoved);
        // 5. 赋值为空，GC回收，值得我们学习
        elementData[--size] = null; // clear to let GC do its work
        // 6. 返回旧值
        return oldValue;
    }
```

### 5.源码总结

我们由上面可以总结出，新增和删除方法都会进行数据的拷贝和大量元素的移动，会导致性能不是很高。所以在我们实际的开发中要合理使用数据结构。

- add(index)和add(index, element)这两个方法都可能会导致数组需要扩容，数组长度是固定的，默认初始大小是10个元素，如果不停的往数组里插入数据，可能会导致瞬间数组不停的扩容，影响系统的性能
- set(index)、get(index)可以定位到随机的位置并进行替换或者获取对应元素。这个是比较靠谱的，基于数组来实现随机位置的定位，性能是很高的