#剑指offer刷题  
***
##1、斐波那契数列  
###*1）递归法  

###*2）迭代法  
    class Solution {  
	　　public int fib(int n) {
	　　　int a = 0;
	　　　int b = 1;
	　　　int sum;
	　　　for(int i=0;i<n;i++){
	　　　　　sum = (a+b)%1000000007;
	　　　　　a = b;
	　　　　　b = sum; 
	　　　}
	　　　return a;
	　　}
    }
###*3）动态规划  


###大佬算法(动态规划）：
  

	class Solution {
	　　public int fib(int n) {
	　　　　int[] dp = new int[2];
	　　　　dp[0]=0;
	　　　　dp[1]=1;
	　　　　for(int i=2;i<=n;i++){
	　　　　　　dp[i%2]=（dp[0]+dp[1]）%1000000007; 
	　　　　}
	　　　　return dp[n%2];
	　　}
	}
##2、青蛙跳台阶问题
###*1）动态规划

	class Solution {
	　　public int numWays(int n) {
	　　　　int[] a= new int[n+3];
	　　　　a[0]=1;
	　　　　a[1]=1;
	　　　　a[2]=2;
	　　　　for(int i=3;i<=n;i++){
	　　　　　　a[i]=(a[i-1]+a[i-2])%1000000007;
	　　　　}
	　　　　return a[n];
	　　}
	}
##3、翻转单词顺序
###1)层层建数组(前期思路未完成)
###2)切分+拼接
###3）双指针　　
###方法一：前期思路：（未完成）
>####遍历每个字符，将每个空格的索引存入数组，遍历索引数组获取相邻索引值间的字串，存入另一子串数组，遍历字串数组，倒序输出子串拼接为字符串。  
  
    class Solution {
	    public String reverseWords(String s) {
	        String res="";
	        String res1="";
	        ArrayList<Integer> arr = new ArrayList<Integer>();
	        ArrayList resArr = new ArrayList();
	        if( !String.valueOf(s.charAt(0)).equals(" ")){
	                arr.add(0);
	        }
	        //遍历查找空格索引
	        for(int i=0;i<s.length();i++){
	            if( String.valueOf(s.charAt(i)).equals(" ")){
	                arr.add(i);
	            }
	        }
	        if( !String.valueOf(s.charAt(s.length()-1)).equals(" ")){
	                arr.add(s.length());
	        }
	        //根据索引数组切分字符串，并将子串存入子串数组
	        for(int n=1;n<arr.size();n++){ 
	            if((arr.get(n).intValue())-(arr.get(n-1).intValue())>1){
	                String substr = s.substring(arr.get(n-1).intValue(),arr.get(n).intValue());
	                resArr.add(substr);
	            }
	        }
	        //倒序拼接子串数组
	        for(int m=0;m<resArr.size()-1;m++){
	            res += resArr.get(resArr.size()-m-1).toString();
	        }
	        //添加空格细节
	        if(resArr.get(0).toString().startsWith(" ")){
	            res1=res+resArr.get(0).toString();
	        }else{
	            res1=res+" "+resArr.get(0).toString();
	        }       
	        String res2=res1.substring(1);
	        return res2;
	    }
    }

#####缺点
>####遍历次数过多，需要考虑的细节太多，比如存在连续空格时，需要额外判断。 

###方法二：改进思路
>###直接split以空格切分字符串，非空字符串倒序输出并拼接。  

	class Solution {
	    public String reverseWords(String s) {
	        String res;
	        StringBuffer res1 = new StringBuffer();
	         if(s.equals("")){
	            return "";
	        }//判断输入为空的情况	 
	        String[] strArr = s.split("\\s+");
	        if(strArr.length==0){
	            return "";
	        }判断输入均为空格的情况	
	        for(int i=strArr.length-1;i>=0;i--){
	           if(!strArr[i].equals("")){
	               res1.append(strArr[i]+" ");
	           }
	        }//倒序遍历，剔除空字符
	        res = res1.toString().substring(0,res1.toString().length()-1);
	        return res;	
	    }
	}

###方法三：大佬思路（双指针）  

	class Solution {
	    public String reverseWords(String s) {
	        s = s.trim(); // 删除首尾空格
	        int j = s.length() - 1, i = j;
	        StringBuilder res = new StringBuilder();
	        while(i >= 0) {
	            while(i >= 0 && s.charAt(i) != ' ') i--; // 搜索首个空格
	            res.append(s.substring(i + 1, j + 1) + " "); // 添加单词
	            while(i >= 0 && s.charAt(i) == ' ') i--; // 跳过单词间空格
	            j = i; // j 指向下个单词的尾字符
	        }
	        return res.toString().trim(); // 转化为字符串并返回
	    }
	}

##知识点：
> ####1、split切分后若首位、末位为切分符，则会返回空字符串"";  
> ####2、StringBuffer的常用方法(String、StringBuffer、StringBuilder）(StringBuilder与之相比线程不安全，速度快，用法一样）
> 
	StringBuffer append(String str) 把任意类型数据添加到字符串缓冲区里面，并返回  
	StringBuffer append(int offset,String str) 指定位置插入  
	StringBuffer delete(int start,int end) 删除指定位置开始到指定位置结束的内容，并返回本身  
	StringBuffer replace(int start,int end,String str) 从start到end用str代替  
	StringBuffer reverse() 字符串反转  
	StringBuffer insert(int i,String str) 在i处插入字符串str  
	void setCharAt(int index,char ch) 给定索引处的字符设置为ch  
	void setLength(int newLength) 设置字符序列的长度  
>####3、StringBuffer、StringBuffer转string：.toString()
> ####4、split分隔符总结  
>##### 　　1.字符"|","*","+"都得加上转义字符，前面加上"\\"  
>##### 　　2.而如果是"\"，那么就得写成"\\\\"  
>##### 　　3.如果一个字符串中有多个分隔符，可以用"|"作为连字符。
> ####5、trim():删除首尾空格　 
> ####6、int类型add进ArrayList自动装箱为Integer类型，Integer转Int使用.intValue()

##4、左旋转单词
###1)切片+拼接
###2)列表遍历拼接
###3）字符串遍历拼接
###时间复杂度均为O(N)，空间复杂度均为O(N)
###方法一：切片+拼接(前期思路)　  

	class Solution {
	    public String reverseLeftWords(String s, int n) {
	        String str1 = s.substring(0,n);
	        String str2 = s.substring(n);
	        return str2+str1;
	    }
	}  
###方法二：列表遍历拼接(若规定不能用切片函数则采取该方法)
>#####1、新建一个 list(Python)、StringBuilder(Java) ，记为 resres ；  
>#####2、先向 resres 添加 “第 n + 1n+1 位至末位的字符” ；  
>#####3、再向 resres 添加 “首位至第 nn 位的字符” ；  
>#####4、将 resres 转化为字符串并返回。  
 
	class Solution {
	    public String reverseLeftWords(String s, int n) {
	        StringBuilder res = new StringBuilder();
	        for(int i = n; i < s.length(); i++)
	            res.append(s.charAt(i));
	        for(int i = 0; i < n; i++)
	            res.append(s.charAt(i));
	        return res.toString();
	    }
	}
###方法三：字符串遍历拼接(若规定只允许采用String则采用该方法)  
>####思路与方法二相同  

	class Solution {
	    public String reverseLeftWords(String s, int n) {
	        String res = "";
	        for(int i = n; i < s.length(); i++)
	            res += s.charAt(i);
	        for(int i = 0; i < n; i++)
	            res += s.charAt(i);
	        return res;
	    }
	}

##知识点
>####切片函数时间复杂度为O(N)  

##5、和为s的两个数字（注意本题为递增有序数组）   
###方法一：前期思路：两数和问题通用(超时）    

	class Solution {
	    public int[] twoSum(int[] nums, int target) {
	        List<Integer> list =  new ArrayList();
	        for(int i=0;i<nums.length;i++){
	            if(list.contains(target-nums[i])){
	                return new int[]{nums[i],target-nums[i]};
	            }else{
	                list.add(nums[i]);
	            }
	        }
	        return null;
	    }
	}  

