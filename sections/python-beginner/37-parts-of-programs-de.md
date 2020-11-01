# Bestandteile von Programmen

## Bestandteile von Programmen

- Programme
  - Codeblöcke
    - Anweisungen
      - Ausdrücke

## Leere Codeblöcke

_leere_ Codeblöcke mittels des `pass`-Statements:

```py
# TODO: warn the user if path doesn't exist

if not os.path.exists(my_path):
    pass
```

## Anweisungen über mehrere Zeilen

Wenn ein Statement über mehrere Zeilen gehen soll, wird es üblicherweise in Klammern gesetzt

```py
a = (2 + 3 + 4 + 5 + 6 +
     7 + 8 + 9 + 10)
```

Alternative: Escapen von Zeilenumbrüchen mit `\`

```py
a = 2 + 3 + 4 + 5 + 6 + \
    7 + 8 + 9 + 10
```
