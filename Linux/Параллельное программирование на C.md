## 1. Введение в параллельное программирование

**Параллельное программирование** — это метод написания программ, где задачи выполняются одновременно с использованием нескольких вычислительных ресурсов. Цели:

- Увеличение скорости выполнения программ.
- Повышение эффективности использования аппаратных ресурсов (многоядерных процессоров).
- Возможность обработки больших объемов данных.

В целом типа используешь потоки чтобы делать вещи параллельно, используешь атомарные операции и примитивы синхронизации, чтобы не было рейс кондишнов.

lessgo


## 2. Потоки в C (POSIX Threads)

***2.1. Что такое поток?***
Поток — это легковесная единица выполнения, которая делит адресное пространство с основным процессом. Потоки позволяют нескольким задачам выполняться одновременно в рамках одного процесса.

***2.2. POSIX Threads (pthreads)***
 — это стандартный интерфейс для работы с потоками в Unix-подобных системах. Основные функции:
- `pthread_create()` — создание потока.
- `pthread_join()` — ожидание завершения потока.
- `pthread_mutex_lock()` и `pthread_mutex_unlock()` — управление доступом к общим ресурсам. (*o них ниже*)

#### **Функция `pthread_create`**
```c
int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void *(*start_routine)(void *), void *arg);

```
**Аргументы:**
1. **`pthread_t *thread`**  
    Указатель на переменную типа `pthread_t`, куда будет записан идентификатор создаваемого потока. Этот идентификатор используется для управления потоком, например, в `pthread_join`.
    
2. **`const pthread_attr_t *attr`**  
    Указатель на структуру атрибутов потока. Если указать `NULL`, используются атрибуты по умолчанию.  
    _Пример атрибутов:_ размер стека, уровень приоритета, детаченный режим выполнения и т.д.
    
3. **`void *(*start_routine)(void *)`**  
    Указатель на функцию, которую будет выполнять поток. Функция должна иметь сигнатуру `void *function(void *)` и возвращать указатель `void *`.
    
4. **`void *arg`**  
    Аргумент, передаваемый в функцию, выполняемую потоком (`start_routine`). Это может быть указатель на данные, которые поток должен обработать.
    

#### **Функция `pthread_join`**
```c
int pthread_join(pthread_t thread, void **retval);
```
**Аргументы:**
1. **`pthread_t thread`**  
    Идентификатор потока, который нужно завершить. Он передаётся в функцию `pthread_join` и должен быть результатом `pthread_create`.
    
2. `void **retval`  
    Указатель на указатель, куда будет записано возвращаемое значение из функции `start_routine`. Если результат не нужен, можно передать `NULL`.
    
***2.3. Пример создания потока***
```c
#include <stdio.h>
#include <pthread.h>

void* print_numbers(void* arg) {
    for (int i = 0; i < 10; i++) {
        printf("Thread: %d\n", i);
    }
    return NULL;
}

int main() {
    pthread_t thread;

    // Создание потока
    pthread_create(&thread, NULL, print_numbers, NULL);

    // Ожидание завершения потока
    pthread_join(thread, NULL);

    printf("Main thread completed.\n");
    return 0;
}
```



## 3. Примитивы синхронизации в C

Примитивы синхронизации используются для предотвращения проблем, связанных с состояниями гонки и некорректным доступом к общим ресурсам.

### Задача производитель-потребитель: способы решения

Задача **производитель-потребитель** — это классическая проблема синхронизации процессов, возникающая при взаимодействии двух или более потоков/процессов.

- **Производитель** генерирует данные и помещает их в общий буфер.
- **Потребитель** извлекает данные из буфера для обработки.

Основная проблема — обеспечение корректного доступа к общему буферу, чтобы избежать:

- **Переполнения** (производитель не должен добавлять данные в полный буфер).
- **Пустого чтения** (потребитель не должен извлекать данные из пустого буфера).


##### Способы решения

Для решения задачи используются механизмы синхронизации:

1. **Мьютексы (Mutex)**

- Мьютекс обеспечивает взаимное исключение, гарантируя, что только один поток имеет доступ к буферу в любой момент времени.
- Проблема переполнения/опустошения буфера решается путем проверки состояния буфера.

