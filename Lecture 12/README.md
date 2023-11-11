## Adjacency list and matrix

**Adjacency list**

![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/61975454-9761-4231-a090-8c992a75f078)


```c++
#include <iostream>
#include <vector>

using namespace std;

class Graph {
private:
    int vertices;  // вершина
    vector<vector<int>> adjList;

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
## Edge list

## BFS
