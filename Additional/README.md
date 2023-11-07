
Алгоритм Хаффмана - это метод сжатия данных, который используется для уменьшения размера файла или сообщения путем кодирования символов с разной вероятностью появления так, чтобы более часто встречающиеся символы имели более короткие коды, а менее часто встречающиеся символы имели более длинные коды.
Принцип работы алгоритма Хаффмана следующий:

1. Создание таблицы частот: Сначала анализируется входной текст или файл, и для каждого символа определяется его частота появления. Это позволяет построить таблицу, в которой символы упорядочены по убыванию частоты.

2. Создание дерева Хаффмана: Далее, символы и их частоты используются для построения двоичного дерева, называемого деревом Хаффмана. Алгоритм начинает с создания листьев дерева для каждого символа, соответствующего их частоте. Затем происходит объединение наименее часто встречающихся символов, создавая узлы дерева, и присвоение им суммарной частоты. Этот процесс продолжается до тех пор, пока не будет сформировано единое дерево.

3. Присвоение кодов: Дерево Хаффмана используется для присвоения кодов символам. При движении по дереву от корня к листьям, код для каждого символа строится следующим образом: при переходе по левой ветке добавляется "0", при переходе по правой ветке добавляется "1". Каждый символ получает уникальный бинарный код, который является префиксным кодом, то есть никакой код не является префиксом другого.

4. Кодирование данных: Теперь можно использовать полученные коды для кодирования исходных данных. Это означает замену каждого символа его соответствующим бинарным кодом в сообщении или файле.

5. Декодирование данных: Для декодирования данных используется построенное дерево Хаффмана. При чтении битового потока алгоритм двигается по дереву, начиная с корня, и восстанавливает исходные символы.

Алгоритм Хаффмана обеспечивает эффективное сжатие данных, особенно в случаях, когда некоторые символы встречаются намного чаще, чем другие.


![image](https://github.com/Arlan-Z/Algorithms-and-data-structures/assets/122739941/702c8f52-2098-466a-bb11-c3430aea831a)


```c++
#include <iostream>
#include <string>
#include <map>
#include <queue>

struct HuffmanNode {
    char data;
    int frequency;
    HuffmanNode* left;
    HuffmanNode* right;

    HuffmanNode(char data, int frequency) : data(data), frequency(frequency), left(nullptr), right(nullptr) {}
};

struct CompareNodes {
    bool operator()(HuffmanNode* a, HuffmanNode* b) {
        return a->frequency > b->frequency;
    }
};

HuffmanNode* buildHuffmanTree(const std::map<char, int>& frequencies) {
    std::priority_queue<HuffmanNode*, std::vector<HuffmanNode*>, CompareNodes> minHeap;

    for (const auto& entry : frequencies) {
        minHeap.push(new HuffmanNode(entry.first, entry.second));
    }

    while (minHeap.size() > 1) {
        HuffmanNode* left = minHeap.top();
        minHeap.pop();

        HuffmanNode* right = minHeap.top();
        minHeap.pop();

        HuffmanNode* parent = new HuffmanNode('\0', left->frequency + right->frequency);
        parent->left = left;
        parent->right = right;
        minHeap.push(parent);
    }

    return minHeap.top();
}

void buildHuffmanCodes(HuffmanNode* root, const std::string& code, std::map<char, std::string>& huffmanCodes) {
    if (root == nullptr) {
        return;
    }

    if (root->data != '\0') {
        huffmanCodes[root->data] = code;
    }

    buildHuffmanCodes(root->left, code + "0", huffmanCodes);
    buildHuffmanCodes(root->right, code + "1", huffmanCodes);
}

std::string encodeMessage(const std::map<char, std::string>& huffmanCodes, const std::string& message) {
    std::string encodedMessage = "";
    for (char c : message) {
        encodedMessage += huffmanCodes.at(c);
    }
    return encodedMessage;
}

std::string decodeMessage(HuffmanNode* root, const std::string& encodedMessage) {
    std::string decodedMessage = "";
    HuffmanNode* current = root;

    for (char bit : encodedMessage) {
        if (bit == '0') {
            current = current->left;
        } else {
            current = current->right;
        }

        if (current->data != '\0') {
            decodedMessage += current->data;
            current = root;
        }
    }

    return decodedMessage;
}

int main() {
    std::map<char, int> frequencies;
    std::string message = "hello, world!";

    for (char c : message) {
        frequencies[c]++;
    }

    HuffmanNode* root = buildHuffmanTree(frequencies);
    std::map<char, std::string> huffmanCodes;
    buildHuffmanCodes(root, "", huffmanCodes);

    std::string encodedMessage = encodeMessage(huffmanCodes, message);
    std::string decodedMessage = decodeMessage(root, encodedMessage);

    std::cout << "Исходное сообщение: " << message << std::endl;
    std::cout << "Закодированное сообщение: " << encodedMessage << std::endl;
    std::cout << "Декодированное сообщение: " << decodedMessage << std::endl;

    return 0;
}

```

## подробное объяснение:

https://www.geeksforgeeks.org/huffman-coding-greedy-algo-3/


