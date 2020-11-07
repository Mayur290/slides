# React testing library - Intermediate

## Andere Abfragen

- `getByText`: z.B. für _divs_, _spans_
- `getByLabelText`: findet Input nach Label
- `getByAltText`: z.B. für Bilder
- `getByTitle`: z.B. für Bilder / Links
- `getByTestId`: für explizite Test-IDs via `data-testid="..."`
- ... (siehe <https://testing-library.com/docs/dom-testing-library/api-queries#queries>)

## getByText

sinnvoll für _divs_ / _spans_ (keine _role_ vordefiniert)

Bemerkung: kann eventuell durch `getByRole` ersetzt werden, wenn die Rolle explizit vergeben wird (z.B. `role="presentation"`)

## getByLabelText

Inputs, die _Text_ beinhalten, können mittels `getByRole` abgefragt werden:

```js
screen.getByRole('textbox', 'first name');
```

Für andere Inputtypen sollten wir `getByLabelText` verwenden:

```js
screen.getByLabelText('birth year');
```

## Testen von asynchronem Code und APIs

Wir verwenden `findByRole` anstatt `getByRole`, um darauf zu warten, dass ein Element erscheint

`findByRole` sucht wiederholt nach einem Element bis es existiert (standardmäßig alle 0.05 Sekunden für maximal 1 Sekunde)

## Testen von asynchronem Code

Aufgabe: Testen einer `ChuckNorrisJoke`-Komponente, die ein API abfragt:

```js
const ChuckNorrisJoke = () => {
  const [joke, setJoke] = useState(null);
  useEffect(() => {
    fetch('https://api.chucknorris.io/jokes/random')
      .then((res) => res.json())
      .then((data) => setJoke(data.value));
  }, []);
  if (!joke) {
    return <div role="status">loading...</div>;
  }
  return <article>{joke}</article>;
};
```

## Testen von asynchronem Code

Testen mit echtem API:

```js
test('load Chuck Norris joke from API', async () => {
  render(<ChuckNorrisJoke />);
  const jokeElement = await screen.findByRole('article');
  // joke should have at least 3 characters
  expect(jokeElement).toHaveTextContent(/.../);
});
```

## Testen von asynchronem Code

Testen mit einem Mock-API:

```js
const data = {
  value: 'Chuck Norris counted to infinity. Twice.',
};
globalThis.fetch = () =>
  Promise.resolve({ json: () => Promise.resolve(data) });
```

## Testen von Fehlern

_TodoItem_-Komponente:

```jsx
test('throw an error if the title is missing', () => {
  const testFn = () => {
    render(<TodoItem />);
  };
  expect(testFn).toThrow(
    'property "title" must be present'
  );
});
```

## Testen des Nicht-Vorhandenseiens eines Elements

```js
expect(() => getByRole('listitem')).toThrow();
```

oder mittels _queryBy..._:

```js
expect(queryByRole('listitem')).toEqual(null);
```

_queryBy..._ gibt _null_ zurück statt eine Exception auszulösen

## Ressourcen

- [How to use React Testing Library Tutorial, Robin Wieruch](https://www.robinwieruch.de/react-testing-library)
- [react-testing-examples.com](https://react-testing-examples.com/)
- [JavaScript Testing Masterclass, Gabriel Vasile](https://docs.google.com/presentation/d/1ljMA8glel6hCopJ9Ib221A-pZ6brnibuwpzRLf1A3OM/)
