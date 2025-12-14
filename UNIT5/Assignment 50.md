# Assignment No 50

# Write a Program to implement Dijkstra’s algorithm to find shortest distance between two nodes of a user defined graph. Use Adjacency List to represent a graph.

## Theory
Dijkstra’s algorithm is a greedy algorithm used to find the shortest path between nodes in a graph, which may represent, for example, road networks. It was conceived by computer scientist Edsger W. Dijkstra in 1956 and published three years later. The algorithm works by maintaining a set of nodes whose shortest distance from the source is known and repeatedly selecting the node with the smallest tentative distance to explore its neighbors. It uses a priority queue to efficiently select the next node to process.

The algorithm assumes that all edge weights are non-negative. It starts by setting the distance to the source node as 0 and all other nodes as infinity. Then, it iteratively relaxes the edges, updating the shortest distances as it explores the graph. The process continues until all nodes have been visited or the priority queue is empty.

Key components:
- **Adjacency List**: A list of lists where each index represents a vertex, and the list contains pairs of (neighbor, weight).
- **Priority Queue**: Used to always expand the node with the currently smallest distance.
- **Distance Array**: Keeps track of the shortest distance from the source to each node.

Time complexity: O((V + E) log V) where V is the number of vertices and E is the number of edges, due to priority queue operations.

## Code

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <climits>
#include <stack>

using namespace std;

typedef pair<int, int> pii; // pair<distance, vertex>

void dijkstra(const vector<vector<pii>> &adj, int n, int src, vector<int> &dist, vector<int> &parent)
{
    dist.assign(n, INT_MAX);
    parent.assign(n, -1);
    dist[src] = 0;
    priority_queue<pii, vector<pii>, greater<pii>> pq; // Min-heap: {distance, vertex}
    pq.push({0, src});

    while (!pq.empty())
    {
        int u = pq.top().second;
        int d = pq.top().first;
        pq.pop();

        if (d > dist[u])
            continue; // Skip if a better distance was found

        for (auto &neighbor : adj[u])
        {
            int v = neighbor.first;
            int weight = neighbor.second;
            if (dist[u] != INT_MAX && dist[u] + weight < dist[v])
            {
                dist[v] = dist[u] + weight;
                parent[v] = u;
                pq.push({dist[v], v});
            }
        }
    }
}

void printPath(const vector<int> &parent, int dest)
{
    if (parent[dest] == -1)
    {
        cout << "No path to destination." << endl;
        return;
    }
    stack<int> path;
    for (int v = dest; v != -1; v = parent[v])
    {
        path.push(v);
    }
    cout << "Shortest Path: ";
    while (!path.empty())
    {
        cout << path.top();
        path.pop();
        if (!path.empty())
            cout << " -> ";
    }
    cout << endl;
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
        adj[u].push_back({v, w}); // Directed graph
    }

    int src, dest;
    cout << "Enter the source vertex: ";
    cin >> src;
    cout << "Enter the destination vertex: ";
    cin >> dest;

    vector<int> dist, parent;
    dijkstra(adj, n, src, dist, parent);

    if (dist[dest] == INT_MAX)
    {
        cout << "No path exists from " << src << " to " << dest << "." << endl;
    }
    else
    {
        cout << "Shortest Distance from " << src << " to " << dest << ": " << dist[dest] << endl;
        printPath(parent, dest);
    }

    return 0;
}

```

## Output

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS50.exe'
Enter the number of vertices: 5
Enter the number of edges: 6
Enter the edges (u v weight) where u and v are vertices (0 to 4):
0 1 2
1 2 4
1 2 1
1 3 7
2 4 3
3 4 2
Enter the source vertex: 0
Enter the destination vertex: 4
Shortest Distance from 0 to 4: 6
Shortest Path: 0 -> 1 -> 2 -> 4

```

## Conclusion
Dijkstra’s algorithm successfully computes the shortest path in a graph with non-negative edge weights using an adjacency list representation. The implementation demonstrates the use of a priority queue for efficiency. The program handles user input for graph construction and path queries, providing clear output for the adjacency list and shortest distances. This approach is suitable for sparse graphs due to the adjacency list structure but can be optimized further for dense graphs using adjacency matrices. Overall, the algorithm is fundamental in routing, navigation, and network optimization applications.
