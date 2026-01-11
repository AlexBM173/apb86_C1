Errors
======

Errors occur in Python programs for a variety of reasons and cause the program to crash. Exceptions are a way of handling errors, or can be raised
intentionally to signal that something unexpected has occurred, usually as a result of a logical error that would not be caught by Python itself,
but which would cause the program to behave incorrectly.

1. Types of error in Python
---------------------------

There are many different types of error in Python, some of the most common are:

- SyntaxError: Raised when the code is not written with correct Python syntax, e.g. missing a colon at the end of a function definition.
- ZeroDivisionError: Raised when attempting to divide a number by zero.
- OverflowError: Raised when a calculation produces a result that is too large to be represented.
- NameError: Raised when trying to access a variable that has not been defined.
- IndexError: Raised when trying to access an index that is out of range for a list or other sequence.
- KeyError: Raised when trying to access a key that does not exist in a dictionary.
- TypeError: Raised when an operation is performed on a value of an inappropriate type.
- ValueError: Raised when a function receives an argument of the correct type but an inappropriate value e.g. trying to convert a non-numeric string
to an integer.
- AssertionError: Raised when an assert statement fails.
- ImportError: Raised when an import statement fails to find the module or one of its attributes.
- AttributeError: Raised when trying to access an attribute that does not exist on an object.
- FileNotFoundError: Raised when trying to open a file that does not exist.

2. Exception handling
---------------------

It is possible to handle specific exceptions in Python using try and except blocks. This allows the program to continue running even if an error
occurs. Such a block is composed of a try clause, which contains code that may raise an exception, and one or more except clauses, which contain
code that is executed if a specific exception is raised. For example:

.. code-block:: python

    try:
        x = 1 / 0
    except ZeroDivisionError:
        print("Cannot divide by zero")

When an exception is raised, it may have values associated with it, known as arguments. These can be accessed in the except clause by specifying
the exception as a variable using the `as` keyword. For example:

.. code-block:: python

    try:
        x = int("not a number")
    except ValueError as e:
        print(f"ValueError occurred: {e}")

It is possible to use the generic ``Exception`` class to catch any exception that is raised, but this is not recommended as it can make debugging 
more difficult and may hide unexpected errors. It is better to catch specific exceptions that are expected to occur.