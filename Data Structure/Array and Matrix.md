# 数组
## 一、知识点
### 1. 概述
(1) 数组是存放在**连续内存空间**上的**相同类型**数据的集合。可通过下标索引(**0-based**)获取到下标下对应的数据。

(2) 因为数组的在内存空间的地址是连续的，所以在删除或者增添元素的时候，就难免要**移动其他元素的地址**。

(3) **数组的元素是不能删的，只能覆盖**。(?!)

(4) 数组本身是引用数据类型，它的元素相当于类的成员变量。

### 2. 一维数组
(1) 声明和初始化
```java
// 声明:声明时不能指定长度
int[] a1;
int a[];
String[] c; // 引用类型变量数组
// 初始化
// 静态初始化
int arr[] = new int[]{ 3, 9, 8};
// 动态初始化
String names[] = new String[3];
```
<1> **数组一旦初始化，其长度(arr.length)是不可变的。**

<2> 对于基本数据类型，默认初始化值各有不同(byte, short, int: 0; long: 0L; float: 0.0F; double: 0.0; char: 0 或写为:’\u0000’(表现为空))。对于引用数据类型，默认初始化值为null。
    
### 3. 二维数组

(1) 二维数组的空间地址

**Java是没有指针的，同时也不对程序员暴露其元素的地址，寻址操作完全交给虚拟机。所以看不到每个元素的地址情况。**
```java
public static void test_arr() {
    int[][] arr = {{1, 2, 3}, {3, 4, 5}, {6, 7, 8}, {9,9,9}};
    System.out.println(arr[0]);
    System.out.println(arr[1]);
    System.out.println(arr[2]);
    System.out.println(arr[3]);
}
```
```html
[I@7852e922
[I@4e25154f
[I@70dea4e
[I@5c647e05
```
不是真正的地址，而是经过处理过后的数值。二维数组的每一行头结点的地址是没有规则的，更谈不上连续。

Java的二维数组可能是如下排列的方式：
<div align="center"> <img src="https://img-blog.csdnimg.cn/20201214111631844.png" width="350px"> </div><br>

(2) 二维数组转稀疏数组(sparsearray)

<1> 遍历原始二维数组，得到有效数据的个数N。

<2> 创建稀疏数组 sparseArr int[N + 1][3]。sparseArr[0][0] = row, sparseArr[0][1] = col, sparseArr[0][2] = sum。

<3> 遍历原始二维数组，将有效数据的行、列、值存入稀疏数组。

### 4. 从题目中获取的新语法知识
(1) 数组的复制
```Java
// 1365. 有多少小于当前数字的数字
// 原数组：nums
int[] res = Arrays.copyOf(nums, nums.length);
```

### 5. 技巧
(1) 从后往前遍历 (1365. 有多少小于当前数字的数字)。   
(2) **数组题记得使用双指针！** (941. 有效的山脉数组)。
(3) 旋转数组/字符串的解法：整体翻转 + 局部翻转。(189. 旋转数组)。

