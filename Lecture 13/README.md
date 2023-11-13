## DFS

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

![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/849bb09c-2577-4b23-9d78-c19366368daa)

Затем мы берем следующий элемент сверху стека, т.е. к вершину “1”, и переходим к ее соседним вершинам. Поскольку вершина “0” уже пройдена, следующая вершина “2”.

![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/ed7903a2-68fa-404d-9988-73f08a8b28ef)

Вершина “2” смежна непройденной вершине “4”, следовательно мы добавляем ее наверх стека и проходим ее.

![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/94b8f819-df1f-45cd-8c6b-7b515b16ea0e)

Вершина “2” смежна непройденной вершине “4”, следовательно мы помещаем ее в верх стека.

![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/913fc54a-ebcc-4416-8b06-8e3a23c7e173)

![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/a1838058-db37-44e0-b17c-d9cdba122e89)

Добавляем вершину “4” в список “Пройденные” после прохождения.

После того, как мы пройдем последний элемент (вершину “3”), в стеке не останется непройденных смежных вершин, и таким образом мы завершили обход графа в глубину.

![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/48adad7c-4eae-4d70-a7bd-4f99fe6a188f)

После проверки всех смежных вершин для вершины “3” стек остался пустым, а значит алгоритм обхода графа в глубину завершил свою работу.

Дополнительно написал функцию поиска цикла который по логике является модифицированным DFS


Для AdjList
```c++
#include <iostream>
#include <vector>
#include <queue>
#include <stack>

using namespace std;

class Graph {
private:
    int vertices;
    vector<vector<int>> adjList;

public:
    Graph(int V) : vertices(V), adjList(V) {}

    void addEdge(int v, int w) {
        adjList[v].push_back(w);
        adjList[w].push_back(v);
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

    void BFS(int startVertex) {
        vector<bool> visited(vertices, false);
        queue<int> bfsQueue;

        visited[startVertex] = true;
        bfsQueue.push(startVertex);

        cout << "BFS starting from vertex " << startVertex << ": ";

        while (!bfsQueue.empty()) {
            int currentVertex = bfsQueue.front();
            bfsQueue.pop();

            cout << currentVertex << " ";

            for (const auto &neighbor : adjList[currentVertex]) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    bfsQueue.push(neighbor);
                }
            }
        }
        cout << endl;
    }

    void DFS(int startVertex) {
        vector<bool> visited(vertices, false);
        stack<int> dfsStack;

        visited[startVertex] = true;
        dfsStack.push(startVertex);

        cout << "DFS starting from vertex " << startVertex << ": ";

        while (!dfsStack.empty()) {
            int currentVertex = dfsStack.top();
            dfsStack.pop();

            cout << currentVertex << " ";

            for (const auto &neighbor : adjList[currentVertex]) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    dfsStack.push(neighbor);
                }
            }
        }

        cout << endl;
    }

    bool isCyclicUtil(int v, vector<bool>& visited, vector<bool>& recStack) {
        if (!visited[v]) {
            visited[v] = true;
            recStack[v] = true;

            for (const auto &neighbor : adjList[v]) {
                if (!visited[neighbor] && isCyclicUtil(neighbor, visited, recStack)) {
                    return true;
                } else if (recStack[neighbor]) {
                    return true;
                }
            }
        }

        recStack[v] = false;
        return false;
    }

    bool isCyclic() {
        vector<bool> visited(vertices, false);
        vector<bool> recStack(vertices, false);

        for (int i = 0; i < vertices; ++i) {
            if (isCyclicUtil(i, visited, recStack)) {
                return true;
            }
        }

        return false;
    }
};

int main() {
    int vertices = 5;
    Graph graph(vertices);

    graph.addEdge(0, 1);
    graph.addEdge(0, 4);
    graph.addEdge(1, 2);
    graph.addEdge(1, 3);
    graph.addEdge(1, 4);
    graph.addEdge(2, 3);
    graph.addEdge(3, 4);

    graph.printAdjList();

    graph.DFS(0);

    if (graph.isCyclic()) {
        cout << "The graph contains a cycle.\n";
    } else {
        cout << "The graph does not contain a cycle.\n";
    }

    return 0;
}


```

