# DSA

## GRAPH CREATION USING ADJLIST  BFS(QUEUE) AND DFS(RECURSSION)

```

import java.util.*;

class Graph{
    int ver;
   List<List<Integer>> adj;
   boolean vis[];
   
    Graph(int v){
        this.ver=v;
        adj=new ArrayList<>();
        for(int i=0;i<v;i++){
            adj.add(new ArrayList<>());
        }
        vis=new boolean[v];
        
    }

    public void addedge(int s,int d){
        adj.get(s).add(d);
    }
### BFS
    public void bfs(int s){
        
            Queue<Integer> q=new LinkedList<>();
            vis[s]=true;
            q.add(s);

            while(!q.isEmpty()){
                int vertix=q.poll();
                System.out.print(vertix+" ");

                List<Integer> l=adj.get(vertix);

                for(Integer i:l){
                    if(!vis[i]){
                        q.add(i);
                        vis[i]=true;
                    }
                }

            }

    }
### DFS
    public void dfs(int s){
        vis[s]=true;
        System.out.print(s+" ");

        for(Integer i:adj.get(s)){
            if(!vis[i]){
                dfs(i);
            }
        }
    }

    public static void main(String[] args) {
        Graph g=new Graph(6);

        g.addedge(0, 1);
        g.addedge(2, 1);
        g.addedge(1, 2);
        g.addedge(0, 3); 
        g.addedge(3, 4);
        g.addedge(4, 5);
        g.addedge(5, 3);
        g.bfs(0);
        Arrays.fill(g.vis, false);
        System.out.println();
        g.dfs(0);
    
    }
}




```

## DETECT CYCLE IN UNDIRECTED GRAPH USING BFS
```
import java.util.*;

class Solution {
    public boolean isCycle(int V, ArrayList<ArrayList<Integer>> adj) {
        boolean[] visited = new boolean[V];
        
        for (int i = 0; i < V; i++) {
            if (!visited[i]) {
                if (detectCycle(i, adj, visited)) {
                    return true;
                }
            }
        }
        return false;
    }

    private boolean detectCycle(int s, ArrayList<ArrayList<Integer>> adj, boolean[] visited) {
        visited[s] = true;
        Queue<Pair> queue = new LinkedList<>();
        queue.add(new Pair(s, -1));
        
        while (!queue.isEmpty()) {
            int curr = queue.peek().node;
            int parent = queue.peek().parent;
            queue.poll();
            
            for (int neighbor : adj.get(curr)) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    queue.add(new Pair(neighbor, curr));
                } else if (neighbor != parent) {
                    return true;
                }
            }
        }
        return false;
    }
}

class Pair {
    int node;
    int parent;

    Pair(int node, int parent) {
        this.node = node;
        this.parent = parent;
    }
}


```
## BFS APPlICATION
```
class Solution {
    public int orangesRotting(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        Queue<Pair> q = new LinkedList<>();
        int[][] vis = new int[m][n];
        int freshOranges = 0;
        int maxTime = 0;

         Initialize the queue with all rotten oranges and count fresh oranges
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 2) {
                    q.add(new Pair(i, j, 0));
                    vis[i][j] = 2;
                } else {
                    vis[i][j] = 0;
                }

                if (grid[i][j] == 1) {
                    freshOranges++;
                }
            }
        }

        int[] dr = {1, -1, 0, 0};
        int[] dc = {0, 0, 1, -1};
        int rottedOranges = 0;

        while (!q.isEmpty()) {
            int r = q.peek().i;
            int c = q.peek().j;
            int time = q.peek().t;
            maxTime = Math.max(time, maxTime);
            q.remove();

            for (int k = 0; k < 4; k++) {
                int nr = r + dr[k];
                int nc = c + dc[k];
                if (nr >= 0 && nr < m && nc >= 0 && nc < n && vis[nr][nc] == 0 && grid[nr][nc] == 1) {
                    grid[nr][nc] = 2;
                    q.add(new Pair(nr, nc, time + 1));
                    vis[nr][nc] = 2;
                    rottedOranges++;
                }
            }
        }

        // If there are still fresh oranges left, return -1
        if (rottedOranges != freshOranges) {
            return -1;
        }

        return maxTime;
    }
}

class Pair {
    int i;
    int j;
    int t;

    Pair(int i, int j, int t) {
        this.i = i;
        this.j = j;
        this.t = t;
    }
}
```