###方法二：大佬算法（对撞双指针）  
>#####利用 HashMap 可以通过遍历数组找到数字组合，时间和空间复杂度均为 O(N)O(N) ；  
>#####注意本题的 numsnums 是 排序数组 ，因此可使用 双指针法 将空间复杂度降低至 O(1)O(1) 。 
 
>#####**算法流程：**  
1、**初始化**： 双指针 ii , jj 分别指向数组 numsnums 的左右两端 （俗称对撞双指针）。  
2、**循环搜索**： 当双指针相遇时跳出；  
　　1）计算和 s = nums[i] + nums[j]s=nums[i]+nums[j] ；  
　　2）若 s > targets>target ，则指针 jj 向左移动，即执行 j = j - 1j=j−1 ；  
　　3）若 s < targets<target ，则指针 ii 向右移动，即执行 i = i + 1i=i+1 ；  
　　4）若 s = targets=target ，立即返回数组 [nums[i], nums[j]][nums[i],nums[j]] ；  
3、**返回空数组**，代表无和为 targettarget 的数字组合。  
  
	class Solution {
	    public int[] twoSum(int[] nums, int target) {
	        int i = 0, j = nums.length - 1;
	        while(i < j) {
	            int s = nums[i] + nums[j];
	            if(s < target) i++;
	            else if(s > target) j--;
	            else return new int[] { nums[i], nums[j] };
	        }
	        return new int[0];
	    }
	}

###知识点  
> ####1、获得长度方法：  
> length：是针对**数组**来说的，如果写了一个数组，想要知道数组的长度，则可以使用这个属性    
> 
length()：是针对**字符串**String来说的,如果想看这个字符串的长度则用length()这个方法，也可以用size() 
   
>size()：是针对泛型集合来说的，如果想要知道这个泛型集合中有多少元素，即可使用size()这个方法，比如**ArrayList**  
>####2、不同对象取出元素方式：
>数组：s[index]  
>
>字符串：s.charAt(index)，返回char类型，转成string使用String.valueOf(s.charAt(i))  
>
>集合：s.get(i) 

##6、求1+2+...+n(要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）)  
###方法一：变相递归（利用逻辑运算符&&的短路性质）  


	class Solution {
	    public int sumNums(int n) {
	        boolean flag = n > 0 && (n += sumNums(n - 1)) > 0;
	        return n;
	    }
	}
###复杂度分析  

~时间复杂度：O(n)。递归函数递归n次，每次递归中计算时间复杂度为O(1)，因此总时间复杂度为O(n)O(n)。  
~空间复杂度：O(n)。递归函数的空间复杂度取决于递归调用栈的深度，这里递归函数调用栈深度为O(n)，因此空间复杂度为O(n)。  


###方法二：快速乘递归（待定）  

##7、滑动窗口的最大值  
###方法一：前期思路（双指针取出窗口，窗口中取最大值） 

  
	class Solution {
	    public int[] maxSlidingWindow(int[] nums, int k) {
	        int i=0;
	        int j=k-1;
	        if(nums.length==0){
	            return new int[]{};
	        }
	        int[] res = new int[nums.length-k+1];
	        while(i<nums.length-k+1){
	            int max= nums[i];
	            for(int m=i;m<=j;m++){
	                if(nums[m]>max){
	                    max =nums[m];
	                }                
	            }
	            res[i] = max;
	            i++;
	            j++;
	        }
	        return res;
	    }
	}  
时间复杂度O(nk)  
#####难点在于如何将查找最大值的时间复杂度由O(n)降为O(1)
###方法二：单调队列（deque）（关联-剑指offer30.包含min函数的栈）  
算法流程：
>1、初始化： 双端队列deque，结果列表res，数组长度n ；  
2、滑动窗口： 左边界范围i∈[1−k,n−k]，右边界范围j∈[0,n−1] ；  
　1）若i > 0且 队首元素deque[0] == 被删除元素nums[i−1] ：则队首元素出队；  
　2）删除deque内所有< nums[j]的元素，以保持deque递减；  
　3）将nums[j]添加至deque尾部；  
　4）若已形成窗口（即i≥0 ：）将窗口最大值（即队首元素deque[0]）添加至列表res；  
3、返回值：返回结果列表res ；  

	class Solution {
	    public int[] maxSlidingWindow(int[] nums, int k) {
	        if(nums.length == 0 || k == 0) return new int[0];
	        Deque<Integer> deque = new LinkedList<>();
	        int[] res = new int[nums.length - k + 1];
	        for(int j = 0, i = 1 - k; j < nums.length; i++, j++) {
	            // 删除 deque 中对应的 nums[i-1]
	            if(i > 0 && deque.peekFirst() == nums[i - 1])
	                deque.removeFirst();
	            // 保持 deque 递减
	            while(!deque.isEmpty() && deque.peekLast() < nums[j])
	                deque.removeLast();
	            deque.addLast(nums[j]);
	            // 记录窗口最大值
	            if(i >= 0)
	                res[i] = deque.peekFirst();
	        }
	        return res;
	    }
	}  

####时间复杂度O(n)，空间复杂度O(k)

##8、顺时针打印矩阵  

##9、0~n-1中缺失的数字  
###方法一：个人思路  
>设置一个从0递增+1的变量tmp，便于数组与之一一比较，不同的index几位缺失值   
>
	class Solution {
	    public int missingNumber(int[] nums) {
	        if(nums.length==0){
	            return 0;
	        }
	        int tmp = 0;
	        for(int i=0;i<nums.length;i++){
	            if(nums[i]!=tmp){
	                break;
	            }
	            tmp++;
	        }
	    return tmp;
	    }
	}

###方法二：二分法（排序数组中的搜索问题，首先想到二分法）  
	class Solution {
	    public int missingNumber(int[] nums) {
	       int i=0,j=nums.length-1;
	       while(i<=j){
	           int m=(i+j)/2;
	           if(nums[m]==m){
	               i=m+1;
	           }else{
	               j=m-1;
	           }
	       }
	    return i;
	    }
	}  
##10、扑克牌中的顺子  
###方法一：排序+遍历  
>####首先排序；   
>####遍历得到大小王的个数；  
>####遍历判断是否存在重复数；  
>####数组最大值与最小值相减<5。 
> 
	class Solution {
	    public boolean isStraight(int[] nums) {
	       int joker=0;//记录大小王的个数
	       Arrays.sort(nums);//排序
	       for(int i=0;i<4;i++){
	           if(nums[i]==0){
	               joker++;
	           }else if(nums[i+1]==nums[i]){//判断是否有重复数字
	               return false;
	           }
	       }
	       return nums[4]-nums[joker]<5;
	    }
	}  

###方法二：Set+遍历   

	class Solution {
	    public boolean isStraight(int[] nums) {
	        Set<Integer> repeat = new HashSet<>();
	        int max = 0, min = 14;
	        for(int num : nums) {
	            if(num == 0) continue; // 跳过大小王
	            max = Math.max(max, num); // 最大牌
	            min = Math.min(min, num); // 最小牌
	            if(repeat.contains(num)) return false; // 若有重复，提前返回 false
	            repeat.add(num); // 添加此牌至 Set
	        }
	        return max - min < 5; // 最大牌 - 最小牌 < 5 则可构成顺子
	    }
	}

##11、旋转数组的最小数字  
###方法一：个人思路（遍历判断后一个数小于前一个数）  
	class Solution {
	    public int minArray(int[] numbers) {
	        int res = numbers[0];
	        for(int i=0;i<numbers.length-1;i++){
	            if(numbers[i+1]<numbers[i]){
	                res = numbers[i+1];
	            }
	        }
	        return res;
	    }
	}  

###方法二：改进思路（二分法）  
>####第一种情况是numbers[m]<numbers[j]。如下图所示，这说明numbers[m] 是最小值右侧的元素，因此我们可以忽略二分查找区间的右半部分。  
>####第二种情况是numbers[m]>numbers[j]。如下图所示，这说明numbers[m] 是最小值左侧的元素，因此我们可以忽略二分查找区间的左半部分。  
>####第三种情况是numbers[m]==numbers[j]。如下图所示，由于重复元素的存在，我们并不能确定numbers[m] 究竟在最小值的左侧还是右侧，因此我们不能莽撞地忽略某一部分的元素。我们唯一可以知道的是，由于它们的值相同，所以无论numbers[j] 是不是最小值，都有一个它的「替代品」numbers[m]，因此我们可以忽略二分查找区间的右端点。


	class Solution {
	    public int minArray(int[] numbers) {
	        int i=0,j=numbers.length-1;
	        while(i<j){
	            int m=i+(j-i)/2;
	            if(numbers[m]>numbers[j]){
	                i=m+1;
	            }else if(numbers[m]<numbers[j]){
	                j=m;
	            }else{
	                j-=1;
	            }
	        }
	        return numbers[i];
	    }
	}

