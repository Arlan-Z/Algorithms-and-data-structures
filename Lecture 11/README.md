## Алгоритм  Кнута Морриса-Пратта по поиску под строки

![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/b4959428-0ef9-4a08-a5ea-d6c5d99377fe)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/1724aa42-7ca3-484f-8915-33ed2d50930e)
![Uploading image.png…]()



```c++
#include <iostream>
#include <vector>

using namespace std;

// Функция для построения префикс-функции
vector<int> computePrefixFunction(const string& pattern) { 
    int m = pattern.length();
    vector<int> prefix(m, 0);
    int j = 0;

    for (int i = 1; i < m; ++i) {
        // Пока символы не совпадают и не достигнут конец префикса,
        // уменьшаем значение j, используя префикс-функцию
        while (j > 0 && pattern[i] != pattern[j]) {
            j = prefix[j - 1];
        }

        // Если символы совпадают, увеличиваем значение j
        if (pattern[i] == pattern[j]) {
            ++j;
        }

        // Сохраняем значение префикс-функции для текущей позиции
        prefix[i] = j;
    }

    return prefix;
}

// Функция для поиска всех вхождений подстроки в строку с использованием KMP
vector<int> kmpSearch(const string& text, const string& pattern) {
    int n = text.length();
    int m = pattern.length();
    vector<int> result;

    // Построение префикс-функции для образца
    vector<int> prefix = computePrefixFunction(pattern);

    int i = 0;  // Индекс для строки text
    int j = 0;  // Индекс для образца pattern

    while (i < n) {
        // Если символы совпадают, увеличиваем индексы i и j
        if (pattern[j] == text[i]) {
            ++i;
            ++j;
        }

        // Если найдено вхождение образца, сохраняем позицию и обновляем j
        if (j == m) {
            result.push_back(i - j);
            j = prefix[j - 1];
        } 
        // Если символы не совпадают, обновляем индекс j с использованием префикс-функции
        else if (i < n && pattern[j] != text[i]) {
            if (j != 0) {
                j = prefix[j - 1];
            } else {
                ++i;
            }
        }
    }

    return result;
}

int main() {
    // Пример использования
    string text = "ababcababcabcabc";
    string pattern = "abc";

    // Поиск вхождений подстроки в строку
    vector<int> occurrences = kmpSearch(text, pattern);

    // Вывод результатов
    if (occurrences.empty()) {
        cout << "Подстрока не найдена в строке." << endl;
    } else {
        cout << "Подстрока найдена в позициях: ";
        for (int pos : occurrences) {
            cout << pos << " ";
        }
        cout << endl;
    }

    return 0;
}

```

1. **Функция `computePrefixFunction`:**
    ```cpp
    vector<int> computePrefixFunction(const string& pattern) {
        // ...
    }
    ```
   - Эта функция строит префикс-функцию для заданной подстроки (образца).
   - Вектор `prefix` используется для хранения значений префикс-функции.
   - Индекс `j` указывает на текущую позицию в префикс-функции.

2. **Цикл в `computePrefixFunction`:**
    ```cpp
    for (int i = 1; i < m; ++i) {
        // ...
    }
    ```
   - Цикл начинается с 1, так как значение префикс-функции для первой позиции всегда равно 0.

3. **Обновление префикс-функции в `computePrefixFunction`:**
    ```cpp
    while (j > 0 && pattern[i] != pattern[j]) {
        j = prefix[j - 1];
    }
    ```
   - Если символы не совпадают и не достигнут конец префикса, уменьшаем значение `j` с использованием префикс-функции.

4. **Увеличение `j` в `computePrefixFunction`:**
    ```cpp
    if (pattern[i] == pattern[j]) {
        ++j;
    }
    ```
   - Если символы совпадают, увеличиваем значение `j`.

5. **Сохранение значения вектора `prefix` в `computePrefixFunction`:**
    ```cpp
    prefix[i] = j;
    ```
   - Сохраняем текущее значение `j` в векторе префикс-функции для текущей позиции `i`.

6. **Функция `kmpSearch`:**
    ```cpp
    vector<int> kmpSearch(const string& text, const string& pattern) {
        // ...
    }
    ```
   - Эта функция использует префикс-функцию для поиска всех вхождений подстроки в строке.

7. **Объявление переменных в `kmpSearch`:**
    ```cpp
    int n = text.length();
    int m = pattern.length();
    vector<int> result;
    ```
   - `n` и `m` хранят длины строки `text` и образца `pattern`.
   - `result`

 будет содержать позиции вхождений подстроки в строку.

8. **Построение префикс-функции в `kmpSearch`:**
    ```cpp
    vector<int> prefix = computePrefixFunction(pattern);
    ```
   - Строим префикс-функцию для образца `pattern`.

9. **Цикл в `kmpSearch`:**
    ```cpp
    while (i < n) {
        // ...
    }
    ```
    - Цикл продолжается до тех пор, пока не пройдем всю строку `text`.

10. **Увеличение индексов в `kmpSearch`:**
    ```cpp
    if (pattern[j] == text[i]) {
        ++i;
        ++j;
    }
    ```
    - Если символы совпадают, увеличиваем индексы `i` и `j`.

11. **Сохранение позиции в `kmpSearch`:**
    ```cpp
    if (j == m) {
        result.push_back(i - j);
        j = prefix[j - 1];
    }
    ```
    - Если найдено вхождение образца, сохраняем позицию и обновляем `j`.

12. **Обновление `j` в `kmpSearch`:**
    ```cpp
    else if (i < n && pattern[j] != text[i]) {
        if (j != 0) {
            j = prefix[j - 1];
        } else {
            ++i;
        }
    }
    ```
    - Если символы не совпадают, обновляем индекс `j` с использованием префикс-функции.

для лучшего понимания:
https://www.youtube.com/watch?v=2ogqPWJSftE
https://www.youtube.com/watch?v=4jY57Ehc14Y
https://www.youtube.com/watch?v=kBW6oPaVjq0
