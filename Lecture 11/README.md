## Алгоритм  Кнута Морриса-Пратта по поиску под строки

Его временная сложность O(n)
Ну если рассматривать его полностью если префикс функция занимает k и сам алгоритм n то временная сложность будет O(n+k)

![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/b4959428-0ef9-4a08-a5ea-d6c5d99377fe)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/952b0ddc-8658-4acb-8272-067579f8de08)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/1fda41b4-bb67-4857-bc0c-99b1bd6f2801)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/03f8f15a-d73a-4860-b090-fc2976183303)
![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/b703ee86-7036-4b40-9821-ffff9746807d)

1. Текст: "onionionspl"
2. Образец: "onions" pattern length = 6
3. Префикс-функция: [0, 0, 0, 1, 2, 0]

- Индекс i для текста начинается с 0, индекс j для образца также начинается с 0.

префикс-функция это либо увеличение i если j==0, либо j = prefix[j-1]

Идем по тексту и сравниваем символы:

- text[i] = 'o', pattern[j] = 'o': Совпадение! Увеличиваем i и j (i = 0, j = 0).
- text[i] = 'n', pattern[j] = 'n': Совпадение! Увеличиваем i и j (i = 1, j = 1).
- text[i] = 'i', pattern[j] = 'i': Совпадение! Увеличиваем i и j (i = 2, j = 2).
- text[i] = 'o', pattern[j] = 'o': Совпадение! Увеличиваем i и j (i = 3, j = 3).
- text[i] = 'n', pattern[j] = 'n': Совпадение! Увеличиваем i и j (i = 4, j = 4).
- text[i] = 'i', pattern[j] = 's': (i = 5, j = 5) Несовпадение! Используем префикс-функцию для обновления индекса (i oставляем так как j!=0) j: j = prefix[j - 1] = prefix[5-1] = 2.
- text[i] = 'i', pattern[j] = 'i': Совпадение! Увеличиваем i и j (i = 5, j = 2).
- text[i] = 'o', pattern[j] = 'o': Совпадение! Увеличиваем i и j (i = 6, j = 3). 
- text[i] = 'n', pattern[j] = 'n': Совпадение! Увеличиваем i и j (i = 7, j = 4).
- text[i] = 's', pattern[j] = 's': Совпадение! Увеличиваем i и j (i = 8, j = 5).
- j == 6 те j == pattern length это означает мы нашли вхождение на позиции i - j = 8 - 5 = 3;
  
Таким образом, вхождение "onions" найдено в позиции 3 в тексте "onionionspl".

```c++
#include <iostream>
#include <vector>

using namespace std;

// Функция для построения префикс-функции
vector<int> computePrefixFunction(const string& pattern) { 
    int m = pattern.length();
    vector<int> prefix(m, 0);
    int j = 0; // указывает на длину текущего суффикса-префикса подстроки до позиции i

    for (int i = 1; i < m; ++i) {
        // Пока символы не совпадают и не достигнут конец префикса,
        // уменьшаем значение j, используя префикс-функцию
        while (j > 0 && pattern[i] != pattern[j]) {
            j = prefix[j - 1]; // pi[j - 1] возвращает длину суффикса-префикса подстроки для позиции j - 1 то есть пред последнюю самую большую длинну. 
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
```c++
// упрощенная computePrefixFunction
vector<int> prefix(string p){
    int m = p.size();
    vector<int> pref(m);
    int j = 0; // j - и длинна подстроки и индекс до длинны префикса
    int i = 1;
    while(i < m){
        while(j > 0 && p[i] != p[j]){
            j = pref[j - 1]; // ищем макс длинну префикаса если не совпало уменьшшаем ее пока не совпадет
        }
        if(p[i] == p[j]){
            j++; // увеличиваем размер если совпадение
        }
        pref[i] = j; // ставим этот размер на i если j 0 то i это начало этого префикса
        i++;
    } 
    return pref;
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
   Если символы не совпадают, то мы изменяем значение j, используя значение из префикс функции для индекса j - 1.
   Это значит, что мы "откатываемся" назад по паттерну, чтобы найти более короткий префикс, который также является суффиксом части паттерна, совпадающей с префиксом.
   в условии проверяется, что j больше 0. Это гарантирует, что мы не выйдем за пределы массива при обращении к prefix[j - 1].

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

моя дедукция при знакомстве с этим алгоритмом
с начала я понял что i это индекс который начинается от начала до конца строки p так как ее используют как индекс от p и его макс размер это размер строки очевидно что это индекс по строке . Далее обычный цикл по строке и внутри него еще один цикл. В нем значение j присваивается значению массива на индексе j-1 и это повторяется до тех пор пока p[i] != p[j] это значит что цикл будет до тех пор пока символ на i не будет равен на j .после цикла есть что то это означает что прошлый цикл рано или поздно закончится , есть условие если символы совпали тогда увеличиваем j это означает j это число которое увеличивается при совпадении с i . после этого есть prefix[i]=j это означает что на индексе i ...я понял что j указывает на длину под строки которая повторяется
и исходя из того что ее используют как индекс
вывел гипотезу что j указывает на начало прошлой подстроки которая совпадает с текущим
проверил это на примере
пример из lab 9 c:

```сpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

vector<int> prefix(string p){
    int m = p.size();
    vector<int> pref(m);
    int j = 0; // j - и длинна подстроки и индекс до длинны префикса
    int i = 1;
    while(i < m){
        while(j > 0 && p[i] != p[j]){
            j = pref[j - 1]; // ищем макс длинну префикаса если не совпало уменьшшаем ее пока не совпадет
        }
        if(p[i] == p[j]){
            j++; // увеличиваем размер если совпадение
        }
        pref[i] = j; // ставим этот размер на i если j 0 то i это начало этого префикса
        i++;
    } 
    return pref;
}
//ababaca
//0012301  j-1 указывает на индекс совпадения
int kmp(string t, string p){
    int m = p.size();
    int n = t.size();
    int i = 0;
    int j = 0; // индекс в p
    vector<int> occur;
    vector<int> pref = prefix(p);
    while(i < n){
        if(t[i]==p[j]){
            i++;
            j++;
        }
        if(j == m){ // p полностью входит в t 
            occur.push_back(i - j);// вставляем индекс начала этой под строки которая совпала с p
            j = pref[j - 1]; // уменьшаем размер подстроки в p для проверки
        }
        else if(t[i] != p[j]){// если символы не совпали 
            if(j > 0){
                j = pref[j - 1];
            }
            else{ // длинна префикса отрицательна следовательно мы переходим на следующий индекс 
                i++;
            }
            
        }
    }
    if(!occur.empty()){ 
        return n  - occur[0] - m; // выводим совпадения
    }
    else{
        return -1;
    }
}

int main() {
    string text = "";
    string pattern = "";
    cin>>text>>pattern;
    if(text.size()!=pattern.size()){
        cout<<-1<<endl;
    }
    else{
        text += text;
        int res = kmp(text, pattern);
        cout<<res<<endl;
    }
    return 0;
}

// dtsize - 1 - (first + plen - 1)
// zabcd
// cdzab +2
// abcdz +2  => 4

// abcabc
//   cab1

// zabcdzabcd 9 - 5 
//  abcdz1234

//cfab
//abcf
//cfabcfab
//  abcf12
```
