# 638. Shopping Offers

> In LeetCode Store, there are some kinds of items to sell. Each item has a price.
> 
> However, there are some special offers, and a special offer consists of one or more different kinds of items with a sale price.
> 
> You are given the each item's price, a set of special offers, and the number we need to buy for each item. The job is to output the lowest price you have to pay for exactly certain items as given, where you could make optimal use of the special offers.
> 
> Each special offer is represented in the form of an array, the last number represents the price you need to pay for this special offer, other numbers represents how many specific items you could get if you buy this offer.
> 
> You could use any of special offers as many times as you want.

输入给了三个数组，一个是价格数组，一个是组合购买的数组，一个是所需数量的数组。

可以组合购买，也可以单个购买，条件是不能超过购买数量必须等于所需数量，且总价最低。

解题思路：

首先排除边界，所需全为0时的情况。

这一题可按照dp的思路写，也可按照dfs的思路写。

这里我们使用比较容易理解的方法，用dfs来解决。

每次先算按当前传入需求清单单价买的价格，
然后对组合购买数组循环，
若数量在需求范围内，就把剩余所需需求数量存入临时数组temp中。并按照dfs的方式向下递归。
若不在范围内，就不再继续递归。
这样一层层递归下去，就可以找到最小的价格。

```Java
class Solution {
    public int shoppingOffers(List<Integer> price, List<List<Integer>> special, List<Integer> needs) {
        if(needs.stream().allMatch(x -> x == 0)) return 0;
        return helper(price, special, needs, 0);
        
    }
    
    private int helper(List<Integer> price, List<List<Integer>> special, List<Integer> needs, int pos) {
    	int local_min = directPurchase(price, needs);
    	for (int i = pos; i < special.size(); i++) {
    		List<Integer> offer = special.get(i);
    		List<Integer> temp = new ArrayList<Integer>();
        	for (int j= 0; j < needs.size(); j++) {
        		if (needs.get(j) < offer.get(j)) { // check if the current offer is valid
        			temp =  null;
        			break;
        		}
        		temp.add(needs.get(j) - offer.get(j));
        	}
        	
    		if (temp != null) { // use the current offer and try next
    			local_min = Math.min(local_min, offer.get(offer.size() - 1) + helper(price,                     special, temp, i)); 
    		}
    	}

    	return  local_min;
    }
    
    private int directPurchase(List<Integer> price, List<Integer> needs) {
    	int total = 0;
    	for (int i = 0; i < needs.size(); i++) {
    		total += price.get(i) * needs.get(i);
    	}
    	
    	return total;
    }
}
```
