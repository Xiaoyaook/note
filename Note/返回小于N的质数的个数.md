# 返回小于N的质数的个数

想法：
* 直接能想到的就是试除法，在2~n-1中取数循环取余
* 然后我们可以对这个思路进行优化，无需取到n-1，只需到Math.sqrt(n)，即n的开平方就行
* 不过这个想法并非最优，为了使性能最优，我们使用筛选法，即**埃氏筛选法**和**欧拉筛选法**

## 埃氏筛选法

Eratosthenes筛选法虽然效率高，但是Eratosthenes筛选法做了许多无用功，一个数会被筛到好几次，最后的时间复杂度是O(nloglogn)

思路：

先去掉2的倍数，再去掉3的倍数，再去掉4的倍数，……依此类推，最后剩下的就是素数。 
如求100以内的素数，我们只要到去掉Math.sqrt(100)的倍数就可以了，这是因为10的2倍已经被2的倍数去掉了，10的3倍已经被3的倍数去掉了，所以到10的时候只剩下10的10倍以上的素数还存在。 
同样的原因，我们在去掉3的倍数的时候，只要从3的3倍开始去掉就好了，因为3的2倍已经被2的倍数去掉了。

Java实现
```Java
public class PrimeNumber {
    //求素数
    public static void IsPrime(int n){

        boolean[] mark = new boolean[n+1];
        //用于标记，true表示是素数，false表示非素数
        for(int i=2;i<=n;i++){
            mark[i] = true;
        }

        for(int i=2;i<=Math.sqrt(n);i++){
            if(mark[i] == true){
                for(int j = i;j*i<=n;j++){
                    mark[i*j] = false;
                }
            }
        }

        int count=0; // 开始计数
        for(int i = 2;i<=n;i++){
            if(mark[i] == true){
                count++;
                System.out.print(i + " ");
                if(count%10 == 0){  // 一行只打印10个数
                    System.out.println();
                }
            }
        }
    }

}
```

## 欧拉筛选法

欧拉算法是一种空间换时间的算法，时间复杂度仅仅为O(n)

给出一个别人的实现：
```Java
public class Prime2 {
    public static int calculateNumber(int Nmax) {
        boolean[] isPrime = new boolean[Nmax + 1];
        int[] prime = new int[Nmax / 10];
        int totalPrimes = 1;
        for (int i=3; i<= Nmax; i+=2) isPrime[i] = true;
        isPrime[2] = true;
        prime[0] = 2;
        for (int i=3; i<=Nmax; i+=2) {
            if (isPrime[i]) prime[totalPrimes++] = i;
            for (int j=1; i*prime[j]<=Nmax && j < totalPrimes;j++) {
                isPrime[i*prime[j]] = false;
                if (i%prime[j]==0) break;
            }
        }
        return totalPrimes;
    }

    public static void main(String[] args) {
        final int Nmax = 2000000;
        double startTime = System.currentTimeMillis();
        int primeNum = Prime2.calculateNumber(Nmax);
        double timeSpent = (System.currentTimeMillis() - startTime) / 1000;
        System.out.println("The prime numbers from 1 to " + Nmax + " is " + primeNum);
        System.out.println("Time spent : " + timeSpent + " s");
    }
}
```

---

知乎上对此的讨论[如何编程最快速度求出两百万以内素数个数（不限语言和算法）？](https://www.zhihu.com/question/24942373/answer/29599552)