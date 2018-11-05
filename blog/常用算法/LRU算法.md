<center><h2>LRU算法</h2></center>

#### LRU 算法是什么

LRU 全称 Least Recently Used, 也就是最近最少使用的意思，是一种内存管理的算法。

它是基于一种假设来实现的，即长期不被使用的数据，在未来被用到的几率也不大。

该算法实现的数据结构是 **哈希链表**



##### 哈希链表

首先，哈希表是由若干个key-value所组成的，在“逻辑”上，这些key-value是无所谓的排列顺序所组成的，谁先谁后都一样。

![img](https://mmbiz.qpic.cn/mmbiz_png/NtO5sialJZGpzr3eMdzxP8iawGvibB1dc8uA8sEzFj9LvfDtj8GG0h0d444S9oVGHvw8WSLpceyT1j7kRKT7lpicLw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

在哈希链表当中，这些Key-Value不再是彼此无关的存在，而是被一个链条串了起来。每一个Key-Value都具有它的前驱Key-Value、后继Key-Value，就像双向链表中的节点一样。

![img](https://mmbiz.qpic.cn/mmbiz_png/NtO5sialJZGpzr3eMdzxP8iawGvibB1dc8uOBxcqEUTZce68tYHHC4Mbt6VbZFlGfzKXqoWzxxKhuglf4x2KiaN4Lw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

这样，原本无序的哈希表就拥有固定的排列顺序。



举个例子。

1. 假设当前有一个哈希链表来存储用户信息，假设最长可以存储5个，但是目前已经按顺序存储了4个，存储方法十每来一个新用户，则该哈希链表尾部加1.

2. 这是又来了一个用户，判断这个用户是否在哈希链表中

   1. 在、则将该用户从哈希链表原有位置拿出来，放到该哈希链表的末尾， 即从哈希链表中删除该元素，再放到末尾。
   2. 不在、不在的话直接将新新用户信息放到哈希链表末尾

3. 假设上一个用户不在哈希链表中，则加到末尾，此时，哈希链表已经满了，若再来一个用户，则重复步骤2

   但是，现在还分两种情况

   1. 在，还是直接拿出来，放末尾
   2. 不在，因为此时的链表已经满了，为了能将该用户信息放到哈希链表的末尾，则需要将最开始的一个元素删除掉，即将最不常用的元素给删除掉。



###### 代码实现如下

```java
private Node head;
private Node end;
// 缓存存储上线
private int limit;

private HashMap<String, Node> hashMap;

public LRUCache(int limit) {
    this.limit = limit;
    hashMap = new HashMap<String, Node>();
}

class Node {
    Node(String key, String value) {
        this.key = key;
        this.value = value;
    }
    public Node pre;
    public Node next;
    public String key;
    public String value;
}
public String get(String key) {
    Node node = hashMap.get(key);
    if (node == null) {
        return null;
    }
    refreshNode(node);
    return node.value;
}
public void put(String key, String value) {
    Node node = hashMap.get(key);
    if (node == null) {
        // 如果key不存在，插入key-value
        if (hashMap.size() >= limit) {
            String oldKey = removeNode(head);
            hashMap.remove(oldKey);
        }
        node = new Node(key, value);
        addNode(node);
        hashMap.put(key,value);
    } else {
        // 如果key存在，刷新key-value
        node.value = value;
        refreshNode(node);
    }
}
public void remove(String key) {
    Node node = hashMap.get(key);
    removeNode(node);
    hashMap.remove(key);
}
/**
 * 刷新被访问的节点位置
 * @param  node 被访问的节点
 */
private void refreshNode(Node node) {
    // 如果访问的是尾节点，无需移动节点
    if (node == end) {
        return;
    }
    // 移动节点
    removeNode(node);
    // 重新插入节点
    addNode(node);
}
/**
 * 删除节点
 * @param  node 要删除的节点
 */
private String removeNode(Node node) {
    if (node == end) {
        // 移除尾节点
        end = end.pre;
    } else if (node == head) {
        // 移除头结点
        head = head.next;
    } else {
        // 移除中间节点
        node.pre.next = node.next;
        node.next.pre = node.pre;
    }
    return node.key;
}
/**
 * 尾部插入节点
 * @param  node 要插入的节点
 */
private void addNode (Node node) {
    if (end != null) {
        end.next = node;
        node.pre = end;
        node.next = null;
    }
    end = node;
    if (head == null) {
        head = node;
    }
}

public static void main(String [] args) {
    LEUCache lrcCache = new LRUCache(5);
    lruCache.put("001","用户1信息");
    lruCache.put("002","用户1信息");
    lruCache.put("003","用户1信息");
    lruCache.put("004","用户1信息");
    lruCache.put("005","用户1信息");
    lruCache.get("002");
    lruCache.put("004", "用户2信息更新");
    lruCache.put("006", "用户6信息更新");
    System.out.println(lruCache.get("001"));
    System.out.println(lruCache.get("006"));
}
```



*需要注意的是，这段不是线程安全的，要想做到线程安全，需要加上synchronized修饰符。*