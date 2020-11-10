# Dateiformate

## Dateiformate

Möglichkeiten zum Speichern / Lesen:

- Text-Dateien
- JSON
- CSV
- XML
- Python-Objektdateien (mittels pickle und shelve)
- Binärdateien

## CSV

CSV ist ein Textdateiformat, das tabellarische Daten beinhalten kann

Beispiel:

```csv
ISO,Country,Capital,Languages
AD,Andorra,Andorra la Vella,"ES,FR"
AE,United Arab Emirates,Abu Dhabi,"AE,fa,en,hi,ur"
AF,Afghanistan,Kabul,"AF,tk"
```

## CSV schreiben

```py
import csv

data = [
    ['code', 'area', 'population'],
    ['CN', 9.6, 1386],
    ['RU', 17, 144],
    ['US', 9.8, 327]
]

with open('data.csv', 'w', encoding='utf-8', newline='') as f:
    writer = csv.writer(f)
    writer.writerows(data)
```

## CSV lesen

```py
import csv

with open('countries.csv', encoding='utf-8', newline='') as csv_file:
    reader = csv.reader(csv_file)
    for row in reader:
        print(row)
```

## JSON

## JSON speichern

```py
import json

data = ["one", "two", "three"]
jsonstring = json.dumps(data)

with open("numbers.json", mode="w", encoding="utf-8") as jsonfile:
    jsonfile.write(jsonstring)
```

## JSON lesen

```py
import json

with open("numbers.json", encoding="utf-8") as jsonfile:
    jsonstring = jsonfile.read()
data = json.loads(jsonstring)
```

## XML

zwei Pakete in der Python-Standardlibrary:

- `xml.etree.ElementTree`
- `xml.dom.minidom`

externe Library (Erweiterung von ElementTree):

- `lxml`

## XML mit ElementTree: erstellen

```py
import xml.etree.ElementTree as et

person = et.Element('person')
name = et.SubElement(person, 'name')
name.text = 'Adam'
age = et.SubElement(person, 'age')
age.text = '40'
age.set("unit", "years")
```

## XML mit ElementTree: speichern

Beim Speichern von XML immer ein Encoding angeben!

```py
xmlbytestring: bytes = et.tostring(person, encoding='utf-8')
with open("myfile.xml", mode="wb") as file:
    file.write(xmlbytestring)

# or
xmlstring: str = et.tostring(person, encoding='unicode')
with open("myfile.xml", encoding="utf-8", mode="w") as file:
    file.write(xmlstring)

# or
tree = et.ElementTree(person)
tree.write("myfile.xml", encoding="utf-8")
```

## XML mit ElementTree: lesen

```py
import xml.etree.ElementTree as et

person = et.fromstring(xmlstring)
for childnode in person:
    print(childnode.tag)
    print(childnode.text)
    print(childnode.attrib)
```

## Pickle

Eigenes Dateiformat, in dem verschiedene Python-Dateitypen gespeichert werden können

Achtung:

- Pickle-Dateien können nur von Python gelesen werden
- Pickle-Dateien können bösartigen Code enthalten

## Pickle

```py
import pickle
import datetime

now = datetime.datetime.now()

serialized = pickle.dumps(now)

with open("datetime.pickle", mode="wb") as picklefile:
    picklefile.write(serialized)
```

## Pickle

```py
import pickle

with open("datetime.pickle", mode="rb") as picklefile:
    serialized = picklefile.read()
earlier = pickle.loads(serialized)
```

## Übung

Speichern und Lesen eines Tic-Tac-Toe-Feldes in verschiedenen Formaten

Python-Datenstruktur:

```py
field = [
  ['X', 'O', None],
  ['X', 'X', 'O'],
  ['O', 'O', 'X']
]
```
