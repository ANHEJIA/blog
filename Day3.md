# Day 3

## 203. 移除链表元素

利用好现有的构造方法！

```Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode removeElements(ListNode head, int val) {
        // 利用此构造方法
        ListNode dummy = new ListNode(-1, head);
        ListNode cur = dummy;
        while (cur.next != null) {
            // 链表节点的值与目标值相等
            if (cur.next.val == val) {
                // 将指针指向下下个位置，即跳过下一个
                cur.next = cur.next.next;
            } else {
                cur = cur.next;
            }
        }
        // 返回新链表的头节点
        return dummy.next;
    }
}
```

## 707. 设计链表

感受虚拟头节点的作用！这里选择单链表。

在链表类中实现这些功能：

- get(index)：获取链表中第 index 个节点的值。如果索引无效，则返回-1。
- addAtHead(val)：在链表的第一个元素之前添加一个值为 val 的节点。插入后，新节点将成为链表的第一个节点。
- addAtTail(val)：将值为 val 的节点追加到链表的最后一个元素。
- addAtIndex(index,val)：在链表中的第 index 个节点之前添加值为 val 的节点。如果 index 等于链表的长度，则该节点将附加到链表的末尾。如果 index 大于链表长度，则不会插入节点。如果index小于0，则在头部插入节点。
- deleteAtIndex(index)：如果索引 index 有效，则删除链表中的第 index 个节点。

```Java
// 单链表
class ListNode {
    int val;
    ListNode next;

    ListNode() {
    }

    ListNode(int val) {
        this.val = val;
    }
}

class MyLinkedList {
    // size存储链表元素的个数
    int size;
    // 虚拟头结点
    ListNode head;

    // 初始化链表
    public MyLinkedList() {
        size = 0;
        head = new ListNode(0);
    }

    // 1. 获取第index个节点的数值，注意index是从0开始的，第0个节点就是头结点
    public int get(int index) {
        // 如果index非法，返回-1
        if (index < 0 || index >= size) {
            return -1;
        }
        ListNode currentNode = head;
        // 包含一个虚拟头节点，所以查找第index + 1个节点
        for (int i = 0; i <= index; i++) {
            currentNode = currentNode.next;
        }
        return currentNode.val;
    }

    // 2. 在链表最前面插入一个节点，等价于在第0个元素前添加。借助函数4
    public void addAtHead(int val) {
        addAtIndex(0, val);
    }

    // 3. 在链表的最后插入一个节点，等价于在(末尾+1)个元素前添加。借助函数4
    public void addAtTail(int val) {
        addAtIndex(size, val);
    }

    // 4. 在第 index 个节点之前插入一个新节点
    public void addAtIndex(int index, int val) {
        // 如果 index 大于链表长度，则不会插入节点
      	if (index > size) {
            return;
        }
        // 如果 index 大于链表的长度，则返回空
        if (index < 0) {
            index = 0;
        }
        size++;
        // 找到要插入节点的前驱
        ListNode pred = head;
        for (int i = 0; i < index; i++) {
            pred = pred.next;
        }
        ListNode toAdd = new ListNode(val);
        toAdd.next = pred.next;
        pred.next = toAdd;
    }

    // 5. 删除第index个节点
    public void deleteAtIndex(int index) {
        if (index < 0 || index >= size) {
            return;
        }
        size--;
        if (index == 0) {
            head = head.next;
            return;
        }
        ListNode pred = head;
        for (int i = 0; i < index; i++) {
            pred = pred.next;
        }
        pred.next = pred.next.next;
    }
}

```



## 206. 反转链表

经典例题，必须拿下。

```Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        while (cur != null) {
            // 记录下一个节点
            ListNode next = cur.next;
            // 交换方向
            cur.next = pre;
            // 下一位
            pre = cur;
            cur = next;
        }
        // 返回新的头节点
        return pre;
    }
}
```

如果白板写，主方法也要会写！写一下测试用例。

```Java
public class Main {
    public static void main(String[] args) {
        // 创建链表 1 -> 2 -> 3 -> 4 -> 5
        ListNode head = new ListNode(1);
        head.next = new ListNode(2);
        head.next.next = new ListNode(3);
        head.next.next.next = new ListNode(4);
        head.next.next.next.next = new ListNode(5);
        
        // 打印原链表
        System.out.println("Original List:");
        printList(head);
        
        // 反转链表
        Solution solution = new Solution();
        ListNode reversedHead = solution.reverseList(head);
        
        // 打印反转后的链表
        System.out.println("Reversed List:");
        printList(reversedHead);
    }
    
    // 辅助函数：打印链表
    public static void printList(ListNode head) {
        ListNode current = head;
        while (current != null) {
            System.out.print(current.val + " ");
            current = current.next;
        }
        System.out.println();
    }
}
```