2. **Семафоры**
- Используются два семафора:
    - **Заполняющий семафор (full)**: указывает количество заполненных слотов в буфере.
    - **Освобождающий семафор (empty)**: указывает количество свободных слотов.
- Семафор обеспечивает синхронизацию между производителем и потребителем.

3. **Условные переменные**
- Используются вместе с мьютексами для более тонкой синхронизации.
- Производитель ждет, пока в буфере появится свободное место.
- Потребитель ждет, пока в буфере появятся данные.

Давайте разберём каждый из них.

### 3.1. Мьютекс (Mutex)

**Мьютекс** (“mutual exclusion”) — это объект, позволяющий ограничить доступ к общему ресурсу. Только один поток может владеть мьютексом в любой момент времени.

Основные функции:
####  `pthread_mutex_init()` — инициализация мьютекса.
  ```c
int pthread_mutex_init(pthread_mutex_t *mutex, const pthread_mutexattr_t *attr);
  ```
  **Аргументы:**
1. **`pthread_mutex_t *mutex`**  
    Указатель на переменную типа `pthread_mutex_t`, которая будет представлять мьютекс. Эта переменная должна быть доступна всем потокам, которые будут использовать этот мьютекс.
    
2. **`const pthread_mutexattr_t *attr`**  
    Указатель на структуру атрибутов мьютекса. Если указать `NULL`, используются атрибуты по умолчанию.  
    _Пример атрибутов:_ тип мьютекса (`PTHREAD_MUTEX_NORMAL`, `PTHREAD_MUTEX_RECURSIVE`, и т.д.).

#### `pthread_mutex_lock()` — захват мьютекса.
```c
int pthread_mutex_lock(pthread_mutex_t *mutex);
```
**Аргументы:**
1. **`pthread_mutex_t *mutex`** 
    Указатель на уже инициализированный мьютекс. Поток, вызвавший эту функцию, захватит мьютекс и получит исключительный доступ к защищённому ресурсу.

**Особенности:**
- Если мьютекс уже захвачен другим потоком, текущий поток блокируется до освобождения мьютекса.
- Для рекурсивных мьютексов тот же поток может вызвать `pthread_mutex_lock` несколько раз.

####  `pthread_mutex_unlock()` — освобождение мьютекса.
```c
int pthread_mutex_unlock(pthread_mutex_t *mutex);
```
**Аргументы:**
1. **`pthread_mutex_t *mutex`**  
    Указатель на захваченный мьютекс, который должен быть освобождён.  
    Поток, вызвавший `pthread_mutex_unlock`, снимает блокировку, позволяя другим потокам получить доступ к мьютексу.

**Особенности:**
- Освобождать мьютекс может только поток, который его захватил.
- Если другие потоки ожидают мьютекс, один из них будет разблокирован.

#### `pthread_mutex_destroy()` — уничтожение мьютекса.
```c
int pthread_mutex_destroy(pthread_mutex_t *mutex);
```
**Аргументы:**

1. **`pthread_mutex_t *mutex`**  
    Указатель на мьютекс, который нужно уничтожить. После вызова этой функции мьютекс больше нельзя использовать.

**Особенности:**

- Мьютекс должен быть разрушен только после того, как больше не используется в программе.
- Если мьютекс всё ещё захвачен, вызов `pthread_mutex_destroy` приведёт к неопределённому поведению.

Пример:
```c
#include <stdio.h>
#include <pthread.h>

pthread_mutex_t mutex;
int counter = 0;

void* increment_counter(void* arg) {
    for (int i = 0; i < 1000; i++) {
        pthread_mutex_lock(&mutex);
        counter++;
        pthread_mutex_unlock(&mutex);
    }
    return NULL;
}

int main() {
    pthread_t thread1, thread2;

    pthread_mutex_init(&mutex, NULL);

    pthread_create(&thread1, NULL, increment_counter, NULL);
    pthread_create(&thread2, NULL, increment_counter, NULL);

    pthread_join(thread1, NULL);
    pthread_join(thread2, NULL);

    pthread_mutex_destroy(&mutex);

    printf("Final counter value: %d\n", counter);
    return 0;
}
```



### 3.2. Спинлок (Spinlock)

**Спинлок** — это более простой примитив синхронизации, где поток активно ожидает (крутится в цикле), пока ресурс не станет доступен. Используется для задач, где время ожидания мало, и переключение контекста неэффективно.

