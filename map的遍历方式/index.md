# Map的遍历方式


![](https://gitee.com/shilongshen/image-bad/raw/master/img/20201227091431.png)

```java
package MapTest;

import java.util.Iterator;
import java.util.Map;
import java.util.Set;
import java.util.TreeMap;

/**
 * ClassName:    Maptest02
 * Package:    Maptest
 * Description: Map集合的遍历
 * Datetime:    2020/10/4   下午9:01
 * Author:   shilongshen
 */
public class Maptest02 {
    public static void main(String[] args) {
        Map<Integer, String> map = new TreeMap<>();
        map.put(1, "hello");
        map.put(2, "world");

        /*
         * 方式1：获取所有的key,通过遍历key来获取value
         * */
        Set<Integer> keys = map.keySet();//KeySet()返回的是一个Set集合
        for (Integer key : keys) {//通过foreach
            String value = map.get(key);
            System.out.println("key=" + key + "   value=" + value);

        }
        System.out.println("--------------");
        Iterator<Integer> it = keys.iterator();
        while (it.hasNext()) {//通过迭代器
            Integer key = it.next();
            String value = map.get(key);
            System.out.println("key=" + key + "   value=" + value);
        }

        System.out.println("--------------");
        /*
         * 方式2：通过entrySet方式来遍历
         * */
       //Set<Map.Entry<Integer, String>> node = map.entrySet();//entrySet()返回一个Set集合，集合元素的类型是Map.Entry<>
        for (Map.Entry<Integer, String> n : map.entrySet()) {
            Integer key = n.getKey();
            String value = n.getValue();
            System.out.println("key=" + key + "   value=" + value);
        }

        System.out.println("--------------");
        Iterator<Map.Entry<Integer, String>> it2= node.iterator();
        while (it2.hasNext()){
            Map.Entry<Integer, String> node2= it2.next();
            System.out.println("key=" + node2.getKey() + "   value=" + node2.getValue());
        }

        System.out.println("----遍历----");
        map.forEach((key,value)-> System.out.println("key="+key+"   value="+value));

    }


}

```

​	