##12、在排序数组中查找数字1（统计同一数字出现次数）  
  
###方法一：个人常规思路（遍历累加计数）  
	class Solution {
	    public int search(int[] nums, int target) {
	        int count = 0;
	        for(int num:nums){
	            if(num==target){
	                count++;
	            }
	        }
	    return count;
	    }
	}  
###方法二：二分法  
思路  
>由于数组 numsnums 中元素都为整数，因此可以分别二分查找 targettarget 和 target - 1target−1 的右边界，将两结果相减并返回即可。


	class Solution {
	    public int search(int[] nums, int target) {
	        return helper(nums, target) - helper(nums, target - 1);
	    }
	    int helper(int[] nums, int tar) {
	        int i = 0, j = nums.length - 1;
	        while(i <= j) {
	            int m = (i + j) / 2;
	            if(nums[m] <= tar) i = m + 1;
	            else j = m - 1;
	        }
	        return i;
	    }
	}

时间复杂度：O(logN)
空间复杂度：O(1)
##知识点  
>####二分法为对数级复杂度  

##13、最小的k个数  
###方法一：排序取前k个值  
	class Solution {
	    public int[] getLeastNumbers(int[] arr, int k) {
	        Arrays.sort(arr);
	        int[] res = new int[k];
	        for(int i=0;i<k;i++){
	            res[i]=arr[i];
	        }
	    return res;
	    }
	}  
>####时间复杂度：O(nlogn)，n是数组长度。  
>####空间复杂度：O(logn)。

###方法二：堆   
####思路
>####用一个**大根堆**实时维护数组的前 kk 小值。首先将前 kk 个数插入大根堆中，随后从第 k+1k+1 个数开始遍历，如果当前遍历到的数比大根堆的堆顶的数要小，就把堆顶的数弹出，再插入当前遍历到的数。最后将大根堆里的数存入数组返回即可。

	class Solution {
	    public int[] getLeastNumbers(int[] arr, int k) {
	        int[] vec = new int[k];
	        if (k == 0) { // 排除 0 的情况
	            return vec;
	        }
	        PriorityQueue<Integer> queue = new PriorityQueue<Integer>(new Comparator<Integer>() {
	            public int compare(Integer num1, Integer num2) {
	                return num2 - num1;
	            }
	        });
	        for (int i = 0; i < k; ++i) {
	            queue.offer(arr[i]);
	        }
	        for (int i = k; i < arr.length; ++i) {
	            if (queue.peek() > arr[i]) {
	                queue.poll();
	                queue.offer(arr[i]);
	            }
	        }
	        for (int i = 0; i < k; ++i) {
	            vec[i] = queue.poll();
	        }
	        return vec;
	    }
	}
####时间复杂度：O(nlogk)  
####空间复杂度：O(k)
###方法三：快排思想（未摸透）  

	class Solution {
	    public int[] getLeastNumbers(int[] arr, int k) {
	        if (k == 0 || arr.length == 0) {
	            return new int[0];
	        }
	        // 最后一个参数表示我们要找的是下标为k-1的数
	        return quickSearch(arr, 0, arr.length - 1, k - 1);
	    }
	
	    private int[] quickSearch(int[] nums, int lo, int hi, int k) {
	        // 每快排切分1次，找到排序后下标为j的元素，如果j恰好等于k就返回j以及j左边所有的数；
	        int j = partition(nums, lo, hi);
	        if (j == k) {
	            return Arrays.copyOf(nums, j + 1);
	        }
	        // 否则根据下标j与k的大小关系来决定继续切分左段还是右段。
	        return j > k? quickSearch(nums, lo, j - 1, k): quickSearch(nums, j + 1, hi, k);
	    }
	
	    // 快排切分，返回下标j，使得比nums[j]小的数都在j的左边，比nums[j]大的数都在j的右边。
	    private int partition(int[] nums, int lo, int hi) {
	        int v = nums[lo];
	        int i = lo, j = hi + 1;
	        while (true) {
	            while (++i <= hi && nums[i] < v);
	            while (--j >= lo && nums[j] > v);
	            if (i >= j) {
	                break;
	            }
	            int t = nums[j];
	            nums[j] = nums[i];
	            nums[i] = t;
	        }
	        nums[lo] = nums[j];
	        nums[j] = v;
	        return j;
	    }
	}
####时间复杂度：O(n)  
##知识点：
####PriorityQueue（优先队列）的用法  
>####说明  
队列中的元素可以默认自然排序，也可以通过传入的compartor进行实时排序  
不允许有空值，并且不支持不可比较的对象  
PriorityQueue是无界队列，可以在初始化的时候指定容量。当我们添加元素的时候，队列大小会自动增加  
PriorityQueue是线程不安全的  
原理是通过小顶堆实现的。  
  
>####主要方法  
add()和offer()
都是向优先队列中插入元素，区别是，但插入失败时，add()抛出异常，offer()返回false  
element()和peek()
只获取队首元素，但不删除，区别是，获取失败，element()抛出异常，peek()返回null  
remove()和poll()
获取并删除队首元素，区别是，获取失败时，remove()抛出异常，poll()返回null  
remove(Object o)
remove(Object o)方法用于删除队列中跟o相等的某一个元素（如果有多个相等，只删除一个）  


##14、包含min函数的栈  
####解题思路：
>####普通栈的push()和pop()函数的复杂度为O(1)；而获取栈最小值min()函数需要遍历整个栈，复杂度为O(N) 。

####本题难点：将min()函数复杂度降为O(1)，可通过建立辅助栈实现；
>####数据栈A：栈A用于存储所有元素，保证入栈push()函数、出栈pop()函数、获取栈顶top()函数的正常逻辑。  
>####辅助栈B：栈B中存储栈A中所有非严格降序的元素，则栈A中的最小元素始终对应栈B的栈顶元素，即min()函数只需返回栈B的栈顶元素即可。  
>####因此，只需设法维护好栈B的元素，使其保持非严格降序，即可实现min()函数的O(1)复杂度。

	class MinStack {
	    Stack<Integer> A,B;
	
	    /** initialize your data structure here. */
	    public MinStack() {
	        A = new Stack<>();
	        B = new Stack<>(); 
	    }
	    
	    public void push(int x) {
	        A.add(x);
	        if(B.empty()||B.peek()>=x){
	            B.add(x);
	        }
	
	    }
	    
	    public void pop() {
	        if(A.pop().equals(B.peek())){
	            B.pop();
	        };
	    }
	    
	    public int top() {
	        return A.peek();
	    }
	    
	    public int min() {
	        return B.peek();
	    }
	}

##知识点  
>栈（stack）：  
>1）pop和peek
相同点：
peek，pop都是返回栈顶元素；
不同点：
peek()函数返回栈顶的元素，但不弹出该栈顶元素；
pop()函数返回栈顶的元素，并且将该栈顶元素出栈  
2）add和push
相同点：
add，push都可以向stack中添加元素；
不同点：
add是继承自Vector的方法，且返回值类型是boolean；
push是Stack自身的方法，返回值类型是参数类类型；  
3）empty()：判断栈是否为空 结果为false

##15、不用加减乘除做加法   
###方法： 位运算 
####思路  
>两数想与值与进位值相同，两数异或值与非进位值相同，循环相加直至进位值变为0  
>
	class Solution {
	    public int add(int a, int b) {
	        while(b!=0){
	            int c = (a&b)<<1;
	            a=a^b;
	            b=c;
	        }
	    return a;
	    }
	}  
##16、判断是否是对称二叉树  
###方法：递归，比较树的各对称节点
###算法流程：  
>####isSymmetric(root) ：  

