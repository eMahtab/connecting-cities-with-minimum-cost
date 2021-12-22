# Connecting cities with minimum cost
## https://leetcode.com/problems/connecting-cities-with-minimum-cost


# Implementation 1 : Minimum Spanning Tree (Prim's Algorithm)
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
# Implementation 2 : Minimum Spanning Tree (Kruskal's Algorithm using Union Find)
```java
class Solution {
    
    public int minimumCost(int n, int[][] connections) {
        int minCost = 0;
        int edgesConnected = 0;
        Arrays.sort(connections, (e1, e2) -> e1[2] - e2[2]);
        int[] parents = new int[n];
        int[] rank = new int[n];
        for(int i = 0; i < n; i++) {
            parents[i] = i;
            rank[i] = 1;
        }
        
        for(int[] connection : connections) {
            boolean flag = union(connection[0] - 1, connection[1] - 1, parents, rank);
            if(flag) {
                minCost += connection[2];
                edgesConnected++;
            }        
        }
        
        return edgesConnected == n - 1 ? minCost : -1;
    }
    
    private boolean union(int v1, int v2, int[] parents, int[] rank) {
        int leader1 = find(v1, parents);
        int leader2 = find(v2, parents);
        if(leader1 != leader2) {
            if(rank[leader1] > rank[leader2]) {
                parents[leader2] = leader1;
            } else if(rank[leader2] > rank[leader1]) {
                parents[leader1] = leader2;
            } else {
                parents[leader2] = leader1;
                rank[leader1]++;
            }
            return true;
        } else {
            return false;
        }
    }
    
    private int find(int v, int[] parents) {
        if(parents[v] == v)
            return parents[v];
        parents[v] = find(parents[v], parents);
        return parents[v];
    }
}
```

# References :
https://www.youtube.com/watch?v=Vw-sktU1zmc ( Hindi, very good explanation)
