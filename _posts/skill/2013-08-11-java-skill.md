---
layout: post
title: "Java 经验积累"
categories: [skill, java]
---

记录Java的常用技巧

###Top
1. Java中没有逗号运算符
1. `new BufferedReader(new InputStreamReader(new FileInputStream(file), "UTF-8"));`

### 内部匿名类

不用显式地声明，而是直接构造一个类，并生成一个该类的对象，这样类就称为匿名类

匿名类没有定义名字，构造后不能再被调用，所以在构造的时候就生成一个该类的对象

匿名类须要‘基于’其他已有的非final的**类或接口**，如使用`java.util.Comparator`接口构造内部类对象：

    new java.util.Comparator<Integer>(){
        @Override
        public int compare(Integer o1, Integer o2) {
            // TODO Auto-generated method stub
            return 0;
        }
    };

使用`java.lang.Runnable`接口的例子：

    Runnable r = new Runnable(){
        @Override
        public void run() {
            // TODO Auto-generated method stub
        }
    };
    r.run();

### 小心java中的‘引用传递’

最近写递归程序经常要回朔变量的状态，这时候，如果变量不是Immutable的，就一定要注意要构建新对象

    public void tranverse(TreeNode root, int sum, 
        ArrayList<Integer> cur, ArrayList<ArrayList<Integer>> res){
        if(root!=null){
            sum -= root.val;
            cur.add(root.val);
            if(root.left==null && root.right==null && sum==0){
                // find one path
                res.add(new ArrayList<Integer>(cur)); // WARN: remeber to construct new object
            } else {
                tranverse(root.left, sum, cur, res);
                tranverse(root.right, sum, cur, res);
            }
            cur.remove(cur.size()-1); // WARN: 回朔对象状态
        }
    }

### 自动装箱技术和容器类

先看测试代码

    ArrayList<Integer> list = new ArrayList<Integer>();
    list.add(2); // 自动装箱
    // list.remove(2); //出错 
    list.remove(new Integer(2)); // OK

ArrayList中的remove方法有两个重载方法
    
    //Removes the first occurrence of the specified element from this list, if it is present.
    public boolean remove(Object o);

    //Removes the element at the specified position in this list. Shifts any subsequent elements to the left (subtracts one from their indices).
    public E remove(int index);
   
还有一点更像是java的陷阱：

java中有很多final和immutable类(关键是重写了equals方法)，使用ArrayList的remove(object)方法时很容易弄错，还是看测试代码：

    ArrayList<Integer> list = new ArrayList<Integer>();
    list.add(new Integer(2));
    list.add(new Integer(1));
    list.add(new Integer(2));
    list.remove(new Integer(2)); // 假设你想删除刚add进去的2时，结果会令你意外的 ==！
    System.out.println(list); // result: 1 2

测试代码中用的是Integer类，其他primitive类型的封装类也是如此，还有字符串String也有问题

问题的根源在于remove(object)方法是从头遍历，找到第一个和目标对象相同的删掉，其中比较用的对象的equals方法，Integer类的equals方法如下：


    public boolean equals(Object obj) {
        if (obj instanceof Integer) {
            return value == ((Integer)obj).intValue(); //可以看出比较的只是‘数值’
        }
        return false;
    }

equals方法继承于Object类中，在Object类中，equals方法比较的是**两个对象的引用的‘地址’**，当且仅当两个引用只向同一个对象或都为空，该函数返回true

    public boolean equals(Object obj) {
        return (this == obj);
    }

额，又引用了大量源码。。。


###容器排序

**List**

Java中没有排序的List，因为List本来就是有序了，如LinkedList是按照插入顺序排的，ArrayList固定了顺序，如果想改变顺序可以使用collections.sort(...)方法；

如果有动态排序需求，可以使用优先队列PriorityQueue，如果要求容器大小固定，可以使用TreeSet来手动实现，如果还需要元素可以重复，可以考虑使用Guava包中的相关实现