Основные функции:

#### `pthread_spin_init()` — инициализация спинлока.
```c
int pthread_spin_init(pthread_spinlock_t *lock, int pshared);
```
**Аргументы:**
1. **`pthread_spinlock_t *lock`**  
    Указатель на переменную типа `pthread_spinlock_t`, которая будет представлять спинлок. Эта переменная должна быть доступна всем потокам, которые будут использовать данный спинлок.
    
2. **`int pshared`**  
    Указывает, может ли спинлок быть разделяемым между процессами:
    
    - `PTHREAD_PROCESS_PRIVATE` (0): Спинлок используется только в пределах одного процесса.
    - `PTHREAD_PROCESS_SHARED` (1): Спинлок может быть разделён между процессами. (Требуется поддержка операционной системы и системы разделяемой памяти.)
    
#### `pthread_spin_lock()` — захват спинлока.
```c
int pthread_spin_lock(pthread_spinlock_t *lock);
```
**Аргументы:**
1. **`pthread_spinlock_t *lock`**  
    Указатель на уже инициализированный спинлок. Поток, вызвавший эту функцию, попытается захватить спинлок.

**Особенности:**
- Если спинлок уже захвачен другим потоком, текущий поток будет активно ожидать (крутиться в цикле), пока спинлок не станет доступным.

#### `pthread_spin_unlock()` — освобождение спинлока.
```c
int pthread_spin_unlock(pthread_spinlock_t *lock);
```
**Аргументы:**
1. **`pthread_spinlock_t *lock`**  
    Указатель на захваченный спинлок, который должен быть освобождён.

**Особенности:**
- Спинлок должен быть освобождён тем же потоком, который его захватил.

#### `pthread_spin_destroy()` — уничтожение спинлока.
**Аргументы:**
1. **`pthread_spinlock_t *lock`**  
    Указатель на спинлок, который нужно уничтожить.

**Особенности:**
- Спинлок должен быть разрушен только после того, как больше не используется.
- Если спинлок всё ещё захвачен, вызов `pthread_spin_destroy` приведёт к неопределённому поведению.

Пример:
```c
#include <stdio.h>
#include <pthread.h>

pthread_spinlock_t spinlock;
int counter = 0;

void* increment_counter(void* arg) {
    for (int i = 0; i < 1000; i++) {
        pthread_spin_lock(&spinlock);
        counter++;
        pthread_spin_unlock(&spinlock);
    }
    return NULL;
}

int main() {
    pthread_t thread1, thread2;

    pthread_spin_init(&spinlock, PTHREAD_PROCESS_PRIVATE);

    pthread_create(&thread1, NULL, increment_counter, NULL);
    pthread_create(&thread2, NULL, increment_counter, NULL);

    pthread_join(thread1, NULL);
    pthread_join(thread2, NULL);

    pthread_spin_destroy(&spinlock);

    printf("Final counter value: %d\n", counter);
    return 0;
}
```



### 3.3. Условные переменные (Condition Variables)

**Условные переменные** используются для координации выполнения потоков, которые должны ожидать определённого состояния.

Основные функции:

#### `pthread_cond_init()` — инициализация условной переменной.
```c
int pthread_cond_init(pthread_cond_t *cond, const pthread_condattr_t *attr);
```
**Аргументы:**
1. **`pthread_cond_t *cond`**  
    Указатель на переменную типа `pthread_cond_t`, которая будет представлять условную переменную. Эта переменная должна быть доступна всем потокам, которые будут использовать её для ожидания или сигналов.
2. **`const pthread_condattr_t *attr`**  
    Указатель на атрибуты условной переменной. Если указать `NULL`, используются атрибуты по умолчанию.  
    Атрибуты можно настроить с помощью функций `pthread_condattr_*`.
    
#### `pthread_cond_wait()` — ожидание сигнала.
```c
int pthread_cond_wait(pthread_cond_t *cond, pthread_mutex_t *mutex);
```
**Аргументы:**

1. **`pthread_cond_t *cond`**  
    Указатель на условную переменную, на которую будет ждать текущий поток.
    
