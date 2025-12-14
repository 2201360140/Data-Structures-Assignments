# Assignment No 42

## Write a Program to implement Prim’s algorithm to find minimum spanning tree of a user defined graph. Use Adjacency List to represent a graph.

## Theory

This program implements Prim's algorithm to find the Minimum Spanning Tree (MST) of a user-defined graph using an adjacency list representation. Prim's algorithm is a greedy algorithm that starts from an arbitrary vertex and grows the MST by adding the smallest edge that connects a vertex in the MST to a vertex outside it, until all vertices are included.

Key features of the implementation:
- **Adjacency List Representation**: A vector of vectors where each inner vector holds pairs of (weight, vertex) for connected edges. This is efficient for sparse graphs with O(V + E) space complexity.
- **Prim's Algorithm**: Uses a priority queue (min-heap) to always select the edge with the minimum weight. It maintains arrays for key values (minimum edge weights), inMST (to track included vertices), and parent (to reconstruct the tree).
- **Time Complexity**: O((V + E) log V) due to priority queue operations, where V is vertices and E is edges.
- **Space Complexity**: O(V + E) for the adjacency list and auxiliary arrays.

The algorithm ensures the graph is connected and undirected, producing a tree that connects all vertices with the minimum total edge weight.

## Code

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <climits>

using namespace std;

typedef pair<int, int> pii; // pair<weight, vertex>

void primMST(const vector<vector<pii>> &adj, int n, int start)
{
    vector<int> key(n, INT_MAX);                       // Minimum weight to connect to MST
    vector<int> parent(n, -1);                         // Parent of each vertex in MST
    vector<bool> inMST(n, false);                      // Whether vertex is in MST
    priority_queue<pii, vector<pii>, greater<pii>> pq; // Min-heap: {weight, vertex}

    key[start] = 0;
    pq.push({0, start});

    int totalWeight = 0;

    while (!pq.empty())
    {
        int u = pq.top().second;
        pq.pop();

        if (inMST[u])
            continue; // Skip if already in MST
        inMST[u] = true;
        totalWeight += key[u];

        for (auto &neighbor : adj[u])
        {
            int v = neighbor.first;
            int weight = neighbor.second;

            if (!inMST[v] && weight < key[v])
            {
                key[v] = weight;
                parent[v] = u;
                pq.push({key[v], v});
            }
        }
    }

    // Output the MST
    cout << "Minimum Spanning Tree Edges:" << endl;
    for (int i = 0; i < n; i++)
    {
        if (parent[i] != -1)
        {
            cout << parent[i] << " - " << i << " (weight: " << key[i] << ")" << endl;
        }
    }
    cout << "Total Weight of MST: " << totalWeight << endl;
}

int main()
{
    int n, e;
    cout << "Enter the number of vertices: ";
    cin >> n;
    cout << "Enter the number of edges: ";
    cin >> e;

    vector<vector<pii>> adj(n);

    cout << "Enter the edges (u v weight) where u and v are vertices (0 to " << n - 1 << "):" << endl;
    for (int i = 0; i < e; i++)
    {
        int u, v, w;
        cin >> u >> v >> w;
        adj[u].push_back({v, w});
        adj[v].push_back({u, w}); // Undirected graph
    }

    int start;
    cout << "Enter the starting vertex for Prim's algorithm: ";
    cin >> start;

    primMST(adj, n, start);

    return 0;
}


```

## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS42.exe'
Enter the number of vertices: 5
Enter the number of edges: 6
Enter the edges (u v weight) where u and v are vertices (0 to 4):
0 1 2
0 3 6
1 2 3
1 3 8
2 4 7
Enter the starting vertex for Prim's algorithm: 0
Minimum Spanning Tree Edges:
0 - 1 (weight: 2)
1 - 2 (weight: 3)
0 - 3 (weight: 6)
1 - 4 (weight: 5)
Total Weight of MST: 16

```

## Conclusion

This program effectively demonstrates Prim's algorithm for finding the Minimum Spanning Tree using an adjacency list, which is suitable for sparse graphs. The implementation uses a priority queue for efficient edge selection, ensuring optimal performance. By allowing user-defined graphs, it provides a practical tool for understanding MST concepts. The adjacency list representation offers better space efficiency compared to matrices for graphs with fewer edges, and the algorithm guarantees a minimum weight tree connecting all vertices, applicable in network design, clustering, and optimization problems.
