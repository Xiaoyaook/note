# Largest Number

Given a list of non negative integers, arrange them such that they form the largest number.

给一个由非负整数构成的列表，请给出由这些整数可组合成的最大整数，由于整数可能过大，使用字符串来表示。

## 解题思路

首先把整数数组转换成字符串数组。

核心部分就是创建一个String比较器，

这里要注意，我们比较的是**两个字符串相加**的情况，所以对于两个字符串来说，只有两种情况，比较一下即可。

```Java
class Solution {
    public String largestNumber(int[] nums) {
        if(nums == null || nums.length == 0)
		    return "";
		
		// 把int数组转换为string数组
		String[] s_num = new String[nums.length];
		for(int i = 0; i < nums.length; i++)
		    s_num[i] = String.valueOf(nums[i]);
			
		// 创建一个比较器为string数组排序
		Comparator<String> comp = new Comparator<String>(){
		    @Override
		    public int compare(String str1, String str2){
		        String s1 = str1 + str2;
                String s2 = str2 + str1;
                return s2.compareTo(s1); // 从大到小排列
                }
	        };
		
		Arrays.sort(s_num, comp);
        // 考虑边界情况，当数组中的数都为0时，返回0
        if(s_num[0].charAt(0) == '0')
            return "0";
            
		StringBuilder sb = new StringBuilder();
		for(String s: s_num)
	            sb.append(s);
		
		return sb.toString();
    }
}
```