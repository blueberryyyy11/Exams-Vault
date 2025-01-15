# Навигация

- [1. Функциональные последовательности и ряды](#1)
- [2. Необходимое и достаточное условие равномерной сходимости функциональных последовательностей и рядов](#2)
- [3. Непрерывность суммы функционального ряда и функциональных последовательностей](#3)
- [4. Почленный переход к пределу в функциональных рядах](#4)
- [5. Почленное интегрирование функциональных рядов](#5)
- [6. Почленное дифференцирование функциональных рядов](#6)
- [7. Интеграл зависящий от параметра. Необходимые и достаточные условия равномерной сходимости функций. Непрерывность предельной функции](#7)
- [8. Предельный переход по параметру под знаком интеграла. Непрерывность интеграла по параметру.](#8)
- [9. Дифференцирование интегралов по параметру](#9)
- [10. Интегрирование интегралов от параметра](#10)

# 1.

# Функциональные последовательности и ряды

Последовательность, членами которой являются функции, определенные на одной и той же области $X$ ($X$ - область определения функций), называется _функциональной последовательностью_:

$$
\{ f_n(x) \} = f_1(x), f_2(x), \dots, f_n(x), \dots \tag{1}
$$

Если зафиксируем $x \in X$, то получим числовую последовательность.

Допустим, что при $\forall x \in X$ последовательность $(1)$ имеет конечный предел. При других $x$ предел будет другой. Заметим, что предел зависит от $x$ и обозначим этот предел через $f(x)$:

$$
f(x) = \lim_{n \to \infty} f_n(x).
$$

$f(x)$ - предельная функция, которая зависит от $x$. Это называется _поточечная ("по точкам") сходимость_.

При фиксированном $x$:

$$
\forall \varepsilon > 0 \,\, \exists N(\varepsilon), \, n > N: |f_n(x) - f(x)| < \varepsilon.
$$

Заметим, что при изменении $x$ меняется $N$. Можно ли выбрать $N$ таким образом, чтобы оно зависело только от $\varepsilon$, но не зависело от выбора $x$?

##### Определение равномерной сходимости

Если:

1. Для функциональной последовательности $(1)$ существует предельная функция $\exists f(x) = \lim_{n \to \infty} f_n(x)$,
2. $\forall \varepsilon > 0 \,\, \exists N(\varepsilon) > 0; \,\, n > N$ имеет место неравенство $|f_n(x) - f(x)| < \varepsilon \,\, \forall x \in X$,

то мы скажем, что последовательность _равномерно сходится_ к $f(x)$ на множестве $X$ ($f_n(x) \rightrightarrows f(x)$).

---

[Примеры]

**Равномерная сходимость**:

$$
f_n(x) = \frac{x}{1 + n^2x^2}, \quad 0 \leq x \leq 1
$$

$$
f(x) = \lim_{n \to \infty} f_n(x) = \lim_{n \to \infty} \frac{x}{1 + n^2x^2} = \lim_{n \to \infty} \frac{\frac{x}{n^2}}{\frac{1}{n^2} + x^2} = 0.
$$

$$
|f_n(x) - f(x)| = \frac{x}{1 + n^2x^2} = \frac{1}{2n} \cdot \frac{2nx}{1 + n^2x^2} \leq \frac{1}{2n}.
$$

$$
n > \frac{1}{2\varepsilon}, \quad N(\varepsilon) = \left\lfloor \frac{1}{2\varepsilon} \right\rfloor.
$$

Данная последовательность равномерно сходится к предельной функции $f(x)$.

**Поточечная сходимость:**

$$
f_n(x) = \frac{nx}{1 + n^2x^2}, \quad 0 < x \leq 1.
$$

$$
f(x) = \lim_{n \to \infty} f_n(x) = \lim_{n \to \infty} \frac{nx}{1 + n^2x^2} = 0.
$$

$$
|f_n(x) - f(x)| = \frac{nx}{1 + n^2x^2} < \frac{1}{nx}.
$$

В данном случае равномерная сходимость отсутствует.

### Функциональные ряды

Рассмотрим ряд, члены которого функции, определенные на множестве $X$:

$$
\sum_{k=1}^\infty u_k(x). \tag{3}
$$

$$
f_n(x) = u_1(x) + u_2(x) + \dots + u_n(x).
$$

Если $x$ фиксировано, то получается числовой ряд, а $f_n(x)$ - _частичная сумма._

Пусть ряд $(3)$ сходится при $\forall x \in X$:

$$
\lim_{n \to \infty} f_n(x) = f(x) \quad \text{(поточечная сходимость)}.
$$

$$
\begin{cases}
\forall \varepsilon > 0 \, \exists N(\varepsilon), \, n > N: |f_n(x) - f(x)| < \varepsilon, \\
\varphi_n(x) = \sum_{k=n+1}^\infty u_k(x) \quad \text{(остаточный ряд)}, \\
|\varphi_n(x)| < \varepsilon.
\end{cases}
$$

Ряд $(3)$ _равномерно сходится_ на множестве $X$, если:

1. Последовательность частичных сумм имеет предельную функцию $f(x)$ на $X$: $\exists f(x) = \lim_{n \to \infty} f_n(x)$,
2. $\forall \varepsilon > 0 \quad \exists N(\varepsilon) > 0; \quad n > N: |f_n(x) - f(x)| < \varepsilon \iff |\varphi_n(x)| < \varepsilon \quad \forall x \in X$.

# 2.

# Необходимое и достаточное условие равномерной сходимости функциональных последовательностей и рядов

##### Теорема (Необходимое и достаточное условие равномерной сходимости последовательностей)

Для того, чтобы функциональная последовательность $(1)$ имела предельную функцию и сходилась к этой функции равномерно относительно $x \in X$, необходимо и достаточно, чтобы:

$$
\forall \varepsilon > 0 \; \exists N(\varepsilon), \; n > N \; m = 1, 2, \ldots,
$$

$$
|f_{n+m}(x) - f_n(x)| < \varepsilon \quad \text{для } \forall x \in X. \tag{4}
$$

[Доказательство]

1. **Необходимость**

Пусть $f_n(x) \rightrightarrows f(x)$ для $\forall x \in X$. Тогда:

$$
\forall \varepsilon > 0 \; \exists N(\varepsilon), \; n > N:
$$

$$
|f_n(x) - f(x)| < \frac{\varepsilon}{2}. \tag{5}
$$

И если $n + m > n > N$, то:

$$
|f_{n+m}(x) - f(x)| < \frac{\varepsilon}{2}. \tag{6}
$$

Следовательно:

$$
|f_{n+m}(x) - f_n(x)| = |f_{n+m}(x) - f(x) + f(x) - f_n(x)| \leq |f_{n+m}(x) - f(x)| + |f(x) - f_n(x)| = \frac{\varepsilon}{2} + \frac{\varepsilon}{2} = \varepsilon.
$$

2. **Достаточность**

$$
\forall \varepsilon > 0 \; \exists N(\varepsilon), \; n > N \; m = 1, 2, \ldots:
$$

$$
|f_{n+m}(x) - f_n(x)| < \varepsilon.
$$

_Зафиксируем_ $\forall x \in X$. Так как при фиксированном $x$ выполняется условие Больцано-Коши, то последовательность сходится.
_При изменении_ $x$ существует предельная функция $f(x) = \lim_{n \to \infty} f_n(x)$ (поточечная сходимость).

Сделаем предельный переход $m \to \infty$:

$$
\lim_{m \to \infty} f_{n+m}(x) = f(x).
$$

_Тогда_:

$$
|f(x) - f_n(x)| \leq \varepsilon \quad \text{при } n > N \text{ и } \forall x \in X.
$$

Следовательно, по 2-му условию:

$$
f_n(x) \rightrightarrows f(x).
$$

---

# Признак Вейерштрасса

##### Теорема (Признак Вейерштрасса)

Пусть у нас есть функциональный ряд:

$$
\sum_{k=1}^\infty u_k(x),
$$

где каждый член ряда определен на $X$.

1. $|u_k(x)| \leq c_k \quad \forall x \in X,$
2. числовой ряд: $\sum_{k=1}^\infty c_k$ сходится.

_Тогда_: $\sum_{k=1}^\infty u_k(x)$ равномерно сходится.

[Доказательство]

Рассмотрим сумму ($m \in \mathbb{N}$):

$$
\left| \sum_{k=n+1}^{n+m} u_k(x) \right| = |u_{n+1}(x) + u_{n+2}(x) + \ldots + u_{n+m}(x)| \leq |u_{n+1}(x)| + |u_{n+2}(x)| + \ldots + |u_{n+m}(x)| \leq c_{n+1} + c_{n+2} + \ldots + c_{n+m}.
$$

Из условия сходимости числового ряда:

$$
\forall \varepsilon > 0 \; \exists N(\varepsilon), \; n > N \; m = 1, 2, \ldots:
$$

$$
|c_{n+1} + c_{n+2} + \ldots + c_{n+m}| < \varepsilon.
$$

Получаем:

$$
\left| \sum_{k=n+1}^{n+m} u_k(x) \right| < c_{n+1} + c_{n+2} + \ldots + c_{n+m} < \varepsilon.
$$

# 3.

# Непрерывность суммы функционального ряда и функциональных последовательностей

Пусть имеем функциональный ряд

$$
\sum_{n=1}^\infty u_n(x),
$$

1. члены ряда определены на $[a;b]$ и непрерывны на $[a;b]$
2. этот ряд равномерно сходится на $[a;b]$.

_Тогда_ сумма ряда также будет непрерывной функцией на $[a;b]$.

[Доказательство] Возьмём $\forall x_0 \in [a;b]$ и докажем, что $f(x)$ непрерывна в этой точке.

$$
f_n(x) = u_1(x) + u_2(x) + \dots + u_n(x)
$$

$$
f(x) = f_n(x) + \varphi_n(x) \quad (2)
$$

$$
f(x_0) = f_n(x_0) + \varphi_n(x_0) \quad (3)
$$

$$
(2) - (3) = f(x) - f(x_0) = f_n(x) - f_n(x_0) + \varphi_n(x) - \varphi_n(x_0)
$$

$$
|f(x) - f(x_0)| \le |f_n(x) - f_n(x_0)| + |\varphi_n(x)| + |\varphi_n(x_0)| \quad (5)
$$

Ряд равномерно сходится на $[a;b]$ $\Rightarrow$ $\forall \varepsilon > 0$ $\exists N(\varepsilon)$, $n > N$ :

$$
|\varphi_n(x)| \le \frac{\varepsilon}{3} \quad \text{для } \forall x \in [a;b] \quad (6)
$$

$$
|\varphi_n(x_0)| \le \frac{\varepsilon}{3} \quad (7)
$$

$f_n(x)$ — сумма конечного числа непрерывных функций $\Rightarrow f_n(x)$ — непрерывная функция, в частности в точке $x_0$. То по определению:

$$
\forall \varepsilon > 0\;\; \exists \delta(\varepsilon) > 0 : |x - x_0| < \delta \;\; \Rightarrow \;\; |f_n(x) - f_n(x_0)| < \frac{\varepsilon}{3} \quad (8)
$$

Получим, что

$$
\forall \varepsilon > 0\;\; \exists \delta(\varepsilon) > 0 : |x - x_0| < \delta \;\; \Rightarrow \;\; |f(x) - f(x_0)| < \varepsilon 
$$

(используя (6), (7), (8))

$\Rightarrow f(x)$ непрерывна в точке $x_0$. ■
_эта теорема достаточна, но не необходима_

---

Приведём пример ряда, состоящего из непрерывных функций, этот ряд неравномерно сходится, но тем не менее сумма является непрерывной функцией.

**Пример:**

$$
\sum_{n=1}^\infty \left[\frac{nx}{1 + n^2x^2} - \frac{(n-1)x}{1 + (n-1)^2x^2}\right], \quad 0 \le x \le 1
$$

$$
u_n(x) = \frac{nx}{1 + n^2x^2} \;-\; \frac{(n-1)x}{1 + (n-1)^2x^2}
$$

$$
f_n(x) = \sum_{k=1}^n u_k(x) = u_1(x) + u_2(x) + \dots + u_n(x)
$$

$$
= \;\frac{x}{1 + x^2}\; -\; 0 \;+\; \frac{2x}{1 + 2^2x^2}\; -\; \frac{x}{1 + x^2} \;+\; \dots \;+\; \frac{nx}{1 + n^2x^2} \;-\; \frac{(n-1)x}{1 + (n-1)^2x^2}
$$

$$
= \frac{nx}{1 + n^2x^2}
$$

$$
f(x) = \lim_{n \to \infty} \frac{nx}{1 + n^2x^2} = 0
$$

Ряд неравномерно сходится, $u_n(x)$ непрерывны, $f(x)$ непрерывна на $[0;1]$. □

# 4.

# Почленный переход к пределу в функциональных рядах

Точка $a$ является для множества $X$ _точкой сгущения_ (предельной точкой), если $(a - \delta; a + \delta)$ имеется точка из $X$, отличная от $a$.

##### Теорема (о почленном переходе к пределу)

1. Пусть имеем ряд $\sum_{n=1}^\infty u_n(x),$ каждый член которого определён на множестве $X$.
2. $a$ — точка сгущения для множества $X$
3. $\lim_{x \to a} u_n(x) = c_n \quad \text{для } \forall n = 1, 2, \dots .$
4. Этот функциональный ряд равномерно сходится на $X$.

_Тогда_:

1. $\displaystyle \sum_{n=1}^\infty c_n = C$ сходится
2. $\displaystyle \lim_{x \to a} f(x) = C$

**Почленный переход под знаком суммы:**

$$
\lim_{x \to a} f(x) = C 
\quad \Longleftrightarrow \quad
\lim_{x \to a} \sum_{k=1}^\infty u_k(x) 
= \sum_{k=1}^\infty c_k 
= \sum_{k=1}^\infty \lim_{x \to a} u_k(x)
$$

[Доказательство]

_Докажем 1-ое_ (необходимое и достаточное условие):

$$
\forall \varepsilon > 0 \quad \exists N(\varepsilon), \quad n > N, \quad m = 1, 2, \dots
$$

$$
|u_{n+1}(x) + u_{n+2}(x) + \dots + u_{n+m}(x)| < \varepsilon 
\quad (1)
$$

при $x \to a$ переходим к пределу:

$$
|c_{n+1} + c_{n+2} + \dots + c_{n+m}| \le \varepsilon 
\quad (2)
$$

$$
\Rightarrow \sum_{n=1}^\infty c_n = C.
$$

_Докажем 2-ое_:

$$
f(x) = f_n(x) + \varphi_n(x)
\quad (3)
$$

$$
C = C_n + \gamma_n
\quad (4)
$$

$$
f(x) - C = f_n(x) - C_n + \varphi_n(x) - \gamma_n
$$

$$
|f(x) - C| \le |f_n(x) - C_n| + |\varphi_n(x)| + |\gamma_n|
\quad (5)
$$

Так как функциональный ряд равномерно сходится, то

$$
\forall \varepsilon > 0 \quad \exists N_1(\varepsilon), \quad n > N_1 \quad 
\Rightarrow \quad |\varphi_n(x)| < \frac{\varepsilon}{3}, \quad \forall x \in X
\quad (6)
$$

И так как
$\displaystyle \sum_{n=1}^\infty c_n$ сходится, то

$$
\forall \varepsilon > 0 \quad \exists N_2(\varepsilon), \quad n > N_2 
\quad \Rightarrow \quad |\gamma_n| < \frac{\varepsilon}{3}
\quad (7)
$$

$$
N = \max(N_1, N_2), \quad n > N
$$

$$
\lim_{x \to a} f_n(x) = \lim_{x \to a} \sum_{k=1}^n u_k(x) 
= \sum_{k=1}^n c_k = C_n
$$

$$
\forall \varepsilon > 0 \quad \exists \delta(\varepsilon) > 0 : |x - a| < \delta 
\quad \Rightarrow \quad |f_n(x) - C_n| < \frac{\varepsilon}{3}
\quad (8)
$$

$$
\forall \varepsilon > 0 \quad \exists \delta(\varepsilon) > 0, 
\quad |x - a| < \delta 
\quad \Rightarrow \quad
$$

$$
(5) = |f(x) - C|
< 
\underbrace{\frac{\varepsilon}{3} + \frac{\varepsilon}{3} + \frac{\varepsilon}{3}}_{\text{(6),(7),(8)}} 
= \varepsilon
$$

$$
\Rightarrow (По Коши)\lim_{x \to a} f(x) = C. 
$$

■

# 5.

# Почленное интегрирование функциональных рядов

##### Теорема (достаточное условие)

1. Пусть имеем ряд $\sum_{n=1}^\infty u_n(x)\quad (1),$ члены ряда непрерывны на $[a;b]$.
2. Ряд $(1)$ равномерно сходится на $[a;b]$. (*необх., но не дост. условие*)

_Тогда_ справедлива формула:

$$
\int_a^b f(x)\,dx 
= \int_a^b \Bigl\{\sum_{n=1}^\infty u_n(x)\Bigr\}\,dx 
= \sum_{n=1}^\infty \int_a^b u_n(x)\,dx 
= \int_a^b u_1(x)\,dx 
+ \int_a^b u_2(x)\,dx 
+ \dots 
\quad (2)
$$

Заметим, что все интегралы в этой формуле существуют.

[Доказательство]

$$
f(x) = f_n(x) + \varphi_n(x), \quad (3)
$$

где

$$
f_n(x) = u_1(x) + u_2(x) + \dots + u_n(x),
$$

$$
\varphi_n(x) = \sum_{k=n+1}^\infty u_k(x)\quad (\text{равн. сх.}).
$$

Так как ряд равномерно сходится, остаточный ряд тоже равномерно сходится и члены ряда непрерывные функции, то $\varphi_n(x)$ _непрерывна_.
$f_n(x)$ _непрерывна_ как сумма непрерывных функций. Поэтому имеем право проинтегрировать обе части равенства $(3)$:

$$
\int_a^b f(x)\,dx 
= \int_a^b f_n(x)\,dx + \int_a^b \varphi_n(x)\,dx 
\quad (4)
$$

Следовательно,

$$
\int_a^b f(x)\,dx - \int_a^b f_n(x)\,dx 
= \int_a^b \varphi_n(x)\,dx.
$$

Нам нужно показать, что

$$
\lim_{n \to \infty} \int_a^b \varphi_n(x)\,dx = 0 
\quad (5).
$$

$$
\forall \varepsilon > 0 \;\; \exists N(\varepsilon), \; n > N 
\quad \Rightarrow \quad 
|\varphi_n(x)| < \varepsilon 
\quad \forall x \in [a;b]
$$

$$
\Bigl|\int_a^b \varphi_n(x)\,dx\Bigr| 
\;\le\; \int_a^b |\varphi_n(x)|\,dx 
\;<\; \int_a^b \varepsilon\,dx 
=\; \varepsilon\,(b-a),
$$

что равносильно $(5)$.

Тогда из формулы $(4)$ будет следовать, что предел левой части также стремится к $0$ и это будет означать, что:

$$
\lim_{n\to\infty} \int_a^b f_n(x)\,dx 
= \int_a^b f(x)\,dx.
$$

Но

$$
\lim_{n\to\infty} \int_a^b f_n(x)\,dx 
= \lim_{n\to\infty} 
\int_a^b \bigl[u_1(x) + u_2(x) + \dots + u_n(x)\bigr]\,dx 
$$

$$
= \int_a^b u_1(x)\,dx + \int_a^b u_2(x)\,dx + \dots 
= \sum_{n=1}^\infty \int_a^b u_n(x)\,dx,
$$

то есть

$$
\int_a^b f(x)\,dx 
= \sum_{n=1}^\infty \int_a^b u_n(x)\,dx.
$$

Запишем $(2)$ в следующем виде:

$$
\int_a^b f(x)\,dx 
= \int_a^b \Bigl(\sum_{k=1}^\infty u_k(x)\Bigr)\,dx 
= \sum_{k=1}^\infty \int_a^b u_k(x)\,dx 
= \lim_{n\to\infty} \sum_{k=1}^n \int_a^b u_k(x)\,dx 
$$

(как сумма конечного числа слагаемых)

$$
=\; \lim_{n\to\infty} \int_a^b \Bigl(\sum_{k=1}^n u_k(x)\Bigr)\,dx 
= \lim_{n\to\infty} \int_a^b f_n(x)\,dx 
\quad (6)
$$

_Условие равномерной сходимости рядов достаточно, но не необходимо._

---

[Пример:]

$$
\sum_{n=1}^\infty \Bigl[\frac{nx}{1 + n^2x^2} - \frac{(n-1)x}{1 + (n-1)^2x^2}\Bigr].
$$

$$
f_n(x) = \frac{nx}{1 + n^2x^2}
\quad\Rightarrow\quad
f_n(x) \not\to 0 \quad x \in [0;1].
$$

Проверим соотношение $(6)$ для этого ряда:

$$
\lim_{n\to\infty} \int_0^1 \frac{nx}{1 + n^2x^2}\,dx 
= \lim_{n\to\infty} \frac{1}{2n} \int_0^1 \frac{d(n^2x^2 + 1)}{1 + n^2x^2}
= \lim_{n\to\infty} \frac{1}{2n}\Bigl[\ln(1 + n^2x^2)\Bigr]_0^1
$$

$$
= \lim_{n\to\infty} \frac{1}{2n} \ln(1 + n^2) 
= \lim_{n\to\infty} \frac{\ln(1 + n^2)}{2n} = 0
= \int_0^1 0 \,dx = 0.
$$

$(6)$ выполняется, но ряд неравномерно сходится. Поэтому равномерная сходимость для почленного интегрирования является достаточным, но не необходимым условием.

# 6.

# Почленное дифференцирование функциональных рядов

##### Теорема (достаточное условие).

1. Пусть дан ряд $\sum_{k=1}^\infty u_k(x),$ члены которого определены и непрерывны на $[a; b]$
2. $u_k(x)$ для $k = 1, 2, \dots$ имеют непрерывные производные.
3. сам ряд $\displaystyle \sum_{k=1}^\infty u_k(x)$ сходится на $[a; b]$ (**поточечно**)
4. ряд $\displaystyle \sum_{k=1}^\infty u_k'(x)$ **равномерно** сходится на $[a; b]$.

_Тогда_ справедлива формула:

$$
f'(x) \;=\; \biggl(\,\sum_{k=1}^\infty u_k(x)\biggr)' \;=\; \sum_{k=1}^\infty u_k'(x).
\quad (1)
$$

[Доказательство]

Обозначим

$$
f(x) \;=\; \sum_{k=1}^\infty u_k(x)
\quad\text{и}\quad
f^*(x) \;=\; \sum_{k=1}^\infty u_k'(x).
$$

$u_k'(x)$ непрерывны и их ряд сходится равномерно
$\Rightarrow f^*(x)$ есть равномерно сходящийся ряд непрерывных функций
$\Rightarrow f^*(x)$ _непрерывна_ на $[a;b]$.

Рассмотрим интеграл от $a$ до $z$, где $z \in [a;b]$:

$$
\int_a^z f^*(t)\,dt 
\;=\; \int_a^z \sum_{k=1}^\infty u_k'(t)\,dt \;=\;\sum_{k=1}^\infty\int_a^z u_k'(t)\,dt.
$$

(_Вторая часть уравнения - по условию равномерной сходимости_)

Каждый $\int_a^z u_k'(t)\,dt$ равен $\bigl[u_k(t)\bigr]_{a}^{z} = u_k(z) - u_k(a)$. Тогда

$$
\sum_{k=1}^\infty \int_a^z u_k'(t)\,dt 
\;=\; \sum_{k=1}^\infty \bigl(u_k(z) - u_k(a)\bigr) 
\;=\; \sum_{k=1}^\infty u_k(z) \;-\; \sum_{k=1}^\infty u_k(a)\;=\; f(z) - f(a).
$$

Таким образом получаем:

$$
\int_a^z f^*(t)\,dt \;=\; f(z) \;-\; f(a).
$$

Это означает, что функция $f(x)$ (при фиксированном $a$) имеет производную $f^*(x)$ на $[a;b]$.
Следовательно, дифференцируя обе части равенства по $x$ _(учитывая свойство интегралов с переменным верхним пределом_ $\int_a^x f^*(t)\,dt \;=\; f(x) - f(a)$ _)_, мы приходим к

$$
\frac{d}{dx} \Bigl[\int_a^x f^*(t)\,dt\Bigr] \;=\; f^*(x)
\quad\text{и}\quad
\frac{d}{dx} \bigl[f(x) - f(a)\bigr] \;=\; f'(x).
$$

Значит,

$$
f'(x) = f^*(x) = \sum_{k=1}^\infty u_k'(x).
$$

Что и требовалось доказать. ■

# 7.

# Интеграл зависящий от параметра. Необходимые и достаточные условия равномерной сходимости функций. Непрерывность предельной функции

Пусть имеем функцию от двух переменных $f(x, y)$, где $x \in [a; b]$, $y \in Y$. Пусть при $\forall$ фиксированном $y$ существует

$$
\int_a^b f(x, y)\,dx.
$$

Меняя $y$, мы получим другое значение. Будем считать, что этот интеграл существует для всех $y \in Y$. Этот интеграл называется **функцией** от параметра $y$:

$$
I(y) = \int_a^b f(x, y)\,dx.
$$

---

## Равномерное стремление к предельной функции

Пусть функция $f(x, y)$ определена на некотором двумерном множестве $M = X \times Y$, и множество $Y$ имеет **конечную** точку сгущения $y_0$.

**Определение (для конечного $y_0$).** Если для функции $f(x, y)$:

1. $\exists \lim\limits_{y \to y_0} f(x, y) = \varphi(x)$, $\forall x \in X$.
2. $\forall \varepsilon > 0\; \exists \delta(\varepsilon) > 0$, не зависящее от $x$, что как только $\lvert y - y_0\rvert < \delta \implies \lvert f(x, y) - \varphi(x)\rvert < \varepsilon$ $\forall x \in X$.

Тогда $f(x, y) \rightrightarrows \varphi(x)$ $\forall x \in X$ (**равномерно сходится** относительно $x$) при $y \to y_0$.

---

Пусть функция $f(x, y)$ определена на некотором двумерном множестве $M = X \times Y$, и множество $Y$ имеет **бесконечную** точку сгущения $y_0 = +\infty$ (или $-\infty$).

**Определение (для бесконечного $y_0$).** Если для функции $f(x, y)$:

1. $\exists \lim\limits_{y \to +\infty(-\infty)} f(x, y) = \varphi(x)$, $\forall x \in X$.
2. $\forall \varepsilon > 0 \; \exists \Delta(\varepsilon) > 0$, не зависящее от $x$, что как только $y > \Delta$ (или $y < -\Delta$) $\implies \lvert f(x, y) - \varphi(x)\rvert < \varepsilon$ $\forall x \in X$.

То $f(x, y) \Longrightarrow \varphi(x)$ $\forall x \in X$ (равномерно сходится относительно $x$) при $y \to +\infty$ (или $y \to -\infty$).

---

## Необходимое и достаточное условие

### Теорема (необходимое и достаточное условие) для конечного $y_0$

Пусть функция $f(x, y)$ определена на некотором двумерном множестве $M = X \times Y$, и множество $Y$ имеет точку сгущения $y_0$. Для того, чтобы $f(x, y)$ при $y \to y_0$ имело бы предельную функцию $\varphi(x)$ и стремилась бы к ней равномерно, **необходимо и достаточно**, чтобы

$$
\forall \varepsilon > 0\;\; \exists \delta(\varepsilon) > 0,\; \text{не зависящее от } x,\; |y - y_0| < \delta
\;\;\Longrightarrow\;\; |\,f(x,y) - f(x,y_0)\,| < \varepsilon \quad \forall x \in X.
\quad (1)
$$

#### Доказательство (для конечного $y_0$)

1. **Необходимость.**\
   Допустим, (1) и (2) (см. определение равномерной сходимости) выполняются, т.е. $\lim\limits_{y \to y_0} f(x,y) = \varphi(x)$. Тогда $\,f(x,y)\Rightarrow \varphi(x)$ относительно $x$ при $y \to y_0$.\
   Воспользуемся определением равномерной сходимости:
   $$
   \forall \varepsilon > 0\;\; \exists \delta(\varepsilon) > 0,\; \text{не зависящее от } x:\; 
   |y - y_0| < \delta \implies |\,f(x,y) - \varphi(x)\,| < \frac{\varepsilon}{2} \quad \forall x \in X.
   $$

$$
   Аналогично для $y'$:  
$$

|y' - y_0| < \delta \implies |,f(x,y') - \varphi(x),| < \frac{\varepsilon}{2} \quad \forall x \in X.

$$
   Тогда  
$$

|,f(x,y') - f(x,y),| ;=; |,f(x,y') - \varphi(x) + \varphi(x) - f(x,y),|
;\le; |,f(x,y') - \varphi(x),| + |,\varphi(x) - f(x,y),|
< \frac{\varepsilon}{2} + \frac{\varepsilon}{2} = \varepsilon,
\quad \forall x \in X.

$$

2. **Достаточность.**  
   Пусть выполняется условие (1). Зафиксируем $\forall z \in X$. Зафиксируем $y$ так, чтобы $|y-y_0| < \delta$. Переходим к пределу при $y \to y_0$ в (1):  
$$

|\varphi(x) - f(x,z)| ;\le; \varepsilon \quad \forall x \in X
\quad (4)

$$
   $\implies \; f(x,y) \Rightarrow \varphi(x)$ относительно $x$ при $y \to y_0$.

---

### Теорема (для бесконечного $y_0$)

Для того, чтобы $f(x,y)$ при $y \to y_0 = +\infty$ (или $-\infty$) имело бы предельную функцию $\varphi(x)$ и стремилось бы к ней равномерно в области $X$, **необходимо и достаточно**, чтобы

$$

\forall \varepsilon > 0; \exists \Delta(\varepsilon) > 0,;\text{не зависящее от }x,;
y > \Delta ;\text{(или } y < -\Delta\text{)}
;;\Longrightarrow;;
|,f(x,y) - \varphi(x),| < \varepsilon
;; \forall x \in X.
\quad (1)

$$

---

## Непрерывность предельной функции

**Теорема.** В качестве $X$ возьмём $[a; b]$. Пусть $f(x, y)$ при $\forall$ фиксированном $y \in Y$ непрерывна на $X$ и известно, что при $y \to y_0$ $f(x, y) \Longrightarrow \varphi(x)$. Тогда $\varphi(x)$ также будет непрерывной функцией на $X$.

**Доказательство.** 

Рассмотрим последовательность $\{y_n\} \to y_0$, $y_n \in Y$. По условию $f(x,y) \Longrightarrow \varphi(x)$ при $y \to y_0$, то есть
$$

\forall \varepsilon > 0; \exists \delta(\varepsilon) > 0,; \text{не зависящее от }x,;
|y - y_0| < \delta
;;\Longrightarrow;;
|,f(x,y) - \varphi(x),| < \varepsilon
\quad \forall x \in [a;b].

$$

Выберем $n$ — большое, чтобы $|\,y_n - y_0\,| < \delta$. Тогда
$$

|,f(x, y_n) - \varphi(x),| < \varepsilon
\quad \forall x \in [a;b].

$$

Определим
$$

\psi_n(x) = f(x, y_n) ;;\Rightarrow;; \psi_n(x) \xrightarrow[n\to\infty]{} \varphi(x).

$$
Но $\psi_n(x) = f(x,y_n)$ — непрерывна на $[a;b]$, значит $\psi_n \to \varphi$ равномерно, а потому $\varphi(x)$ будет непрерывна на $[a;b]$.

Если имеем последовательность непрерывных функций $\{\psi_n(x)\} \subset [a;b]$, которая **равномерно** сходится к $\varphi(x)$, то предельная функция $\varphi(x)$ будет также непрерывна на $[a;b]$. 

# 8.
# Предельный переход по параметру под знаком интеграла. Непрерывность интеграла по параметру.
Рассмотрим интеграл 
$$

I(y) = \int_a^b f(x, y),dx.

$$$$$

Если
1) $f(x,y) \,\,\forall y\in Y$ непрерывна по $x\in [a,b]$
2) $f(x,y)\rightrightarrows \varphi (x)$ при $y \rightarrow y_0$

тогда
$$\lim\limits_{y \to y_0} I(y) = \lim\limits_{y \to y_0} \int_a^b f(x, y) dx = \int_a^b \varphi (x) dx = \int_a^b \lim\limits_{y \to y_0} f(x,y) dx$$

---

[Доказательство]

$\varphi (x)$ непрерывная функция $\Rightarrow$ $\exists \int_a^b \varphi (x) dx$ 

Из 2. имеем
$\forall \varepsilon > 0 \quad \exists \delta(\varepsilon) > 0 \quad|y-y_0| < \delta \,\,=> |f(x,y) - \varphi (x)| < \varepsilon \quad \forall x \ \in X$
$$|\int_a^b f(x,y) dx - \int_a^b \varphi(x) dx| = |\int_a^b(f(x,y) - \varphi(x))dx| \leq \int_a^b |f(x,y) - \varphi(x)|dx < \varepsilon(b-a)$$
$$\Rightarrow \lim\limits_{y\rightarrow y_0} \int_a^b f(x,y) dx = \int_a^b \varphi(x)dx \Rightarrow$$
$$\Rightarrow \forall \varepsilon > 0 \quad \exists \delta(\varepsilon) > 0 \quad|y-y_0| < \delta \quad |\int_a^bf(x,y)dx - \int_a^b\varphi(x)dx| < \varepsilon$$
---
# Непрерывность функции $I(y)$
##### Теорема: 
Пусть $f(x,y)$ непрерывна по совокупности в прямоугольнике $[a,b;c,d]$

Тогда $I(y) = \int_a^b f(x,y)dx$ непрерывна на отрезке $[c,d]$

[Доказательство]
Так как $f(x,y)$ непрерывна по совокупности в прямоугольнике $[a,b;c,d]$, то она будет равномерно непрерывна по теореме Кантора
$$\forall \varepsilon > 0 \quad \exists \delta(\varepsilon) > 0 \quad \forall\left(x',y' \right),(x'',y'')\in [a,b;c,d]$$$$|x''-x'| < \delta \quad |y''-y'| < \delta \quad \Rightarrow$$$$\Rightarrow |\int_a^bf(x'',y'')dx - \int_a^bf(x',y')dx| < \varepsilon$$

Возьмем 
$x''=x'=x$ ($\forall x\in [a,b]$),
$y'=y_0$ ($\forall y_0 \in [c,d]$), $y''=y$ и подставим

Получили $$\forall \varepsilon > 0 \quad \exists \delta(\varepsilon) > 0 \quad |y-y_0| < \delta \quad \Rightarrow$$$$\Rightarrow |\int_a^bf(x,y)dx - \int_a^bf(x,y_0)dx| < \varepsilon$$

Это означает, что $f(x,y) \rightrightarrows f(x,y_0)$

Сделаем предельный переход согласно теореме
$$\lim\limits_{y\rightarrow y_0}I(y) = \lim\limits_{y\rightarrow y_0} \int_a^b f(x,y)dx = \int_a^b \lim\limits_{y \rightarrow y_0} f(x,y)dx = \int_a^b f(x,y_0)dx = I(y_0) \Rightarrow$$
$\Rightarrow$ в силу того, что $y_0$ произвольная точка из $[c,d]$, то 
$I(y)$ непрерывна на $[c,d]$

■

---
$$$$$
