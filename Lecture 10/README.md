## Hash tables 

Хэш таблица - это структура данных, которая позволяет быстро и эффективно хранить и получать данные. Основная идея заключается в использовании хэш-функции для преобразования ключей (например, строк или чисел) в индексы, по которым данные будут храниться в массиве.
То есть хэш таблица по каким то параметрам элемента (ключ), через хэш функцию вычисляет его код то есть индекс , по этому временная сложность нахождения элемента в хэш коде будет за константное время в то время как при обычном линейном поиске в массиве уйдет линейное время.

Простое объяснение:

У нас есть массив, который выглядит как большой ящик со множеством ячеек.

Когда мы хотим сохранить данные (например, значение) под определенным ключом, мы применяем хэш-функцию к этому ключу. Хэш-функция генерирует числовое значение (хэш), которое является индексом ячейки в массиве.

Мы помещаем данные в ячейку с этим индексом.

Когда нам нужно получить данные по ключу, мы снова применяем хэш-функцию к ключу, она возвращает индекс, по которому мы ищем данные в массиве. Таким образом, мы быстро находим данные, не проходя по всему массиву.

Важно, чтобы хэш-функция была хорошей, чтобы минимизировать коллизии (ситуации, когда разные ключи имеют одинаковый хэш) и обеспечивать эффективность хеш-таблицы.

Для операций поиска, вставки и удаления хэш-таблицы имеют среднюю временную сложность O(1). Однако в худшем случае эти операции могут потребовать времени O(n), где n - количество элементов в таблице.

В основном используются строки (string) как ключи и через хэш функцию мы преобразуем строку в число 

![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/89788023-906e-4f31-8fee-83839d0b35f3)

но так как можно использовать любое свойство элемента что отличает его от другого то можно к примеру:
существует класс элемента с полями свойство 1 и свойство 2 
и для каждого элемента то есть объекта этого класса эти свойства будут разными то их можно использовать как ключи

```
class ArrayElement
    integer key
    string value // свойство элемента

function ArrayElementHash(element)
    // Простая хеш-функция: взятие остатка от деления на 10.
    return hash(element.key) % 10



function main()
    // Создаем хеш-таблицу с объектами ArrayElement и хеш-функцией ArrayElementHash
    hashTable = new HashTable(ArrayElement, ArrayElementHash)

    // Создаем объекты ArrayElement
    element1 = new ArrayElement(1, "Value 1")
    element2 = new ArrayElement(2, "Value 2")

    // Вставляем элементы в хеш-таблицу
    hashTable.insert(element1, element1.value)
    hashTable.insert(element2, element2.value)

    // Получаем значение по ключу
    value = hashTable.get(element1)
    print("Значение по ключу 1: " + value)

```

**Проблема коллизии **

Естественно, возникает вопрос, почему невозможно такое, что мы попадем дважды в одну ячейку массива, ведь представить функцию, которая ставит в сравнение каждому элементу совершенно различные натуральные числа просто невозможно. Именно так возникает проблема коллизии, или проблемы, когда хеш-функция выдает одинаковый индекс для разных элементов.

Существует несколько решений данной проблемы:

1. **Метод цепочек (Chaining)**:
   - Каждая ячейка хеш-таблицы содержит связанный список элементов с одинаковыми хешами.
   - При коллизии, элементы просто добавляются в соответствующий связанный список.
   - Этот метод прост и эффективен, но может потребовать дополнительной памяти для хранения связанных списков.

2. **Открытое адресное хеширование (Open Addressing)**:
   - Все элементы хранятся непосредственно в массиве.
   - При коллизии, алгоритм ищет следующую доступную ячейку в массиве.
   - Существует несколько вариантов открытого адресного хеширования, такие как линейное и квадратичное пробирование, двойное хеширование и другие.

3. **Рехеширование (Rehashing)**:
   - При коллизии, можно пересчитать хеш для ключа и попробовать разместить его в другой ячейке.
   - Рехеширование может быть полным (полностью изменяется хеш-функция) или частичным (небольшие изменения хеш-функции).

4. **Линейное пробирование (Linear Probing)**:
   - При коллизии, алгоритм проверяет следующую ячейку в массиве и продолжает двигаться по массиву до тех пор, пока не найдет свободную ячейку.
   - Этот метод может быть подвержен кластеризации (скоплению элементов вблизи друг друга).

5. **Двойное хеширование (Double Hashing)**:
   - При коллизии, алгоритм использует вторую хеш-функцию для вычисления новой ячейки для элемента.
   - Этот метод помогает разрешить коллизии без слияния элементов в одном месте, но требует правильного выбора второй хеш-функции.


