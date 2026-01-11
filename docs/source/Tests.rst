10. Tests
=====

Tests are important parts of software development as they ensure comprehensive verification that your code is functioning as expected. Test
cases test individual components of your code with specific conditions, while test suites are collections of test cases that are run together.
A test case documents a specific input, expected output, and execution steps of part of a software's functionality. They are used to validate
individual components of the software to identify bugs, errors, or unexpected behaviour. It is good practice to write tests for a piece of code
as soon as it is written to catch issues early in the development process.

1. Test suites with pytest
--------------------------

To create a new test suite using pytest, you should create a Python file with a name like `test_*.py` or `*_test.py` in your project directory. In this file, you can define
test functions that start with the prefix `test_`. Here is an example of a simple test function:

.. code-block:: python

    def test_addition():
         assert 1 + 1 == 2

We see that it is based on the Python `assert` statement, which checks if the expression evaluates to `True`. If it does, the test passes; if not, 
it fails. It is possible to assert that a specific exception is raised using `pytest.raises` as follows:

.. code-block:: python

    import pytest

    def test_zero_division():
         with pytest.raises(ZeroDivisionError):
             1 / 0

You can group multiple, usually related, tests into a test class by defining a class that starts with the prefix `Test` and contains test methods
that also start with the prefix `test_`. Here is an example:

.. code-block:: python

    class TestMathOperations:
         def test_addition(self):
             assert 1 + 1 == 2

         def test_subtraction(self):
             assert 2 - 1 == 1

Floating-point comparisons can be done using `pytest.approx` to account for precision issues and works with scalars, lists, and NumPy arrays:

.. code-block:: python

    def test_floating_point():
         assert 0.1 + 0.2 == pytest.approx(0.3)


2. Additional features
----------------------

Fixtures are a feature of pytest that allow you to set up a specific test environment or provide common resources for multiple tests. They are
defined using the `@pytest.fixture` decorator and can be used as arguments in test functions. See https://docs.pytest.org/en/stable/reference/fixtures.html#fixtures
for more details. Some common fixtures include `tmp_path` for creating temporary directories, `monkeypatch` for modifying classes, functions, 
dictionaries, and environment variables during tests, `capsys` for capturing, as text, output to ``stdout`` and ``stderr``, and `subtests` for
enabling the declaration of subtests inside test functions.

Tests can be marked with custom features using the `@pytest.mark` decorator. This allows you to set metadata on your tests, such as filtering certain
warnings, skipping tests, producing expected failures, or performing multiple parametrised test. See https://docs.pytest.org/en/stable/how-to/mark.html#mark
for more details.

3. Running the test suite
-------------------------

In general, pytest follows standard test discovery rules that are detailed in https://docs.pytest.org/en/stable/explanation/goodpractices.html#test-discovery. 
You can invoke pytest using the following basic command:

.. code-block:: bash

   pytest

This will automatically discover and run all tests in the current directory and all its subdirectories. Tests can be run in a module:

.. code-block:: bash

   pytest path/to/test_module.py

In a directory:

.. code-block:: bash

   pytest path/to/test_directory

By keyword expressions:

.. code-block:: bash

    pytest -k "expression"

This will run all test that match the given case-insensitive string expression
You can also run specific tests by providing the path to the test file and appending `::` followed by the test function name:

.. code-block:: bash

    pytest path/to/test_module.py::test_function_name

Tests executed in this way can be run in a module or a class, and can specify specific methods and parametrisations of those tests. You can also
run tests with specific markers using the `-m` option:

.. code-block:: bash

    pytest -m "marker_name"

All of these tests can be run from a file using the `@` prefix before the filename:

.. code-block:: bash

    pytest @file_with_tests.txt

where `file_with_tests.txt` may contain:

.. code-block:: text

    path/to/test_module.py
    path/to/test_module.py::test_function_name
    path/to/test_directory
    -k "expression"

It is often a good idea to run tests in a directory separate from your source code. Tests can be run against an installed version of the package
after executing `pip install .` in the root of the package directory. Tests can also be run against the local copy of an editable install.