>1）特例处理：若根节点 root 为空，则直接返回 true 。  
2）返回值： 即 recur(root.left, root.right) ;  
>####recur(L, R) ：
>1）终止条件：
当 L 和 R 同时越过叶节点： 此树从顶至底的节点都对称，因此返回 true ；
当 L 或 R 中只有一个越过叶节点： 此树不对称，因此返回 false ；
当节点 L 值/节点 R 值： 此树不对称，因此返回 false ；  
2）递推工作：
判断两节点 L.left 和 R.right 是否对称，即 recur(L.left, R.right) ；
判断两节点 L.right 和 R.left 是否对称，即 recur(L.right, R.left) ；  
3）返回值： 两对节点都对称时，才是对称树，因此用与逻辑符 && 连接。

	/**
	 * Definition for a binary tree node.
	 * public class TreeNode {
	 *     int val;
	 *     TreeNode left;
	 *     TreeNode right;
	 *     TreeNode(int x) { val = x; }
	 * }
	 */
	class Solution {
	    public boolean isSymmetric(TreeNode root) {
	        return root==null?true:compare(root.left,root.right);
	    }
	    boolean compare(TreeNode L,TreeNode R){
	        if(L==null&&R==null) return true;
	        if(L==null||R==null) return false;
	        if(L.val!=R.val) return false;
	        return compare(L.left,R.right)&&compare(L.right,R.left);
	    }
	} 

####时间复杂度：O(N)  
####空间复杂度：O(N)  

##**知识点**  
####创建二叉树  
一、构建节点
为了方便引用而不用专门通过set和get方法引用，我们直接将变量都定义为public的
 
	public class Node {
	 
		public long data;
		public Node leftChild;
		public Node rightChild;
			
		/*
		 * 构造方法
		 */
		public Node(long data) {
			this.data=data;
		}
	}
二、构建树

 
	public class Trees {
	 
		public Node root;
		
		/*
		 * 插入节点
		 */
		
		public void insert(long value) {
			//封装节点
			Node newNode = new Node(value);
			//引用当前节点
			Node current = root;
			//引用父节点
			Node parent;
			//如果root为null，也就是第一插入的时候
			if(root==null) {
				root = newNode;
				return;
			}else {
				while(true) {
					//父节点指向当前节点
				parent = current;
				//如果当前节点指向的节点数据比插入的要大，则往左走
				if(current.data > value) {
					current = current.leftChild;
					
					if(current == null) {
						
						parent.leftChild = newNode;
						return;
					}
				}else {
					current = current.rightChild;
					if(current==null) {
						parent.rightChild = newNode;
						return;
					}
				}
			}
			}
		}
		
		//寻找节点
		private void find(long value) {
			
		}
		//删除节点
		private void delete() {
			
		}
	}  

##17、判断是否为平衡二叉树  

###方法一：先序遍历+判断深度（从顶至底）  
####思路  
###算法流程：  
>####isBalanced(root) 函数： 判断树 root 是否平衡

>####特例处理： 若树根节点 root 为空，则直接返回 truetrue ；  
>####返回值： 所有子树都需要满足平衡树性质，因此以下三者使用与逻辑 \&\&&& 连接；  
1）abs(self.depth(root.left) - self.depth(root.right)) <= 1 ：判断 当前子树 是否是平衡树；  
2）self.isBalanced(root.left) ： 先序遍历递归，判断 当前子树的左子树 是否是平衡树；  
3）self.isBalanced(root.right) ： 先序遍历递归，判断 当前子树的右子树 是否是平衡树；  
>####depth(root) 函数： 计算树 root 的深度
1）终止条件： 当 root 为空，即越过叶子节点，则返回高度 0 ；  
2）返回值： 返回左 / 右子树的深度的最大值 +1 。

	/**
	 * Definition for a binary tree node.
	 * public class TreeNode {
	 *     int val;
	 *     TreeNode left;
	 *     TreeNode right;
	 *     TreeNode(int x) { val = x; }
	 * }
	 */
	class Solution {
	    public boolean isBalanced(TreeNode root) {
	        if(root==null){
	            return true;
	        }
	        return Math.abs(depth(root.left)-depth(root.right))<2&&isBalanced(root.left)&&isBalanced(root.right);
	    }
	    int depth(TreeNode root){
	        if(root==null){
	            return 0;
	        }
	        return Math.max(depth(root.right),depth(root.left))+1;
	    }    
	}  
####时间复杂度：O(NlogN)
>最差情况下（为 “满二叉树” 时）， isBalanced(root) 遍历树所有节点，判断每个节点的深度 depth(root) 需要遍历 各子树的所有节点 。
满二叉树高度的复杂度 O(logN) ，将满二叉树按层分为 log(N+1) 层；
通过调用 depth(root) ，判断二叉树各层的节点的对应子树的深度，需遍历节点数量为 N×1,(N-1)/2*2,(N-3)/4*4,...,1*(N+1)/2 。因此各层执行 depth(root) 的时间复杂度为 O(N) （每层开始，最多遍历 N 个节点，最少遍历 1*(N+1)/2 个节点）。  
因此，总体时间复杂度 == 每层执行复杂度  层数复杂度 = O(N×logN) 。

####空间复杂度：O(N)O(N)
> 最差情况下（树退化为链表时），系统递归需要使用 O(N) 的栈空间。 

###方法二：后续遍历+剪枝（从底至顶）（不太懂）  

	class Solution {
	    public boolean isBalanced(TreeNode root) {
	        return recur(root) != -1;
	    }
	
	    private int recur(TreeNode root) {
	        if (root == null) return 0;
	        int left = recur(root.left);
	        if(left == -1) return -1;
	        int right = recur(root.right);
	        if(right == -1) return -1;
	        return Math.abs(left - right) < 2 ? Math.max(left, right) + 1 : -1;
	    }
	}

##18、删除链表的节点  
###方法一：单指针  
####思路  
创建一个指针pre代替head进行遍历  

	/**
	 * Definition for singly-linked list.
	 * public class ListNode {
	 *     int val;
	 *     ListNode next;
	 *     ListNode(int x) { val = x; }
	 * }
	 */
	class Solution {
	    public ListNode deleteNode(ListNode head, int val) {
	        if(head.val==val){
	            return head.next;
	        }
	        ListNode pre = head;
	        while(pre.next.val!=val&&pre.next!=null){
	            pre=pre.next;            
	        }
	        if(pre.next.val==val){
	                pre.next=pre.next.next;
	            }
	    return head;
	    }
	}
###方法二：迭代  

	class Solution {
	    public ListNode deleteNode(ListNode head, int val) {
	        if(head.val==val){
	            return head.next;
	        }
	        head.next = deleteNode(head.next,val);
	        return head;   
	    }
	}

###方法三：双指针    

	class Solution {
	    public ListNode deleteNode(ListNode head, int val) {
	        if(head.val == val) return head.next;
	        ListNode pre = head, cur = head.next;
	        while(cur != null && cur.val != val) {
	            pre = cur;
	            cur = cur.next;
	        }
	        if(cur != null) pre.next = cur.next;
	        return head;
	    }
	}


###知识点  
链表：

##19、连续子数组的最大和  

###方法一：动态规划  

	class Solution {
	    public int maxSubArray(int[] nums) {      
	        int res = nums[0];
	        for(int i=1;i<nums.length;i++){
	            if(nums[i-1]>0){
	                nums[i]=nums[i-1]+nums[i];
	                res=Math.max(res,nums[i]);
	            }else{
	                res=Math.max(res,nums[i]);
	            }
	        }
	    return res;
	    }
	}  
时间复杂度：O(N) 、 空间复杂度：O(1)  
###方法二：暴力搜索：O(N^2)、 O(1)  
###方法三：分治思想：O(NlogN)、O(longN)  

##20、第一次只出现一次的字符  
###方法一：哈希表存储频数  
思路：第一次遍历记录频数，第二次遍历找到首先出现的频数为1的字符  

	class Solution {
	    public char firstUniqChar(String s) {
	        Map<Character, Integer> frequency = new HashMap<Character, Integer>();
	        for (int i = 0; i < s.length(); ++i) {
	            char ch = s.charAt(i);
	            frequency.put(ch, frequency.getOrDefault(ch, 0) + 1);
	        }
	        for (int i = 0; i < s.length(); ++i) {
	            if (frequency.get(s.charAt(i)) == 1) {
	                return s.charAt(i);
	            }
	        }
	        return ' ';
	    }
	}


