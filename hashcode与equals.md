越是常见的东西越容易被忽略，常常用到`String`的`hashcode`方法与`equals`方法，但是内部是如何处理的不太清楚，而且大量的文章都提到：<br>
  >要重载hashCode()和equals()两个方法才能实现自定义键在HashMap中的查找，但是为什么要这样以及如果不这样做会产生什么后果,往往语焉不详。<br>
  
[Java 用自定义类型作为HashMap的键](https://segmentfault.com/a/1190000002655085)，把问题产生的来龙去脉讲述的非常清楚。<br>
此外学习String的源码是一个提高姿势水平的最好途径(jkd1.7):
```Java
public int hashCode() {
        int h = hash;
        if (h == 0 && value.length > 0) {
            char val[] = value;

            for (int i = 0; i < value.length; i++) {
                h = 31 * h + val[i];
            }
            hash = h;
        }
        return h;
    }

```

```Java
public boolean equals(Object anObject) {
        if (this == anObject) {
            return true;
        }
        if (anObject instanceof String) {
            String anotherString = (String) anObject;
            int n = value.length;
            if (n == anotherString.value.length) {
                char v1[] = value;
                char v2[] = anotherString.value;
                int i = 0;
                while (n-- != 0) {
                    if (v1[i] != v2[i])
                            return false;
                    i++;
                }
                return true;
            }
        }
        return false;
    }
    
```
 比较好的讲解[Java String类源码分析](https://blog.csdn.net/ylyg050518/article/details/52352993)。
 
