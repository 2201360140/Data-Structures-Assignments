# Assignment No 44

## Write a Program to implement Dijkstra’s algorithm to find shortest distance between two nodes of a user defined graph. Use Adjacency List to represent a graph.

## Theory

This program implements Dijkstra's algorithm to find the shortest distances from a starting node to all other nodes in a user-defined graph using an adjacency list representation. Dijkstra's algorithm is a greedy algorithm that finds the shortest path in a graph with non-negative edge weights by iteratively selecting the vertex with the smallest distance.

Key features of the implementation:
- **Adjacency List Representation**: An array of vectors where each vector holds pairs of (vertex, weight) for connected edges. This is efficient for sparse graphs with O(V + E) space complexity.
- **Dijkstra's Algorithm**: Uses a priority queue (min-heap) to always process the vertex with the current smallest distance. It relaxes edges to update distances if a shorter path is found.
- **Time Complexity**: O((V + E) log V) due to priority queue operations, where V is vertices and E is edges.
- **Space Complexity**: O(V + E) for the adjacency list and distance array.

The algorithm assumes non-negative weights and produces shortest paths from the start vertex to all reachable vertices.

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

PS D:\Kanchan_study_material\DS\DS_Labs\output> & .\'ASS44.exe'
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

This program effectively demonstrates Dijkstra's algorithm for finding shortest paths using an adjacency list, which is efficient for sparse graphs. The implementation uses a priority queue for optimal vertex selection, ensuring correctness for non-negative weights. By allowing user-defined graphs and start vertices, it provides a practical tool for understanding shortest path concepts. The adjacency list representation offers space efficiency, and the algorithm is applicable in routing, network optimization, and pathfinding problems.
