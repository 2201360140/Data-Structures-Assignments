# Assignment No 48

# Title : Write a Program to implement Prim’s algorithm to find minimum spanning tree of a user defined graph. Use Adjacency List to represent a graph.

## Theory
Prim’s algorithm is a greedy algorithm that finds a minimum spanning tree (MST) for a weighted undirected graph. It starts from an arbitrary vertex and grows the MST by adding the smallest edge that connects a vertex in the MST to a vertex outside it, without forming a cycle. The algorithm uses a priority queue to always select the next edge with the minimum weight.

The algorithm assumes the graph is connected and undirected with non-negative edge weights. It maintains three arrays: key (minimum weight to connect to MST), parent (to track the MST edges), and visited (to mark included vertices). It begins by initializing the key of the start vertex to 0 and pushing it into the priority queue. In each step, it extracts the vertex with the smallest key, marks it visited, and updates the keys of its unvisited neighbors if a smaller weight is found.

Key components:
- **Adjacency List**: A list of lists where each index represents a vertex, and the list contains pairs of (neighbor, weight).
- **Priority Queue**: Used to select the vertex with the smallest key value efficiently.
- **Key Array**: Stores the minimum weight edge connecting each vertex to the MST.
- **Parent Array**: Keeps track of the parent of each vertex in the MST.

Time complexity: O((V + E) log V) where V is the number of vertices and E is the number of edges, dominated by priority queue operations.

## Code

```cpp

#include <iostream>
#include <vector>
#include <queue>
#include <climits>

using namespace std;

typedef pair<int, int> pii; // pair<weight, vertex>

void prim(const vector<vector<pii>> &adj, int n, int src, vector<int> &parent, vector<int> &key)
{
    vector<bool> visited(n, false);
    priority_queue<pii, vector<pii>, greater<pii>> pq; // Min-heap: {weight, vertex}
    key.assign(n, INT_MAX);
    parent.assign(n, -1);
    key[src] = 0;
    pq.push({0, src});

    while (!pq.empty())
    {
        int u = pq.top().second;
        pq.pop();

        if (visited[u])
            continue;
        visited[u] = true;

        for (auto &neighbor : adj[u])
        {
            int v = neighbor.first;
            int weight = neighbor.second;
            if (!visited[v] && weight < key[v])
            {
                key[v] = weight;
                parent[v] = u;
                pq.push({key[v], v});
            }
        }
    }
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

    int src;
    cout << "Enter the starting vertex for Prim's algorithm: ";
    cin >> src;

    vector<int> parent, key;
    prim(adj, n, src, parent, key);

    // Output MST
    cout << "Minimum Spanning Tree:" << endl;
    int totalWeight = 0;
    for (int i = 0; i < n; i++)
    {
        if (parent[i] != -1)
        {
            cout << parent[i] << " - " << i << " : " << key[i] << endl;
            totalWeight += key[i];
        }
    }
    cout << "Total Weight: " << totalWeight << endl;

    return 0;
}

```

## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS48.exe'
Enter the number of vertices: 5
Enter the number of edges: 6
Enter the edges (u v weight) where u and v are vertices (0 to 4):
0 1 2
0 3 6
1 2 3
1 3 8
1 4 5
2 4 7
Enter the starting vertex for Prim's algorithm: 0
Minimum Spanning Tree:
0 - 1 : 2
1 - 2 : 3
0 - 3 : 6
1 - 4 : 5
Total Weight: 16

```

## Conclusion

Prim’s algorithm effectively constructs a minimum spanning tree for a connected undirected graph with non-negative weights using an adjacency list representation. The implementation leverages a priority queue for optimal edge selection, ensuring efficiency. The program accepts user input for graph construction and displays the MST edges along with the total weight. This approach is particularly efficient for sparse graphs due to the adjacency list structure. Overall, Prim’s algorithm is essential in network design, clustering, and optimization problems where minimizing total connection cost is crucial.
