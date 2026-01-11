18. Algorithms and complexity
=============================

1. Introduction to complexity
-----------------------------

Euclid's algorithm for finding the greatest common divisor (GCD) of two integers :math:`m` and :math:`n`:

i. Divide :math:`m` by :math:`n` and get the remainder :math:`r`.
ii. If :math:`r = 0`, the algorithm terminates and :math:`n` is the GCD.
iii. Otherwise, set :math:`m = n` and :math:`n = r`, and repeat from step i.

An algorithm is a finite set of rules giving a sequence of operations for solving a specific type of a problem. An algorithm has the properties:

- Finiteness: An algorithm must termiante in a finite number of steps.
- Definiteness: Each algorithm must be precisely defined; algorithms must be unambiguously defined for each case. This requires inputs types to be
well specified.
- Input: An algorithm has zero or more inputs.
- Output: An algorithm has one or more outputs.
- Effectiveness: All operations must be able to be performed exactly and in a finite length of time.

Computational method: A quadruple :math:`(Q,I, \Omega, f)` in which :math:`Q` is a set of states containing subsets :math:`I` and :math:`\Omega`, and
:math:`f` is a function from :math:`Q` to itself. :math:`f` should leave :math:`\Omega` pointwise fixed; that is :math:`f(q) = q \forall\;q\;\in\Omega`.
The four quantities are meant to represent the states of the computation, the inputs, the outputs, and the computational rule. Each :math:`x\in I`
defines a computational sequence :math:`x_0=x,\; x_{k+1} = f(x_k)` for :math:`k\ge 0`. The computational sequence terminates in k steps if k 
is the smallest integer for which :math:`x_k\in\Omega`. A computational method is an algorithm if it terminates in finitely many steps
:math:`\forall x\in I`. Note: This does not describe effectiveness.

Let :math:`f,g` be two real functions:

.. math:: 
    
    f(n) = \mathcal{O}(g(n)) \text{ iff }\exists n_0\in\mathbb{N}, c\in\mathbb{R}_+^*,\forall n\ge n_0, 0\le f(n) \le c g(n)

.. math::

    f(n) = \Omega(g(n)) \text{ iff }\exists n_0\in\mathbb{N}, c\in\mathbb{R}_+^*,\forall n\ge n_0, 0\le c g(n) \le f(n)

