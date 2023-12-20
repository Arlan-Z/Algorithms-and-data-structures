## Spanning tree algorithms (Kruskal, Prima)

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <stack>
#include <string>
#include <cmath>
#include <algorithm>
#include <climits>
#include <set>

using namespace std;
//FINDING MST
//Edge List(adjEdge)
class Edge {
public:
    int src, dest, weight;
};

class Graph {
private:
    int V, E; // Число вершин и рёбер
    vector<Edge> edges; // Вектор для хранения рёбер графа

public:
    Graph(int v, int e) : V(v), E(e) {}

    void addEdge(int src, int dest, int weight) {
        Edge edge;
        edge.src = src;
        edge.dest = dest;
        edge.weight = weight;
        edges.push_back(edge); // Добавление ребра в вектор рёбер
    }

    // Находит номер подмножества элемента i
    int find(vector<int>& parent, int i) {
        if (parent[i] == -1) // если это вершина без множества тогда создаем множество под ярлыком этой вершины
            return i;
        
        // Рекурсивно ищем родителя элемента
        return find(parent, parent[i]); // идем к предыдущему те к родителю пока не дойдем к родителю множества и его числу (вершине)
    }

    // Объединяет два подмножества в одно подмножество
    void Union(vector<int>& parent, int x, int y) {
        int xset = find(parent, x);
        int yset = find(parent, y);

        // Делаем одно подмножество поддеревом другого
        parent[xset] = yset;
    }

    // Реализация алгоритма Краскала для нахождения минимального остовного дерева
    void KruskalMST() {
        vector<Edge> result; // Вектор для хранения рёбер остовного дерева
        int e = 0; // Индекс рёбер в результате

        // Сортировка всех рёбер по весу
        sort(edges.begin(), edges.end(), [](const Edge& a, const Edge& b) {return a.weight < b.weight;}); // создаем компарато   

        vector<int> parent(V, -1); // Вектор для хранения родителя каждой вершины для проверки циклов

        while (e < V - 1 && e < E) {
            Edge next_edge = edges[e++]; // Получение следующего ребра из отсортированных

            int x = find(parent, next_edge.src); 
            int y = find(parent, next_edge.dest); 

            // Если добавление этого ребра не создает цикл, добавляем его к результату и объединяем подмножества
            if (x != y) {
                result.push_back(next_edge); // Добавление ребра в остовное дерево
                Union(parent, x, y); // Объединение подмножеств
            }
        }

        // Вывод рёбер остовного дерева
        // cout << "Edges in the constructed MST:" << endl;
        // for (const auto& edge : result)
        //     cout << edge.src << " - " << edge.dest << " : " << edge.weight << endl;
        int sum = 0;
        for(const auto& edge : result){
            sum++;
        }
        cout << sum << endl;
    }
};

class LGraph {
private:
    int V; // Количество вершин
    vector<set<pair<int, int>>> adjList; // Список смежности c добавлением веса

public:
    LGraph(int V) : V(V) {
        adjList.resize(V);
    }

    void addEdge(int u, int v, int weight) {
        adjList[u].insert({v, weight});
        adjList[v].insert({u, weight});
    }
    int minKey(const vector<int>& key, const vector<bool>& visited) {
        int min = INT_MAX, min_index = -1;

        for (int v = 0; v < V; ++v) {
            if (!visited[v] && key[v] < min) {
                min = key[v];
                min_index = v;
            }
        }

        return min_index;
    }
    void primMST() {
        vector<int> parent(V, -1); // Массив для хранения ребер остовного дерева
        vector<int> key(V, INT_MAX); // Массив для ключей (весов)
        vector<bool> visited(V, false); // Массив флагов для отметки вершин остовного дерева visited

        key[0] = 0; // Начинаем с вершины 0

        for (int count = 0; count < V - 1; ++count) {
            int u = minKey(key, visited);
            visited[u] = true;

            for (auto& neighbor : adjList[u]) {
                int v = neighbor.first;
                int weight = neighbor.second;

                if (!visited[v] && weight < key[v]) {
                    parent[v] = u;
                    key[v] = weight;
                }
            }
        }

        printMST(parent);
    }
    void printMST(const vector<int>& parent) {
        cout << "Edge \tWeight\n";
        for (int i = 1; i < V; ++i)
            cout << parent[i] << " - " << i << "\t" << adjList[i].find({parent[i], INT_MAX})->second << endl;
    }
};


int main() {
    // Для Краскла
    int V = 4; // Число вершин
    int E = 5; // Число рёбер
    Graph graph(V, E); // Создание объекта графа

    // Добавление рёбер графа с указанием их весов
    graph.addEdge(0, 1, 10);
    graph.addEdge(0, 2, 6);
    graph.addEdge(0, 3, 5);
    graph.addEdge(1, 3, 15);
    graph.addEdge(2, 3, 4);

    // Запуск алгоритма Краскала для поиска минимального остовного дерева
    graph.KruskalMST();


    // Для Прима
    V = 5;
    LGraph g(V);

    g.addEdge(0, 1, 2);
    g.addEdge(0, 3, 8);
    g.addEdge(1, 2, 3);
    g.addEdge(1, 3, 5);
    g.addEdge(1, 4, 6);
    g.addEdge(2, 4, 7);
    g.addEdge(3, 4, 9);

    g.primMST();
    return 0;
}
```

https://www.youtube.com/watch?v=JZBQLXgSGfs
https://www.youtube.com/watch?v=0jNmHPfA_yE
https://www.youtube.com/watch?v=ayW5B2W9hfo
