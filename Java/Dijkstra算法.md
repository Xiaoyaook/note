# Dijkstra算法

Dijkstra算法：用于单源最短路径的求解。用于计算给定的一个源节点到其他各个节点之间的最短路径。

该算法的思路是：已选定的节点集为A，待选的节点集为B。开始时节点A中仅包含源节点。

选择B中距离源节点最近的一个点，然后将它加入到节点集A中。该点加入后，更新B中所有节点到A的路径长度（借助刚加入的点）。之后重复这一步骤，直至所有点都加入。

算法实现：

```Java
public class Dijkstra {
	
	private final int INF = Integer.MAX_VALUE;
	
	int[][] Matrix;
	char[] Nodes;
	
	public Dijkstra(char[] Nodes, int[][] Matrix){
		this.Nodes = Nodes;
		this.Matrix = Matrix;
	}
	
	/**
	 * dijkstra算法
	 * 
	 * 参数说明：
	 * @param node  起点
	 * @param distance 长度数组，distance[i]表示从起点到i点的最短路径
	 */
	public void dijkstra(int node, int[] distance){
		
		boolean[] flag = new boolean[Nodes.length];
		
		//初始化
		for(int i=0; i<Nodes.length; i++){
			flag[i] = false;
			distance[i] = Matrix[node][i];
		}
		
		//对顶点node本身进行初始化
		flag[node] = true;
		distance[node] = 0;
		
		//遍历Nodes.length - 1次，每次找出一个顶点点最短路径
		int k = 0;
		for(int i=1; i<Nodes.length; i++){
			int min = INF;
			
			//寻找最短路径
			for(int j=0; j<Nodes.length; j++){
				if(flag[j] == false && distance[j] < min){
					k = j;
					min = distance[j];
				}
			}
			flag[k] = true;
			
			//更新Matrix点值
			for(int j=0; j<Nodes.length; j++){
				int len = Matrix[k][j] == INF ? INF : min + Matrix[k][j];
				if(flag[j] == false && len < distance[j]){
					distance[j] = len;
				}
			}
		}
		
		System.out.printf("Dijkstra(%c): \n", Nodes[node]);
		
		for (int i=0; i < Nodes.length; i++) 
            System.out.printf("  shortest(%c, %c)=%d\n", Nodes[node], Nodes[i], distance[i]);
		
	}
	
}
```

---
[Dijkstra算法的java实现](https://blog.csdn.net/sinat_22013331/article/details/50999446)