Для AdjMatrix
```c++
#include <iostream>
#include <vector>
#include <stack>

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

    void DFS(int startVertex) {
        vector<bool> visited(vertices, false);
        stack<int> dfsStack;

        visited[startVertex] = true;
        dfsStack.push(startVertex);

        cout << "DFS starting from vertex " << startVertex << ": ";

        while (!dfsStack.empty()) {
            int currentVertex = dfsStack.top();
            dfsStack.pop();

            cout << currentVertex << " ";

            for (int i = 0; i < vertices; ++i) {
                if (adjMatrix[currentVertex][i] == 1 && !visited[i]) {
                    visited[i] = true;
                    dfsStack.push(i);
                }
            }
        }

        cout << endl;
    }
    bool isCyclicUtil(int v, vector<bool>& visited, vector<bool>& recStack) { // функция для нахождения цикла в графе
        if (!visited[v]) {
            visited[v] = true;
            recStack[v] = true;

            for (int i = 0; i < vertices; ++i) {
                if (adjMatrix[v][i] == 1) {
                    if (!visited[i] && isCyclicUtil(i, visited, recStack)) {
                        return true;
                    } else if (recStack[i]) {
                        return true;
                    }
                }
            }
        }

        recStack[v] = false;
        return false;
    }

    bool isCyclic() {
        vector<bool> visited(vertices, false);
        vector<bool> recStack(vertices, false);

        for (int i = 0; i < vertices; ++i) {
            if (isCyclicUtil(i, visited, recStack)) {
                return true;
            }
        }

        return false;
    }
};

int main() {
    // Create a graph with 5 vertices
    Graph graph(5);

    // Add edges to create a sample graph
    graph.addEdge(0, 1);
    graph.addEdge(1, 2);
    graph.addEdge(2, 0);
    graph.addEdge(1, 3);
    graph.addEdge(3, 4);

    // Print the adjacency matrix of the graph
    cout << "Adjacency Matrix:\n";
    graph.printAdjMatrix();

    // Perform DFS starting from vertex 0
    graph.DFS(0);

    // Check if the graph has a cycle
    if (graph.isCyclic()) {
        cout << "The graph contains a cycle.\n";
    } else {
        cout << "The graph does not contain a cycle.\n";
    }

    return 0;
}

```

Функция `isCyclicUtil` предназначена для обнаружения циклов в ориентированном графе с использованием поиска в глубину (DFS):

```cpp
bool isCyclicUtil(int v, vector<bool>& visited, vector<bool>& recStack) {
    if (!visited[v]) {
        visited[v] = true;
        recStack[v] = true;

        for (int i = 0; i < vertices; ++i) {
            if (adjMatrix[v][i] == 1) {
                if (!visited[i] && isCyclicUtil(i, visited, recStack)) {
                    return true;
                } else if (recStack[i]) {
                    return true;
                }
            }
        }
    }

    recStack[v] = false;
    return false;
}
```

Вот пошаговое объяснение:

1. Функция принимает три параметра:
   - `v`: текущая вершина, которую мы посещаем.
   - `visited`: вектор для отслеживания посещенных вершин и избежания повторного посещения вершины во время DFS.
   - `recStack`: вектор для отслеживания вершин в текущем стеке рекурсии. Это используется для обнаружения циклов.

2. Если текущая вершина (`v`) еще не посещена:
   - Помечаем ее как посещенную (`visited[v] = true`).
   - Помечаем ее как часть текущего стека рекурсии (`recStack[v] = true`).

3. Проходим по всем вершинам (`i`) в графе:
   - Если есть ребро от текущей вершины (`v`) к вершине (`i`), и `i` еще не посещена:
     - Рекурсивно вызываем `isCyclicUtil(i, visited, recStack)`.
     - Если рекурсивный вызов возвращает true, это означает, что есть цикл, и функция возвращает true.
     - Если `i` уже находится в текущем стеке рекурсии (`recStack[i]` равно true), это означает, что есть обратное ребро, указывающее на цикл. Функция возвращает true.

4. Если текущая вершина была посещена и обработана, помечаем ее как не принадлежащую текущему стеку рекурсии (`recStack[v] = false`) перед возвратом false. Этот шаг важен для правильной работы алгоритма.

5. Если цикл не обнаружен во время DFS, функция возвращает false.

Идея заключается в использовании DFS для обхода графа и в поддержании стека рекурсии (`recStack`), чтобы отслеживать вершины в текущем пути рекурсии. Если во время DFS мы сталкиваемся с вершиной, которая уже находится в стеке рекурсии, это означает наличие цикла в графе.

Для EdgeList

