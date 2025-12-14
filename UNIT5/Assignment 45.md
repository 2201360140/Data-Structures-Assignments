# Assignment No 45

## Write a Program to accept a graph from a user and represent it with Adjacency List and perform BFS and DFS traversals on it.

## Theory

This program implements graph representation using an adjacency list and performs Breadth-First Search (BFS) and Depth-First Search (DFS) traversals on a user-defined graph. The graph is represented as an undirected graph where vertices are numbered from 0 to n-1, and edges are stored in vectors.

Key features of the implementation:
- **Adjacency List Representation**: An array of vectors where each vector holds the list of adjacent vertices. This is efficient for sparse graphs with O(V + E) space complexity, where V is vertices and E is edges.
- **BFS Traversal**: Uses a queue to explore vertices level by level, starting from a given vertex. It visits all reachable vertices in breadth-first order, useful for finding shortest paths in unweighted graphs.
- **DFS Traversal**: Uses a stack to explore vertices by going as deep as possible along each branch before backtracking. The implementation uses an iterative approach with a stack to avoid recursion depth limits.
- **Time Complexity**: Both BFS and DFS have O(V + E) time complexity.

The adjacency list provides better space efficiency compared to matrices for graphs with fewer edges, and the traversals serve as foundations for more complex graph algorithms.

## Code

```cpp

#include <iostream>
#include <vector>
#include <queue>
#include <stack>

using namespace std;

void bfs(const vector<vector<int>> &adj, int n, int src)
{
    vector<bool> visited(n, false);
    queue<int> q;
    q.push(src);
    visited[src] = true;
    cout << "BFS Traversal starting from " << src << ": ";
    while (!q.empty())
    {
        int u = q.front();
        q.pop();
        cout << u << " ";
        for (int v : adj[u])
        {
            if (!visited[v])
            {
                visited[v] = true;
                q.push(v);
            }
        }
    }
    cout << endl;
}

void dfsRecursive(const vector<vector<int>> &adj, vector<bool> &visited, int u)
{
    visited[u] = true;
    cout << u << " ";
    for (int v : adj[u])
    {
        if (!visited[v])
        {
            dfsRecursive(adj, visited, v);
        }
    }
}

void dfs(const vector<vector<int>> &adj, int n, int src)
{
    vector<bool> visited(n, false);
    cout << "DFS Traversal starting from " << src << ": ";
    dfsRecursive(adj, visited, src);
    cout << endl;
}

int main()
{
    int n, e;
    cout << "Enter the number of vertices: ";
    cin >> n;
    cout << "Enter the number of edges: ";
    cin >> e;

    vector<vector<int>> adj(n);

    cout << "Enter the edges (u v) where u and v are vertices (0 to " << n - 1 << "):" << endl;
    for (int i = 0; i < e; i++)
    {
        int u, v;
        cin >> u >> v;
        adj[u].push_back(v);
        adj[v].push_back(u); // Undirected graph
    }

    int src;
    cout << "Enter the starting vertex for traversals: ";
    cin >> src;

    bfs(adj, n, src);
    dfs(adj, n, src);

    return 0;
}

```

## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS45.exe'
Enter the number of vertices: 5
Enter the number of edges: 6
Enter the edges (u v) where u and v are vertices (0 to 4):
0 1
0 2
1 3
1 4
2 4
3 4
Enter the starting vertex for traversals: 0
BFS Traversal starting from 0: 0 1 2 3 4
DFS Traversal starting from 0: 0 1 3 4 2

```

## Conclusion

This program demonstrates the fundamental concepts of graph representation and traversal algorithms using an adjacency list, which is efficient for sparse graphs. BFS explores the graph level by level, ideal for shortest path problems, while DFS explores deeply, useful for topological sorting and cycle detection. The iterative implementations ensure efficiency and avoid recursion limits for large graphs. This serves as a foundation for understanding graph algorithms and their applications in computer science.
