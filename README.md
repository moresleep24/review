# JAVA基础
### 操作字符串的类有哪些
String 线程安全  
StringBuilder 线程不安全  
StringBuffer 线程安全，使用synchronized  
StringJoinner 线程不安全  
StringUtils hasText(),hasLength()
### 操作容器的类有哪些
#### List 可重复，有序，可以存多个null  
ArrayList 数组结构，默认大小10，每次扩容1.5倍  
LinkedList 链表结构，可实现队列，先进先出，可实现栈，先进后出  
#### Set 不可重复，无序，只能存一个null  
HashSet HashMap的数据结构，通过HashCode和Equal去重  
LinkedHashSet 链表加HashMap结构  
TreeSet 红黑树结构  
#### Map KV结构，键可以存一个null，值可以存多个null  
HashMap 是一个数组加链表/红黑树的结构，默认有16个桶，负载因子是0.75，当元素数量大于16*0.75时，会将桶的数量扩容为2倍即32个桶  
扩容后，需要移动node到对应的桶中，node中存储了hash值，只需要和11111做与运算，就可以得到下标。  
链表长度>=8时，如果桶的数量>=64，会转为红黑树，当红黑树元素<=6时会转为链表  
put时，先获取hashcode，再取低四位，取值为桶的下标，构造node对象，存储kv值到数组中  
LinkedHashMap HashMap+双向链表，可保证写入顺序  
TreeMap 红黑树结构，按key排序  
Hashtable HashMap方法上加synchronized  
ConCurrentHashMap 线程安全HashMap  