2. **`pthread_mutex_t *mutex`**  
    Указатель на уже захваченный мьютекс, который освобождается на время ожидания. После пробуждения поток снова захватывает этот мьютекс.
    

**Особенности:**

- Поток будет блокирован до тех пор, пока другой поток не вызовет `pthread_cond_signal` или `pthread_cond_broadcast` для этой условной переменной.
- Условная переменная всегда используется совместно с мьютексом.

#### `pthread_cond_signal()` — пробуждение одного потока.
```c
int pthread_cond_signal(pthread_cond_t *cond);
```
**Аргументы:**
1. **`pthread_cond_t *cond`**  
    Указатель на условную переменную, для которой нужно разбудить один поток, ожидающий на этой переменной.

**Особенности:**
- Если ни один поток не ожидает на `cond`, сигнал будет проигнорирован.

#### `pthread_cond_broadcast()` — пробуждение всех потоков.
```c
int pthread_cond_broadcast(pthread_cond_t *cond);
```
**Аргументы:**

1. **`pthread_cond_t *cond`**  
    Указатель на условную переменную, для которой нужно разбудить все потоки, ожидающие на этой переменной.

**Особенности:**

- Все ожидающие потоки будут пробуждены. Они поочерёдно захватят связанный мьютекс.

#### `pthread_cond_destroy()` — уничтожение условной переменной.
```c
int pthread_cond_destroy(pthread_cond_t *cond);
```
**Аргументы:**

1. **`pthread_cond_t *cond`**  
    Указатель на условную переменную, которую нужно уничтожить.

**Особенности:**

- Условная переменная должна быть разрушена только после того, как больше не используется.
- Если есть потоки, ожидающие на `cond`, вызов этой функции приведёт к неопределённому поведению.

Пример:
```c
#include <stdio.h>
#include <pthread.h>

pthread_mutex_t mutex;
pthread_cond_t cond;
int ready = 0;

void* worker(void* arg) {
    pthread_mutex_lock(&mutex);
    while (!ready) {
        pthread_cond_wait(&cond, &mutex);
    }
    printf("Thread %lu is working\n", pthread_self());
    pthread_mutex_unlock(&mutex);
    return NULL;
}

int main() {
    pthread_t thread;

    pthread_mutex_init(&mutex, NULL);
    pthread_cond_init(&cond, NULL);

    pthread_create(&thread, NULL, worker, NULL);

    sleep(1); // Имитация работы

    pthread_mutex_lock(&mutex);
    ready = 1;
    pthread_cond_signal(&cond);
    pthread_mutex_unlock(&mutex);

    pthread_join(thread, NULL);

    pthread_mutex_destroy(&mutex);
    pthread_cond_destroy(&cond);

    return 0;
}
```



### 3.4. Семафор (Semaphore)

**Семафор** — это счетчик, который управляет доступом к ресурсам. В отличие от мьютексов, семафоры могут позволять доступ нескольким потокам одновременно.

#### Основные функции:

#### `sem_init()` — инициализация семафора.
```c
int sem_init(sem_t *sem, int pshared, unsigned int value);
```
**Аргументы:**

1. **`sem_t *sem`**  
    Указатель на переменную типа `sem_t`, которая представляет семафор. Эта переменная должна быть доступна всем потокам, использующим данный семафор.
    
2. **`int pshared`**  
    Указывает, будет ли семафор использоваться между потоками одного процесса или разделяться между процессами:
    
    - `0` (или `PTHREAD_PROCESS_PRIVATE`): Семафор используется только потоками внутри одного процесса.
    - `1` (или `PTHREAD_PROCESS_SHARED`): Семафор может быть разделён между процессами. (Требуется поддержка ОС.)
3. **`unsigned int value`**  
    Начальное значение семафора, определяющее количество доступных ресурсов.
    
#### `sem_wait()` — захват семафора (уменьшение значения).
```c
int sem_wait(sem_t *sem);
```
**Аргументы:**

1. **`sem_t *sem`**  
    Указатель на уже инициализированный семафор.

**Особенности:**

- Если значение семафора больше 0, оно уменьшается на 1, и поток продолжает выполнение.
- Если значение семафора равно 0, поток блокируется до тех пор, пока другой поток или процесс не вызовет `sem_post`.

#### `sem_post()` — освобождение семафора (увеличение значения).
```c
int sem_post(sem_t *sem);
```
**Аргументы:**