```c++
#include <iostream>
#include <vector>
#include <queue>
#include <stack>

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

    vector<int> bfs(int startVertex) {
        vector<int> visited(edges.size(), 0);
        vector<int> result;

        queue<int> q;
        q.push(startVertex);
        visited[startVertex] = 1;

        while (!q.empty()) {
            int currentVertex = q.front();
            q.pop();
            result.push_back(currentVertex);

            for (const auto &edge : edges) {
                if (edge.source == currentVertex && !visited[edge.destination]) {
                    q.push(edge.destination);
                    visited[edge.destination] = 1;
                }
            }
        }

        return result;
    }

    vector<int> dfs(int startVertex) {
        vector<int> visited(edges.size(), 0);
        vector<int> result;

        stack<int> s;
        s.push(startVertex);
        visited[startVertex] = 1;

        while (!s.empty()) {
            int currentVertex = s.top();
            s.pop();
            result.push_back(currentVertex);

            for (const auto &edge : edges) {
                if (edge.source == currentVertex && !visited[edge.destination]) {
                    s.push(edge.destination);
                    visited[edge.destination] = 1;
                }
            }
        }

        return result;
    }

    bool isCyclicUtil(int v, vector<int>& visited, vector<int>& recStack) {
        if (!visited[v]) {
            visited[v] = 1;
            recStack[v] = 1;

            for (const auto &edge : edges) {
                if (edge.source == v) {
                    if (!visited[edge.destination] && isCyclicUtil(edge.destination, visited, recStack)) {
                        return true;
                    } else if (recStack[edge.destination]) {
                        return true;
                    }
                }
            }
        }

        recStack[v] = 0;
        return false;
    }

    bool isCyclic() {
        vector<int> visited(edges.size(), 0);
        vector<int> recStack(edges.size(), 0);

        for (const auto &edge : edges) {
            if (isCyclicUtil(edge.source, visited, recStack)) {
                return true;
            }
        }

        return false;
    }
};

int main() {
    EdgeList edgeList;

    edgeList.addEdge(0, 1, 5);
    edgeList.addEdge(0, 2, 3);
    edgeList.addEdge(1, 2, 1);
    edgeList.addEdge(2, 3, 7);

    cout << "Edge List:\n";
    edgeList.printEdgeList();

    cout << "\nBFS Traversal starting from vertex 0:\n";
    vector<int> bfsResult = edgeList.bfs(0);

    for (int vertex : bfsResult) {
        cout << vertex << " ";
    }

    cout << "\nDFS Traversal starting from vertex 0:\n";
    vector<int> dfsResult = edgeList.dfs(0);

    for (int vertex : dfsResult) {
        cout << vertex << " ";
    }

    if (edgeList.isCyclic()) {
        cout << "\nThe graph contains a cycle.\n";
    } else {
        cout << "\nThe graph does not contain a cycle.\n";
    }

    return 0;
}


```


Временная сложность DFS:

В худшем случае временная сложность DFS для графа представленного списком смежности составляет O(V + E), где V - количество вершин, E - количество рёбер. Это потому, что каждая вершина и каждое ребро графа посещаются ровно один раз.
Для графа, представленного матрицей смежности, временная сложность может быть выражена как O(V^2), так как проверка смежности для каждой пары вершин требует O(1) времени, и всего есть V^2 пар вершин.
Пространственная сложность DFS:

Пространственная сложность DFS определяется максимальной глубиной рекурсии или размером стека (если реализовано без рекурсии). В худшем случае она составляет O(V) для графа представленного списком смежности, так как максимальная глубина рекурсии равна количеству вершин.
Для графа, представленного матрицей смежности, пространственная сложность может быть O(V) или O(V^2), в зависимости от того, как реализован алгоритм DFS. Если используется рекурсия, то O(V), если используется стек, то O(V^2).

![fs](dfs(1).gif)

DFS отличен от BFS в коде только тем что в DFS новые элементы (те соседа текущего) кидает в начало стека (буферной кучи) c использованием stack и не запутайтесь так как у stack push это закидывание в начало а у BFS наоборот соседа кидает в конец кучи с использованием queue у нее push это закидывание в конец
из-за этой разницы в dfs visited будут вершины ветви и потом она будет переходить на следующую ветвь а bfs будет посещать ближайшие элементы т.е те которые на одном уровне с текущим 

![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/1aace7dc-e99b-49d4-9949-2138bb5719ba)

![gf](DFSvsBFS.gif)

**DFS (Depth-First Search):**

*Плюсы:*
1. **Простота реализации:** DFS легко реализовать с использованием рекурсии или стека.
2. **Эффективность в поиске в глубину:** DFS может быть эффективным для поиска в глубину в деревьях и графах.

*Минусы:*
1. **Не гарантированная наименьшая длина пути:** DFS не всегда находит кратчайший путь между двумя точками.
2. **Не работает хорошо в случае циклов:** В графах с циклами DFS может зациклиться.

*Когда использовать:*
1. **Кратчайший путь не является приоритетом:** Если не требуется нахождение кратчайшего пути, DFS может быть хорошим выбором.
2. **Рекурсивная логика:** Когда удобно использовать рекурсивные вызовы.
3. **Нахождение цикла**
4. **Нахождение компонента связанности**

**BFS (Breadth-First Search):**

*Плюсы:*
1. **Гарантированный кратчайший путь:** BFS всегда находит кратчайший путь между двумя точками в невзвешенном графе.
2. **Работает хорошо в случае циклов:** Не зацикливается в графах с циклами.

*Минусы:*
1. **Большее использование памяти:** Требует больше памяти для хранения информации о текущем уровне графа.
2. **Сложность реализации:** Реализация BFS может потребовать больше усилий, чем у DFS.

*Когда использовать:*
1. **Кратчайший путь важен:** Если важно найти кратчайший путь между двумя точками.
2. **Плоский граф:** Когда граф не имеет большой глубины, и требуется обходить его в ширину.
3. **Не рекурсивная логика:** Когда неудобно использовать рекурсию, и предпочтительна итеративная реализация.

Выбор между DFS и BFS зависит от конкретной задачи, структуры данных (графа) и требований к результату.


## Topological Sort

## AdjMatrix , AdjList через Хэш таблицу

реализация через хэш таблицу самая эффективная 

**AdjMatrix**

```c++
```

**AdjList**

```c++

```

## Рекурсивная реализация DFS