**Использование хэш таблицы на cpp где элементы это строки**
```c++
#include <iostream>
#include <unordered_map>
#include <string>
using namespace std;

int main() {
    // Создаем хеш-таблицу для хранения строк (ключей) и их значений
    unordered_map<string, string> hashMap;

    // Вставляем элементы в хеш-таблицу
    hashMap["apple"] = "A sweet fruit";
    hashMap["banana"] = "A tropical fruit";
    hashMap["cherry"] = "A small, red fruit";

    // Получаем значения по ключам
    string key = "apple";
    if (hashMap.find(key) != hashMap.end()) {
        cout << "Значение для ключа '" << key << "': " << hashMap[key] << endl;
    } else {
        cout << "Ключ '" << key << "' не найден." << endl;
    }

    key = "grape";
    if (hashMap.find(key) != hashMap.end()) {
        cout << "Значение для ключа '" << key << "': " << hashMap[key] << endl;
    } else {
        cout << "Ключ '" << key << "' не найден." << endl;
    }

    return 0;
}

```
**Хэш таблица написанная без библиотеки**
```c++
#include <iostream>
#include <vector>
#include <string>

using namespace std;

// Простая хеш-функция, которая возвращает сумму кодов символов ключа
int simpleHash(const string& key, int tableSize) {
    int hash = 0;
    for (char ch : key) {
        hash += static_cast<int>(ch);
    }
    return hash % tableSize;
}

int main() {
    const int tableSize = 100;
    vector<pair<string, string>> hashTable[tableSize];

    // Вставляем элементы в хеш-таблицу
    hashTable[simpleHash("apple", tableSize)].emplace_back("apple", "A sweet fruit");
    hashTable[simpleHash("banana", tableSize)].emplace_back("banana", "A tropical fruit");
    hashTable[simpleHash("cherry", tableSize)].emplace_back("cherry", "A small, red fruit");

    // Получаем значение по ключу
    string key = "apple";
    int index = simpleHash(key, tableSize);
    string value = "Key not found";

    for (const auto& entry : hashTable[index]) {
        if (entry.first == key) {
            value = entry.second;
            break;
        }
    }

    cout << "Value for 'apple': " << value << endl;

    key = "grape";
    index = simpleHash(key, tableSize);
    value = "Key not found";

    for (const auto& entry : hashTable[index]) {
        if (entry.first == key) {
            value = entry.second;
            break;
        }
    }

    cout << "Value for 'grape': " << value << endl;

    return 0;
}

```

без встроенной функции static_cast<int> :

```c++
#include <iostream>
#include <vector>
#include <string>

using namespace std;

// Простая хеш-функция, которая возвращает сумму кодов символов ключа
int simpleHash(const string& key, int tableSize) {
    int hash = 0;
    for (char ch : key) {
        hash += ch;
    }
    return hash % tableSize;
}

int main() {
    const int tableSize = 100;
    vector<pair<string, string>> hashTable[tableSize];

    // Вставляем элементы в хеш-таблицу
    hashTable[simpleHash("apple", tableSize)].emplace_back("apple", "A sweet fruit");
    hashTable[simpleHash("banana", tableSize)].emplace_back("banana", "A tropical fruit");
    hashTable[simpleHash("cherry", tableSize)].emplace_back("cherry", "A small, red fruit");

    // Получаем значение по ключу
    string key = "apple";
    int index = simpleHash(key, tableSize);
    string value = "Key not found";

    for (const auto& entry : hashTable[index]) {
        if (entry.first == key) {
            value = entry.second;
            break;
        }
    }

    cout << "Value for 'apple': " << value << endl;

    key = "grape";
    index = simpleHash(key, tableSize);
    value = "Key not found";

    for (const auto& entry : hashTable[index]) {
        if (entry.first == key) {
            value = entry.second;
            break;
        }
    }

    cout << "Value for 'grape': " << value << endl;

    return 0;
}
```

**Хэш функция которая выдает именно индекс в массиве и не превышает его размер**
```c++
int customHash(const string& key, int tableSize) {
    int hash = 0;
    int prime = 31;  // Простое число для улучшения равномерного распределения хешей 

    for (char ch : key) {
        hash = (hash * prime + static_cast<int>(ch)) % tableSize;
    }
    
    return hash;
}
```

без встроенной функции static_cast<int> :

```c++
int customHash(const string& key, int tableSize) {
    int hash = 0;
    int prime = 31;

    for (char ch : key) {
        hash = (hash * prime + ch) % tableSize;
    }
    
    return hash;
}

```

## Хэш функции

**Полиномиальная**
Полиномиальная хеш-функция - это один из типов хеш-функций, которые используют многочлены для вычисления хеш-кода ключа (например, строки) путем применения формулы, которая включает коэффициенты многочлена и значения символов ключа.

Примером полиномиальной хеш-функции может быть следующая формула:

```
hash(key) = (a_n * x^n + a_(n-1) * x^(n-1) + ... + a_2 * x^2 + a_1 * x + a_0) % M
```

Где:
- `a_n, a_(n-1), ..., a_1, a_0` - коэффициенты многочлена.
- `x` - некоторое число (часто выбирается простое число).
- `n` - длина ключа.
- `M` - размер хеш-таблицы.

Полиномиальные хеш-функции могут быть полезны для равномерного распределения ключей в хеш-таблице, что помогает уменьшить коллизии. Они используются в различных алгоритмах и структурах данных, таких как хеш-таблицы и строковые таблицы.