1. **`sem_t *sem`**  
    Указатель на семафор, значение которого нужно увеличить.

**Особенности:**

- Увеличивает значение семафора на 1.
- Если есть потоки, заблокированные в `sem_wait`, один из них будет разблокирован.

#### `sem_destroy()` — уничтожение семафора.
```c
int sem_destroy(sem_t *sem);
```

Пример:
```c
#include <stdio.h>
#include <pthread.h>
#include <semaphore.h>

sem_t sem;

void* worker(void* arg) {
    sem_wait(&sem);
    printf("Thread %lu is working\n", pthread_self());
    sleep(1);
    sem_post(&sem);
    return NULL;
}

int main() {
    pthread_t threads[5];

    sem_init(&sem, 0, 2); // Одновременно работают 2 потока

    for (int i = 0; i < 5; i++) {
        pthread_create(&threads[i], NULL, worker, NULL);
    }

    for (int i = 0; i < 5; i++) {
        pthread_join(threads[i], NULL);
    }

    sem_destroy(&sem);

    return 0;
}
```

## 4. Атомарные операции

### 4.1. Что такое атомарные операции?

**Атомарные операции** — это операции, которые выполняются как единое неделимое действие. Они либо выполняются полностью, либо не выполняются вообще, предотвращая состояния гонки и обеспечивая корректность работы многопоточных программ.

Примеры атомарных операций:

- Инкремент и декремент переменной.
- Сравнение и замена (Compare-And-Swap, CAS).
- Установка значений.
    

### 4.2. Атомарные операции в C (через GCC Built-ins)

Компилятор GCC предоставляет встроенные функции (`__sync` и `__atomic`), которые позволяют работать с атомарными операциями. Эти функции генерируют инструкции, поддерживающие атомарность на уровне процессора.

Пример атомарного инкремента
```c
#include <stdio.h>
#include <pthread.h>

int counter = 0;

void* increment(void* arg) {
    for (int i = 0; i < 1000; i++) {
        __sync_fetch_and_add(&counter, 1);  // Атомарный инкремент
    }
    return NULL;
}

int main() {
    pthread_t thread1, thread2;

    pthread_create(&thread1, NULL, increment, NULL);
    pthread_create(&thread2, NULL, increment, NULL);

    pthread_join(thread1, NULL);
    pthread_join(thread2, NULL);

    printf("Final counter value: %d\n", counter);
    return 0;
}
```

##### 4.3. Основные операции

1. **Инкремент и декремент**:
    
    - `__sync_fetch_and_add(&var, value)` — добавляет `value` к `var` и возвращает старое значение.
        
    - `__sync_fetch_and_sub(&var, value)` — вычитает `value` из `var`.
        
2. **Compare-And-Swap (CAS)**:
    
    - `__sync_bool_compare_and_swap(&var, old_val, new_val)` — если `var == old_val`, то устанавливает `var = new_val` и возвращает `true`. В противном случае возвращает `false`.
        
3. **Установка значения**:
    
    - `__sync_lock_test_and_set(&var, value)` — устанавливает `var = value` и возвращает старое значение.
        
    - `__sync_lock_release(&var)` — сбрасывает значение переменной (обычно используется для реализации примитивов блокировки).
        

Пример CAS (Compare-And-Swap)
```
#include <stdio.h>
#include <pthread.h>
#include <stdbool.h>

int shared_value = 0;

void* compare_and_swap_example(void* arg) {
    int expected = 0;
    int new_value = 42;

    if (__sync_bool_compare_and_swap(&shared_value, expected, new_value)) {
        printf("Thread %lu updated value to %d\n", pthread_self(), new_value);
    } else {
        printf("Thread %lu failed to update value\n", pthread_self());
    }

    return NULL;
}

int main() {
    pthread_t thread1, thread2;

    pthread_create(&thread1, NULL, compare_and_swap_example, NULL);
    pthread_create(&thread2, NULL, compare_and_swap_example, NULL);

    pthread_join(thread1, NULL);
    pthread_join(thread2, NULL);

    printf("Final value: %d\n", shared_value);
    return 0;
}
```

##### 4.4. Преимущества использования атомарных операций

