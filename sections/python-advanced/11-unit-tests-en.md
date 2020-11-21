# Unit tests

## assert

## Doctests

Code examples and unit tests in one - inside the doc string

## Doctests

```py
def add(a, b):
    """Add two numbers.

    >>> add(2, 3)
    5
    """
```

## Running doctests

```py
if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

## Doctests: long outputs

```py
"""
>>> bubblesort(list(range(10))) #doctest: +NORMALIZE_WHITESPACE
[0, 1, 2, 3, 4, 5,
6, 7, 8, 9]
>>> bubblesort(list(range(10))) #doctest: +ELLIPSIS
[0, 1, 2, ..., 8, 9]
"""
```

## Unit tests

Possibilities:

- unittest (standard library)
- pytest
- nose

## unittest

test_tictactoe.py

```py
import unittest
from tictactoe import has_won

class HasWon(unittest.TestCase):
    def test_has_won_first_row_x(self):
        board = [["X", "X", "X"],
                 [None, None, None],
                 [None, None, None]]
        w = has_won(board, "X")
        self.assertTrue(w)

if __name__ == "__main__":
    unittest.main()
```

## unittest

Running all tests in files that match _test\*.py_:

```bash
python -m unittest
```

Search for a different pattern:

```bash
python -m unittest discover -p "*_test.py"
```

Note: in order to be discovered all packages must contain a file named _\_\_init\_\_.py_ (see https://bugs.python.org/issue35617)

## unittest - assertions

assertions:

- `.assertEqual(a, 3)`
- `.assertTrue(b)`
- `.assertFalse(c)`
- `.assertIsNone(d)`
- `.assertIn(a, [2, 3, 4])`
- `.assertIsInstance(a, int)`
- `.assertRaises(TypeError, len)`
- ...

there are also contrary assertions, e.g. `.assertNotEqual(a, 3)`

## unittest - setUp and tearDown

Defining functions that are executed before / after each test:

```py
import unittest

class WidgetTestCase(unittest.TestCase):
    def setUp(self):
        self.widget = Widget('The widget')

    def tearDown(self):
        self.widget.dispose()
```

## unittest - test coverage

PIP package _coverage_

execution:

```bash
coverage run test_shorten.py
coverage report
```

Example output:

```
Name              Stmts   Miss  Cover
-------------------------------------
shorten.py            4      0   100%
test_shorten.py      11      0   100%
-------------------------------------
TOTAL                15      0   100%
```
