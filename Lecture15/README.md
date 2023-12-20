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
