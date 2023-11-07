## Quick Sort


Начнем с выбора элемента, который называется pivot который будет в равен правому элементу, затем перестроим массив таким образом, чтобы все элементы, меньшие, чем pivot, находились слева от него, а все элементы, большие, чем pivot, - справа. Эта операция называется разбиением.
Теперь мы просто рекурсивно применяем тоже самое для элементов от левого до pivot - 1 и от pivot до правого элемента находя pivot для каждого из массивов и их левые и правые части

```c++
#include <iostream>
#include <vector>
using namespace std;

int partition(vector<int> &v, int l, int r) {
    int pivot = v[r];
    int j = l;
    for(int i = l; i < r; i++) { 
        if(v[i] <= pivot) {
            swap(v[i], v[j]);
            j++;
        }
    }
    swap(v[j], v[r]);
    return j;
}

void quickSort(vector<int> &v, int l, int r) {
    if(l < r){
        int p = partition(v, l, r);
        quickSort(v, l, p - 1);
        quickSort(v, p + 1, r);
    }
}

int main() {
    vector<int> arr = {12, 11, 13, 5, 6, 7};
    int n = arr.size();

    cout << "Unsorted array: ";
    for (int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;

    quickSort(arr, 0, n - 1);

    cout << "Sorted array: ";
    for (int i = 0; i < n; i++) {
        cout << arr[i] << " ";
    }
    cout << endl;

    return 0;
}

```
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/a66cb9e9-10c2-4463-bf19-356efec51e9d)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/117ed317-0eea-4d0a-b470-c40dacc1957d)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/0c18b31c-c1d1-4fd7-8411-d9e2f0fbda93)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/e8bf572a-6e40-40d7-9a77-f68aba76df65)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/22f35550-b3bf-4bae-b40f-ac7b77bc5ad6)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/1d17c2da-0611-4519-9cb9-2e180913f048)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/a005afd3-7755-4fd9-83dd-59fc90895580)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/1263b69a-76cd-488c-b1fb-403682008b0d)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/d868df91-88a1-46d3-bd1c-ff3d51ec2dcd)

**сложность алгоритма:**
Временная:

В среднем:

O(N*log2(N))

В худшем случае :

O(N^2)

Плохой выбор опорного элемента: Если в качестве опорного элемента всегда выбирать минимальный или максимальный элемент, то в худшем случае алгоритм будет делать n-1 сравнение на каждом шаге, и общее количество сравнений составит O(n^2). Это случается, например, когда входной массив уже отсортирован или почти отсортирован.

Неравномерное разделение: Если на каждом этапе разделения элементы делятся на две подгруппы неравного размера (например, если опорный элемент всегда выбирается близким к одному из концов массива), то быстрая сортировка может также иметь худшую временную сложность O(n^2).


Пространственная:

O(N)


для лучшего понимания:
https://www.youtube.com/watch?v=Vtckgz38QHs