时间复杂度：O(n)  
空间复杂度：O(|E|),E为字符集  
###方法二：哈希表存储索引  
思路：value存储索引，若重复出现，索引记作-1，最后遍历返回value不为-1且最小时对应的key  

	class Solution {
	    public char firstUniqChar(String s) {
	        Map<Character, Integer> position = new HashMap<Character, Integer>();
	        int n = s.length();
	        for (int i = 0; i < n; ++i) {
	            char ch = s.charAt(i);
	            if (position.containsKey(ch)) {
	                position.put(ch, -1);
	            } else {
	                position.put(ch, i);
	            }
	        }
	        int first = n;
	        for (Map.Entry<Character, Integer> entry : position.entrySet()) {
	            int pos = entry.getValue();
	            if (pos != -1 && pos < first) {
	                first = pos;
	            }
	        }
	        return first == n ? ' ' : s.charAt(first);
	    }
	}

复杂度与方法一相同  

###方法三：队列  
>###思路：  
>####类似于方法二，哈希表存储字符和索引，利用队列的排队属性记录顺序

	class Solution {
	    public char firstUniqChar(String s) {
	        Map<Character, Integer> position = new HashMap<Character, Integer>();
	        Queue<Pair> queue = new LinkedList<Pair>();
	        int n = s.length();
	        for (int i = 0; i < n; ++i) {
	            char ch = s.charAt(i);
	            if (!position.containsKey(ch)) {
	                position.put(ch, i);
	                queue.offer(new Pair(ch, i));
	            } else {
	                position.put(ch, -1);
	                while (!queue.isEmpty() && position.get(queue.peek().ch) == -1) {
	                    queue.poll();
	                }
	            }
	        }
	        return queue.isEmpty() ? ' ' : queue.poll().ch;
	    }
	    class Pair {
	        char ch;
	        int pos;
	
	        Pair(char ch, int pos) {
	            this.ch = ch;
	            this.pos = pos;
	        }
	    }
	}

复杂度同上
###知识点 
####1、map：  
>map.containsKey("")  
put()  
get()
remove()   
####2、LinkedList可以当作Queue来用
>offer()  
poll()取值并移除  
peek()或element()取值但不移除  
  

##21、调整数组顺序使奇数位于偶数前面  

###方法一：个人思路（性能差）  
>####奇数、偶数分别放在两个不同的list中，然后合并两个list  

	class Solution {
	    public int[] exchange(int[] nums) {
	        List<Integer> odd = new ArrayList();
	        List<Integer> even = new ArrayList();
	        for(int i=0;i<nums.length;i++){
	            if(nums[i]%2==0){
	                even.add(nums[i]);
	            }else{
	                odd.add(nums[i]);
	            }
	        }
	        odd.addAll(even);
	        int[] res = new int[odd.size()];
	        for(int j=0;j<res.length;j++){
	             res[j]=((Integer)odd.get(j)).intValue();
	        }
	        return res;
	
	    }
	}  

###方法二：首位双指针  
###思路：  
>首尾各设置一个指针，l向右搜索偶数，r向左搜索奇数，搜索到时交换两者位置  

	class Solution {
	    public int[] exchange(int[] nums) {
	        int l=0,r=nums.length-1;
	        int tmp;
	        while(l<r){
	            //找到偶数
	            while((nums[l]&1)==1&&l<r) l++;
	            //找到奇数
	            while((nums[r]&1)==0&&l<r) r--;
	            tmp = nums[l];
	            nums[l]=nums[r];
	            nums[r]= tmp;
	            l++;
	            r--;
	        }
	        return nums;
	
	    }
	}

###方法三：快慢双指针  

###思路：  
>####设置快慢两指针，快指针搜索到奇数，将奇数放置到慢指针处，慢指针相当于是一个新的数组，只不过利用了旧数组的空间  

	class Solution {
	    public int[] exchange(int[] nums) {
	        int f=0,l=0;
	        while(f<nums.length){
	            if((nums[f]&1)==1){
	                int tmp = nums[f];
	                nums[f]=nums[l];
	                nums[l]= tmp;
	                l++;
	            }
	            f++;
	        }
	        return nums;
	
	    }
	}  

###知识点  
>####1)合并两ArrayList：.addAll()  
>####2)ArrayList转Array：同Array()，返回类型为Object[]  
>####3)Array转ArrayList：Arrays.asList(array)只会返回一个定长List，不支持add、remove等修改长度的操作，推荐使用List<T> list=new ArrayList<T>(Arrays.asList(array))  

##22、圆圈中最后剩下的数字  
###方法一：模拟链表  

	class Solution {
	    public int lastRemaining(int n, int m) {
	        ArrayList<Integer> list = new ArrayList<>(n);
	        for (int i = 0; i < n; i++) {
	            list.add(i);
	        }
	        int idx = 0;
	        while (n > 1) {
	            idx = (idx + m - 1) % n;
	            list.remove(idx);
	            n--;
	        }
	        return list.get(0);
	    }
	}
复杂度：O(n^2)

###方法二：大佬思路（递归变形）  

	class Solution {
	    public int lastRemaining(int n, int m) {
	        int x = 0;
	        for (int i = 2; i <= n; i++) {
	            x = (x + m) % i;//f(n)和f(n-1)的转移表达式；f(1)必留下0；
	        }
	        return x;
	    }
	}

复杂度：O(n)
###方法三：递归（方法二的直观形式）  

class Solution {
    public int lastRemaining(int n, int m) {
        return f(n, m);
    }

    public int f(int n, int m) {
        if (n == 1) {
            return 0;
        }
        int x = f(n - 1, m);
        return (m + x) % n;
    }
}
##23、数组中重复的数字  
###方法一：排序+前后比较  

	class Solution {
	    public int findRepeatNumber(int[] nums) {
	        //排序
	        Arrays.sort(nums);
	        for(int i=0;i<nums.length-1;i++){
	            if(nums[i+1]==nums[i]){
	                return nums[i];
	            }
	        }
	        return -1;
	
	    }
	}
>####时间复杂度：O(nlogn)  
>####空间复杂度：O(1)  
###方法二：利用ArrayList的contains判断重复（超时，应该使用hashset，利用hashset的不可重复性质）
	class Solution {
	    public int findRepeatNumber(int[] nums) {
	        List arr = new ArrayList();
	        for(int i=0;i<nums.length;i++){
	            if(!arr.contains(nums[i])){
	                arr.add(nums[i]);
	            }else{
	                return nums[i];
	            }
	        }
	        return -1;
	
	    }
	}  
时间复杂度：O(n)  
空间复杂度：O(n)  

###方法三：原地置换(充分利用已知条件）   
###思路：  
>####索引和值一一对应，若有多个值对应同一索引，则返回该值，否则，调换位置  
 
	class Solution {
	    public int findRepeatNumber(int[] nums) {
	        int i = 0;
	        while(i < nums.length) {
	            if(nums[i] == i) {
	                i++;
	                continue;
	            }
	            if(nums[nums[i]] == nums[i]) return nums[i];
	            int tmp = nums[i];
	            nums[i] = nums[tmp];
	            nums[tmp] = tmp;
	        }
	        return -1;
	    }
	}  
>####时间复杂度：o(n)  
>####空间复杂度：O(1)  
###知识点  
>####向set中添加重复元素时，会返回false  

##24、数组中出现次数超过一半的数字  
###方法一：hashset记录各元素频数（）  
####思路：  
>####map记录频数，然后遍历map找到value超过数组长度一半的key
	class Solution {
	    public int majorityElement(int[] nums) {
	      Map count = new HashMap();
	      for(int i=0;i<nums.length;i++){
	          if(count.containsKey(nums[i])){
	              count.put(nums[i],(int)count.get(nums[i])+1);
	          }else{
	              count.put(nums[i],1);
	          }
	      }
	      for(int m=0;m<nums.length;m++){
	          if(((int)count.get(nums[m]))>=(nums.length/2+1)){
	              return nums[m];              
	          }
	      }
	      return -1;	
	    }
	}  

时间复杂度：O(n)
空间复杂度：O(n)  