**Map**

* Sort by keys, 使用SortedMap, 如TreeMap
* Sort by values, 只能把map转化成list(`map.entrySet()`)，对list进行排序(`Collections.sort(...)`)，之后再插入到map(如LinkedHashMap,保持插入顺序)中

    // sort reverse map by length of value
    List<Entry<String, Set<String>>> revIdxList = new LinkedList<Entry<String, Set<String>>>(
            revIdxMap.entrySet());
    Collections.sort(revIdxList,
            new Comparator<Entry<String, Set<String>>>() {
                @Override
                public int compare(Entry<String, Set<String>> o1,
                        Entry<String, Set<String>> o2) {
                    Integer size1 = o1.getValue().size();
                    Integer size2 = o2.getValue().size();
                    return size1.compareTo(size2);
                }
            });
    
    // tree map with comparator
    TreeMap<Integer, String> mapper = new TreeMap<Integer, String>(
	    new Comparator<Integer>() {
		@Override
		public int compare(Integer i1, Integer i2) {
		    return i2.compareTo(i1);
		}
	    });

###eclipse中调用java的命令一般是：

    /usr/local/java/jre1.7.0_07/bin/java -Xmx1500m -Dfile.encoding=UTF-8 -classpath /media/Ubuntu/wksp/eclipse_wksp/BaiduReco/bin:/media/Ubuntu/wksp/eclipse_wksp/BaiduReco/lib/commons-collections-3.2.1.jar:/media/Ubuntu/wksp/eclipse_wksp/BaiduReco/lib/commons-configuration-1.7.jar:/media/Ubuntu/wksp/eclipse_wksp/BaiduReco/lib/log4j-1.2.16.jar baidu.zjl.train.ZjlTrain

###传值vs传引用

java中参数传递都可以看成值传递，primivte的参数就不用说了，一般对象引用参数的传递，相当与传递了这个**引用的拷贝** [备忘](http://tjuking.iteye.com/blog/1405532)

    public class Swap {  
      public static void main(String[] args) {  
          ObjectSample o1 = new ObjectSample("hello");  
          ObjectSample o2 = new ObjectSample("ä½ å¥½");  
          System.out.println("before swap o1:"+o1.getTitle()+" o2:"+o2.getTitle());  
          Swap.swapObject(o1, o2);  
          System.out.println("after swap o1:"+o1.getTitle()+" o2:"+o2.getTitle());  
      }  
      static void swapObject(ObjectSample o1, ObjectSample o2){  
          ObjectSample temp = new ObjectSample("temp");  
          temp = o1;  
          o1 = o2;  
          o2 = temp;  
      }  
    }  

    class ObjectSample{  
      private String title;  
        
      ObjectSample(String title){  
          this.title = title;  
      }  
        
      public String getTitle(){  
          return title;  
      }  
    } 

###判断中文字符

    private static final boolean isChinese(char c) {
        Character.UnicodeBlock ub = Character.UnicodeBlock.of(c);
        if (ub == Character.UnicodeBlock.CJK_UNIFIED_IDEOGRAPHS
            || ub == Character.UnicodeBlock.CJK_COMPATIBILITY_IDEOGRAPHS
            || ub == Character.UnicodeBlock.CJK_UNIFIED_IDEOGRAPHS_EXTENSION_A || ub == Character.UnicodeBlock.GENERAL_PUNCTUATION
            || ub == Character.UnicodeBlock.CJK_SYMBOLS_AND_PUNCTUATION
            || ub == Character.UnicodeBlock.HALFWIDTH_AND_FULLWIDTH_FORMS) {
            return true;
        }
        return false;
        }

        public static final boolean containChinese(String strName) {
        char[] ch = strName.toCharArray();
        for (int i = 0; i < ch.length; i++) {
            char c = ch[i];
            if (isChinese(c)) {
            return true;
            }
        }
        return false;
    }
