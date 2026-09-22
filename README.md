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
### Synchronized和ReentrantLock区别
Sync是关键字，Lock是类  
Sync自动加锁释放锁，Lock手动加锁释放锁，lock，unlock方法  
Sync只支持非公平锁，Lock支持公平锁和非公平锁，构造器中设值TRUE，表示公平锁，使用AQS实现，包含state和queue两部分，queue为空，尝试获取锁，非空添加到queue中  
Sync阻塞时无法中断，Lock阻塞时lockInterruptibly()可中断锁，也可以设置超时时间  
Sync不能跨方法使用, Lock可以跨方法使用  
### Spring循环依赖
A依赖B，B依赖A的情况属于循环依赖  
Setter注入类型使用三级缓存的设计处理，三级缓存是三个map，一级缓存存储完整的bean，二级缓存存储未注入属性的bean，三级缓存存储带引用的ObjectFactory  
创建A对象后放入三级缓存中，注入B对象，依次查询三级缓存，没有找到B对象，创建B对象  
创建B对象后放入三级缓存中，注入A对象，依次查询三级缓存，第三级缓存缓存中有A对象，移除第三级缓存中的A对象，放入第二级缓存中，B对象创建完毕，放入一级缓存  
完成A对象中B对象的注入工作，查询到第三级缓存中的B对象，移除第三级缓存中的B对象，放入第二级缓存中，A对象创建完毕，放入一级缓存  
之所以，移除三级缓存，放入二级缓存，是要保证后续引入A对象或B对象，是同一对象。  
Construcutor注入类型可以使用@Lazy注解，在创建对象时会先放置一个代理对象，直到真正调用注入对象时，会通过代理对象去调用方法
可以将AB对象中公共的逻辑抽取到C对象中，AB对象依次引入C对象，可解决循环依赖
### ACID
原子性：全成功或全失败  
一致性：数据必须满足规则和约束  
隔离性：多个事务执行互不影响  
持久性：数据可以永久存储  
### 四大隔离级别
脏读：读到了还没提交的数据
不可重复读：一个事务中先后两次读取已更新的数据，读到的两次数据不一致  
幻读： 一个事务中先后两次读取已新增或删除的数据，读到的数据缺失或多余  
读未提交：NONE  
读已提交：解决脏读  
可重复读：解决脏读，不可重复读  
串行化：解决脏读，不可重复读，幻读  
