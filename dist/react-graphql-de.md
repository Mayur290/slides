# React & GraphQL

## Präsentationen

<https://marko-knoebl.github.io/slides/>

## Ihr Trainer

Marko Knöbl

- aus Wien
- ehemaliger Mathematiklehrer
- Programmierthemen:
  - JavaScript, TypeScript und React
  - Python, Data Science

## Vorstellung der Teilnehmer

- Aktuelle Projekte
- Vorkenntnisse
- Erwartungen / Wünsche

## Organisatorisches

- Kursdauer
- Pausen
- Mittagessen
- Unterlagen
- Fragen, Feedback? - Jederzeit erwünscht

## Code

Code verfügbar unter: <https://github.com/marko-knoebl/courses-code>

# GraphQL

## GraphQL

"A query language for your API"

## GraphQL

Verwendung für ein einzelnes APIs, das wiederum mit folgenden Datenquellen kommunizieren kann:

- andere APIs
- Datenbanken

## GraphQL vs REST

### Vorteile von REST

- _etabliert_
- _einfacher_

### Vorteile von GraphQL

- _flexibler_
- _effizienter_

## Vorteile von GraphQL

_Flexibilität_ von GraphQL:

kann komplexe Abfragen beschreiben - serverseitiges API muss für neue Anwendungsfälle nicht angepasst werden

_Effizienz_ von GraphQL:

der Client kann mittels eines Requests genau die benötigten Objekte und Felder anfordern

