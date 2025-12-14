# Assignment No 46

## Write a Program to implement Kruskal’s algorithm to find the minimum spanning tree of a user defined graph. Use Adjacency Matrix to represent a graph.

## Theory

This program implements Kruskal's algorithm to find the Minimum Spanning Tree (MST) of a user-defined graph using an adjacency matrix representation. Kruskal's algorithm is a greedy algorithm that sorts all edges by weight and adds them to the MST if they do not form a cycle, using a Union-Find data structure to detect cycles.

Key features of the implementation:
- **Adjacency Matrix Representation**: A 2D vector of size n x n where n is the number of vertices. A value indicates the edge weight between vertices, 0 indicates no edge. This uses O(n²) space, suitable for dense graphs.
- **Kruskal's Algorithm**: Sorts edges by weight and uses Union-Find with path compression and union by rank for efficient cycle detection. It builds the MST by adding edges that connect different components.
- **Union-Find Structure**: Includes find (with path compression) and unionSets operations to manage disjoint sets.
- **Time Complexity**: O(E log E) due to sorting edges, where E is edges; Union-Find operations are nearly O(1) amortized.
- **Space Complexity**: O(V²) for the adjacency matrix, where V is vertices.

The algorithm assumes the graph is connected and undirected, producing a tree with the minimum total edge weight.

## Code

```cpp
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

struct Edge
{
    int u, v, weight;
};

bool compare(const Edge &a, const Edge &b)
{
    return a.weight < b.weight;
}

int find(vector<int> &parent, int i)
{
    if (parent[i] != i)
    {
        parent[i] = find(parent, parent[i]);
    }
    return parent[i];
}

void unionSets(vector<int> &parent, vector<int> &rank, int x, int y)
{
    int xroot = find(parent, x);
    int yroot = find(parent, y);
    if (rank[xroot] < rank[yroot])
    {
        parent[xroot] = yroot;
    }
    else if (rank[xroot] > rank[yroot])
    {
        parent[yroot] = xroot;
    }
    else
    {
        parent[yroot] = xroot;
        rank[xroot]++;
    }
}

int main()
{
    int n, e;
    cout << "Enter the number of vertices: ";
    cin >> n;
    cout << "Enter the number of edges: ";
    cin >> e;

    vector<vector<int>> adj(n, vector<int>(n, 0)); // Adjacency Matrix

    cout << "Enter the edges (u v weight) where u and v are vertices (0 to " << n - 1 << "):" << endl;
    for (int i = 0; i < e; i++)
    {
        int u, v, w;
        cin >> u >> v >> w;
        adj[u][v] = w;
        adj[v][u] = w; // Undirected graph
    }

    // Extract edges from adjacency matrix
    vector<Edge> edges;
    for (int i = 0; i < n; i++)
    {
        for (int j = i + 1; j < n; j++)
        {
            if (adj[i][j] != 0)
            {
                edges.push_back({i, j, adj[i][j]});
            }
        }
    }

    // Sort edges by weight
    sort(edges.begin(), edges.end(), compare);

    // Union-Find
    vector<int> parent(n);
    vector<int> rank(n, 0);
    for (int i = 0; i < n; i++)
    {
        parent[i] = i;
    }

    vector<Edge> mst;
    int mstWeight = 0;

    for (auto &edge : edges)
    {
        int x = find(parent, edge.u);
        int y = find(parent, edge.v);
        if (x != y)
        {
            mst.push_back(edge);
            mstWeight += edge.weight;
            unionSets(parent, rank, x, y);
        }
    }

    // Output MST
    cout << "Minimum Spanning Tree:" << endl;
    for (auto &edge : mst)
    {
        cout << edge.u << " - " << edge.v << " : " << edge.weight << endl;
    }
    cout << "Total Weight: " << mstWeight << endl;

    return 0;
}

```

## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS46.exe'
Enter the number of vertices: 5
Enter the number of edges: 6
Enter the edges (u v weight) where u and v are vertices (0 to 4):
0 1 2
0 3 6
1 2 3
1 3 8
1 4 5
2 4 7
Minimum Spanning Tree Edges:
0 - 1 (weight: 2)
1 - 2 (weight: 3)
1 - 4 (weight: 5)
0 - 3 (weight: 6)
Total Weight of MST: 16

```

## Conclusion

This program effectively demonstrates Kruskal's algorithm for finding the Minimum Spanning Tree using an adjacency matrix, which is suitable for dense graphs. The implementation leverages sorting and Union-Find for cycle detection, ensuring efficiency. By allowing user-defined graphs, it provides a practical tool for understanding MST concepts. The adjacency matrix representation offers simplicity for edge queries, and the algorithm guarantees a minimum weight tree, applicable in network design and optimization problems.
