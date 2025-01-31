
# Дополнительные вопросы для экзамена по ООП (Часть 3)

## 1. Вопросы по r-value ссылкам и перемещениям

**Вопрос 1:** Какой из приведенных вариантов объявления r-value ссылки является корректным?
a) `const int&& a = 10;`  
b) `int&& b = std::move(a);`  
c) `int& c = std::move(a);`  
d) `auto&& d = getTemporary();`  

**Вопрос 2:** Рассмотрите следующий код. Что будет выведено?

```cpp
#include <iostream>

void print(int&& a) {
    std::cout << "r-value";
}

void print(const int& a) {
    std::cout << "l-value";
}

int main() {
    int x = 10;
    print(std::move(x));
}
```
a) r-value  
b) l-value  
c) Ошибка компиляции  
d) Неопределенное поведение  

---

## 2. Вопросы по конструкторам

**Вопрос 3:** Какой конструктор вызывается при создании объекта `std::string s1 = "text";`?  
a) Конструктор копирования  
b) Конструктор перемещения  
c) Конструктор инициализации от C-style строки  
d) Конструктор по умолчанию  

**Вопрос 4:** Рассмотрите следующий код. Какой конструктор вызван?

```cpp
#include <iostream>
#include <string>

struct A {
    A(int a) { std::cout << "int constructor"; }
    A(std::string b) { std::cout << "string constructor"; }
};

int main() {
    A obj("example");
}
```
a) Конструктор с параметром `int`  
b) Конструктор с параметром `std::string`  
c) Ошибка компиляции  
d) Неопределенное поведение  

---

## 3. Полиморфизм и переопределение

**Вопрос 5:** Какой из следующих вариантов переопределяет виртуальную функцию базового класса?  
a) `virtual void foo();`  
b) `void foo() override;`  
c) `void foo(int a) override;`  
d) `virtual void foo(int a) const;`  

**Вопрос 6:** Рассмотрите следующий код. Что будет выведено?

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
}
```
a) Base  
b) Derived  
c) Ошибка компиляции  
d) Неопределенное поведение  

---

## 4. Исключения и обработка ошибок

**Вопрос 7:** Рассмотрите следующий код. Какое исключение будет выброшено?

```cpp
#include <stdexcept>

void divide(int a, int b) {
    if (b == 0) throw std::runtime_error("Division by zero");
}

int main() {
    divide(10, 0);
}
```
a) std::exception  
b) std::runtime_error  
c) Ошибка компиляции  
d) Никакое  

**Вопрос 8:** Как правильно захватить любое исключение?  
a) `catch (std::exception& e)`  
b) `catch (...)`  
c) `catch (std::runtime_error& e)`  
d) `catch (const char* e)`  

---

## 5. Шаблонные функции

**Вопрос 9:** Напишите шаблонную функцию, которая возвращает минимальное из двух значений.  

**Вопрос 10:** Рассмотрите следующий код. Что будет выведено?

```cpp
#include <iostream>

template <typename T>
void print(T value) {
    std::cout << value;
}

int main() {
    print<int>(10);
    print("Hello");
}
```
a) Ошибка компиляции  
b) 10Hello  
c) 10  
d) Hello  