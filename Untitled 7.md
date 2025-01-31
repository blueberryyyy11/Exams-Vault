# Дополнительные экзаменационные вопросы по ООП (Часть 4)

### 1. Работа с `std::move` и r-value ссылками

**Вопрос:** Какой вывод будет у следующего кода?

```cpp
#include <iostream>
#include <string>

void process(const std::string& str) {
    std::cout << "L-value: " << str << "\n";
}

void process(std::string&& str) {
    std::cout << "R-value: " << str << "\n";
}

int main() {
    std::string s = "Hello";
    process(s);
    process(std::move(s));
    process("World");
}
```

- a) L-value: Hello, L-value: Hello, L-value: World
- b) L-value: Hello, R-value: Hello, R-value: World
- c) L-value: Hello, R-value: Hello, L-value: World
- d) Undefined Behavior

---

### 2. Классы с виртуальными функциями

**Вопрос:** Какой результат выполнения следующего кода?

```cpp
#include <iostream>

struct Base {
    virtual void show() { std::cout << "Base"; }
};

struct Derived : Base {
    void show() override { std::cout << "Derived"; }
};

int main() {
    Base* obj = new Derived();
    obj->show();
    delete obj;
}
```

- a) Base
- b) Derived
- c) Compilation Error
- d) Undefined Behavior

---

### 3. Перегрузка операторов

**Вопрос:** Что выведет следующий код?

```cpp
#include <iostream>

class Number {
    int value;
public:
    Number(int v) : value(v) {}
    Number operator+(const Number& other) {
        return Number(value + other.value);
    }
    void print() { std::cout << value; }
};

int main() {
    Number a(5), b(10);
    Number c = a + b;
    c.print();
}
```

- a) 15
- b) Compilation Error
- c) Undefined Behavior
- d) Garbage Value

---

### 4. Исключения

**Вопрос:** Что произойдет при выполнении следующего кода?

```cpp
#include <iostream>
#include <stdexcept>

void checkValue(int value) {
    if (value < 0) throw std::invalid_argument("Negative value");
    std::cout << "Value: " << value;
}

int main() {
    try {
        checkValue(10);
        checkValue(-5);
    } catch (const std::exception& e) {
        std::cerr << e.what();
    }
}
```

- a) Value: 10Value: -5
- b) Value: 10Negative value
- c) Value: 10 Negative value
- d) Compilation Error

---

### 5. Шаблонные функции

**Вопрос:** Напишите шаблонную функцию, которая принимает два аргумента и возвращает наибольший из них.

**Ответ:**

```cpp
template <typename T>
T maxValue(const T& a, const T& b) {
    return (a > b) ? a : b;
}

int main() {
    std::cout << maxValue(5, 10);
    std::cout << maxValue(3.14, 2.71);
    return 0;
}
```

---

### 6. Конструкторы и деструкторы

**Вопрос:** Какой будет порядок вызова конструкторов и деструкторов в следующем коде?

```cpp
#include <iostream>

struct A {
    A() { std::cout << "A"; }
    ~A() { std::cout << "~A"; }
};

struct B {
    B() { std::cout << "B"; }
    ~B() { std::cout << "~B"; }
};

int main() {
    A a;
    B b;
}
```

- a) AB~~A~~B
- b) BA~~B~~A
- c) AB~~B~~A
- d) Compilation Error

---

### 7. Работа с `std::initializer_list`

**Вопрос:** Что выведет следующий код?

```cpp
#include <iostream>
#include <initializer_list>

class MyClass {
    std::initializer_list<int> data;
public:
    MyClass(std::initializer_list<int> list) : data(list) {}
    void print() {
        for (auto x : data) std::cout << x << " ";
    }
};

int main() {
    MyClass obj = {1, 2, 3, 4};
    obj.print();
}
```

- a) 1 2 3 4
- b) Compilation Error
- c) Undefined Behavior
- d) Garbage Value

---

### 8. RAII (Resource Acquisition Is Initialization)

**Вопрос:** Какой результат выполнения следующего кода?

```cpp
#include <iostream>
#include <memory>

class Resource {
public:
    Resource() { std::cout << "Acquired\n"; }
    ~Resource() { std::cout << "Released\n"; }
};

int main() {
    {
        std::unique_ptr<Resource> res = std::make_unique<Resource>();
    }
    std::cout << "End\n";
}
```

- a) Acquired End Released
- b) Acquired Released End
- c) Acquired Released
- d) Compilation Error

---

### 9. Абстрактные классы

**Вопрос:** Можно ли создать объект абстрактного класса? Объясните с примером.

**Ответ:**

Нельзя создать объект абстрактного класса напрямую, так как он содержит хотя бы одну чисто виртуальную функцию. Пример:

```cpp
#include <iostream>

struct Abstract {
    virtual void func() = 0; // Чисто виртуальная функция
};

struct Derived : Abstract {
    void func() override { std::cout << "Derived implementation"; }
};

int main() {
    // Abstract obj; // Ошибка
    Derived obj;
    obj.func();
}
```

---

### 10. Итераторы STL

**Вопрос:** Что произойдет при выполнении следующего кода?

```cpp
#include <iostream>
#include <vector>

int main() {
    std::vector<int> v = {1, 2, 3, 4};
    for (auto it = v.begin(); it != v.end(); ++it) {
        std::cout << *it;
    }
}
```

- a) 1234
- b) Compilation Error
- c) Undefined Behavior
- d) Garbage Value