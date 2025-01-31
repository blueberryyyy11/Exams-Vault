### Материал для изучения и вопросы для экзамена по ООП

#### 1. Работа с ссылками и r-value выражениями

**Пример задачи:** Что будет результатом выполнения следующего кода?

```cpp
#include <iostream>

int main() {
    int x = 6;
    int& y = x;
    const int& z = x;
    ++y;
    std::cout << x << " " << y << " " << z;
}
```

**Пояснение:**

- `int& y = x;` — обычная ссылка на переменную x.
- `const int& z = x;` — константная ссылка на x.
- Изменение `y` повлияет на значение `x` и, следовательно, на `z`, так как все три элемента ссылаются на одно и то же значение в памяти.

#### 2. Виртуальные функции и их использование

**Пример задачи:** Рассмотрите следующий код:

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

Какой будет вывод программы? **Ответ:** Вывод будет `B`, так как вызов виртуальной функции определяется типом объекта, а не ссылкой или указателем.

#### 3. Конструкторы и операторы присваивания

**Пример задачи:** Определите правильный вариант для класса с копирующим конструктором и оператором присваивания:

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

**Ответ:**

- Конструктор по умолчанию вызван при создании `c1`.
- Копирующий конструктор вызван для инициализации `c2`.
- Оператор присваивания вызван при присваивании `c2 = c1`.

#### 4. Работа с `std::string`

**Пример задачи:** Что будет результатом выполнения кода?

```cpp
#include <string>
#include <iostream>

void h(const std::string& x) {
    std::cout << "1";
}

void h(std::string&& x) {
    std::cout << "2";
}

int main() {
    std::string s = "123";
    h(s + "4");
    h(s);
    h(std::move(s));
}
```

**Ответ:**

- `h(s + "4")` — вызовется версия с r-value ссылкой, так как результат выражения `s + "4"` временный объект.
- `h(s)` — вызовется версия с `const std::string&`, так как `s` — это l-value.
- `h(std::move(s))` — вызовется версия с r-value ссылкой, так как `std::move` делает объект временным.

#### 5. Шаблонные функции и классы

**Пример задачи:** Напишите шаблонную функцию, которая принимает объект любого типа и выводит его значение. **Решение:**

```cpp
template <typename T>
void print(const T& value) {
    std::cout << value;
}
```

**Пример задачи:** Создайте шаблонный класс, который имеет поле типа T и метод для вывода этого поля. **Решение:**

```cpp
template <typename T>
class MyClass {
    T value;
public:
    MyClass(T v) : value(v) {}
    void print() { std::cout << value; }
};
```

#### 6. Полиморфизм и перегрузка функций

**Пример задачи:** Определите пример перегрузки функций и переопределения в одном коде:

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

int main() {
    print(10);        // Перегрузка
    print(10.5);      // Перегрузка

    Base* obj = new Derived;
    obj->display();   // Переопределение
    delete obj;
}
```

**Ответ:**

- Перегрузка — использование функций с одним именем, но разными типами параметров.
- Переопределение — изменение поведения виртуальной функции базового класса в производном.

#### 7. Исключения в C++ и RAII

**Пример задачи:** Что произойдет при выполнении следующего кода?

```cpp
#include <iostream>
#include <stdexcept>

void divide(int a, int b) {
    if (b == 0) throw std::runtime_error("Деление на ноль");
    std::cout << a / b;
}

int main() {
    try {
        divide(10, 2);
        divide(10, 0);
    } catch (const std::runtime_error& e) {
        std::cerr << "Ошибка: " << e.what();
    }
}
```

**Ответ:**

- При первом вызове `divide(10, 2)` программа выведет результат `5`.
- При втором вызове `divide(10, 0)` будет выброшено исключение, которое обработается в блоке `catch`.
- Вывод: `5Ошибка: Деление на ноль`.

#### 8. Использование конструкторов с `std::initializer_list`

**Пример задачи:** Напишите класс, поддерживающий инициализацию через список значений:

```cpp
#include <initializer_list>
#include <iostream>

class MyClass {
    std::vector<int> data;
public:
    MyClass(std::initializer_list<int> list) : data(list) {}
    void print() {
        for (int x : data) std::cout << x << " ";
    }
};

int main() {
    MyClass obj = {1, 2, 3, 4, 5};
    obj.print();
}
```

**Ответ:**

- Конструктор `MyClass(std::initializer_list<int> list)` позволяет передавать список значений при инициализации объекта.
- Программа выведет: `1 2 3 4 5`.