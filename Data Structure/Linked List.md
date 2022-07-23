# 链表
## 一、知识点
1. 头指针(head)：不存放具体的数据；作用就是表示单链表的头。
2. 在面试的时候，需要自己手写链表！！！
## 二、题目
* [234. 回文链表](#234-回文链表)

```Java
public class ListNode {
	int val;
	ListNode next;
	ListNode() {}
	ListNode(int val) { this.val = val; }
	ListNode(int val, ListNode next) { this.val = val; this.next = next; }
}

// 707.设计链表
public class MyLinkedList {
	int size;
	ListNode head;
	
	public MyLinkedList() {
		size = 0;
		head = new ListNode(-1);
    }
    
    public int get(int index) {
    	if (index < 0 || index >= size) return -1;
    	ListNode temp = head;
    	for (int i = 0; i <= index; i++) {
    		temp = temp.next;
    	}
    	return temp.val;
    }
    
    public void addAtHead(int val) {
    	addAtIndex(0, val);
    }
    
    public void addAtTail(int val) {
    	addAtIndex(size, val);
    }
    
    public void addAtIndex(int index, int val) {
    	if (index > size) return;  // 学习这一句：虽然是void，但是也可以写return来提前结束。
    	if (index < 0) index = 0;
    	ListNode temp = head;
    	for (int i = 0; i < index; i++) temp = temp.next;
    	ListNode node = new ListNode(val);
    	node.next = temp.next;
    	temp.next = node;
    	size++;
    }
    
    public void deleteAtIndex(int index) {
    	if (index < 0 || index >= size) return;
    	ListNode temp = head;
    	for (int i = 0; i < index; i++) temp = temp.next;
    	temp.next = temp.next.next;
    	size--;
    }
}

// 203.移除链表元素
	/**
	public static ListNode removeElements(ListNode head, int val) {
		if (head == null) return head;
		ListNode dummyNode = new ListNode(-1, head);
		ListNode temp = dummyNode;
		while (temp.next != null) {
			if (temp.next.val == val) {
				temp.next = temp.next.next;
			}else {
				temp = temp.next;
			}
		}
		return dummyNode.next;
    }
    **/
	
	// 206.反转链表
	/**
	public static ListNode reverseList(ListNode head) {
		ListNode cur = head;
		ListNode pre = null; // 初始为null
		// 注意：如果写ListNode pre = new ListNode()；
		// 那么pre是有值的，pre.val = 0， pre.next = null。
		ListNode temp;
		while (cur != null) {
			temp = cur.next;
			cur.next = pre;
			pre = cur;
			cur = temp;
		}
		return pre;
    }
    **/
	
	// 24. 两两交换链表中的节点
	// 虚拟头结点：更换头结点时，并且更换的头结点不是最后。若是最后，解法参考206.翻转链表。
	/**
	public static ListNode swapPairs(ListNode head) {
		// if (head == null) return head;
		ListNode dummyNode = new ListNode(-1, head);
		ListNode cur = head;
		ListNode pre = dummyNode;
		ListNode temp;
		while (cur!= null && cur.next != null) {
			temp = cur.next.next;
			pre.next = cur.next; //更改顺序：从前往后修改。
			cur.next.next = cur;
			cur.next = temp;
			cur = temp;
			pre = pre.next.next;
		}
		return dummyNode.next;
    }
    **/
	
	// 19.删除链表的倒数第N个节点
	/**
	public static ListNode removeNthFromEnd(ListNode head, int n) {
		//if (head.next == null) return null;
		// 首先这里我推荐大家使用虚拟头结点，这样方面处理删除实际头结点的逻辑。
		// 定义fast指针和slow指针，初始值为虚拟头结点。
		// 注意初始值，要是dummyNode而不是head。
		ListNode dummyNode = new ListNode(-1, head);
		ListNode fast = dummyNode;
		ListNode slow = dummyNode;
		for (int i = 0; i < n; i++) fast = fast.next;
		while(fast.next != null) {
			fast = fast.next;
			slow = slow.next;
		}
		slow.next = slow.next.next;
		return dummyNode.next;
    }
    **/
	
	// 面试题 02.07. 链表相交
	/**
	public static ListNode getIntersectionNode(ListNode headA, ListNode headB) {
		int lengthA = 0;
		int lengthB = 0;
		ListNode curA = headA;
		ListNode curB = headB;
		while (curA != null) {
			lengthA++;
			curA = curA.next;
		}
		while (curB != null) {
			lengthB++;
			curB = curB.next;
		}
		curA = headA;
        curB = headB;
		if (lengthA > lengthB) {
			int gap = lengthA - lengthB;
			while (gap-- > 0)	curA = curA.next;
		}else {
			int gap = lengthB - lengthA;
			while (gap-- > 0)	curB = curB.next;
		}
		while (curA != null) {
			if (curA == curB) return curA;
			curA = curA.next;
			curB = curB.next;
		}
		return null;
	}
	**/
	
	// 142.环形链表II
	/**
	 * 解释：为何慢指针第一圈走不完一定会和快指针相遇： 首先，第一步，快指针先进入环 第二步：当慢指针刚到达环的入口时，快指针此时在环中的某个位置(也可能此时相遇) 第三步：设此时快指针和慢指针距离为x，若在第二步相遇，则x = 0； 第四步：设环的周长为n，那么看成快指针追赶慢指针，需要追赶n-x； 第五步：快指针每次都追赶慢指针1个单位，设慢指针速度1/s，快指针2/s，那么追赶需要(n-x)s 第六步：在n-x秒内，慢指针走了n-x单位，因为x>=0，则慢指针走的路程小于等于n，即走不完一圈就和快指针相遇
	**/
	public static ListNode detectCycle(ListNode head) {
		if (head == null || head.next == null) return null;
		ListNode fast = head;
		ListNode slow = head;
		while (fast != null && fast.next != null) {
			fast = fast.next.next;
			slow = slow.next;
			if (fast == slow) {
				ListNode ptr = head;
				while (slow != ptr) {
					slow = slow.next;
					ptr = ptr.next;
				}
				return ptr;
			}
		}
		return null;
    }
	
	// eclipse查看源码：ctrl + shift + T
    
```
### 234. 回文链表
* 要求：给你一个数组 nums，对于其中每个元素 nums[i]，请你统计数组中比它小的所有数字的数目。   
* 举例：Input: nums = [8,1,2,2,3]，Output: [4,0,1,1,3]   
* 难度：简单。有点难。   
* 思路：   
  * 方法一：双层for循环暴力查找。   
  * 方法二：   
    (1) 复制出新数组并排序。  
    (2) 从后往前遍历新数组得到比遍历数字小的数字的数目，并存入哈希表。  
    (3) 遍历原数组，从哈希表中获取结果。  
* 代码：
```Java
    public int[] smallerNumbersThanCurrent(int[] nums) {
        int[] res = Arrays.copyOf(nums, nums.length);
        Arrays.sort(res);
        int[] map = new int[101];
        for (int i = res.length - 1; i >= 0; i--){  // 技巧：从后往前遍历。
            map[res[i]] = i;
        }
        for (int i = 0; i < nums.length; i++){
            res[i] = map[nums[i]];
        }
        return res;
    }
```
