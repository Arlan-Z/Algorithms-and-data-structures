## Adjacency list and matrix

**Adjacency list**

![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/72effdf3-cdc8-44aa-9c04-67048e93fad9)


```c++
#include <iostream>
#include <vector>

using namespace std;

class Graph {
private:
    int vertices;  // вершина
    vector<vector<int>> adjList; // можно использовать vector<linked list<int>> ....

public:
    Graph(int V) : vertices(V), adjList(V) {}
    
    void addEdge(int v, int w) { // добавить ребро
        adjList[v].push_back(w);
        adjList[w].push_back(v); // Uncomment for undirected graph
    }

    void printAdjList() {
        for (int i = 0; i < vertices; ++i) {
            cout << "Adjacency list of vertex " << i << ": ";
            for (const auto &neighbor : adjList[i]) {
                cout << neighbor << " ";
            }
            cout << endl;
        }
    }
};

int main() {
    int vertices = 5; // Change this as needed
    Graph graph(vertices);

    // Add edges
    graph.addEdge(0, 1);
    graph.addEdge(0, 4);
    graph.addEdge(1, 2);
    graph.addEdge(1, 3);
    graph.addEdge(1, 4);
    graph.addEdge(2, 3);
    graph.addEdge(3, 4);

    // Print adjacency list
    graph.printAdjList();

    return 0;
}


```

![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/32167ca5-3b71-41eb-b770-4095b2dfef3a)

```c++
    void addEdge(int v, int w) { 
        adjList[v].push_back(w);
        adjList[w].push_back(v); 
    }
```
при создании графа в нем будет V вершин , и не более V векторов или связанных списков 
для v того вектора или связанного списка добавить w
для w того вектора или связанного списка добавить v

![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/9ebbff1c-d08e-4db1-87e5-e184fcd3ca25)


для понимания : 

https://www.youtube.com/watch?v=ee6zIj4J3-Y  

https://www.youtube.com/watch?v=dhgKr8942rs


**Adjacency matrix**

![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/b715455f-7708-4d67-b277-a0474f55ac36)


```c++
#include <iostream>
#include <vector>

using namespace std;

class Graph {
private:
    int vertices;
    vector<vector<int>> adjMatrix;

public:
    Graph(int V) : vertices(V), adjMatrix(V, vector<int>(V, 0)) {}

    void addEdge(int v, int w) {
        adjMatrix[v][w] = 1;
        adjMatrix[w][v] = 1; // Uncomment for undirected graph
    }

    void printAdjMatrix() {
        for (int i = 0; i < vertices; ++i) {
            for (int j = 0; j < vertices; ++j) {
                cout << adjMatrix[i][j] << " ";
            }
            cout << endl;
        }
    }
};

int main() {
    int vertices = 5; // Change this as needed
    Graph graph(vertices);

    // Add edges
    graph.addEdge(0, 1);
    graph.addEdge(0, 4);
    graph.addEdge(1, 2);
    graph.addEdge(1, 3);
    graph.addEdge(1, 4);
    graph.addEdge(2, 3);
    graph.addEdge(3, 4);

    // Print adjacency matrix
    graph.printAdjMatrix();

    return 0;
}


```

для понимания: https://www.youtube.com/watch?v=B28xAWEerK8

## Edge list

```c++
#include <iostream>
#include <vector>

using namespace std;

class Edge {
public:
    int source, destination, weight;

    Edge(int src, int dest, int w) : source(src), destination(dest), weight(w) {}
};

class EdgeList {
private:
    vector<Edge> edges;

public:
    EdgeList() {}

    void addEdge(int src, int dest, int weight) {
        Edge ne(src, dest, weight);
        edges.push_back(ne);
    }

    void printEdgeList() {
        for (const auto &edge : edges) {
            cout << "Edge: " << edge.source << " -> " << edge.destination;
            cout << " (Weight: " << edge.weight << ")\n";
        }
    }
};

int main() {
    EdgeList edgeList;

    edgeList.addEdge(0, 1, 5);
    edgeList.addEdge(0, 2, 3);
    edgeList.addEdge(1, 2, 1);
    edgeList.addEdge(2, 3, 7);

    edgeList.printEdgeList();

    return 0;
}
```

## BFS
