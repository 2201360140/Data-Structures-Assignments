# Assignment 41

## Write a Program to accept a graph from a user and represent it with Adjacency Matrix and perform BFS and DFS traversals on it.

## Theory

This program implements graph representation using an adjacency matrix and performs Breadth-First Search (BFS) and Depth-First Search (DFS) traversals. The graph is represented as an undirected graph where vertices are numbered from 0 to n-1, and edges are stored in a 2D vector matrix.

Key features of the implementation:
- **Adjacency Matrix Representation**: A 2D vector of size n x n where n is the number of vertices. A value of 1 indicates an edge between vertices, 0 indicates no edge.
- **BFS Traversal**: Uses a queue to explore vertices level by level, starting from a given vertex. It visits all reachable vertices in breadth-first order.
- **DFS Traversal**: Uses a stack to explore vertices by going as deep as possible along each branch before backtracking. The implementation uses an iterative approach with a stack.

The adjacency matrix provides O(1) time for edge queries but uses O(n²) space. BFS and DFS both have O(n + e) time complexity where n is vertices and e is edges.

## Code

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <stack>

using namespace std;

void bfs(const vector<vector<int>>& adj, int start, int n) {
    vector<bool> visited(n, false);
    queue<int> q;
    q.push(start);
    visited[start] = true;
    cout << "BFS Traversal: ";
    while (!q.empty()) {
        int u = q.front();
        q.pop();
        cout << u << " ";
        for (int v = 0; v < n; v++) {
            if (adj[u][v] && !visited[v]) {
                visited[v] = true;
                q.push(v);
            }
        }
    }
    cout << endl;
}

void dfs(const vector<vector<int>>& adj, int u, vector<bool>& visited, int n) {
    visited[u] = true;
    cout << u << " ";
    for (int v = 0; v < n; v++) {
        if (adj[u][v] && !visited[v]) {
            dfs(adj, v, visited, n);
        }
    }
}

void dfsTraversal(const vector<vector<int>>& adj, int start, int n) {
    vector<bool> visited(n, false);
    cout << "DFS Traversal: ";
    dfs(adj, start, visited, n);
    cout << endl;
}

int main() {
    int n, e;
    cout << "Enter the number of vertices: ";
    cin >> n;
    cout << "Enter the number of edges: ";
    cin >> e;

    vector<vector<int>> adj(n, vector<int>(n, 0));

    cout << "Enter the edges (u v) where u and v are vertices (0 to " << n-1 << "):" << endl;
    for (int i = 0; i < e; i++) {
        int u, v;
        cin >> u >> v;
        adj[u][v] = 1;
        adj[v][u] = 1; // Assuming undirected graph
    }

    int start;
    cout << "Enter the starting vertex for traversals: ";
    cin >> start;

    bfs(adj, start, n);
    dfsTraversal(adj, start, n);

    return 0;
}

```

## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS41.exe'
Enter the number of vertices: 5
Enter the number of edges: 4
Enter the edges (u v) where u and v are vertices (0 to 4):
0 1
0 2
1 3
2 4
Enter the starting vertex for traversals: 0
BFS Traversal: 0 1 2 3 4 
DFS Traversal: 0 1 3 2 4 

```

## Conclusion

This program demonstrates the fundamental concepts of graph representation and traversal algorithms. The adjacency matrix provides a simple and efficient way to store graph connectivity, especially for dense graphs. BFS explores the graph level by level, useful for finding shortest paths in unweighted graphs, while DFS explores deeply along branches, useful for topological sorting and detecting cycles. The iterative implementations using queue and stack ensure efficiency and avoid recursion depth limits for large graphs. This implementation serves as a foundation for more complex graph algorithms and applications in computer science.
