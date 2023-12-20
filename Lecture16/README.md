## Алгоритм Белмана Форда по поиску кратчайших расстоянии от одной вершины до других

```cpp
#include <iostream>
#include <vector>
#include <limits>

using namespace std;

const int INF = numeric_limits<int>::max();

struct Edge {
    int source, destination, weight;
};

void bellmanFord(vector<Edge>& edges, int V, int start) { // edge List
    vector<int> distances(V, INF);
    distances[start] = 0;

    // Проходим по всем вершинам
    for (int i = 0; i < V - 1; ++i) {
        for (const auto& edge : edges) {
            if (distances[edge.source] != INF &&
                distances[edge.source] + edge.weight < distances[edge.destination]) {
                distances[edge.destination] = distances[edge.source] + edge.weight;
            }
        }
    }

    // Проверка наличия отрицательных циклов
    for (const auto& edge : edges) {
        if (distances[edge.source] != INF &&
            distances[edge.source] + edge.weight < distances[edge.destination]) {
            cout << "Граф содержит отрицательный цикл" << endl;
            return;
        }
    }

    // Вывод результатов
    cout << "Расстояния от вершины " << start << " к остальным вершинам:" << endl;
    for (int i = 0; i < V; ++i) {
        cout << "Вершина " << i << ": ";
        if (distances[i] == INF) {
            cout << "INF" << endl;
        } else {
            cout << distances[i] << endl;
        }
    }
}

int main() {
    int V, E; // Количество вершин и рёбер в графе
    cout << "Введите количество вершин и рёбер в графе: ";
    cin >> V >> E;

    vector<Edge> edges(E);
    cout << "Введите рёбра в формате исходная_вершина целевая_вершина вес:" << endl;
    for (int i = 0; i < E; ++i) {
        cin >> edges[i].source >> edges[i].destination >> edges[i].weight;
    }

    int startVertex; // Начальная вершина для поиска кратчайших путей
    cout << "Введите начальную вершину: ";
    cin >> startVertex;

    bellmanFord(edges, V, startVertex);

    return 0;
}
```

**Алгоритм Форда-Беллмана (Ford-Bellman):** 

**Плюсы:** 
adjList, adjMatrix, Edge list: Работает с отрицательными весами рёбер. 
Edge list: Удобен для реализации на основе ребер 

**Минусы:** 
adjList: Может быть менее эффективен по времени выполнения на некоторых графах из-за необходимости многократного обновления расстояний. 
adjMatrix: Требует много памяти для больших графов из-за матрицы смежности. 
Edge list: Неэффективен при доступе к определенным вершинам и их связям. 

**Сложности:**  
adjList, adjMatrix: В лучшем случае: O(VE), в худшем случае: O(VE) 
Edge list: В лучшем случае: O(E), в худшем случае: O(EV)