- **Производительность**: Избегают необходимости блокировок (мьютексов).
    
- **Простота**: Снижают сложность реализации некоторых алгоритмов синхронизации.
    
- **Эффективность**: Поддерживаются на уровне аппаратуры, что делает их очень быстрыми.
    

---
## 5. Пул потоков (Thread Pool)

**Пул потоков** — это шаблон проектирования в программировании, используемый для достижения конкурентного выполнения задач в программе. Этот подход, также называемый моделью "работающих потоков" или "бригады рабочих", представляет собой механизм, который поддерживает пул потоков, ожидающих назначения задач для их выполнения.
![[Drawing 2025-01-21 15.29.28.excalidraw]]

Использование пула потоков позволяет повысить производительность и избежать задержек, возникающих из-за частого создания и уничтожения потоков для выполнения короткоживущих задач. Пул потоков адаптируется под доступные вычислительные ресурсы, что делает его оптимальным решением для параллельного выполнения задач.


***Производительность***

Размер пула потоков (количество потоков, поддерживаемых в резерве для выполнения задач) является настраиваемым параметром приложения и играет важную роль в оптимизации производительности.

##### Преимущества использования пула потоков:

1. **Снижение накладных расходов:**  
    Создание и уничтожение потоков требует значительных затрат времени и ресурсов. Пул потоков ограничивает эти расходы, создавая потоки только один раз, при инициализации пула.
    
2. **Повышение стабильности:**  
    Избыточное создание потоков может привести к нехватке памяти и снижению стабильности системы. Использование пула потоков позволяет избежать этого, поддерживая фиксированное или адаптивное количество потоков.
    
3. **Управляемое использование ресурсов:**  
    Пул потоков помогает оптимально распределить вычислительные ресурсы, предотвращая создание слишком большого количества потоков, которые могут привести к избыточной смене контекста и снижению производительности.
    

##### Потенциальные проблемы:

- **Слишком большой пул:**  
    Избыточное количество потоков может привести к излишнему потреблению памяти и дополнительным затратам на переключение контекста между потоками.
    
- **Слишком маленький пул:**  
    Недостаточное количество потоков в пуле может замедлить обработку задач, так как некоторые задачи будут вынуждены ждать завершения текущих потоков.
    

***Адаптация пула потоков***
Количество потоков в пуле может динамически изменяться в процессе работы программы в зависимости от числа ожидающих задач. Например, веб-сервер может увеличить количество потоков при возрастании числа запросов и уменьшить их при снижении нагрузки.

##### Алгоритмы управления:

1. **Избыточное создание потоков:**  
    Создание слишком большого количества потоков тратит ресурсы и приводит к избыточным накладным расходам.
    
2. **Избыточное уничтожение потоков:**  
    Уничтожение потоков в большом количестве приводит к необходимости их последующего создания, что замедляет работу системы.
    
3. **Медленное создание потоков:**  
    Если потоки создаются слишком медленно, это может вызывать длительное ожидание задач и ухудшение производительности.
    
4. **Медленное уничтожение потоков:**  
    Если потоки уничтожаются слишком медленно, это может привести к избыточному использованию ресурсов и негативно сказаться на других процессах.
    


### Дополнительные преимущества пула потоков

1. **Упрощённое управление задачами:**  
    Пулы потоков предоставляют удобные интерфейсы для постановки задач в очередь, управления конкурентным доступом и синхронизации.
    
2. **Использование на серверных фермах:**  
    Хотя обычно пул потоков применяется в пределах одного компьютера, концептуально его можно сравнить с серверными фермами, где главный процесс (или поток) распределяет задачи между рабочими процессами на разных компьютерах.
    

### Пример имплементации тредпула (полный треш)
parallel-scehduler.h:
```cpp
#include <cstddef>
#include <queue>
#include <vector>

#include <pthread.h>

typedef void (*task_fn_t)(void*);

struct QueueTask {
    task_fn_t func;
    void* args;
};

class ThreadPool {
  public:
    ThreadPool(size_t);
    ~ThreadPool();

    ThreadPool(const ThreadPool&) = delete;  // just create a new thread pool
    ThreadPool(ThreadPool&&) = delete;  // threads store pointers to the pool

    void add_task(task_fn_t task, void* args);
    void wait_all();

  private:
    std::queue<QueueTask> tasks;
    pthread_mutex_t mutex;
    pthread_cond_t
        wake_workers;          // main thread notifies workers to work or stop
    pthread_cond_t wake_main;  // workers notify main that they are finished
                               // working or stopped
    size_t threads_alive;
    size_t threads_working;
    bool stop;

    static void* worker_thread(void*);
};
```