###方法二：排序后中点位置的值即为众数  
 
	class Solution {
	    public int majorityElement(int[] nums) {     
	      Arrays.sort(nums);
	      return nums[nums.length/2];
	    }
	}  
时间复杂度：o(nlogn)  
空间复杂度：o(1)
###方法三：摩尔投票法（最佳解法）（众数和菲众数一一抵消，剩余值即为众数值）  
	class Solution {
	    public int majorityElement(int[] nums) {
	        int vec = 0,x=0;
	        for(int num:nums){
	            if(vec==0){
	                x=num;                
	            }
	            vec+=(x==num?1:-1);
	        }
	        return x;
	    }
	}  
#####
时间复杂度：O(n)  
#####空间复杂度：o(1)
###知识点  
>map的常用api：get(key)、put(key,value)、containsKey(key)、.getKey()、.getValue()  

##25、和为s的连续正数序列  

###方法一：求和公式  
###思路：  

>####根据求和公式，已知左边界与和，集合推算出有边界，判断若有边界不为整数，说明不存在这种情况，移动左边界  

	class Solution {
	    public int[][] findContinuousSequence(int target) {
	        int i = 1;
	        double j = 2.0;
	        List<int[]> res = new ArrayList<>();
	        while(i < j) {
	            j = (-1 + Math.sqrt(1 + 4 * (2 * target + (long) i * i - i))) / 2;
	            if(i < j && j == (int)j) {
	                int[] ans = new int[(int)j - i + 1];
	                for(int k = i; k <= (int)j; k++)
	                    ans[k - i] = k;
	                res.add(ans);
	            }
	            i++;
	        }
	        return res.toArray(new int[0][]);
	    }
	}
#####时间复杂度：O(target)  
#####空间复杂度：O(1)

###方法二：双指针（滑动窗口）  

###思路：  
>####每次移动左右边界，比较窗口内和与target的大小，若<target则扩大窗口，若相等，即为所求连续序列  


	class Solution {
	    public int[][] findContinuousSequence(int target) {
	        int i = 1, j = 2, s = 3;
	        List<int[]> res = new ArrayList<>();
	        while(i < j) {
	            if(s == target) {
	                int[] ans = new int[j - i + 1];
	                for(int k = i; k <= j; k++)
	                    ans[k - i] = k;
	                res.add(ans);
	            }
	            if(s >= target) {
	                s -= i;
	                i++;
	            } else {
	                j++;
	                s += j;
	            }
	        }
	        return res.toArray(new int[0][]);
	    }
	}
时间复杂度：O(target)  
空间复杂度：O(1)  

##26、二进制中1的个数  
###方法一：逐位判断  
###思路：  
>####若n&1=0，则可以判断最右一位是0，若=1，说明最右一位是1，count++，然后n无符号右移，当n==0时跳出  
  

	public class Solution {
	    public int hammingWeight(int n) {
	        int res = 0;
	        while(n != 0) {
	            res += n & 1;
	            n >>>= 1;
	        }
	        return res;
	    }
	}

#####时间复杂度：O(log2n)  逐位需要循环log2n次，移位、与、加等基本运算占用O(1)  
#####空间复杂度：O(1)  

###方法二：巧用n&(n-1)  

###思路：  
>####(n−1) 解析： 二进制数字 n 最右边的 1 变成 0 ，此 1 右边的 0 都变成 1   
>####n&(n−1) 解析： 二进制数字 n 最右边的 1 变成 0 ，其余不变  
>![n&(n-1)](C:\Users\张如飞\Desktop\大数据相关笔记\剑指offer刷题截图\微信截图_20210402124541.png)  

	public class Solution {
	    public int hammingWeight(int n) {
	        int res = 0;
	        while(n != 0) {
	            res++;
	            n &= n - 1;
	        }
	        return res;
	    }
	}

#####时间复杂度：O(M) M为1的个数  
#####空间复杂度：O(1)  

##27、替换空格  
###方法一：遍历添加
###思路：  
>####遍历依次将字符添加到StringBuilder中，若该字符为空格，则添加“%20”    

	class Solution {
	    public String replaceSpace(String s) {
	        StringBuilder res = new StringBuilder();
	        for(int i=0;i<s.length();i++){
	            if(s.charAt(i)==' '){
	                res.append("%20");
	            }else{
	                res.append(s.charAt(i));
	            }
	        }
	        return res.toString();
	
	    }
	}  

#####时间复杂度：O(n)  
#####空间复杂度：O(1)  

###方法：原地替换（因为C++中string可变，因此只C++可用）  

##28、输入n，输出n位数内所有数  
###方法一：个人思路（依次将n位数最大值以内每个数添加到数组内返回）

	class Solution {
	    public int[] printNumbers(int n) {
	        int i=1;
	        int a = (int)Math.pow(10,n);
	        int[] res = new int[a-1];
	        while(i<a){
	            res[i-1]=i;
	            i++;
	        }
	        return res;
	    }
	}
时间复杂度：O(10^n)  
空间复杂度：O(1)   

###扩展（有待练习）:  
###本体简单难度未考虑大数越界，主要考点应是大数越界情况下的打印.需要解决以下三个问题：  
1. 表示大数的变量类型：
无论是 short / int / long ... 任意变量类型，数字的取值范围都是有限的。因此，大数的表示应用字符串 String 类型。

2. 生成数字的字符串集：
使用 int 类型时，每轮可通过 +1 生成下个数字，而此方法无法应用至 String 类型。并且， String 类型的数字的进位操作效率较低，例如 "9999" 至 "10000" 需要从个位到千位循环判断，进位 4 次。  
观察可知，生成的列表实际上是 n 位 0 - 9 的 全排列 ，因此可避开进位操作，通过递归生成数字的 String 列表。  

3. 递归生成全排列：
基于分治算法的思想，先固定高位，向低位递归，当个位已被固定时，添加数字的字符串。例如当 n=2 时（数字范围 1−99 ），固定十位为 0 - 9 ，按顺序依次开启递归，固定个位 0 - 9 ，终止递归并添加数字字符串。  

		class Solution {
		    int[] res;
		    int count = 0;	
		    public int[] printNumbers(int n) {
		        res = new int[(int)Math.pow(10, n) - 1];
		        for(int digit = 1; digit < n + 1; digit++){
		            for(char first = '1'; first <= '9'; first++){
		                char[] num = new char[digit];
		                num[0] = first;
		                dfs(1, num, digit);
		            }
		        }
		        return res;
		    }	
		    private void dfs(int index, char[] num, int digit){
		        if(index == digit){
		            res[count++] = Integer.parseInt(String.valueOf(num));
		            return;
		        }
		        for(char i = '0'; i <= '9'; i++){
		            num[index] = i;
		            dfs(index + 1, num, digit);
		        }
		    }
		}  


###知识点  
>#####求平方：Math.pow(a,n)  
>#####求平方根：Math.sqrt(a,n)  
>#####求绝对值：Math.abs(a,n)   

##29、二叉树的深度（深度优先搜索（DFS）、广度优先搜索（BFS））  
###方法一：个人解法-递归（后序遍历DFS）  
###思路：  
>####判断左右子树，递归  

	/**
	 * Definition for a binary tree node.
	 * public class TreeNode {
	 *     int val;
	 *     TreeNode left;
	 *     TreeNode right;
	 *     TreeNode(int x) { val = x; }
	 * }
	 */
	class Solution {
	    public int maxDepth(TreeNode root) {
	        if(root==null){
	            return 0;
	        }
	        if(root.right==null&&root.left==null){
	            return 1;//不必要写这个if条件，直接返回最大值+1
	        }else{
	            return Math.max(maxDepth(root.right),maxDepth(root.left))+1;
	        }
	
	    }
	}  

>######时间复杂度：O(N)  N为树的节点数  
>######空间复杂度：O(N) 树退化成链表时，递归深度为N  

###方法二：层序遍历（BFS）  

	class Solution {
	    public int maxDepth(TreeNode root) {
	        if(root == null) return 0;
	        List<TreeNode> queue = new LinkedList<>() {{ add(root); }}, tmp;
	        int res = 0;
	        while(!queue.isEmpty()) {
	            tmp = new LinkedList<>();
	            for(TreeNode node : queue) {
	                if(node.left != null) tmp.add(node.left);
	                if(node.right != null) tmp.add(node.right);
	            }
	            queue = tmp;
	            res++;
	        }
	        return res;
	    }
	}

