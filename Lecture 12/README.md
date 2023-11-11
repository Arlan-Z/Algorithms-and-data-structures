## Adjacency list and matrix (Репрезентации матрицы 3 способа)

Графы:

https://rpruim.github.io/m252/S19/from-class/graphs/introduction-to-graphs.html


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

**Edge list**

![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/73593be2-f2be-48b1-872d-6c98bceb261b)


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

доролнительная инормация :

https://www.geeksforgeeks.org/comparison-between-adjacency-list-and-adjacency-matrix-representation-of-graph/


## BFS

“Поиск в глубину” или “обход в глубину” — это рекурсивный алгоритм по поиску всех вершин графа или дерева. Обход подразумевает под собой посещение всех вершин графа.

Алгоритм поиска в глубину
Стандартная реализация поиска в глубину помещает каждую вершину (узел, node) графа в одну из двух категорий:

Пройденные (Visited).

Не пройденные (Not Visited).

Цель алгоритма состоит в том, чтобы пометить каждую вершину как “Пройденная”, избегая при этом циклов.

Алгоритм поиска в глубину работает следующим образом:

Начните с того, что поместите любую вершину графа на вершину стека.

Возьмите верхний элемент стека и добавьте его в список “Пройденных”.

Создайте список смежных вершин для этой вершины. Добавьте те вершины, которых нет в списке “Пройденных”, в верх стека.

Необходимо повторять шаги 2 и 3, пока стек не станет пустым.

Пример реализации поиска в глубину
Предлагаю рассмотреть на примере, как работает алгоритм поиска в глубину. Мы будем использовать неориентированный граф с пятью вершинами.

![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/751ed3b5-42d2-420d-8059-d643a216ffe0)

Начнем мы с вершины “0”. В первую очередь алгоритм поиска в глубину поместит ее саму в список “Пройденные” (на изображении “Visited”), а ее смежные вершины — в стек.

для понимания :

https://www.geeksforgeeks.org/breadth-first-search-or-bfs-for-a-graph/