parallel-scheduler.cpp
```cpp
#include <pthread.h>
#include <cstddef>

#include "parallel-scehduler.h"

void* ThreadPool::worker_thread(void* untyped_args) {
    ThreadPool* pool = (ThreadPool*)untyped_args;

    pthread_mutex_lock(&pool->mutex);

    while (true) {
        // wait for main to tell us to either do a task or stop
        while (pool->tasks.empty() && !pool->stop) {
            pthread_cond_wait(&pool->wake_workers, &pool->mutex);
        }

        if (pool->tasks.empty() && pool->stop) {
            break;
        }

        QueueTask task = pool->tasks.front();
        pool->tasks.pop();

        pool->threads_working += 1;
        pthread_mutex_unlock(&pool->mutex);
        task.func(task.args);
        pthread_mutex_lock(&pool->mutex);
        pool->threads_working -= 1;

        // tell main we are done with the task
        pthread_cond_signal(&pool->wake_main);
    }

    pool->threads_alive -= 1;
    pthread_cond_signal(&pool->wake_main);
    pthread_mutex_unlock(&pool->mutex);

    return nullptr;
}

ThreadPool::ThreadPool(size_t thread_count)
    : threads_alive(thread_count), threads_working(0), stop(false) {
    pthread_mutex_init(&mutex, nullptr);
    pthread_cond_init(&wake_workers, nullptr);
    pthread_cond_init(&wake_main, nullptr);

    for (size_t i = 0; i < thread_count; ++i) {
        pthread_t thread;
        pthread_create(&thread, nullptr, ThreadPool::worker_thread, this);
        pthread_detach(thread);  // threads will never be joined
    }
}

ThreadPool::~ThreadPool() {
    pthread_mutex_lock(&mutex);

    // wait for all tasks to complete
    while (!tasks.empty() || threads_working != 0) {
        pthread_cond_wait(&wake_main, &mutex);
    }

    stop = true;

    // wake all threads so they know they need to stop
    pthread_cond_broadcast(&wake_workers);

    // wait for all threads to stop
    while (threads_alive != 0) {
        pthread_cond_wait(&wake_main, &mutex);
    }

    pthread_cond_destroy(&wake_workers);
    pthread_cond_destroy(&wake_main);

    pthread_mutex_destroy(&mutex);
}

void ThreadPool::add_task(task_fn_t task, void* args) {
    pthread_mutex_lock(&mutex);
    tasks.push(QueueTask(task, args));
    pthread_cond_signal(&wake_workers);
    pthread_mutex_unlock(&mutex);
}

void ThreadPool::wait_all() {
    pthread_mutex_lock(&mutex);
    while (!tasks.empty() || threads_working != 0) {
        pthread_cond_wait(&wake_main, &mutex);
    }
    pthread_mutex_unlock(&mutex);
}
```

demo-application.cpp
```cpp
#include <stdatomic.h>
#include <iostream>

#include "parallel-scehduler.h"

int main() {
    atomic_int local = 0;

    ThreadPool pool(16);

    for (int i = 0; i < 10000; ++i) {
        pool.add_task(
            [](void* void_args) {
                atomic_int* data = (atomic_int*)void_args;
                *data += 1;
            },
            &local);
    }

    pool.wait_all();

    std::cout << "Computed value: " << local << "\n";
}
```

### Итоги

Пул потоков — это мощный инструмент для управления многопоточностью. Он позволяет эффективно распределять ресурсы, снижать накладные расходы на создание потоков и обеспечивать стабильность работы системы. Правильная настройка и управление пулом потоков критически важны для достижения оптимальной производительности программ.


## 6. Заключение

Примитивы синхронизации, такие как мьютексы, спинлоки, условные переменные и семафоры, являются основными инструментами для управления доступом к ресурсам в параллельных программах на C. Их правильное использование позволяет избежать состояния гонки и обеспечить корректное выполнение программ.