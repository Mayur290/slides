# Builtins, Standard Library

## Builtins, Standard Library

- _Builtins_: Funktionen und Objekte, die oft verwendet werden und immer verfügbar sind
- _Standard Library_: Sammlungen von zusätzlichen Modulen und Paketen, die importiert werden können

Dokumentation: https://docs.python.org/3/library/index.html

## Builtins

unter anderem:

- `print()`
- `input()`
- `len()`
- `max()`
- `min()`
- `open()`
- `range()`
- `round()`
- `sorted()`
- `sum()`
- `type()`

## Standard Library

Die _Standard Library_ bietet zusätzliche Module, die importiert werden können.

Beispiel (abrunden):

```py
import math

print(math.floor(3.6))
```

oder

```py
from math import floor

print(floor(3.6))
```

## Standard Library

interessante Module:

- `pprint` (formatierte Ausgabe)
- `random`
- `math`
- `datetime`
- `os` (Betriebssystem, Dateisystem)
- `urllib.request` (HTTP-Anfragen)
- `webbrowser` (einfache Steuerung des Standard-Browsers)

## print und pprint

Ausgabe mehrerer Elemente:

```py
print(1, 2, 3, sep=", ", end="\n\n")
```

```bash
1, 2, 3


```

## print und pprint

```py
import pprint

pprint.pprint(['Mercuy', 'Venus', 'Earth', 'Mars', 'Jupiter',
               'Saturn', 'Uranus', 'Neptune', 'Pluto'])
```

```txt
['Mercuy',
 'Venus',
 'Earth',
 'Mars',
 'Jupiter',
 'Saturn',
 'Uranus',
 'Neptune',
 'Pluto']
```

## open

Textdatei zum Schreiben öffnen:

```py
file = open("message.txt", "w", encoding="utf-8")
file.write("hello\n")
file.write("world\n)
file.close()
```

Datei muss zuvor nicht existieren

## random

```py
import random

print(random.randint(1, 6))
print(random.choice(["heads", "tails"]))
```

## urllib.request

Abfrage von Web-Inhalten

```py
from urllib.request import urlopen

content = urlopen("https://google.com").read()
print(content)
print(len(content))
```

## webbrowser

Beispiel:

```py
import webbrowser

webbrowser.open(
    "https://docs.python.org/3/library/webbrowser.html"
)
```