[Video: GraphQL is the better REST](https://www.howtographql.com/basics/1-graphql-is-the-better-rest/)

## Unterschiede zwischen GraphQL und REST

REST: Endpunkt (z.B. `/todos`) und Methode (z.B. `PUT`) sind Teil des APIs; für jede Art von Anfrage muss ein eigener Endpunkt erstellt werden

GraphQL: nur ein Endpunkt (z.B. `/api`), nur POST-Requests

## Unterschiede zwischen GraphQL und REST

REST: Für komplexere Fälle sind mehrere HTTP-Anfragen nötig

GraphQL: Daten werden mit einer einzigen HTTP-Anfrage abgefragt

## Unterschiede zwischen GraphQL und REST

REST: Für jeden Endpunkt werden immer die gleichen Felder zurückgegeben

GraphQL: Der Client kann genau die benötigten Objekte und Felder anfragen

## Anwendungsfälle

- API services: z.B. Generieren einer Zufallszahl zwischen 1 und 100
- Datenbankabfrage: z.B. Abfragen aller Login Namen von Freunden eines bestimmten Benutzers

## Beispiel: Service für Zufallszahlen

Anfrage (GraphQL):

```graphql
query {
  random(min: 1, max: 100)
}
```

Antwort (JSON):

```json
{
  "random": 23
}
```

## Beispiel: Freunde eines Benutzers

Anfrage:

```graphql
query {
  user(login: "john") {
    login
    friends {
      login
    }
  }
}
```

## Beispiel: Feunde eines Benutzers

Antwort:

```json
{
  "user": {
    "login": "john",
    "friends": [
      { "login": "mike" },
      { "login": "stephanie" }
    ]
  }
}
```

## Definieren und Abfragen eines GraphQL APIs

Abfragen:

- Query (ausgedrückt in GraphQL)
- evtl Abfrageparameter (in JSON)

Definieren:

- GraphQL Schema
- Resolver-Funktionen

## Beispiel Zufallszahlen

Beispiel zeigt Implementierung und Verwendung eines API für Zufallszahlen - mit Parametern für die Anzahl und den Höchstwert

## Beispiel Zufallszahlen: Definition

Schemadefinition:

```graphql
type Query {
  rand(max: Int!, quantity: Int!): [Int]!
}
```

Resolver-Funktion (abhängig von der verwendeten Library):

```js
(root, args, context) =>
  Array.from({ length: args.quantity }, () =>
    Math.floor(Math.random() * args.max)
  );
```

## Beispiel Zufallszahlen: Feste Abfrage

```graphql
query random {
  rand(max: 10, quantity: 3)
}
```

## Beispiel Zufallszahlen: Parametrische Abfrage

Abfrage:

```graphql
query random($max: Int!, $quantity: Int!) {
  rand(max: $max, quantity: $quantity)
}
```

Abfrageparameter (JSON):

```json
{
  "max": 10,
  "quantity": 3
}
```

## Ausprobieren

Vordefiniertes API mit Posts und Benutzern:

<https://api.graph.cool/simple/v1/cjmj9v4mk1zs00182rnrzdrai>

<!--
source of the predefined API:

howtographql.com - Core Concepts - last "Play" button

try Subscriptions
-->

## Resourcen

- <https://graphql.org/learn/>
- <https://www.howtographql.com/>

# GraphQL vs REST: Beispiel

## GraphQL vs REST

Szenario:

Social media App, in der wir eine Liste von Freunden anzeigen können. Ein Klick auf einen Freund zeigt dessen letzte Posts.

## API in REST

```http
GET /users/$myuserid/friends
```

```http
GET /users/$otheruserid/posts
```

## API in GraphQL

```graphql
{
  users(id: "myuserid") {
    friends {
      userid
      name
    }
  }
}
```

## API in GraphQL

```graphql
{
  users(id: "otheruserid") {
    posts {
      date
      title
      body
    }
  }
}
```

## Funktionalität hinzufügen: Feed neuer Posts

Anzeigen neuer Posts aller Freunde

## Funktionalität hinzufügen: Feed neuer Posts

Umsetzung in REST:

- Möglichkeit 1: mehrere Requests senden
- Möglichkeit 2: neuer Endpunkt in der API, z.B. `/postsoffriends/$userid`

## Funktionalität hinzufügen: Feed neuer Posts

Umsetzung in GraphQL:

```graphql
{
  users(id: "$myuserid") {
    friends {
      posts {
        date
        title
        body
      }
    }
  }
}
```

# GraphQL verglichen mit SQL

## GraphQL verglichen mit SQL

```graphql
query {
  user(login: "my-username") {
    login
    name
  }
}
```

```sql
SELECT login, name
  FROM user
  WHERE login='my-username';
```

## GraphQL verglichen mit SQL

GraphQL: Parameter haben keine vordefinierte Bedeutung

In SQL: `WHERE login='my-username'` hat klare Bedeutung

GraphQL: Bedeutung von `login: "my-username"` ist der Implementierung am Server überlassen

## GraphQL verglichen mit SQL

SQL: Beziehungen zwischen Tabellen (Joins) werden in der Query definiert

GraphQL: kennt die Beziehungen bereits → einfachere Queries

## GraphQL verglichen mit SQL

```graphql
query {
  user(login: "my-username") {
    posts {
      title
    }
  }
}
```

```sql
SELECT post.title
  FROM user
  LEFT JOIN post ON user.id = post.userId
  WHERE user.login = 'my-username';
```

(extra Code: `LEFT JOIN post ON user.id = post.userId`)

## GraphQL verglichen mit SQL

**OpenCRUD**: spezifischerer Standard, der auf GraphQL basiert - er kann anstelle von SQL verwendet werden

# Beispiel-APIs

## Beispiele für GraphQL-APIs

von <https://github.com/APIs-guru/graphql-apis>:

- GitHub (login benötigt)
- Reddit (GraphQL Hub)
- GraphQL Pokémon (zweiter Eintrag!)
- Star Wars
- SpaceX Land
- Todo list
- FakeQL: Selbstdefinierte Mock APIs

## FakeQL

<https://fakeql.com/>

Template für einfache Todos bei FakeQL:

```json
{
  "todos": [
    { "id": 1, "title": "Go shopping", "completed": true },
    { "id": 49, "title": "Do taxes", "completed": false },
    { "id": 50, "title": "Do dishes", "completed": false }
  ]
}
```

# GraphiQL Explorer

## GraphiQL Explorer

Graph<em>i</em>QL: browserbasierter Explorer für GraphQL APIs

- Abfragestruktur / Datenstruktur ansehen (_Docs_ oben rechts in der Ansicht)
- Abfragen senden

# Einfache Beispiele

## Einfache Beispiele

[Star Wars API](https://graphql.org/swapi-graphql/):

- Liste von Titeln aller Star Wars Filme
- Liste von Planeten und Einwohnerzahl aus Star Wars
- Liste von Schiffen, gruppiert nach Filmen, in denen sie vorkommen

## Übung: Liste von Titeln

```graphql
query getTitles {
  allFilms {
    films {
      title
    }
  }
}
```

## Übung: Liste von Planeten und Einwohnerzahlen

```graphql
query getPlanetsWithPopulations {
  allPlanets {
    planets {
      name
      population
    }
  }
}
```

## Übung: Liste von Schiffen gruppiert nach Filmen

```graphql
query getStarshipsByFilm {
  allFilms {
    films {
      title
      starshipConnection {
        starships {
          name
        }
      }
    }
  }
}
```

# Parametrische Abfragen

## Abfrageparameter

Beispiel unter <https://api.spacex.land/graphql/>:

```graphql
{
  launchesUpcoming(limit: 4) {
    launch_date_utc
    mission_name
  }
}
```

```graphql
{
  rocket(id: "falcon9") {
    cost_per_launch
    wikipedia
  }
}
```

## Abfrageparameter

Alle _Falcon 9_-Starts im Jahr 2019:

```graphql
{
  launchesPast(
    find: { launch_year: "2019", rocket_name: "Falcon 9" }
  ) {
    launch_date_utc
    launch_site {
      site_name
      site_name_long
    }
  }
}
```

Anmerkung: Die server-seitige Implementierung entscheidet über die unterstützten Parameter (z.B. _find_, _id_, _limit_, ...)

## Verpflichtende und optionale Parameter

Verpflichtende Parameter sind (üblicherweise) mit `!` gekennzeichnet - ebenso wie zurückgegebene Attribute, die immer vorhanden sind.

## Variablen

Abfrage:

```graphql
query getLaunchesByYear($year: String!) {
  launchesPast(find: { launch_year: $year }) {
    launch_date_utc
    launch_site {
      site_name
      site_name_long
    }
  }
}
```

Variablen:

```json
{
  "year": "2019"
}
```

# Mutationen

## Mutationen

<https://todo-mongo-graphql-server.herokuapp.com/>

(nur Definition _einzelner_ Queries möglich)

## Mutationen

Befehl, der die `add`-Aktion am Server triggert und die id des neuen Todos zurückliefert:

```graphql
mutation addTodo($title: String!) {
  add(title: $title) {
    id
  }
}
```

```json
{
  "title": "shopping"
}
```

## Mutationen

```graphql
mutation toggleTodo($id: String!) {
  toggle(id: $id) {
    id
    completed
  }
}
```

## Mutationen

```graphql
mutation addOneAndClearCompleted($title: String!) {
  add(title: $title) {
    id
  }
  clearCompleted {
    id
  }
}
```

## Mutationen

Aufgabe: Schreibe eine Query, die alle bisherigen Einträge löscht und zwei neue erstellt

## Mutationen

```graphql
mutation reset {
  toggleAll(checked: true) {
    id
  }
  clearCompleted {
    id
  }
  add(title: "get some rest") {
    id
  }
}
```

# Übungen (GitHub)

## Übungen

- Frage alle "follower von followern" für einen bestimmten GitHub-Account ab
- Frage für einen bestimmten GitHub-Account alle Projekte mit Namen und Sternenanzahl ab

## Übungen - Lösungen

```
query {
  user (login: "marko-knoebl") {
    followers (first: 10) {
      nodes {
        login,
        followers (first: 10) {
          nodes {
            login
          }
        }
      }
    }
  }
}
```

## Übungen - Lösungen

```graphql
query {
  user(login: "marko-knoebl") {
    id
    email
    repositories(
      first: 100
      orderBy: { field: STARGAZERS, direction: DESC }
    ) {
      nodes {
        name
        stargazers {
          totalCount
        }
      }
    }
  }
}
```

# Datentypen

## Datentypen

Bei GraphQL sind die zurückgegebenen Datentypen immer bekannt.

## Datentypen

verfügbare Typen:

- Boolean
- Int: 32-bit int (signed)
- Float: 64-bit Gleitkommazahl
- String: UTF-8 Zeichenkette
- ID: Eindeutige ID als String
- Object: Objekt mit vordefinierten Einträgen
- List: Liste, die bestimmte andere Typen beinhaltet

# GraphQL Clients

## GraphQL Clients

- _graphql.js_: Referenzimplementierung
- _Apollo Client_: einfache Integration mit React sowie mit Vue, Angular, ...
- _Relay_: fortgeschrittene Integration mit React

# GraphQL mit reinem JavaScript

## Senden von Queries an den Server

Queries werden mittesl POST-Requests gesendet

Payload ist ein JSON-Objekt mit einer `query` string property (auch bei Mutationen) und optional einer `variables` property.

## Senden von Queries an den Server

Testen aus der Browserkonsole (wir müssen uns auf der gleichen Seite befinden):

```js
const requestBody = {
  query:
    'mutation addTodo($title: String!) { add(title: $title) { id } }',
  variables: '{"title": "test"}',
};

const requestBodyStr = JSON.stringify(requestBody);

fetch('https://todo-mongo-graphql-server.herokuapp.com', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json' },
  body: requestBodyStr,
}).then(console.log);
```

## Beispiel: reddit API

```js
const queryTemplate = `
{
  reddit {
    subreddit(name: "javascript") {
      newListings(limit: 2) {
        title
      }
    }
  }
}`;
```

## Beispiel: reddit API

```js
fetch('https://www.graphqlhub.com/graphql', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    Accept: 'application/json',
  },
  body: JSON.stringify({ query: queryTemplate }),
})
  .then(r => r.json())
  .then(data => console.log('data returned:', data));
```

# Apollo Client

## Apollo Client

<https://www.apollographql.com/docs/react/>

## Apollo Client

Gründe für die Verwendung:

- Automatisches Senden von Queries über das Netzwerk
- Automatisches Caching
- Automatische Einbindung in das (Re)rendering von React

## Installation

Benötigte npm-Pakete:

- `graphql`
- `graphql-tag`
- `apollo-client`
- `apollo-cache-inmemory`
- `apollo-link-http`
- `react-apollo` (für Verwendung mit React)

## Setup

```js
import { ApolloClient } from 'apollo-client';
import { InMemoryCache } from 'apollo-cache-inmemory';
import { HttpLink } from 'apollo-link-http';
import gql from 'graphql-tag';

const client = new ApolloClient({
  cache: new InMemoryCache(),
  link: new HttpLink({
    uri: 'https://api.spacex.land/graphql/',
  }),
});
```

## Beispiel für eine Abfrage

```js
// via a tagged template string
const LAUNCHES_QUERY = gql`
  query recentLaunches {
    launchesPast(limit: 10) {
      mission_name
    }
  }
`;

client
  .query({ query: LAUNCHES_QUERY })
  .then(result => console.log(result));
```

## Lokale Daten

Apollo kann auch lokale Daten / lokalen State verwalten

Setzen von lokalem State:

- via `client.writeData` für einfache Fälle
- mittels `@client`-Direktive in Mutationen, und lokalen Resolvern

Auslesen von lokalem State:

- mittels `@client`-Direktive in Queries

## Lokale Daten

Einfaches direktes Setzen von lokalem State (ähnlich wie Reacts `setState`):

```js
const client = useApolloClient();

client.writeData({ data: { inputText: '' } });
```

## Lokale Daten

lokale Resolver für Mutationen:

<https://www.apollographql.com/docs/react/data/local-state/#local-resolvers>

## Lokale Daten

Auslesen von lokalem State (via `@client`):

```js
const INPUT_TEXT_QUERY = gql`
  query {
    inputText @client
  }
`;

client
  .query({ query: INPUT_TEXT_QUERY })
  .then(result => console.log(result));
```

## Apollo Client Developer Tools

Erweiterung für Chrome

Laut Bewertungen unzuverlässig (3.2 / 5 Sternen)

Funktionen:

- Betrachten des aktuellen Caches
- Inspizieren der Struktur von Queries / Mutationen
- Ausführen von Queries (und Mutationen)

# Apollo Client mit React

## Apollo Client mit React

<https://www.apollographql.com/docs/react/data/queries/>

## React mit einem Apollo Client verbinden

Eine Anwendung kommuniziert meist mit einem einzigen API

```js
import { ApolloProvider } from 'react-apollo';
```

```jsx
<ApolloProvider client={client}>
  <App />
</ApolloProvider>
```

## Definition einer Query

```js
const LAUNCHES_QUERY = gql`
  query recentLaunches {
    launchesPast(limit: 10) {
      mission_name
    }
  }
`;
```

## useQuery

```js
function RecentLaunches() {
  const { data, loading, error } = useQuery(LAUNCHES_QUERY);
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error!</div>;

  return (
    <div>
      <h1>Launches</h1>
      {data.launchesPast.map(launch => (
        <div>{launch.mission_name}</div>
      ))}
    </div>
  );
}
```

## useQuery: Parameter

```js
const LAUNCHES_QUERY = gql`
  query recentLaunches($numLaunches: Int!) {
    launchesPast(limit: $numLaunches) {
      mission_name
    }
  }
`;

function RecentLaunches({ numLaunches }) {
  const { data, loading, error } = useQuery(
    LAUNCHES_QUERY,
    { variables: { numLaunches } }
  );
  ...
}
```

## useQuery: Polling & Refetching

Daten alle 5 Sekunden aktualisieren:

```js
const { data, loading, error } = useQuery(LAUNCHES_QUERY, {
  pollInterval: 5000,
});
```

Funktion, deren Aufruf ein neues Laden der Daten bewirkt:

```js
const { data, loading, error, refetch } = useQuery(
  LAUNCHES_QUERY
);
...
refetch()
```

## useMutation

Beispielfall Todo:

```js
const SET_COMPLETED = gql`
  mutation setCompleted($id: ID!, $completed: Boolean!) {
    updateTodo(id: $id, input: { completed: $completed }) {
      id
      completed
    }
  }
`;
```

## useMutation

Gundlegende Verwendung:

```jsx
const [setCompleted] = useMutation(SET_COMPLETED);
```

Ausführliche Form (vgl. `useState`):

```jsx
const [
  setCompleted,
  { data, loading, error },
] = useMutation(SET_COMPLETED);
```

Der State wird am Server und danach auch lokal entsprechend abgeändert

## useMutation

Update des lokalen Caches:

- **automatisch**, falls ein zuvor existierendes Objekt geändert wurde
- **manuell**, falls Objekte in einem Array hinzugefügt / entfernt wurden

## useMutation: manuelles Update des Caches

Zugriff auf cache und API-Antwort in der `update`-Funktion:

```js
const [addTodo] = useMutation(ADD_TODO, {
  update: (cache, reply) => {
    // cache: local cache
    // reply: reply from the API
    console.log(cache);
    console.log(reply);
    // TODO: update the local cache based on the reply
  },
});
```

## useMutation: manuelles Update des Caches

```js
const [addTodo] = useMutation(ADD_TODO, {
  update: (cache, reply) => {
    // get old todos from cache
    const oldTodos = cache.readQuery({ query: GET_TODOS })
      .todos;
    // build newTodos array based on the server response
    const newTodos = [...oldTodos, reply.data.createTodo];
    // TODO: update the local cache with the newTodos array
  },
});
```

## useMutation: manuelles Update des Caches

```js
const [addTodo] = useMutation(ADD_TODO, {
  update: (cache, reply) => {
    const oldTodos = cache.readQuery({ query: GET_TODOS })
      .todos;
    const newTodos = [...oldTodos, reply.data.createTodo];
    cache.writeQuery({
      query: GET_TODOS,
      data: { todos: newTodos },
    });
  },
});
```

# Queries - Fortgeschritten

## Standardwerte für Variablen

```graphql
query getPokemonByName($name: String = "Pikachu") {
  pokemon(name: $name) {
    number
    image
  }
}
```

## Aliases

Aufgabe: Nummer von Pikachu und Raichu (Pokémon API)

## Aliases

Die bekannte Art klappt nicht:

```graphql
query getTwo {
  pokemon(name: "Pikachu") {
    number
  }
  pokemon(name: "Raichu") {
    number
  }
}
```

## Aliases

Die Antwort hätte die folgende Struktur:

```json
{
  "data": {
    "pokemon": {
      "number": "025"
    },
    "pokemon": {
      "number": "026"
    }
  }
}
```

Der Key `pokemon` wäre doppelt.

## Aliases

Ausweg aus dem Problem:

```graphql
query getTwo {
  pokemon1: pokemon(name: "Pikachu") {
    number
  }
  pokemon2: pokemon(name: "Raichu") {
    number
  }
}
```

## Aliases

Antwort:

```json
{
  "data": {
    "pokemon1": {
      "number": "025"
    },
    "pokemon2": {
      "number": "026"
    }
  }
}
```

## Fragmente

Fragmente bieten "Vorlagen" für Queries - weniger Wiederholung

## Fragmente

```graphql
query getTwo {
  pokemon1: pokemon(name: "Pikachu") {
    ...essentialData
  }
  pokemon2: pokemon(name: "Raichu") {
    ...essentialData
    id
  }
}

fragment essentialData on Pokemon {
  number
  maxHP
  image
}
```
