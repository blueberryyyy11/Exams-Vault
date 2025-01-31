
# Дополнительные вопросы для экзамена по ООП

### 1. Работа с const и r-value ссылками
**Вопрос:** Какой будет результат выполнения следующего кода?
```cpp
#include <iostream>
void foo(const int& x) { std::cout << "L-value"; }
void foo(int&& x) { std::cout << "R-value"; }
int main() {
    int a = 10;
    foo(a);
    foo(20);
    foo(std::move(a));
}
```
**Варианты ответа:**
a) `L-value L-value L-value`  
b) `L-value R-value R-value`  
c) `L-value R-value L-value`  
d) `L-value R-value R-value`

---

### 2. Конструкторы с `std::initializer_list`
**Вопрос:** Напишите класс с поддержкой инициализации через `std::initializer_list`.
```cpp
class MyClass {
public:
    MyClass(std::initializer_list<int> list);
};
```
**Какая из следующих реализаций конструктора корректна?**  
a) `MyClass(std::initializer_list<int> list) : data(list) {}`  
b) `MyClass(const std::initializer_list<int>& list) : data(list) {}`  
c) `MyClass(std::vector<int> list) : data(list) {}`  
d) Все вышеперечисленные корректны

---

### 3. Перегрузка операторов
**Вопрос:** Что выведет следующая программа?
```cpp
#include <iostream>
struct A {
    int value;
    A(int v) : value(v) {}
    A operator+(const A& other) {
        return A(value + other.value);
    }
};
int main() {
    A a1(5), a2(10);
    A a3 = a1 + a2;
    std::cout << a3.value;
}
```
**Варианты ответа:**  
a) 15  
b) Ошибка компиляции  
c) 5  
d) 10

---

### 4. Виртуальные функции и наследование
**Вопрос:** Определите, какой будет вывод следующего кода:
```cpp
#include <iostream>
struct Base {
    virtual void print() { std::cout << "Base"; }
};
struct Derived : Base {
    void print() override { std::cout << "Derived"; }
};
int main() {
    Base* obj = new Derived;
    obj->print();
    delete obj;
}
```
**Варианты ответа:**  
a) Base  
b) Derived  
c) Ошибка компиляции  
d) BaseDerived

---

### 5. Исключения в C++
**Вопрос:** Что произойдет при выполнении данного кода?
```cpp
#include <stdexcept>
#include <iostream>
int main() {
    try {
        throw std::runtime_error("Error!");
    } catch (const std::runtime_error& e) {
        std::cout << e.what();
    } catch (...) {
        std::cout << "Unknown error";
    }
}
```
**Варианты ответа:**  
a) Программа выведет `Error!`  
b) Программа выведет `Unknown error`  
c) Ошибка компиляции  
d) Программа завершится без вывода

---

### 6. Шаблонные функции
**Вопрос:** Какой будет результат выполнения следующей программы?
```cpp
#include <iostream>
template <typename T>
void print(T value) {
    std::cout << value;
}
int main() {
    print<int>(10);
    print<double>(5.5);
}
```
**Варианты ответа:**  
a) `105.5`  
b) Ошибка компиляции  
c) Ничего не выведет  
d) `10 5.5`

---

### 7. Управление памятью и RAII
**Вопрос:** Что произойдет при выполнении следующего кода?
```cpp
#include <memory>
#include <iostream>
class Test {
public:
    Test() { std::cout << "Constructed"; }
    ~Test() { std::cout << "Destroyed"; }
};
int main() {
    std::unique_ptr<Test> ptr = std::make_unique<Test>();
    return 0;
}
```
**Варианты ответа:**  
a) `Constructed`  
b) `ConstructedDestroyed`  
c) `Destroyed`  
d) Ошибка компиляции

---

### 8. Работа с `std::move`
**Вопрос:** Какой будет результат выполнения следующего кода?
```cpp
#include <iostream>
#include <utility>
class Test {
public:
    Test() { std::cout << "Default"; }
    Test(const Test&) { std::cout << "Copy"; }
    Test(Test&&) { std::cout << "Move"; }
};
int main() {
    Test a;
    Test b = std::move(a);
}
```
**Варианты ответа:**  
a) `DefaultCopy`  
b) `DefaultMove`  
c) `Move`  
d) Ошибка компиляции