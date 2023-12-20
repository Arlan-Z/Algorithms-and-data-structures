## Алгоритмы Дейкстры и Флойда для поиска кратчайших расстоянии между вершинами

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
#include <limits>

using namespace std;

const int INF = numeric_limits<int>::max();

void dijkstra(vector<vector<int>>& graph, int start) { // adj Matrix
    int V = graph.size(); // Количество вершин в графе
    vector<int> distances(V, INF); // Создаем вектор расстояний до всех вершин, изначально устанавливаем их как бесконечность
    vector<bool> visited(V, false); // Вектор для отметки посещенных вершин

    distances[start] = 0; // Расстояние от начальной вершины до самой себя равно 0

    // Проходим по всем вершинам, чтобы найти кратчайшие пути
    for (int i = 0; i < V - 1; ++i) {
        int u = -1; // Индекс текущей вершины с наименьшим расстоянием
        // Ищем вершину с наименьшим расстоянием, которую еще не посетили
        for (int j = 0; j < V; ++j) {
            if (!visited[j] && (u == -1 || distances[j] < distances[u])) {
                u = j;
            }
        }

        // Если расстояние до u осталось бесконечностью, остальные вершины недостижимы
        if (distances[u] == INF) {
            break;
        }

        visited[u] = true; // Отмечаем текущую вершину как посещенную

        // Обновляем расстояния до всех смежных вершин через текущую
        for (int v = 0; v < V; ++v) {
            if (graph[u][v] != 0 && distances[u] + graph[u][v] < distances[v]) {
                distances[v] = distances[u] + graph[u][v];
            }
        }
    }

    // Выводим найденные кратчайшие расстояния от начальной вершины до всех остальных вершин
    cout << "Вершина \t Расстояние от начальной вершины" << endl;
    for (int i = 0; i < V; ++i) {
        cout << i << "\t\t" << distances[i] << endl;
    }
}

void floydWarshall(vector<vector<int>> &graph) { //adj Matrix
    int V = graph.size();

    // Проход по всем вершинам как промежуточным точкам и обновление расстояний
    for (int k = 0; k < V; ++k) {
        for (int i = 0; i < V; ++i) {
            for (int j = 0; j < V; ++j) {
                if (graph[i][k] != -1 && graph[k][j] != -1 &&
                    (graph[i][j] == -1 || graph[i][k] + graph[k][j] < graph[i][j])) {
                    graph[i][j] = graph[i][k] + graph[k][j];
                }
            }
        }
    }

    // Вывод результирующей матрицы кратчайших путей
    cout << "Матрица кратчайших расстояний между парами вершин:" << endl;
    for (int i = 0; i < V; ++i) {
        for (int j = 0; j < V; ++j) {
            if (graph[i][j] == -1) {
                cout << "INF ";
            } else {
                cout << graph[i][j] << " ";
            }
        }
        cout << endl;
    }
}

int main() {
    int V; // Количество вершин в графе
    cout << "Введите количество вершин в графе: ";
    cin >> V;

    vector<vector<int>> graph(V, vector<int>(V, -1));
    cout << "Введите матрицу смежности графа (для отсутствия ребра используйте -1 или INF):" << endl;
    for (int i = 0; i < V; ++i) {
        for (int j = 0; j < V; ++j) {
            cin >> graph[i][j];
        }
    }
    floydWarshall(graph);

    int V; // Количество вершин в графе
    cout << "Введите количество вершин в графе: ";
    cin >> V;

    vector<vector<int>> graph(V, vector<int>(V, 0));
    cout << "Введите матрицу смежности графа:" << endl;
    for (int i = 0; i < V; ++i) {
        for (int j = 0; j < V; ++j) {
            cin >> graph[i][j];
        }
    }

    int startVertex; // Начальная вершина для поиска кратчайших путей
    cout << "Введите начальную вершину: ";
    cin >> startVertex;

    dijkstra(graph, startVertex);

    return 0;
}

```

**Алгоритм Дейкстры (Dijkstra):** 

**Плюсы:** 

adjList: Эффективен для поиска кратчайших путей от одной стартовой вершины до всех остальных. 
adjMatrix: Прост в реализации. Эффективен на плотных графах. 
Edge list: Может быть удобен для реализации на основе рёбер. 

**Минусы:** 
adjList: Не работает с отрицательными весами рёбер. 
adjMatrix: Требует много памяти на больших графах. 
Edge list: Неэффективен при доступе к определенным вершинам и их связям. 

**Сложности:** 
adjList: В лучшем случае: O(ElogV), в худшем случае: O(ElogV) 
adjMatrix: В лучшем случае: O(V^2), в худшем случае: O(V^2) 
Edge list: В лучшем случае: O(ElogE), в худшем случае: O(ElogE)  


**Алгоритм Флойда (Floyd):** 

**Плюсы:** 
adjList, adjMatrix, Edge list: Работает с графами с отрицательными весами рёбер. 
adjMatrix: Эффективен на плотных графах. 

**Минусы:** 
adjList, Edge list: Неэффективен для больших графов из-за повышенной сложности времени выполнения (O(V^3)). 
adjMatrix: Требует много памяти для больших графов из-за матрицы смежности. 

**Сложности:** 
adjList, adjMatrix, Edge list: В лучшем случае: O(V^3), в худшем случае: O(V^3) 
adjMatrix: В лучшем случае: O(V^2), в худшем случае: O(V^2) 