## 二、题目
* [1365. 有多少小于当前数字的数字](#1365-有多少小于当前数字的数字)
* [941. 有效的山脉数组](#941-有效的山脉数组)
* [1207. 独一无二的出现次数](#1207-独一无二的出现次数)
* [189. 旋转数组](#189-旋转数组)
* [941. 有效的山脉数组](#941-有效的山脉数组)
* [941. 有效的山脉数组](#941-有效的山脉数组)

```Java
/**
	*
	* @param arr
	* 数组
	* @param left
	* 左边的索引
	* @param right
	* 右边的索引
	* @param findVal
	* 要查找的值
	* @return 如果找到就返回下标，如果没有找到，就返回 -1
	*/
	// 函数的参数注释：在函数上方输入/**然后回车
	// 前提：有序数组
	public static int binarySearch(int[] arr, int left, int right, int findVal) {
		if (left > right) {
			return -1;
		}
		
		
		int mid = (left + right) / 2;
		int midVal = arr[mid];
		
		if (findVal > midVal) { // 向右递归
			return binarySearch(arr, mid + 1, right, findVal);
		} else if (findVal < midVal) { // 向左递归
			return binarySearch(arr, left, mid - 1, findVal);
		} else {
			return mid;
		}
	}
	
	/*
	* 当一个有序数组中，有多个相同的数值时，将所有的数值都查找到
	* 思路分析:
	* 1. 在找到 mid 索引值，不要马上返回
	* 2. 向 mid 索引值的左边扫描，将所有满足 1000， 的元素的下标，加入到集合 ArrayList
	* 3. 向 mid 索引值的右边扫描，将所有满足 1000， 的元素的下标，加入到集合 ArrayList
	* 4. 将 Arraylist 返回
	*/
	public static List<Integer> binarySearch2(int[] arr, int left, int right, int findVal) {
		if (left > right) {
			return new ArrayList<Integer>();
		}
		int mid = (left + right) / 2;
		int midVal = arr[mid];
		
		if (findVal > midVal) { // 向右递归
			return binarySearch2(arr, mid + 1, right, findVal);
		} else if (findVal < midVal) { // 向左递归
			return binarySearch2(arr, left, mid - 1, findVal);
		} else {
			//eclipse中导入包：Ctrl+Shift+M/Ctrl+Shift+O/Ctrl+1/右单击->Source->Organize Imports
			List<Integer> resIndexlist = new ArrayList<Integer>();
			//向 mid 索引值的左边扫描
			int temp = mid - 1;
			while(true) {
				if (temp < 0 || arr[temp] != findVal) {//退出
					break;
				}
				resIndexlist.add(temp);
				temp -= 1; 
			}
			resIndexlist.add(mid);
			//向 mid 索引值的右边扫描
			temp = mid + 1;
			while(true) {
				if (temp > arr.length - 1 || arr[temp] != findVal) {//退出
					break;
				}
				resIndexlist.add(temp);
				temp += 1;
			}
			return resIndexlist;
		}
	}
    
    // 27. 移除元素  (有改进方法吗？)
	/**
	public static int removeElement(int[] nums, int val) {
		int slow = 0;
		for (int fast = 0; fast < nums.length; fast++) {
			if (nums[fast] != val) {
				nums[slow] = nums[fast];
				slow++;
			}
		}
		return slow;
    }
    **/
	
	// 26.删除排序数组中的重复项   (有改进方法吗？)
	/**
	public static int removeDuplicates(int[] nums) {
		// 升序
		int slow = 0;
		for (int fast = 0; fast < nums.length; fast++) {
			if(nums[fast] != nums[slow]) {
				slow ++;
				nums[slow] = nums[fast];
			}
		}
        return slow + 1;
    }
    **/
	
	// 283.移动零
	// int的数值范围
	//内存上还能再改进吗？
	/**
	public static void moveZeroes(int[] nums) {
		int slow = 0;
		for (int fast = 0; fast < nums.length; fast++) {
			if (nums[fast] != 0) {
				nums[slow] = nums[fast];
				if (fast > slow)
					nums[fast] = 0;
				slow++;
			}
		}
    }
    **/
	
	// 844.比较含退格的字符串
	// 整理的时候加注释
	// 方法一：栈
	// 方法二：双指针
	public static boolean backspaceCompare(String s, String t) {
		int skipS = 0, skipT = 0;
		int i = s.length() - 1, j = t.length() - 1;
		char[] S_arr = s.toCharArray(), T_arr = t.toCharArray();
		while (true) {
			while (i >= 0) {
				if (S_arr[i] == '#') skipS++;
				else {
					if (skipS > 0) skipS--;
					else break; // 需要比较该位置是否相等
				}
				i--;
			}
			while (j >= 0) {
				if (T_arr[j] == '#') skipT++;
				else {
					if (skipT > 0) skipT--;
					else break;
				}
				j--;
			}
			if (i == -1 || j == -1) break; // S或T遍历完了
			if (S_arr[i] != T_arr[j]) return false;
			i--; j--;
		}
		if (i == -1 && j == -1) return true; // S和T都遍历完了
		return false;
    }
    
    // 977.有序数组的平方
	/**
	public static int[] sortedSquares(int[] nums) {
		int[] res = new int[nums.length];
		int i = 0, j = nums.length - 1, k = nums.length - 1;
		while (i <= j) {
			if (nums[i] * nums[i] > nums[j] * nums[j]) {
				res[k] = nums[i] * nums[i];
				i++;
			}else {
				res[k] = nums[j] * nums[j];
				j--;
			}
			k--;
		}
		return res;
    }
    **/
	
	// 209.长度最小的子数组
	// 滑动窗口！
	/**
	public static int minSubArrayLen(int target, int[] nums) {
		int left = 0;
		int sum = 0;
		int res = Integer.MAX_VALUE;
		for (int right = 0; right < nums.length; right++) {
			sum += nums[right];
			while (sum >= target) {
				res = Math.min(res, right - left + 1);
				sum -= nums[left++];
			}
		}
		return res == Integer.MAX_VALUE? 0 : res;
    }
    **/
	
	// 904.水果成篮
	// 下面是自己的写法，看一下代码随想录的题解。
	/**
	public static int totalFruit(int[] fruits) {
		if (fruits.length == 1) 
			return 1;
		int[] temp = {fruits[0], -1};
		int left = 0;
		int res = 1;
		int right;
		for (right = 1; right < fruits.length; right++) {
			if (fruits[right] != temp[0] && fruits[right] != temp[1]) {
				res = Math.max(res, right - left);
				int i = right - 2;
				while (i >= left) {
					if (fruits[i] == fruits[right - 1]) i--;
					else{
						left = i + 1;
						break;
						}
				}
				if (fruits[right - 1] == temp[0]) temp[1] = fruits[right];
				else temp[0] = fruits[right];
			}
		}
		return Math.max(res, right - left);
    }
    **/
	
	// 76.最小覆盖子串
	public static String minWindow(String s, String t) {
		char[] s_arr = s.toCharArray();
		char[] t_arr = t.toCharArray();
		if (s_arr.length < t_arr.length) {
			return "";
		}
		int[] win_freq = new int[128];
		int[] t_freq = new int[128];
		
		// t中字符频率统计
		for(char c: t_arr) {
			t_freq[c]++;
		}

		int left = 0;
		int flag = 0;
		int[] res = {Integer.MAX_VALUE,0,0};
		for (int right = 0; right < s.length(); right++) {
			char r = s_arr[right];
			if (win_freq[r] < t_freq[r]) {
				flag++;
			}
			win_freq[r]++;
			while (flag == t_arr.length) {
				char l = s_arr[left];
				if (t_freq[l] == 0) {
					left++;
					continue;
				}
				if (win_freq[l] == t_freq[l]) {
					if(right - left + 1 < res[0]) {
						res[0] = right - left + 1;
						res[1] = left;
						res[2] = right;
					}
					flag--;
				}
				win_freq[l]--;
				left++;
			}
		}
		return res[0] == Integer.MAX_VALUE? "" : s.substring(res[1], res[2] + 1);
    }	
    
    // 35. 寻找插入位置
	/**
	public static int searchInsert(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        int mid;
        while(left <= right){
            mid = left +((right - left) >> 1);
            if(target > nums[mid])
                left = mid + 1;
            else if(target < nums[mid])
                right = mid - 1;
            else
                return mid;
        }
        return left;
    }
    **/
	
	// 2064. Minimized Maximum of Products Distributed to Any Store
	/**
	public static int minimizedMaximum(int n, int[] quantities) {
		int left = 1;
		int right = quantities[0];
		for(int quantity : quantities) {
			if(right < quantity) 
				right = quantity;
		}
		right += 1;
		int k;
		
		while(left < right) {
			k = left + ((right - left) >> 1);
			boolean can = canDistribute(quantities, n, k);
			if(can) {
				right = k;
			}else{
				left = k + 1;
			}
		}	
		return right;
	}
	
	 private static boolean canDistribute(int[] quantities, int n, int mid) {
		 int rest = n;
		 for (int quantity : quantities) {
			 rest -= quantity / mid + (quantity % mid == 0 ? 0 : 1);
			 if(rest < 0)
				 return false;
		 }
		 return true;
	 }
	 **/
	
	// 34.在排序数组中查找元素的第一个和最后一个位置
	/**
	public static int[] searchRange(int[] nums, int target) {
		int[] res = {-1,-1};
		int left = 0;
		int right = nums.length;
		int mid;
		while(left < right) {
			mid = left + ((right - left) >> 1);
			if(target < nums[mid]) {
				right = mid;
			}else if(target > nums[mid]) {
				left = mid + 1;
			}else {
				int cnt = mid;
				while(cnt >= left) {
					if(nums[cnt] != target)
						break;
					cnt -= 1;
				}
				res[0] = cnt + 1;
				cnt = mid;
				while(cnt < right) {
					if(nums[cnt] != target)
						break;	
					cnt += 1;
				}
				res[1] = cnt - 1;
				return res;
			}
		}
		return res;
    }
    **/
	
	// 69.x 的平方根
	/**
	public static int mySqrt(int x) {
		long left = 0;
		long right = (long)x + 1;  // 注意数值的大小
		long mid;
		while(left < right) {
			mid = left + ((right - left) >> 1);
			if(x < mid * mid) {
				right = mid;
			}else {
				left = mid + 1;
			}
		}
		return (int)right - 1;
    }
    **/
	
	// 367.有效的完全平方数
	public static boolean isPerfectSquare(int num) {
		long left = 0;
		long right = (long)num + 1;
		long mid;
		while(left < right) {
			mid = left + ((right - left) >> 1);
			if (num > mid * mid)
				left = mid + 1;
			else if (num < mid * mid)
				right = mid;
			else
				return true;
		}
		return false;
    }
    
    // 59.螺旋矩阵II
	/**
	public static int[][] generateMatrix(int n) {
		int[][] res = new int[n][n];
		int num = 1;
		int start = 0;
		int end = n - 1;
		int i,j;
		int loop = n / 2;
		while (loop > 0) {
			// 从左到右
			for (j = start; j < end; j++) {
				res[start][j] = num++;
			}
			
			// 从上到下
			for (i = start; i < end; i++) {
				res[i][end] = num++;
			}
			
			// 从右到左
			for (; j > start; j--) {
				res[end][j] = num++;
			}
			
			//从下到上
			for (; i > start; i--) {
				res[i][start] = num++;
			}
			
			loop--;
			start++;
			end--;
		}
		if (n % 2 == 1) res[start][start] = num;
		return res;
    }
    **/
	
	/**
	public static List<Integer> spiralOrder(int[][] matrix) {
		ArrayList<Integer> res = new ArrayList<Integer>();
		int start = 0;
		int m = matrix.length;
		int n = matrix[0].length;
		int endm = m - 1;
		int endn = n - 1;
		int i,j,loop;
		if (m <= n) loop = m >> 1;
		else loop = n >> 1;
		while (loop > 0) {
			// 从左到右
			for (j = start; j < endn; j++) {
				res.add(matrix[start][j]);
			}
						
			// 从上到下
			for (i = start; i < endm; i++) {
				res.add(matrix[i][endn]);
			}
						
			// 从右到左
			for (; j > start; j--) {
				res.add(matrix[endm][j]);
			}
						
			//从下到上
			for (; i > start; i--) {
				res.add(matrix[i][start]);
			}		
			loop--;
			start++;
			endm--;
			endn--;
		}
		if (start == endm && start <= endn) {
			for (j = start; j <= endn; j++) {
				res.add(matrix[start][j]);
			}
		}
		if (start == endn && start < endm) {
			for (i = start; i <= endm; i++) {
				res.add(matrix[i][endn]);
			}
		}
		return res;
    }
    **/
	
	public static int[] spiralOrder(int[][] matrix) {
		int start = 0;
		int m = matrix.length;
		if (m == 0) return new int[0];
		int n = matrix[0].length;
		int[] res = new int[m * n];
		int endm = m - 1;
		int endn = n - 1;
		int i,j,loop,index = 0;
		if (m <= n) loop = m >> 1;
		else loop = n >> 1;
		while (loop > 0) {
			// 从左到右
			for (j = start; j < endn; j++) res[index++] = matrix[start][j];
						
			// 从上到下
			for (i = start; i < endm; i++) res[index++] = matrix[i][endn];
						
			// 从右到左
			for (; j > start; j--) res[index++] = matrix[endm][j];
						
			//从下到上
			for (; i > start; i--) res[index++] = matrix[i][start];
					
			loop--;
			start++;
			endm--;
			endn--;
		}
		if (start == endm && start <= endn) {
			for (j = start; j <= endn; j++) res[index++] = matrix[start][j];
		}
		if (start == endn && start < endm) {
			for (i = start; i <= endm; i++) res[index++] = matrix[i][endn];
		}
		return res;
    }
```

### 1365. 有多少小于当前数字的数字
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
### 941. 有效的山脉数组
* 要求：给定一个整数数组 arr，如果它是有效的山脉数组就返回 true，否则返回 false。  
* 举例：Input: nums = [1,2,3,4]，Output: false  
* 难度：简单。简单，但有要注意的点。  
* 思路：   
  * 双指针：要注意左或右指针没移动的情况！！   
* 代码：
```Java
    // 自己写的。慢。
    public boolean validMountainArray(int[] arr) {
        if (arr.length < 3) return false;
        boolean reachTop = false;
        int top = arr[0];
        for (int i = 1; i < arr.length; i++){
	    if (arr[i] == arr[i - 1]) return false;
	    if (!reachTop && arr[i] < arr[i - 1]){
		reachTop = true;
		top = arr[i - 1];
	    }
	    if (reachTop && arr[i] > arr[i - 1]) return false;
        }
        return top != arr[0] && top != arr[arr.length - 1];
    }

    // 代码随想录：双指针。快。
    public boolean validMountainArray(int[] arr) {
        if (arr.length < 3) return false;
        int left = 0;
        int right = arr.length - 1;
        while (left < arr.length - 1 && arr[left] < arr[left + 1]){
            left++;
        }
        while (right > 0 && arr[right] < arr[right - 1]){
            right--;
        }
        if (left == right && left != 0 && right != arr.length - 1){
            return true;
        }
        return false;
    }
```
### 1207. 独一无二的出现次数
* 要求：给你一个整数数组 arr，请你帮忙统计数组中每个数的出现次数，并判断出现次数是否不重复。  
* 举例：Input: arr = [1,2,2,1,1,3]，Output: true  
* 难度：简单。简单。  
* 思路：两个哈希表。  
* 代码：
```Java
    public boolean uniqueOccurrences(int[] arr) {
        int[] freq = new int[2001];
        for (int i = 0; i < arr.length; i++){
            freq[arr[i] + 1000]++;
        }
        boolean[] exist = new boolean[1001];
        for (int i = 0; i < 2001; i++){
            if (freq[i] > 0){
                if (exist[freq[i]] == true) return false;
                else exist[freq[i]] = true; 
            }
        }
        return true;
    }
```
### 189. 旋转数组
* 要求：给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。    
* 举例：Input: nums = [1,2,3,4,5,6,7], k = 3; Output: [5,6,7,1,2,3,4]   
* 难度：中等。 简单。   
* 思路：整体翻转 + 局部翻转。
* 类似题目：[剑指Offer58-II. 左旋转字符串](#剑指Offer58-II.-左旋转字符串)  
* 代码：
```Java
    public void rotate(int[] nums, int k) {
        k %= nums.length;
        reverse(nums, 0, nums.length - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k, nums.length - 1);
    }

    public void reverse(int[] nums, int left, int right){
        int temp;
        while (left < right){
            temp = nums[left];
            nums[left] = nums[right];
            nums[right] = temp;
            left++;
            right--;
        }
    }
```
