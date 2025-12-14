# Assignment No 43

## Write a Program to implement Kruskal’s algorithm to find the minimum spanning tree of a user defined graph. Use Adjacency List to represent a graph.

## Theory

This program implements Kruskal's algorithm to find the Minimum Spanning Tree (MST) of a user-defined graph using an adjacency list representation. Kruskal's algorithm is a greedy algorithm that sorts all edges by weight and adds them to the MST if they do not form a cycle, using a Union-Find data structure to detect cycles.

Key features of the implementation:
- **Adjacency List Representation**: A vector of vectors where each inner vector holds pairs of (vertex, weight) for connected edges. This is efficient for sparse graphs with O(V + E) space complexity.
- **Kruskal's Algorithm**: Sorts edges by weight and uses Union-Find with path compression and union by rank for efficient cycle detection. It builds the MST by adding edges that connect different components.
- **Union-Find Structure**: Includes makeSet, findSet (with path compression), and unionSets (with union by rank) to manage disjoint sets.
- **Time Complexity**: O(E log E) due to sorting edges, where E is edges; Union-Find operations are nearly O(1) amortized.
- **Space Complexity**: O(V + E) for the adjacency list and Union-Find arrays.

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
    bool operator<(const Edge &other) const
    {
        return weight < other.weight;
    }
};

class UnionFind
{
private:
    vector<int> parent, rank;

public:
    UnionFind(int n)
    {
        parent.resize(n);
        rank.resize(n, 0);
        for (int i = 0; i < n; i++)
            parent[i] = i;
    }
    int find(int x)
    {
        if (parent[x] != x)
            parent[x] = find(parent[x]);
        return parent[x];
    }
    void unite(int x, int y)
    {
        int px = find(x), py = find(y);
        if (px != py)
        {
            if (rank[px] < rank[py])
                parent[px] = py;
            else if (rank[px] > rank[py])
                parent[py] = px;
            else
            {
                parent[py] = px;
                rank[px]++;
            }
        }
    }
};

void kruskalMST(const vector<vector<pair<int, int>>> &adj, int n)
{
    vector<Edge> edges;
    for (int u = 0; u < n; u++)
    {
        for (auto &p : adj[u])
        {
            int v = p.first, w = p.second;
            if (u < v)
            { // To avoid duplicate edges in undirected graph
                edges.push_back({u, v, w});
            }
        }
    }

    sort(edges.begin(), edges.end());

    UnionFind uf(n);
    vector<Edge> mst;
    int totalWeight = 0;

    for (auto &edge : edges)
    {
        if (uf.find(edge.u) != uf.find(edge.v))
        {
            uf.unite(edge.u, edge.v);
            mst.push_back(edge);
            totalWeight += edge.weight;
        }
    }

    // Output the MST
    cout << "Minimum Spanning Tree Edges:" << endl;
    for (auto &edge : mst)
    {
        cout << edge.u << " - " << edge.v << " (weight: " << edge.weight << ")" << endl;
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

    vector<vector<pair<int, int>>> adj(n);

    cout << "Enter the edges (u v weight) where u and v are vertices (0 to " << n - 1 << "):" << endl;
    for (int i = 0; i < e; i++)
    {
        int u, v, w;
        cin >> u >> v >> w;
        adj[u].push_back({v, w});
        adj[v].push_back({u, w}); // Undirected graph
    }

    kruskalMST(adj, n);

    return 0;
}

```

## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS43.exe'
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

This program effectively demonstrates Kruskal's algorithm for finding the Minimum Spanning Tree using an adjacency list, which is ideal for sparse graphs. The implementation leverages sorting and Union-Find for cycle detection, ensuring efficiency. By allowing user-defined graphs, it provides a practical tool for understanding MST concepts. The adjacency list representation offers space efficiency, and the algorithm guarantees a minimum weight tree, applicable in network design and optimization problems.
