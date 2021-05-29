# ArrayList扩容机制


说明：基于JDK15进行分析

JDK15

```java

//默认容量大小为10
private static final int DEFAULT_CAPACITY = 10;
//空数组
 private static final Object[] EMPTY_ELEMENTDATA = {};
//空数组实例的大小
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
//将要被存储的ArrayList元素的数据缓冲
//The capacity of the ArrayList is the length of this array buffer，即ArrayList的容量就是这个缓冲的长度
//Any empty ArrayList with elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA will be expanded to DEFAULT_CAPACITY when the first element is added.即当一个空数组添加元素时，会将数组的容量扩容到DEFAULT_CAPACITY
transient Object[] elementData;
//the size of the ArrayList (the number of elements it contains).
private int size;

//无参构造方法，创建一个大小为DEFAULTCAPACITY_EMPTY_ELEMENTDATA的数组
public ArrayList() {
        this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
    }
//有参构造方法
public ArrayList(int initialCapacity) {
        if (initialCapacity > 0) {
            this.elementData = new Object[initialCapacity];
        } else if (initialCapacity == 0) {
            this.elementData = EMPTY_ELEMENTDATA;
        } else {
            throw new IllegalArgumentException("Illegal Capacity: "+
                                               initialCapacity);
        }
    }
```

`add`方法

```java
private void add(E e, Object[] elementData, int s) {
        if (s == elementData.length)//如果list的长度恰好等于数组容量-->需要扩容了
            elementData = grow();//将容量进行增长
        elementData[s] = e;//如果小于就将元素添加到数据缓冲中
        size = s + 1;//长度加1
    }

    /**
     * Appends the specified element to the end of this list.在数组末尾添加元素
     *
     * @param e element to be appended to this list
     * @return {@code true} (as specified by {@link Collection#add})
     */
    public boolean add(E e) {
        modCount++;//表示list改变的次数，即添加或删除元素的次数
        add(e, elementData, size);//调用重载方法add
        return true;
    }

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
        rangeCheckForAdd(index);
        modCount++;
        final int s;
        Object[] elementData;
        if ((s = size) == (elementData = this.elementData).length)
            elementData = grow();
        System.arraycopy(elementData, index,
                         elementData, index + 1,
                         s - index);
        elementData[index] = element;
        size = s + 1;
    }
```

```java
private Object[] grow() {
        return grow(size + 1);
    }

/**
     * Increases the capacity to ensure that it can hold at least the
     * number of elements specified by the minimum capacity argument.
     *
     * @param minCapacity the desired minimum capacity
     * @throws OutOfMemoryError if minCapacity is less than zero
     */
private Object[] grow(int minCapacity) {//minCapacity=size + 1
        int oldCapacity = elementData.length;//原容量
        if (oldCapacity > 0 || elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
            int newCapacity = ArraysSupport.newLength(oldCapacity,
                    minCapacity - oldCapacity, /* minimum growth */
                    oldCapacity >> 1           /* preferred growth == 原容量右移一位*/);
            return elementData = Arrays.copyOf(elementData, newCapacity);
        } else {
            return elementData = new Object[Math.max(DEFAULT_CAPACITY, minCapacity)];
        }
    }


/*

Calculates a new array length given an array's current length, a preferred
     * growth value, and a minimum growth value.  If the preferred growth value
     * is less than the minimum growth value, the minimum growth value is used in
     * its place.  If the sum of the current length and the preferred growth
     * value does not exceed {@link #MAX_ARRAY_LENGTH}, that sum is returned.
     * If the sum of the current length and the minimum growth value does not
     * exceed {@code MAX_ARRAY_LENGTH}, then {@code MAX_ARRAY_LENGTH} is returned.
     * If the sum does not overflow an int, then {@code Integer.MAX_VALUE} is
     * returned.  Otherwise, {@code OutOfMemoryError} is thrown.
*/
public static int newLength(int oldLength, int minGrowth, int prefGrowth) {
        // assert oldLength >= 0
        // assert minGrowth > 0
    /*
    假设此时list容量为10，元素为10，要再添加一个元素，
    oldLength=10
    minGrowth=11-10=1
    prefGrowth=10>>1=5
    所以newLength = Math.max(minGrowth, prefGrowth) + oldLength=Math.max(1, 5) + 10=15
    */

        int newLength = Math.max(minGrowth, prefGrowth) + oldLength;//扩容的计算方式
        if (newLength - MAX_ARRAY_LENGTH <= 0) {
            return newLength;
        }
        return hugeLength(oldLength, minGrowth);
    }
```




