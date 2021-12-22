# Connecting cities with minimum cost
## https://leetcode.com/problems/connecting-cities-with-minimum-cost


# Implementation : Minimum Spanning Tree (Prim's Algorithm)
```java
class Solution {
    class Edge {
        int v1, v2, cost;
        Edge(int v1, int v2, int cost){
            this.v1 = v1;
            this.v2 = v2;
            this.cost = cost;
        }
    }
    public int minimumCost(int n, int[][] connections) {
        Map<Integer,List<Edge>> map = new HashMap<>();
        for(int[] connection : connections) {
            int v1 = connection[0] - 1;
            int v2 = connection[1] - 1;
            map.putIfAbsent(v1, new ArrayList<Edge>());
            map.get(v1).add(new Edge(v1,v2,connection[2]));
            
            map.putIfAbsent(v2, new ArrayList<Edge>());
            map.get(v2).add(new Edge(v2,v1,connection[2]));
        }
        
        boolean[] visited = new boolean[n];
        PriorityQueue<int[]> pq = new PriorityQueue<>((v1, v2) -> v1[1] - v2[1]);
        pq.add(new int[]{0,0});
        
        int minCost = 0;
        int count = 0;
        while(!pq.isEmpty()) {
            int[] vertex = pq.remove();
            if(!visited[vertex[0]]) {
               minCost += vertex[1]; 
               visited[vertex[0]] = true;
               count++; 
                
               // add unvisited neighbors of current vertex to priority queue
               List<Edge> edges = map.get(vertex[0]);
               for(Edge edge : edges) {
                   if(!visited[edge.v2]) {
                       pq.add(new int[]{edge.v2, edge.cost});
                   }
               } 
            }
        }
        return count == n ? minCost : -1;
    }
}
```
