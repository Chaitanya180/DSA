# DSA
## Search in Rotated Sorted Array
```
 class Solution {
    public int search(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1; // Corrected to nums.length - 1

        while (left <= right) {
            int mid = left + (right - left) / 2;

            if (nums[mid] == target) {
                return mid; // Found the target
            }

            // Check which half is sorted
            if (nums[left] <= nums[mid]) { // Left half is sorted
                if (nums[left] <= target && target < nums[mid]) {
                    right = mid - 1; // Search left half
                } else {
                    left = mid + 1; // Search right half
                }
            } else { // Right half is sorted
                if (nums[mid] < target && target <= nums[right]) {
                    left = mid + 1; // Search right half
                } else {
                    right = mid - 1; // Search left half
                }
            }
        }

        return -1; // Target not found
    }
}

```
## STACK INFIX AND POSTFIX EVALUATION
```

import java.util.HashMap;
import java.util.Stack;

class Solution {
    public int calculate(String s) {
        // Remove all spaces from the input string
        s = s.replace(" ", "");
        
        // Convert infix expression to postfix
        s = postfix(s);
        
        // Evaluate the postfix expression and return the result
        return evaluatePostfix(s);
    }

    public static String postfix(String s) {
        HashMap<Character, Integer> precedenceMap = new HashMap<>();
        StringBuilder postfixBuilder = new StringBuilder();
        
        // Define operator precedence
        precedenceMap.put('+', 1);
        precedenceMap.put('-', 1);
        precedenceMap.put('*', 2);
        precedenceMap.put('/', 2);
        
        Stack<Character> operatorStack = new Stack<>();
        
        for (char c : s.toCharArray()) {
            if (Character.isDigit(c)) {
                // Append digits directly to the postfix expression
                postfixBuilder.append(c);
            } else if (c == '(') {
                // Push opening parentheses to the stack
                operatorStack.push(c);
            } else if (c == ')') {
                // Pop from stack to postfixBuilder until an opening parenthesis is encountered
                while (!operatorStack.isEmpty() && operatorStack.peek() != '(') {
                    postfixBuilder.append(operatorStack.pop());
                }
                operatorStack.pop(); // Remove the '(' from the stack
            } else {
                // Handle operators
                while (!operatorStack.isEmpty() && precedenceMap.containsKey(operatorStack.peek())
                        && precedenceMap.get(c) <= precedenceMap.get(operatorStack.peek())) {
                    postfixBuilder.append(operatorStack.pop());
                }
                operatorStack.push(c);
            }
        }

        // Pop remaining operators from the stack
        while (!operatorStack.isEmpty()) {
            postfixBuilder.append(operatorStack.pop());
        }

        return postfixBuilder.toString();
    }

    public static int evaluatePostfix(String s) {
        Stack<Integer> valueStack = new Stack<>();
        
        for (char c : s.toCharArray()) {
            if (Character.isDigit(c)) {
                // Push digits onto the stack
                valueStack.push(c - '0');
            } else {
                // Pop two operands for the operator
                int b = valueStack.pop();
                int a = valueStack.pop();
                
                // Apply the operator and push the result back to the stack
                switch (c) {
                    case '+':
                        valueStack.push(a + b);
                        break;
                    case '-':
                        valueStack.push(a - b);
                        break;
                    case '*':
                        valueStack.push(a * b);
                        break;
                    case '/':
                        valueStack.push(a / b);
                        break;
                }
            }
        }
        
        // The final result will be the only value in the stack
        return valueStack.pop();
    }

    public static void main(String[] args) {
        Solution sol = new Solution();
        System.out.println(sol.calculate("3 + 2 * 2")); // Expected output: 7
        System.out.println(sol.calculate(" 3/2 "));     // Expected output: 1
        System.out.println(sol.calculate(" 3+5 / 2 ")); // Expected output: 5
    }
}

```

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

## DETECT CYCLE IN DIRECTED GRAPH USING BFS
```
import java.util.*;


class Solution {
    private boolean dfsCheck(int node, ArrayList<ArrayList<Integer>> adj, int vis[], int pathVis[]) {
        vis[node] = 1; 
        pathVis[node] = 1; 
        
        // traverse for adjacent nodes 
        for(int it : adj.get(node)) {
            // when the node is not visited 
            if(vis[it] == 0) {
                if(dfsCheck(it, adj, vis, pathVis) == true) 
                    return true; 
            }
            // if the node has been previously visited
            // but it has to be visited on the same path 
            else if(pathVis[it] == 1) {
                return true; 
            }
        }
        
        pathVis[node] = 0; 
        return false; 
    }

    // Function to detect cycle in a directed graph.
    public boolean isCyclic(int V, ArrayList<ArrayList<Integer>> adj) {
        int vis[] = new int[V];
        int pathVis[] = new int[V];
        
        for(int i = 0;i<V;i++) {
            if(vis[i] == 0) {
                if(dfsCheck(i, adj, vis, pathVis) == true) return true; 
            }
        }
        return false; 
    }
}

public class tUf {
    public static void main(String[] args) {
        int V = 11;
        ArrayList<ArrayList<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < V; i++) {
            adj.add(new ArrayList<>());
        }
        adj.get(1).add(2);
        adj.get(2).add(3);
        adj.get(3).add(4);
        adj.get(3).add(7);
        adj.get(4).add(5);
        adj.get(5).add(6);
        adj.get(7).add(5);
        adj.get(8).add(9);
        adj.get(9).add(10);
        adj.get(10).add(8);

        Solution obj = new Solution();
        boolean ans = obj.isCyclic(V, adj);
        if (ans)
            System.out.println("True");
        else
            System.out.println("False");

    }
}

```
## Is Graph Bipartite?
```
#### using DFS
class Solution {
    public boolean isBipartite(int[][] graph) {
        int n=graph.length;
        int col[]=new int[n];
        Arrays.fill(col,-1);

        for(int i=0;i<n;i++){
            if(col[i]==-1){
                if(!dfs(i,0,graph,col)){
                    return false;
                }
            }
        }
        return true;
        
    }

    public boolean dfs(int i,int c,int[][] graph,int col[]){
        col[i]=c;


        for(int j:graph[i]){
            if(col[j]==-1){
            if(!dfs(j,1-c,graph,col)){
                return false;
            }
            }
            else if(col[i]==col[j]){
               return false;
            }
            
        }

        return true;


    }
}


#### USING BFS

class Solution {
    public boolean isBipartite(int[][] graph) {
        int n=graph.length;
        int color[]=new int[n];
        Arrays.fill(color,-1);
        Queue<Integer> q=new LinkedList<>();
        
        for(int k=0;k<n;k++){
        if(color[k]==-1){
            q.add(k);
          color[k]=0;
        while(!q.isEmpty()){
           int i=q.poll();

           for(int j:graph[i]){
               if(color[j]==-1){
                color[j]=1-color[i];
                q.add(j);
               }else if(color[j]==color[i]){
                      return false;
               }
                
               
           }
        }
        }
        }

        return true;

        
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

