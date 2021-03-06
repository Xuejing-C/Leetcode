# Stack and Queue
## 一、基础知识
## 二、题目
```Java
// 225. 用队列实现栈
public class MyStack {
	Deque<Integer> que;
	public MyStack() {
		que = new ArrayDeque<>();
    }
    
    public void push(int x) {
    	que.addLast(x);
    }
    
    public int pop() {
    	for (int i = 1; i < que.size(); i++) {
    		que.addLast(que.pollFirst());
    	}
    	return que.pollFirst();
    }
    
    public int top() {
    	int res = pop();
    	que.addLast(res);
    	return res;
    }
    
    public boolean empty() {
    	return que.isEmpty();
    }
}

import java.util.Stack;

//232.用栈实现队列
public class MyQueue {
	Stack<Integer> stackIn;
	Stack<Integer> stackOut;
	public MyQueue() {
		 stackIn = new Stack<>();
		 stackOut = new Stack<>();
	}
	    
    public void push(int x) {
    	stackIn.push(x);
    }
    
    public int pop() {
    	if (stackOut.isEmpty()) {
	    	while (!stackIn.isEmpty()) {
	    		stackOut.push(stackIn.pop());
	    	}
    	}
    	return stackOut.pop();
    }
    
    public int peek() {
    	int res = pop();
    	stackOut.push(res);
    	return res;
    }
    
    public boolean empty() {
    	return stackIn.isEmpty() && stackOut.isEmpty();
    }
}


// 20. 有效的括号
	/*
	public boolean isValid(String s) {
		Stack<Character> stk = new Stack<>();
		for (int i = 0; i < s.length(); i++) {
			char c = s.charAt(i);
			if (c == '(') {
				stk.push(')');
			}else if (c == '{') {
				stk.push('}');
			}else if (c == '[') {
				stk.push(']');
			}else if (stk.isEmpty() || c != stk.peek()) {
				return false;
			}else {
				stk.pop();
			}
		}
        return stk.isEmpty();
    }
    */
	
	// 1047. 删除字符串中的所有相邻重复项
	// 方法一：双指针
	/*
	public String removeDuplicates(String s) {
		char[] s_arr = s.toCharArray();
		int slow = -1;
		for (int fast = 0; fast < s_arr.length; fast++) {
			if (slow == -1 || s_arr[fast] != s_arr[slow]) {
				slow++;
				s_arr[slow] = s_arr[fast];
			}else {
				slow--;
			}
		}
		return new String(s_arr, 0 ,slow + 1);
    }
    */
	// 方法二：StringBuffer或者Deque
	
	// 150. 逆波兰表达式求值
	// 7ms很慢，整理时看leetcode上1/2毫秒的写法。
	/*
	public int evalRPN(String[] tokens) {
		Stack<Integer> stk = new Stack<>();
		for (String s : tokens) {
			if (s.equals("+") || s.equals("-") || s.equals("*") || s.equals("/")) {
				int num1 = stk.pop();
				int num2 = stk.pop();
				if (s.equals("+")) stk.push(num2 + num1);
				if (s.equals("-")) stk.push(num2 - num1);
				if (s.equals("*")) stk.push(num2 * num1);
				if (s.equals("/")) stk.push(num2 / num1);
			}else {
				stk.push(Integer.parseInt(s));
			}
		}
		return stk.pop();
    }
    */
	
	// 239. 滑动窗口最大值
	// dq.peek()后面的元素都比它小且比它晚进入。
	/*
	public int[] maxSlidingWindow(int[] nums, int k) {
		if (nums.length == 1) return nums;
		int len = nums.length - k + 1;
		int[] res = new int[len];
		int index = 0;
		myQueue dq  =  new myQueue();
		for (int i = 0; i < k; i++) {
			dq.push(nums[i]);
		}
		res[index++] = dq.peek();
		for (int i = k; i < nums.length; i++) {
			dq.pop(nums[i - k]);
			dq.push(nums[i]);
			res[index++] = dq.peek();
		}
        return res;
    }
	*/
	
	// 347.前 K 个高频元素
	/*
	public int[] topKFrequent(int[] nums, int k) {
		int[] res = new int[k];		
		HashMap<Integer, Integer> map = new HashMap<>();
		for (int num : nums) {
			map.put(num, map.getOrDefault(num, 0) + 1);
		}
		
		Set<Map.Entry<Integer, Integer>> entries = map.entrySet();
		PriorityQueue<Map.Entry<Integer, Integer>> queue = new PriorityQueue<>((o1,o2) -> o1.getValue() - o2.getValue());
		for (Map.Entry<Integer, Integer> entry : entries) {
			queue.offer(entry);
			if (queue.size() > k) {
				queue.poll();
			}
		}
		for (int i = k - 1; i >= 0; i--) {
			res[i] = queue.poll().getKey();
		}
		return res;
    }
	*/
	
	// 71. 简化路径
	public static String simplifyPath(String path) {
		String[] p = path.split("/");
		Deque<String> dq = new ArrayDeque<>();
		for (String str : p) {
			if (".".equals(str) || str.length() == 0) continue;
			if (("..").equals(str)) {
				if (!dq.isEmpty()) dq.pollLast();
				continue;
			}
			dq.offerLast(str);
		}
		StringBuffer res = new StringBuffer();
		if (dq.isEmpty()) res.append("/");
		else {
			while (!dq.isEmpty()) {
				res.append("/");
				res.append(dq.pollFirst());
			}
		}
        return res.toString();
    }
    
    /*
class myQueue {
	Deque<Integer> dq = new LinkedList<>();
	
	void pop(int val) {
		if (!dq.isEmpty() && dq.peek() == val) {
			dq.poll();
		}
	}
	
	void push(int val) {
		while (!dq.isEmpty() && val > dq.getLast()) {
			dq.removeLast();
		}
		dq.add(val);
	}
	
	int peek() {
		return dq.peek();
	}
}
*/
```

