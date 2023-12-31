# Бинарное Дерево
![NULL](https://upload.wikimedia.org/wikipedia/commons/thumb/d/da/Binary_search_tree.svg/1200px-Binary_search_tree.svg.png)

## Binary Tree что это такое
Дерево у каждого разветвления максимум 2 ветви.  
Пусть основание разветвления (элемент) это родитель а элемент левой 
ветви левый ребенок и правой ветви правый ребенок.  
Правило составления гласит, что левый меньше или равен родителя по 
значению и родитель меньше правого по значению.  
### Как его создавать и делать вставку  
Есть несколько способов одним из которых является рекурсивый или 
итеративный метод на основе сравнении по общему правилу дерева.  
Если дерева не существует то и корня не существует тогда нужно 
создать новый элемент и указать что это корень.  
Если элементов в дереве больше 0 то начинаем основной метод.
Мы ищем место куда нужно вставить элемент начиная от корня с 
помощью
указателей двигаясь по ветвям пока не появиться пустое место(нода у которой нет детей), то есть если значение новой ноды больше 
родителя то двигаемся к следующему родителю на правую ветвь иначе,
если меньше или
равен на левую и так повторяем пока следующая не будет пустая тогда у 
последней ставим указатель на новую ноду.

**Рекурсивная реализация**
```c++
void insert(int val){
    root = insert(root,val);
}
Node* insert(Node* root, int val){
    if(!root){
        return new Node(val);
    }
    else{
        if(val > root->val){
            root->right = insert(root->right, val);
        }
        else{
            root->left = insert(root->left, val);
        }
    }
    return root;
}
```

**Итеративная реализация**
```c++
void insert(int val){
    root = insert(root,val);
}
Node* insert(Node* root, int val) {
    Node* newNode = new Node(val);
    if (!root) {
        return newNode;
    }
    Node* cur = root;
    while (cur) {
        if (val > cur->val) {
            if (!cur->right) {
                cur->right = newNode;
                return root; 
            }
            cur = cur->right;
        } else {
            if (!cur->left) {
                cur->left = newNode;
                return root; 
            }
            cur = cur->left;
            }
        }
    return root;
}
```

![NULL](image.png)

## Свойства дерева

![Gg](tree.png)
![GGG](tree1.png)

На рисунке N - количество всех элементов n - количество элементов на уровне h 
h - высота дерева
Полное бинарное дерево обладает свойством увеличения количества элементов на следующем уровне в 2 раза то есть на если i уровне m узлов то на следующем i + 1 будет 2*m узлов, то есть это описывается геометрической прогрессией пусть начальное количесвтво элементов b0 = 1 а i тое bi то bi = b0 * 2^(i) по формуле геометрической прогресии тогда сумма всех количеств и будет n те количество всех элементов, s = b0(2^i-1)/(2 - 1) , s = b0(2^i-1) , s = 2^i - 1 , n = 2^h - 1. Где h - высота дерева тогда 2^h = n + 1 и наконец h = log2(n+1) - это формула высоты бинарного дерева . пусть количество всех элементов n то количество элементов на последнем уровне будет (n-1)/2 + 1

### Как вывести дерево
Чтобы выводить дерево нужно проходиться по всем элементам этого 
дерева и выводить каждый элемент.  
**Инфиксный обход (симметричный обход):** Обходит дерево в порядке увеличения значений узлов. 
Печатает значения узлов в отсортированном порядке, если дерево является бинарным поисковым 
деревом.  
Он идет по обоим ветвям и для них делает тоже самое рекурсивно пока нода не окажется пуста
Для этого просто сделаем функцию по элементу которая вызывает сама себя для левого и правой 
ветви и работает в случае если нода не пустая
и бывают разные реализации этого:

```c++
void inorderTraversal(Node* node) {
    if (node) {
        inorderTraversal(node->left);
        cout << node->val << " ";
        inorderTraversal(node->right);
    }
}
```

**Префиксный обход (прямой обход):** Начинает с корневого узла и постепенно двигается вниз по дереву, обходя корень, затем его левое и правое поддеревья.

```c++
void preorderTraversal(Node* node) {
    if (node) {
        cout << node->val << " ";
        preorderTraversal(node->left);
        preorderTraversal(node->right);
    }
}
```
**Постфиксный обход:** обходит левое и правое поддеревья после корень.
```c++
void postorderTraversal(Node* node) {
    if (node) {
        postorderTraversal(node->left);
        postorderTraversal(node->right);
        cout << node->val << " ";
    }
}
```

**Для лучшего понимания:**


https://www.youtube.com/watch?v=4zVdfkpcT6U

https://www.youtube.com/watch?v=5dySuyZf9Qg

https://www.youtube.com/watch?v=1WxLM2hwL-U

Вывод дерева в вертикальном виде используя **инфиксный обход**:
```c++
#include <iostream>
using namespace std;

void printTreeStructure(Node* node, int level = 0) {
    if (node) {
        printTreeStructure(node->right, level + 1);
        for (int i = 0; i < level; i++) {
            cout << " "; // Вставляем пробелы для отступа
        }
        cout << node->val << endl;
        printTreeStructure(node->left, level + 1);
    }
}
```
### Удаление элемента по значению
Для удаления определенного элемента из бинарного дерева, вам нужно выполнить следующие 
шаги:
1. Найти узел, который соответствует удаляемому элементу.
2. Если узел является листом *(у него нет детей)*, просто удалите этот узел.
![Tree NONE](image-1.png)

3. Eсли у узла есть только один ребенок *(левый или правый)*, то удалите этот узел и замените его на 
своего единственного ребенка.
![ILUVU](image-2.png)
4. Если у узла есть два ребенка, вам нужно найти наименьший элемент в правом поддереве *(или
наибольший элемент в левом поддереве)*. Этот элемент заменит удаляемый узел. Затем 
рекурсивно удалите этот элемент из правого *(или левого)* поддерева.
![He-He I am great Papyrus](image-3.png)

```c++
// Функция для нахождения узла с минимальным значением в дереве.
Node* findMin(Node* node) {
    while (node->left) {
        node = node->left;
    }
    return node;
}

// Функция для удаления узла с заданным ключом из бинарного дерева поиска.
Node* deleteNode(Node* root, int key) {
    if (!root) {
        return root; // Если дерево пустое, возвращаем NULL.
    }

    // Если ключ меньше значения корня, производим удаление в левом поддереве.
    if (key < root->val) {
        root->left = deleteNode(root->left, key);
    }
    // Если ключ больше значения корня, производим удаление в правом поддереве.
    else if (key > root->val) {
        root->right = deleteNode(root->right, key);
    }
    // Если ключ равен значению корня, то это узел, который нужно удалить.
    else {
        // Узел с одним потомком или без потомков.
        if (!root->left) {
            Node* temp = root->right;
            delete root;
            return temp;
        }
        else if (!root->right) {
            Node* temp = root->left;
            delete root;
            return temp;
        }

        // Узел с двумя потомками.
        Node* temp = findMin(root->right); // Находим минимальный узел в правом поддереве (это будет преемник).
        root->val = temp->val; // Копируем значение преемника в узел.
        root->right = deleteNode(root->right, temp->val); // Удаляем преемника.
    }

    return root; // Возвращаем измененное дерево.
}
```
Вызывая `deleteNode(tree.root, key)` вы удалите узел с указанным значением `key` из вашего 
бинарного дерева.
```c++
#include <iostream>
using namespace std;

// Определение узла дерева
class Node {
public:
    int val;               // Значение узла
    Node *left, *right;    // Левый и правый потомки

    // Конструктор узла
    Node(int val) : val(val), left(nullptr), right(nullptr) {}
};

// Определение бинарного дерева поиска
class BinaryTree {
public:
    Node* root;            // Корневой узел дерева

    // Конструктор дерева
    BinaryTree() : root(nullptr) {}

    // Функция вставки значения в дерево
    void insert(int val) {
        Node* node = new Node(val);
        if (!root) {
            root = node;
        } else {
            Node* cur = root;
            while (true) {
                if (val <= cur->val) {
                    if (!cur->left) {
                        cur->left = node;
                        break;
                    }
                    cur = cur->left;
                } else {
                    if (!cur->right) {
                        cur->right = node;
                        break;
                    }
                    cur = cur->right;
                }
            }
        }
    }

    // Функция бинарного поиска в дереве
    Node* BinarySearch(Node* root, int x) {
        if (!root || root->val == x) {
            return root; // Если дерево пустое или корень является искомым значением
        }
        if (root->val < x) {
            return BinarySearch(root->right, x); // Поиск в правом поддереве
        } else {
            return BinarySearch(root->left, x);  // Поиск в левом поддереве
        }
    }
};

int main() {
    BinaryTree tree;
    tree.insert(50); tree.insert(30); tree.insert(20);
    tree.insert(40); tree.insert(70); tree.insert(60); tree.insert(80);

    int x = 6;
    if (tree.BinarySearch(tree.root, x) == nullptr)
        cout << x << " not found" << endl;
    else
        cout << x << " found" << endl;

    x = 60;
    if (tree.BinarySearch(tree.root, x) == nullptr)
        cout << x << " not found" << endl;
    else
        cout << x << " found" << endl;

    return 0;
}
```
Output
```
6 not found
60 found
```

Временная сложность: **O(h)**, где **h** - высота BST (*Binary Search Tree*).
Вспомогательное пространство: **O(h)v**, где **h** - высота BST. Это связано с тем, что максимальный
объем пространства, необходимый для хранения стека рекурсии, будет равен **h**.


## Нахождение количества узлов , ширину и высоту в бинарном дерева
```c++
#include <iostream>
using namespace std;

class TreeNode {
public:
    int data;
    TreeNode* left;
    TreeNode* right;

    TreeNode(int value) : data(value), left(nullptr), right(nullptr) {}
};

int countNodes(TreeNode* root) {
    if (root == nullptr) {
        return 0; // Если дерево пусто, вернуть 0 узлов.
    }

    // Рекурсивно подсчитываем узлы в левом и правом поддеревьях и добавляем 1 за текущий узел.
    return 1 + countNodes(root->left) + countNodes(root->right);
}

int height(TreeNode* root) {
    if (root == nullptr) {
        return 0; 
    }
    int leftHeight = height(root->left);
    int rightHeight = height(root->right);

    return 1 + max(leftHeight, rightHeight);
}

int findWidth(TreeNode* root) {
    if (root == nullptr) {
        return 0; // Пустое дерево не имеет ширины.
    }

    vector<TreeNode*> currentLevel;
    vector<TreeNode*> nextLevel;
    int maxWidth = 1; // Минимальная ширина уровня - хотя бы один узел в корне.

    currentLevel.push_back(root);

    while (!currentLevel.empty()) {
        TreeNode* currentNode = currentLevel.front();
        currentLevel.erase(currentLevel.begin());

        if (currentNode->left) {
            nextLevel.push_back(currentNode->left);
        }

        if (currentNode->right) {
            nextLevel.push_back(currentNode->right);
        }

        if (currentLevel.empty() && !nextLevel.empty()) {
            int width = nextLevel.size();
            if (width > maxWidth) {
                maxWidth = width;
            }
            swap(currentLevel, nextLevel);
        }
    }

    return maxWidth;
}
int getWidth(TreeNode* root, int level) {
    if (root == nullptr) {
        return 0;
    }
    if (level == 1) {
        return 1;
    } else if (level > 1) {
        return getWidth(root->left, level - 1) + getWidth(root->right, level - 1);
    }
    return 0;
}
int findWidthRecurs(TreeNode* root) {
    if (root == nullptr) {
        return 0;
    }

    int maxWidth = 0;
    int height = getHeight(root);

    for (int level = 1; level <= height; level++) {
        int width = getWidth(root, level);
        maxWidth = max(maxWidth, width);
    }

    return maxWidth;
}
int main() {
    // Создаем простое бинарное дерево
    TreeNode* root = new TreeNode(1);
    root->left = new TreeNode(2);
    root->right = new TreeNode(3);
    root->left->left = new TreeNode(4);
    root->left->right = new TreeNode(5);

    int nodeCount = countNodes(root);

    cout << "Количество узлов в бинарном дереве: " << nodeCount << endl;
    cout << "Высота бинарного дерева: " << height(root) << endl;
    cout << "Ширина бинарного дерева: " << findWidth(root) << endl;

    return 0;
}
```
посчет количества нод:
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/afa6e667-3b14-42b8-bb81-8cad94d07200)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/c37e94b5-0193-4419-a7e0-ebd34d21161f)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/9882ebc3-1095-4c7e-8233-853a8ac3f167)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/9c2e83f0-cef0-4d0e-b0d8-ed24359a1afe)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/3429e054-75f1-42d3-9460-3bcb97ee2e0c)