#####时间复杂度：O(N) N为节点数
#####空间复杂度：O(N) 最差情况下为平衡树时，队列queue同时存储N/2个节点  


###知识点  
#####树的遍历方式总体分为两类：深度优先搜索（DFS）、广度优先搜索（BFS）；  

>常见的 DFS ： 先序遍历、中序遍历、后序遍历；（通常利用**遍历**或**栈**现）  
常见的 BFS ： 层序遍历（即按层遍历）（通常利用**队列**实现）。  

##30、从上到下打印二叉树2  
###方法：层序遍历  

	/**
	 * Definition for a binary tree node.
	 * public class TreeNode {
	 *     int val;
	 *     TreeNode left;
	 *     TreeNode right;
	 *     TreeNode(int x) { val = x; }
	 * }
	 */
	class Solution {
	    public List<List<Integer>> levelOrder(TreeNode root) {
	        if(root==null){return Collections.emptyList();}
	        List<List<Integer>> res = new LinkedList<>();
	        res.add(new LinkedList<Integer>(){{add(root.val);}});
	        List<TreeNode> queue = new LinkedList<>(),tmp;
	        queue.add(root);
	        List<Integer> tmpNum;
	        while(!queue.isEmpty()){
	            tmp = new LinkedList<>();
	            tmpNum = new LinkedList<>();
	            for(TreeNode node:queue){
	                if(node.left!=null){
	                    tmp.add(node.left);
	                    tmpNum.add(node.left.val);
	                }
	                if(node.right!=null){
	                    tmp.add(node.right);
	                    tmpNum.add(node.right.val);
	                }
	            }
	            queue = tmp;
	            if(tmpNum.size()!=0){
	                res.add(tmpNum);
	            }
	            
	        }
	        return res;
	    }
	}  

###大佬优化  

	class Solution {
	    public List<List<Integer>> levelOrder(TreeNode root) {
	        Queue<TreeNode> queue = new LinkedList<>();
	        List<List<Integer>> res = new ArrayList<>();
	        if(root != null) queue.add(root);
	        while(!queue.isEmpty()) {
	            List<Integer> tmp = new ArrayList<>();
	            for(int i = queue.size(); i > 0; i--) {
	                TreeNode node = queue.poll();
	                tmp.add(node.val);
	                if(node.left != null) queue.add(node.left);
	                if(node.right != null) queue.add(node.right);
	            }
	            res.add(tmp);
	        }
	        return res;
	    }
	}  

#####时间复杂度：O(N) N 为二叉树的节点数量，即 BFS 需循环 N 次  
#####空间复杂度：O(N) 最差情况下，即当树为平衡二叉树时，最多有 N/2 个树节点同时在 queue 中，使用 O(N) 大小的额外空间。  

##31、二叉搜索树的最近公共祖先  

###方法一：递归  
####思路：  
>#####因为是搜索树，因此p、q值与父节点值的相对大小关系即可表明p、q两值所处的位置，是左还是右  


	/**
	 * Definition for a binary tree node.
	 * public class TreeNode {
	 *     int val;
	 *     TreeNode left;
	 *     TreeNode right;
	 *     TreeNode(int x) { val = x; }
	 * }
	 */
	class Solution {
	    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
	        if(p.val>root.val&&q.val>root.val){
	            return lowestCommonAncestor(root.right, p, q);
	        }else if(p.val<root.val&&q.val<root.val){
	            return lowestCommonAncestor(root.left, p, q);
	        }else{
	            return root;
	        }      
	    }
	}  

#####时间复杂度：O(N) 其中 N 为二叉树节点数；每循环一轮排除一层，二叉搜索树的层数最小为 NlogN （满二叉树），最大为 N （退化为链表）。    
#####空间复杂度：O(N) 最差情况下，即树退化为链表时，递归深度达到树的层数 N 。 

###方法二：迭代   

	class Solution {
	    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
	        while(root != null) {
	            if(root.val < p.val && root.val < q.val) // p,q 都在 root 的右子树中
	                root = root.right; // 遍历至右子节点
	            else if(root.val > p.val && root.val > q.val) // p,q 都在 root 的左子树中
	                root = root.left; // 遍历至左子节点
	            else break;
	        }
	        return root;
	    }
	}

#####时间复杂度：O(N) 其中 N 为二叉树节点数；每循环一轮排除一层，二叉搜索树的层数最小为 NlogN （满二叉树），最大为 N （退化为链表）。    
#####空间复杂度：O(1) 使用常数大小的额外空间。  

##32、二叉树的最近公共祖先  

###方法：后序遍历DFS  

	class Solution {
	    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
	        if(root == null || root == p || root == q) return root;
	        TreeNode left = lowestCommonAncestor(root.left, p, q);
	        TreeNode right = lowestCommonAncestor(root.right, p, q);
	        if(left == null && right == null) return null; // 1.左右都为null，说明未搜寻到所求树
	        if(left == null) return right; // 3.左为空，说明查询值在右子树
	        if(right == null) return left; // 4.右为空，说明查询值在左子树
	        return root; // 2. if(left != null and right != null)
	    }
	}  

#####时间复杂度：O(N)  
#####空间复杂度：O(N)  

##33、完成一个函数，输入一个二叉树，该函数输出它的镜像  
###方法一：递归（个人思路）    
####思路:  
>#####其实该问题只需要实现递归交换每个父节点下两子节点位置即可  

	class Solution {
	    public TreeNode mirrorTree(TreeNode root) {
			if(root==null) return null;
	        TreeNode tmp = root.left;
	        root.left = root.right;
	        root.right = tmp;
	        if(root.left!=null){
	            mirrorTree(root.left);
	        }
	        if(root.right!=null){
	            mirrorTree(root.right);
	        } 
			//中间几步可以替换为以下
	 		//TreeNode tmp = root.left;
	        //root.left = mirrorTree(root.right);
	        //root.right = mirrorTree(tmp);      
	        return root;
	    }
	}

#####时间复杂度：O(N)  
#####空间复杂度：O(N)  最差情况下（当二叉树退化为链表），递归时系统需使用 O(N) 大小的栈空间。

###方法二：辅助栈（或队列）  


	class Solution {
	    public TreeNode mirrorTree(TreeNode root) {
	        if(root == null) return null;
	        Stack<TreeNode> stack = new Stack<>() {{ add(root); }};
	        while(!stack.isEmpty()) {
	            TreeNode node = stack.pop();
	            if(node.left != null) stack.add(node.left);
	            if(node.right != null) stack.add(node.right);
	            TreeNode tmp = node.left;
	            node.left = node.right;
	            node.right = tmp;
	        }
	        return root;
	    }
	} 

#####时间复杂度 O(N) ： 其中 N 为二叉树的节点数量，建立二叉树镜像需要遍历树的所有节点，占用 O(N) 时间。  
#####空间复杂度 O(N) ： 如下图所示，最差情况下，栈 stack 最多同时存储（N+1）/2个节点，占用 O(N) 额外空间。

##34、给定一棵二叉搜索树，请找出其中第k大的节点  

###方法一：后序遍历+排序取值（个人思路，速度太慢）（没注意题干，是搜索树）该方法通用  
####思路：  
>#####后序遍历递归将每个节点值加入集合，集合排序，取出k大的值  

	/**
	 * Definition for a binary tree node.
	 * public class TreeNode {
	 *     int val;
	 *     TreeNode left;
	 *     TreeNode right;
	 *     TreeNode(int x) { val = x; }
	 * }
	 */
	class Solution {
	    public int kthLargest(TreeNode root, int k) {
	        List res = save(root);
	        Collections.sort(res);
	        return (int)res.get(res.size()-k);
	    }
	    private List save(TreeNode root){
	        
	        List res = new ArrayList();
	        if(root.left!=null){
	            res.addAll(save(root.left));
	        }
	        if(root.right!=null){
	            res.addAll(save(root.right));
	        }        
	        res.add(root.val);
	        return res;
	    }
	}  

#####时间复杂度：O(N^2)  
#####空间复杂度：O(N^2)  

