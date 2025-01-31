
# Дополнительные потенциальные вопросы для экзаменационного теста по ООП

### 1. Работа с `const` и r-value ссылками
**Вопрос:** Какой из приведённых ниже вариантов объявления ссылки на переменную с именем `x` (типа `int`) является корректным?
- a) `const int& y = x;`
- b) `auto y = x;`
- c) `int&& y = x;`
- d) `int* y = x;`

**Ответ:** Верные варианты: a), c).

---

### 2. Виртуальные функции
**Вопрос:** Что произойдёт при выполнении следующего кода?

```cpp
#include <iostream>

struct A {
    virtual void f() { std::cout << "A"; }
};

struct B : A {
    void f() override { std::cout << "B"; }
};

int main() {
    B obj;
    A& ref = obj;
    ref.f();
}
```

**Варианты ответа:**
- a) Выведет "A"
- b) Выведет "B"
- c) Ошибка компиляции
- d) Неопределённое поведение

**Правильный ответ:** b).

---

### 3. Перегрузка операторов
**Вопрос:** Рассмотрите следующий код. Какие операции выполняются при создании объектов `c2` и `c2 = c1`?

```cpp
#include <iostream>

struct C {
    C() { std::cout << "1"; }
    C(const C& other) { std::cout << "2"; }
    C& operator=(const C& other) {
        std::cout << "3";
        return *this;
    }
};

int main() {
    C c1;
    C c2 = c1;
    c2 = c1;
}
```

**Варианты ответа:**
- a) Конструктор по умолчанию, копирующий конструктор
- b) Конструктор по умолчанию, оператор присваивания
- c) Только оператор присваивания
- d) Копирующий конструктор, оператор присваивания

**Правильный ответ:** a).

---

### 4. Исключения в C++
**Вопрос:** Что выведет следующая программа?

```cpp
#include <iostream>
#include <stdexcept>

int divide(int a, int b) {
    if (b == 0) throw std::runtime_error("Ошибка деления на ноль");
    return a / b;
}

int main() {
    try {
        std::cout << divide(10, 2);
        std::cout << divide(10, 0);
    } catch (const std::runtime_error& e) {
        std::cerr << e.what();
    }
}
```

**Варианты ответа:**
- a) 5
- b) 5Ошибка деления на ноль
- c) Ошибка компиляции
- d) Программа завершится аварийно

**Правильный ответ:** b).

---

### 5. Работа с `std::initializer_list`
**Вопрос:** Какой из следующих конструкторов подходит для инициализации объекта через список значений?

```cpp
#include <initializer_list>
#include <vector>
#include <iostream>

class MyClass {
    std::vector<int> data;
public:
    // Какой из них подходит?
    MyClass(std::initializer_list<int> list) : data(list) {}
    MyClass(int single_value) : data{single_value} {}
};
```

**Ответ:** Конструктор `MyClass(std::initializer_list<int> list)` позволяет передавать список значений.

---

### 6. Перегрузка функций
**Вопрос:** Какой из примеров ниже относится к перегрузке функций, а какой к переопределению?

```cpp
#include <iostream>

void print(int x) {
    std::cout << "int: " << x;
}

void print(double x) {
    std::cout << "double: " << x;
}

struct Base {
    virtual void display() { std::cout << "Base"; }
};

struct Derived : Base {
    void display() override { std::cout << "Derived"; }
};
```

**Ответ:** 
- Перегрузка: `print(int x)` и `print(double x)`.
- Переопределение: метод `display()` в `Derived`.

---

### 7. Конструкторы и их модификации
**Вопрос:** Что означает следующий код?

```cpp
struct A {
    A() = default;
    A(const A&) = delete;
    A(A&&) noexcept = default;
};
```

**Ответ:** 
- Конструктор по умолчанию разрешён.
- Копирующий конструктор запрещён.
- Перемещающий конструктор разрешён и помечен как noexcept.

---