.. math::

    f(n) = \Theta(g(n)) \text{ iff } \exists (c,c')\in(\mathbb{R}_+^*)^2, n_0\in\mathbb{N},\forall n\ge n_0, 0\le c g(n) \le f(n) \le c' g(n)

.. math::

    f(n)\sim g(n) \text{ when } \lim_{n\to\infty} \frac{f(n)}{g(n)} = 1

.. math::

    f\sim g\implies f=\Theta(g)

.. math::

    f=\Theta(g)\implies g=\Theta(f)

The hierarchy of functions is:

.. math::

    \log n \ll n^a \ll b^n \ll n! \ll n^n

2. Fibonacci sequence
---------------------

The Fibonacci sequence is defined as :math:`F_n=1` for :math:`n=1,2` and :math:`F_n=F_{n-1}+F_{n-2}` otherwise. This is a valid recurrence relation
with a second order polynomial solution. The analytical formula is:

.. math::

    F_n = \frac{1}{\sqrt{5}}\left(\frac{1+\sqrt{5}}{2}\right)^{n + 1} - \frac{1}{\sqrt{5}}\left(\frac{1-\sqrt{5}}{2}\right)^{n + 1}

which becomes inaccurate for large :math:`n` due to floating point precision issues. A basic recursive algorithm to compute :math:`F_n` is:

.. code-block:: c

    int fibo1(int n) {
        if (n <= 1 ) {
            return 1;
        } else {
            return fibonacci(n - 1) + fibonacci(n - 2);
        }
    }

If we let :math:`C(n)` be the number of function calls. :math:`C_{n} = 1 + C_{n-1} + C_{n-2}` with :math:`C_0 = C_1 = 1`. This has the same 
characteristic polynomial as the Fibonacci sequence, so :math:`C_n = F_{n+2}`. Thus the time complexity is :math:`\Theta(\varphi^n)` where 
:math:`\varphi = \frac{1+\sqrt{5}}{2}` is the golden ratio. We can improve this using memoization:

.. code-block:: c

    int fibo2(int n) {
        int *t;
        int i;
        int result;
        t = (int *)malloc((n + 1) * sizeof(int));
        t[0] = 1, t[1] = 1;
        for (i = 2; i <= n; i++) {
            t[i] = t[i - 1] + t[i - 2];
        }
        result = t[n];
        free(t);
        return result;
    }

Both time and space complexity for this algorithm are :math:`\Theta(n)`. We can reduce the space complexity to :math:`\Theta(1)` by only 
storing the last two computed values:

.. code-block:: c

    int fibo3(int n) {
        int a = 1, b = 1, c, i;
        if (n <= 1) {
            return 1;
        }
        for (i = 2; i <= n; i++) {
            c = a + b;
            a = b;
            b = c;
        }
        return b;
    }

This has time complexity :math:`\Theta(n)` and space complexity :math:`\Theta(1)`. We can do better still using matrix exponentiation:

.. math::

    \begin{pmatrix}
    F_n \\
    F_{n-1}
    \end{pmatrix}
    =
    \begin{pmatrix}
    1 & 1 \\
    1 & 0
    \end{pmatrix}^{n-1}
    \begin{pmatrix}
    F_1 \\
    F_0
    \end{pmatrix}

This allows us to compute :math:`F_n` in :math:`\Theta(\log n)` time using exponentiation by squaring:

.. code-block:: c

    void matrix_mult(int F[2][2], int M[2][2]) {
        int x = F[0][0] * M[0][0] + F[0][1] * M[1][0];
        int y = F[0][0] * M[0][1] + F[0][1] * M[1][1];
        int z = F[1][0] * M[0][0] + F[1][1] * M[1][0];
        int w = F[1][0] * M[0][1] + F[1][1] * M[1][1];
        F[0][0] = x;
        F[0][1] = y;
        F[1][0] = z;
        F[1][1] = w;
    }

    void matrix_pow(int F[2][2], int n) {
        if (n == 0 || n == 1) {
            return;
        }
        int M[2][2] = {{1, 1}, {1, 0}};
        matrix_pow(F, n / 2);
        matrix_mult(F, F);
        if (n % 2 != 0) {
            matrix_mult(F, M);
        }
    }

    int fibo4(int n) {
        int F[2][2] = {{1, 1}, {1, 0}};
        if (n == 0) {
            return 1;
        }
        matrix_pow(F, n - 1);
        return F[0][0];
    }

3. Sorting algorithms
---------------------

It can be shown that the complexity of common sorting algorithms are:

+-----------------+-------------------+-------------------+
| Algorithm       | Average case      | Worst case        |
+=================+===================+===================+
| Bubble sort     | :math:`O(n^2)`    | :math:`O(n^2)`    |
+-----------------+-------------------+-------------------+
| Insertion sort  | :math:`O(n^2)`    | :math:`O(n^2)`    |
+-----------------+-------------------+-------------------+
| Selection sort  | :math:`O(n^2)`    | :math:`O(n^2)`    |
+-----------------+-------------------+-------------------+
| Merge sort      | :math:`O(n log n)`| :math:`O(n log n)`|
+-----------------+-------------------+-------------------+
| Quick sort      | :math:`O(n log n)`| :math:`O(n^2)`    |
+-----------------+-------------------+-------------------+

The insertion sort algorithm works by iterating through the array and inserting each element into its correct position in the sorted portion
of the array:

.. code-block:: c

    void insertion_sort(int arr[], int n) {
        int i, key, j;
        for (i = 1; i < n; i++) {
            key = arr[i];
            j = i - 1;
            while (j >= 0 && arr[j] > key) {
                arr[j + 1] = arr[j];
                j = j - 1;
            }
            arr[j + 1] = key;
        }
    }

The worst case complexity occurs when the array is sorted in the reverse order. For insertion sort, this results in :math:`\Theta(n^2)` time 
complexity.

Any configuration of elements in an array is isomorphic to :math:`\sigma\in S_n`. If you label numbers by their size relative to the other numbers,
we get a permutation of :math:`\{1,2,\ldots,n\}`. We need to identify the permutation and invert it.

An inversion is a pair :math:`(a_i, a_j)` such that :math:`i < j` and :math:`a_i > a_j`. The number of inversions in a permutation :math:`\sigma` 
is denoted :math:`I(\sigma)`.

Given a permutation :math:`\sigma`, how many comparisons :math:`C_i(\sigma)` are needed to insert :math:`a_i` in :math:`a_{i-1}`?

An inversion table are numbers :math:`b_j` that are the number of inversions whose second element is :math:`j`. An inversion table uniquely
identifies a permutation. The constraint on the :math:`b_j` is :math:`0 \le b_j \le n - j`. The number of possible permutations of the numbers 
:math:`(1,2,\ldots,n)` is :math:`n!`.

.. math::

    C_i(\sigma)=1+\text{Card}(\{j | j < i \text{ and } a_j > a_i\})

    C(\sigma) = \sum_{i=1}^{n} C_i(\sigma) = n - 1 + I(\sigma)

If :math:`\sigma=(a_1,a_2,\ldots,a_n)` and :math:`\sigma' = (a_n, \ldots, a_2, a_1)`, then:

.. math::

    I(\sigma) + I(\sigma') = \frac{n(n-1)}{2}

The average complexity is then:

.. math::

    C_n = \frac{1}{n!} \sum_{\sigma\in S_n} C(\sigma) = n - 1 + \frac{1}{n!} \sum_{\sigma\in S_n} I(\sigma) = n - 1 + \frac{n(n-1)}{4} = \Theta(n^2)

4. Recursiveness
----------------

The factorial function can be defined recursively as:

.. code-block:: c

    int fact1(int n) {
        if (n == 0) {
            return 1;
        } else {
            return n * fact1(n - 1);
        }
    }

A better way of implementing the algorithm recursively is to use an accumulator:

.. code-block:: c

    int fact2(int n, int a, int b){
        if (n == 0) {
            return b;
        } else {
            return fact2(n - 1, a * n, b);
        }
    }

This has a complexity of :math:`\Theta(n)` time and :math:`\Theta(1)` space, whereas the previous implementation has a space complexity of 
:math:`\Theta(n)` due to the call stack.

Merge sort is another example of a recursive algorithm. You split an array into two halves, sort each half recursively, and then merge the sorted
halves back together. The time complexity is :math:`\Theta(n log n)`. Quick sort is similar, but you select a pivot element and partition the array
into elements less than the pivot and elements greater than the pivot, then sort each partition recursively. The average time complexity is
:math:`\Theta(n log n)`, but the worst case is :math:`\Theta(n^2)` if the pivot is the smallest or largest element. The complexity can be derived
as follows:

.. math::

    C(n) = n - 1 + \frac{1}{n} \sum_{k=0}^{n-1} (C(k) + C(n - k - 1))

    n C(n) = n(n - 1) + 2 \sum_{k=0}^{n-1} C(k)

    (n + 1) C(n + 1) = (n + 2) n + (2 n + 2) C(n)

    \frac{C_n}{n + 1} = \frac{C_{n-1}}{n} + \frac{2(n-1)}{n(n+1)} = \sum_{k=1}^n \frac{2(k-1)}{k(k+1)}

    \implies C(n) = n(2\gamma + \ln n - 4) + 1 + 2\gamma + 2\ln n + \frac{5}{6n} - \frac{1}{6 n^2} +\frac{1}{60n^3} + \mathcal{O}\left(\frac{1}{n^4}\right)

where :math:`\gamma` is the Euler-Mascheroni constant. The time complexity is thus :math:`\Theta(n log n)`.

We can make a decision tree to figure out the depth of a sorting algorithm. Each swap gives a new branch. The worst-case performance is the depth of the tree.
The number of leaves in the tree is the number of permutations, so :math:`n!`. We know that :math:`2^h\le n!\implies h\ge\log_2(n!)`. Using 
Stirling's approximation:

.. math::

    h = \Omega(n \log n)

5. Divide and conquer algorithms
--------------------------------

Divide and conquer algorithms are a class of algorithm where you split the problem into sub problems and recursively solve these. Examples include
merge sort, quick sort, FFT, Karatsuba multiplication, binary search, and Strassen's algorithm for matrix multiplication.

The Master Theorem: Suppose :math:`T(n)=aT(\frac{n}{b}+\mathcal{O}(1))+\mathcal{O}(n^d)`, then:

i. If :math:`a < b^d`, then :math:`T(n) = \Theta(n^d)`.
ii. If :math:`a = b^d`, then :math:`T(n) = \Theta(n^d \log n)`.
iii. If :math:`a > b^d`, then :math:`T(n) = \Theta(n^{\log_b a})`.