###方法二：中序遍历（you、中、左）  
####思路：  
>####递归解析：  
1、终止条件： 当节点 rootroot 为空（越过叶节点），则直接返回；  
2、递归右子树： 即 dfs(root.right)dfs(root.right) ；  
3、三项工作：  
1）提前返回： 若 k = 0k=0 ，代表已找到目标节点，无需继续遍历，因此直接返回；  
2）统计序号： 执行 k = k - 1k=k−1 （即从 kk 减至 00 ）；  
3）记录结果： 若 k = 0k=0 ，代表当前节点为第 kk 大的节点，因此记录 res = root.valres=root.val ；  
4、递归左子树： 即 dfs(root.left)dfs(root.left) ；  

  
	class Solution {
	    int res, k;
	    public int kthLargest(TreeNode root, int k) {
	        this.k = k;
	        dfs(root);
	        return res;
	    }
	    void dfs(TreeNode root) {
	        if(root == null) return;
	        dfs(root.right);
	        if(k == 0) return;
	        if(--k == 0) res = root.val;
	        dfs(root.left);
	    }
	}

#####时间复杂度：O(N)  当树退化为链表时（全部为右子节点），无论 k 的值大小，递归深度都为 N ，占用 O(N) 时间。  
#####空间复杂度：O(N)  当树退化为链表时（全部为右子节点），系统使用 O(N) 大小的栈空间。  

###知识点  
>####中序遍历 为 “左、根、右” 顺序，递归法代码如下：

	// 打印中序遍历
	void dfs(TreeNode root) {
	    if(root == null) return;
	    dfs(root.left); // 左
	    System.out.println(root.val); // 根
	    dfs(root.right); // 右
	}


##35、从尾到头打印链表  

###方法一：（个人方法）先将链表所有值顺序加入数组，再反向打印数组  
	/**
	 * Definition for singly-linked list.
	 * public class ListNode {
	 *     int val;
	 *     ListNode next;
	 *     ListNode(int x) { val = x; }
	 * }
	 */
	class Solution {
	    public int[] reversePrint(ListNode head) {
	        if(head==null){
	            return new int[0];
	        }
	        List<Integer> res = new ArrayList<>();
	        while(head.next!=null){
	            res.add(head.val);
	            head=head.next;
	        }
	        res.add(head.val);        
	        int[] res1 = new int[res.size()];
		    for(int j=res1.length-1;j>=0;j--){
		        res1[res1.length-1-j]=((Integer)res.get(j)).intValue();
	        }
	        return res1;
	    }
	}  

###方法二：递归  

	class Solution {
	    ArrayList<Integer> tmp = new ArrayList<Integer>();
	    public int[] reversePrint(ListNode head) {
	        recur(head);
	        int[] res = new int[tmp.size()];
	        for(int i = 0; i < res.length; i++)
	            res[i] = tmp.get(i);
	        return res;
	    }
	    void recur(ListNode head) {
	        if(head == null) return;
	        recur(head.next);
	        tmp.add(head.val);
	    }
	}

####时间复杂度：O(N)，遍历链表，递归N次  
####空间复杂度：O(N)，系统递归需要使用O(N)的栈空间  

###方法三：栈  

	class Solution {
	    public int[] reversePrint(ListNode head) {
	        LinkedList<Integer> stack = new LinkedList<Integer>();
	        while(head != null) {
	            stack.addLast(head.val);
	            head = head.next;
	        }
	        int[] res = new int[stack.size()];
	        for(int i = 0; i < res.length; i++)
	            res[i] = stack.removeLast();
	    return res;
	    }
	}  
####时间复杂度 O(N)： 入栈和出栈共使用 O(N) 时间。
####空间复杂度 O(N)： 辅助栈 stack 和数组 res 共使用 O(N)的额外空间。


##36、链表中倒数第k个节点  

###方法一：栈（采用35题的思路）    

	class Solution {
	    public ListNode getKthFromEnd(ListNode head, int k) {
	        LinkedList<ListNode> stack = new LinkedList<>();
	        ListNode res=null;
	        while(head!=null){
	            stack.addLast(head);
	            head = head.next;
	        }
	        while(k>0){
	            res = stack.removeLast();
	            k--;
	        }
	        return res;
	    }
	}  
####时间复杂度 O(N) 
####空间复杂度 O(N)

###方法二：双指针  

	class Solution {
	    public ListNode getKthFromEnd(ListNode head, int k) {
	        ListNode former = head, latter = head;
	        for(int i = 0; i < k; i++)
	            former = former.next;
	        while(former != null) {
	            former = former.next;
	            latter = latter.next;
	        }
	        return latter;
	    }
	}
####时间复杂度 O(N) ： N 为链表长度；总体看， former 走了 N 步， latter 走了 (N−k) 步。
####空间复杂度 O(1) ： 双指针 former , latter 使用常数大小的额外空间。


##37、反转链表  
###方法一：把值取出到栈，再加到链表里  
	
	class Solution {
	    public ListNode reverseList(ListNode head) {
	        LinkedList<Integer> stack = new LinkedList<>();
	        ListNode res = head;
	        ListNode res1 = head;
	        while(head!=null){
	            stack.addLast(head.val);
	            head=head.next;
	        } 
	        int i = stack.size();
	        while(i>0){
	            res.val = stack.removeLast();
	            res=res.next;
	            i--;           
	        }
	        return res1;
	    }
	}
###方法二：迭代  

	class Solution {
	    public ListNode reverseList(ListNode head) {
	        ListNode prev = null;
	        ListNode curr = head;
	        while (curr != null) {
	            ListNode next = curr.next;
	            curr.next = prev;
	            prev = curr;
	            curr = next;
	        }
	        return prev;
	    }
	}

时间复杂度：O(n)，其中 n 是链表的长度。需要遍历链表一次。

空间复杂度：O(1)。  

###方法三：递归  

	class Solution {
	    public ListNode reverseList(ListNode head) {
	        if (head == null || head.next == null) {
	            return head;
	        }
	        ListNode newHead = reverseList(head.next);
	        head.next.next = head;
	        head.next = null;
	        return newHead;
	    }
	}

时间复杂度：O(n)，其中 n 是链表的长度。需要对链表的每个节点进行反转操作。

空间复杂度：O(n)，其中 nn 是链表的长度。空间复杂度主要取决于递归调用的栈空间，最多为 n 层。

##38、合并两个排序的链表  
###方法：双指针  

	/**
	 * Definition for singly-linked list.
	 * public class ListNode {
	 *     int val;
	 *     ListNode next;
	 *     ListNode(int x) { val = x; }
	 * }
	 */
	class Solution {
	    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
	        ListNode res = new ListNode(0),dum = res;
	        while(l1!=null&&l2!=null){
	            if(l1.val>l2.val){
	                res.next=l2;
	                l2=l2.next;
	            }else{
	                res.next=l1;
	                l1=l1.next;
	            }          
	            res=res.next;
	        }
	        res.next = l1 != null ? l1 : l2;
	        return dum.next;
	    }
	}  

####时间复杂度 O(M+N) ： M, N分别为两链表的长度，合并操作需遍历两链表。
####空间复杂度 O(1) ： 节点引用 dum , cur 使用常数大小的额外空间。  

##39、两个链表的第一个公共节点   
###方法：另两个节点走相同的长度  

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
	        ListNode a = headA;
	        ListNode b = headB;
	        while(a!=b){
	            if(a!=null){
	                a=a.next;
	            }else{
	                a=headB;
	            }
	            if(b!=null){
	                b=b.next;
	            }else{
	                b=headA;base
	            }            
	        }
	        return a;       
	    }
	}  

简便写法：  

	public class Solution {
	    public ListNode getIntersectionNode(ListNode headA, ListNode headB) {
	        ListNode A = headA, B = headB;
	        while (A != B) {
	            A = A != null ? A.next : headB;
	            B = B != null ? B.next : headA;
	        }
	        return A;
	    }
	}

####时间复杂度 O(a+b) ： 最差情况下（即 ∣a−b∣=1 , c=0 ），此时需遍历 a+b 个节点。
####空间复杂度 O(1) ： 节点指针 A , B 使用常数大小的额外空间。  


###知识点：
>堆排序的时间复杂度O(nlogn)   

##40、
