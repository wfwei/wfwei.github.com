---
layout: post
title: "Java备忘"
categories: [programming, java]
---
### Misc
1. Java中没有逗号运算符
   
###容器排序

List
    * Java中没有排序的List，因为List本来就是有序了，如LinkedList是按照插入顺序排的，ArrayList固定了顺序，如果想改变顺序可以使用collections.sort(...)方法；
    * 如果有动态排序需求，可以使用优先队列PriorityQueue，如果要求容器大小固定，可以使用TreeSet来手动实现，如果还需要元素可以重复，可以考虑死用Guava包中的相关实现

Map
    * Sort by keys, 使用SortedMap, 如TreeMap
    * Sort by values, 只能把map转化成list(`map.entrySet()`)，对list进行排序(`Collections.sort(...)`)，之后再插入到map(如LinkedHashMap,保持插入顺序)中

Gists
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

###Java IO

Java读写文件如果想设置编码，就要使用Stream相关的读写类

    `new BufferedReader(new InputStreamReader(new FileInputStream(file), "UTF-8"));

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
            || ub == Character.UnicodeBlock.CJK_UNIFIED_IDEOGRAPHS_EXTENSION_A
            || ub == Character.UnicodeBlock.GENERAL_PUNCTUATION
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
 


