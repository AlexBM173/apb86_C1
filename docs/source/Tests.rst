Tests
=====

1. Test suites with pytest
--------------------------

To create a new test suite using pytest, you should create a Python file with a name like `test_*.py` or `*_test.py` in your project directory. In this file, you can define
test functions that start with the prefix `test_`. Here is an example of a simple test function:

.. code-block:: python

    def test_addition():
         assert 1 + 1 == 2


2. Additional features
----------------------

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

Tests executed in this way can be run in a module or a class, and can specify specific methods and parametrisations of those tests.

All of these test can be run from a file using the `@` prefix before the filename:

.. code-block:: bash

    pytest @file_with_tests.txt

where `file_with_tests.txt` may contain:

.. code-block:: text

    path/to/test_module.py
    path/to/test_module.py::test_function_name
    path/to/test_directory
    -k "expression"