высота:
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/6e8b1974-6121-4b49-88f1-40b80bb523c1)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/a49911fe-f748-4177-a59d-fcb30abd3754)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/bd77407b-6017-493c-9020-f8536d4fd768)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/91faef36-2878-4d97-b42d-4933e021297e)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/8d7ee247-d9fb-43fa-ba8d-1a70d616b58b)


ширина:
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/663d10ad-c865-43e3-9cd3-5c948c88ff08)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/902ec715-76e5-40c4-b631-dd85552c77f6)

подфункция ширины getWidth
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/c7568858-f297-4006-acef-c9d61d88d61d)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/7b7cf440-211f-4ef5-bfd9-2db9b979706b)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/277d7328-fe16-4e9d-92a6-cfc6a085e5fe)


### Диаметр бинарного дерева
*Диаметр бинарного дерева* - это наибольшее количество рёбер между двумя узлами дерева. Другими словами, это самый длинный путь от одного узла до другого.
  
**Алгоритм вычисления:**  
1. Рекурсивно вычислите глубину каждого узла.  
2. Для каждого узла, диаметр равен сумме глубин его двух поддеревьев (левое + правое).
3. Отслеживайте и обновляйте максимальное значение диаметра на протяжении всего обхода.

![Diam](Diameter.png)

**Реализация**
```c++
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(NULL), right(NULL) {}
};

class BinaryTree {
    int max_diameter = 0;

    int depth(TreeNode* node) {
        if (!node) return 0;

        int left_depth = depth(node->left);
        int right_depth = depth(node->right);

        // Обновляем максимальный диаметр
        max_diameter = max(max_diameter, left_depth + right_depth);

        return 1 + max(left_depth, right_depth);
    }

public:
    int diameter(TreeNode* root) {
        depth(root);
        return max_diameter;
    }
};
```
Данный алгоритм эффективно вычисляет диаметр бинарного дерева с временной сложностью **O(n)**, где **n** - количество узлов в дереве, так как каждый узел 
посещается ровно один раз.

![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/218d3f38-7db8-40f9-bc8b-3aa13fb37804)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/01ff2cba-6883-4c63-8813-462fb40896ce)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/9cd28949-a78c-4b3a-82dd-c13339bf6f34)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/32e2afd3-503b-41c3-90a8-1adc93d4d820)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/974be5d8-1a0c-4d90-9407-6cde1f0d5755)

