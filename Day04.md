# Day 4

## 24. 两两交换链表中的节点

递归写法

```Java
class Solution {
    public ListNode swapPairs(ListNode head) {
        // 节点数量小于2个，不满足交换条件，返回
        if (head == null || head.next == null) {
            return head;
        }
        // 标记第二个节点作为新的头节点
        ListNode newHead = head.next;
        // 对第三个节点开始的链表递归
        head.next = swapPairs(head.next.next);
        // 原来的第二个节点和头节点交换位置，成为新的头节点
        newHead.next = head;
        return newHead;
    }
}
```

## 19. 删除链表的倒数第N个节点

先让快指针走N步，然后一起移动。这样就能定位到倒数第n个节点。

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
    public  ListNode removeNthFromEnd(ListNode head, int n) {
        // n为0, 直接返回head
        if (n == 0) {
            return head;
        }
        // 定义pre指向head节点
        ListNode pre = new ListNode(-1);
        // 定义快慢指针
        ListNode fast = head;
        ListNode slow = pre;
        slow.next=head;
        // 快指针先移动n步
        while (n!=0) {
            fast = fast.next;
            n--;
        }
        // n步后，快慢指针一起移动
        while (fast != null) {
            fast = fast.next;
            slow = slow.next;
        }
        // 快指针遍历到末尾，慢指针需跳过下一个节点
        slow.next = slow.next.next;
        return pre.next;
    }
}
```

## 160. 相交链表

用Hashset存储A中所有节点，然后和B对比。

```Java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) {
 *         val = x;
 *         next = null;
 *     }
 * }
 */
public class Solution {
    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
        Set<ListNode> visited = new HashSet<ListNode>();
        // 把A链表出现过的节点，都存起来
        ListNode temp = headA;
        while (temp != null) {
            visited.add(temp);
            temp = temp.next;
        }
        // 一一对比B链表中的节点
        temp = headB;
        while (temp != null) {
            if (visited.contains(temp)) {
                return temp;
            }
            temp = temp.next;
        }
        return null;
    }
}
```

指针方法待补充。

## 142. 环形链表

用Hashset存储，比对有没有出现过。

```Java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        // 记录当前节点
        ListNode pos = head;
        // 记录遍历过的节点
        Set<ListNode> visited = new HashSet<ListNode>();
        while (pos != null) {
            if (visited.contains(pos)) {
                return pos;
            } else {
                visited.add(pos);
            }
            pos = pos.next;
        }
        return null;
    }
}
```

指针方法待补充。
