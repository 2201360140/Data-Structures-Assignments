# Assignment No 49

# Write a Program to implement Kruskal’s algorithm to find the minimum spanning tree of a user defined graph. Use Adjacency List to represent a graph.

## Theory
Kruskal’s algorithm is a greedy algorithm that finds a minimum spanning tree (MST) for a weighted undirected graph. It works by sorting all the edges in non-decreasing order of their weight and adding them to the MST one by one, as long as they do not form a cycle. The algorithm uses a disjoint-set data structure (Union-Find) to detect cycles efficiently.

The algorithm assumes the graph is connected and undirected with non-negative edge weights. It starts by sorting all edges by weight. Then, it iterates through the sorted edges, adding an edge to the MST if it connects two different components (i.e., no cycle is formed). This is checked using the Union-Find structure, which supports finding the set of a vertex and merging sets.

Key components:
- **Adjacency List**: A list of lists where each index represents a vertex, and the list contains pairs of (neighbor, weight).
- **Edge List**: A collection of all edges with their weights, used for sorting.
- **Union-Find (Disjoint Set)**: Provides find and union operations to manage components and detect cycles.
  - **Find**: Determines the root of the set containing a vertex, with path compression for efficiency.
  - **Union**: Merges two sets, using union by rank to keep the tree balanced.

Time complexity: O(E log E) where E is the number of edges, dominated by sorting the edges.

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

    vector<vector<pair<int, int>>> adj(n); // Adjacency List: {neighbor, weight}

    cout << "Enter the edges (u v weight) where u and v are vertices (0 to " << n - 1 << "):" << endl;
    for (int i = 0; i < e; i++)
    {
        int u, v, w;
        cin >> u >> v >> w;
        adj[u].push_back({v, w});
        adj[v].push_back({u, w}); // Undirected graph
    }

    // Extract edges from adjacency list
    vector<Edge> edges;
    for (int i = 0; i < n; i++)
    {
        for (auto &p : adj[i])
        {
            int j = p.first;
            int w = p.second;
            if (i < j)
            { // To avoid duplicates
                edges.push_back({i, j, w});
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

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS49.exe'
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

Kruskal’s algorithm successfully constructs a minimum spanning tree for a connected undirected graph with non-negative weights using an adjacency list representation. The implementation uses a disjoint-set data structure with path compression and union by rank for efficient cycle detection. The program accepts user input for graph construction and displays the MST edges along with the total weight. This approach is efficient for sparse graphs and is widely used in network design and clustering applications where minimizing connection costs is key.