## 三、单调栈
```Java
// 739. 每日温度
	public static int[] dailyTemperatures(int[] temperatures) {
        int[] res = new int[temperatures.length];
        Deque<Integer> stk = new LinkedList<>();
        stk.push(0);
        for (int i = 1; i < temperatures.length; i++){
            if (temperatures[i] <= temperatures[stk.peek()]){
                stk.push(i);
            }else{
            	while (!stk.isEmpty() && temperatures[i] > temperatures[stk.peek()]) {
	                int j = stk.pop();
	                res[j] = i - j;
            	}
            	stk.push(i);
            }
        }
        return res;
    }
	
	
	// 496.下一个更大元素 I
	public static int[] nextGreaterElement(int[] nums1, int[] nums2) {
        int[] res = new int[nums1.length];
        Arrays.fill(res, -1);
        HashMap<Integer, Integer> hashMap = new HashMap<>();
        for (int i = 0; i < nums1.length; i++){
            hashMap.put(nums1[i], i);
        }
        Deque<Integer> stk = new LinkedList<>();
        stk.push(0);
        for (int i = 1; i < nums2.length; i++){
            if (nums2[i] <= nums2[stk.peek()]){
                stk.push(i);
            }else{
                while (!stk.isEmpty() && nums2[i] > nums2[stk.peek()]){
                    int j = stk.pop();
                    if (hashMap.containsKey(nums2[j])){
                        int index = hashMap.get(nums2[j]);
                        res[index] = nums2[i];
                    }
                }
                stk.push(i);
            }
        }
        return res;
    }
	
	
	// 503.下一个更大元素II
	public int[] nextGreaterElements(int[] nums) {
        int len = nums.length;
        int[] res = new int[len];
        Arrays.fill(res, -1);
        Deque<Integer> stk = new LinkedList<>();
        stk.push(0);
        for (int i = 1; i < 2 * len; i++){
            while (!stk.isEmpty() && nums[i % len] > nums[stk.peek()]){
                int j = stk.pop();
                res[j % len] = nums[i % len]; 
            }
            stk.push(i % len);
        }
        return res;
    }
	
	
	// 42. 接雨水
	// 双指针法
	// 动态规划
	// 单调栈
	public int trap(int[] height) {
        if (height.length <= 2) return 0;
        int res = 0;
        Deque<Integer> stk = new LinkedList<>();
        stk.push(0);
        for (int i = 1; i < height.length; i++){
            if (height[i] < height[stk.peek()]){
                stk.push(i);
            }else if (height[i] == height[stk.peek()]){
                stk.pop();
                stk.push(i);
            }else{
                while (!stk.isEmpty() && (height[i] > height[stk.peek()])){
                    int mid = stk.pop();
                    if(!stk.isEmpty()){
                        int h = Math.min(height[stk.peek()], height[i]) - height[mid];
                        int w = i - stk.peek() - 1;
                        res += h * w;
                    }
                }
                stk.push(i);
            }
        }
        return res;
    }
	
	
	// 84.柱状图中最大的矩形
	// 动态规划
	// 单调栈
	public static int largestRectangleArea(int[] heights) {
        int[] newHeights = new int[heights.length + 2];
        for (int i = 0; i < heights.length; i++){
            newHeights[i + 1] = heights[i];
        }
        Deque<Integer> stk = new LinkedList<>();
        stk.push(0);
        int res = 0;
        for (int i = 1; i < newHeights.length; i++){
            if (newHeights[i] > newHeights[stk.peek()]){
                stk.push(i);
            }else if (newHeights[i] == newHeights[stk.peek()]){
                stk.pop();
                stk.push(i);
            }else{
                while(newHeights[i] < newHeights[stk.peek()]){
                    int mid = stk.pop();
                    int w = i - stk.peek() - 1;
                    int h = newHeights[mid];
                    res = Math.max(res, w * h);
                }
                stk.push(i);
            }
        }
        return res;
    }
```
