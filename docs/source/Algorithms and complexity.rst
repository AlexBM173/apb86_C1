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

3. Sorting algorithms
---------------------

4. Recursiveness
----------------

5. Divide and conquer algorithms
--------------------------------
