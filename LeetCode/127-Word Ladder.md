# Word Ladder

> Given two words (beginWord and endWord), and a dictionary's word list, find the length of shortest transformation sequence from beginWord to endWord, such that:
>
>Only one letter can be changed at a time.
Each transformed word must exist in the word list. Note that beginWord is not a transformed word.
>
>Note:
>
>Return 0 if there is no such transformation sequence.
All words have the same length.
All words contain only lowercase alphabetic characters.
You may assume no duplicates in the word list.
You may assume beginWord and endWord are non-empty and are not the same.

假设输入两个单词beginWord，endWord以及一个字典。每一次操作只能对当前单词修改一个字符成为字典中的一个单词。问至少需要多少次操作可以将beginWord转化为endWord。如果不可转换则返回0。

## 解题思路

BFS解决。

将每一层上可以生成的单词全部列出来，然后再逐层遍历，一旦出现一个单词等于endWord，那么遍历终止，将当前遍历的次数返回。这里要注意的是，需要将已经遍历的单词记录下来，从而不会发生重复的情况。则返回

```Java
class Solution {
    //和字典中单词比较是否可以转换
    public boolean canTransform(String word1, String word2){
         for(int i = 0, count=0 ; i<word1.length() ; i++){
             if(word1.charAt(i)!=word2.charAt(i) && ++count>1) return false;
         }
         return true;
     }
 
     public int ladderLength(String beginWord, String endWord, List<String> wordList){
         Queue<String> q = new LinkedList<String>();
         q.add(beginWord);
         int steps = 0;
         while(!q.isEmpty()){
             int size = q.size();
             // 每次循环，相当于BFS的一层。
             for(int i = 0 ;i<size ; i++){
                 String temp = q.poll();
                 // 如果当前队列元素等于最终结果，则返回。
                 if(temp.equals(endWord)){return steps+1;}
                 for(Iterator<String> iterator = wordList.iterator() ; iterator.hasNext();){
                     String current = iterator.next();
                     // 如果当前循环的wordlist元素，可以被替换，就把该元素加入队列，并从wordlist中移除，避免重复添加。
                     if(canTransform(current, temp)){
                         iterator.remove();
                         q.offer(current);
                     }
                 }
             }
            steps++;

         }
         return 0;
     }
}
```

---
[leetcode127. Word Ladder](https://segmentfault.com/a/1190000010639103)