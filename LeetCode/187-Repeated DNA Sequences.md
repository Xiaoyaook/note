# Repeated DNA Sequences

> All DNA is composed of a series of nucleotides abbreviated as A, C, G, and T, for example: "ACGAATTCCG". When studying DNA, it is sometimes useful to identify repeated sequences within the DNA.
>
> Write a function to find all the 10-letter-long sequences (substrings) that occur more than once in a DNA molecule.

字符串中有四个字母，A，C，G，T，找到所有出现两次以上的十个字母长的子串

## 解题思路

直接利用Set的性质

```Java
class Solution {
    public List<String> findRepeatedDnaSequences(String s) {
        Set seen = new HashSet(), repeated = new HashSet();
        for (int i = 0; i + 9 < s.length(); i++) {
            String ten = s.substring(i, i + 10);
            if (!seen.add(ten))
                repeated.add(ten);
        }
        return new ArrayList(repeated);
    }
}
```