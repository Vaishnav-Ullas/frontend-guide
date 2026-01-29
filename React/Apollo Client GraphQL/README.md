
# Apollo Client + GraphQL (Quick Guide)

Apollo Client is a GraphQL client library for React (and other frameworks).
It has two main engines:
- **Network layer** (links) to talk to your GraphQL server
- **Normalized cache** (InMemoryCache) to store and reuse data

## 1) Apollo Client initialization

```js
import { ApolloClient, InMemoryCache } from "@apollo/client";

const client = new ApolloClient({
  // Network layer (HTTP)
  uri: "https://your-api.com/graphql",

  // Data store (normalized cache)
  cache: new InMemoryCache(),

  // Optional:
  // connectToDevTools: true,
  // ssrMode: false,
});
```

**`uri`** is shorthand for an `HttpLink` that sends the actual HTTP request.  
**`cache`** is a normalized key‑value store, not a raw JSON blob.

## 2) How Apollo Client works under the hood

### A) Request lifecycle (Link chain)
Apollo uses a middleware‑style **Link Chain**.

1. A hook like `useQuery` creates an **Operation** (query + variables).
2. The operation flows through your links (auth, error handling, etc.).
3. A terminating link (usually `HttpLink`) sends the real HTTP request.
4. The response flows back up the chain to Apollo Client.

### B) Cache lifecycle (Normalization)

When data returns:
1. Apollo reads `__typename` and `id` (or `_id`).
2. It stores each object as a unique key (e.g., `User:1`).
3. If `User:1` already exists, fields are merged and deduped.
4. Active queries watching those objects are notified and re‑render.

That is why updating a user in one place can instantly update other screens.

## 3) How `ApolloProvider` works

`ApolloProvider` uses React Context to make the client available everywhere:

```jsx
<ApolloProvider client={client}>
  <App />
</ApolloProvider>
```

Internally, it does:
- `<ApolloContext.Provider value={client}>`
- Hooks like `useQuery` call `useContext(ApolloContext)`

**Why it matters:** if you call `useQuery` outside a provider, it fails because
no client exists in context.


## 4) Anatomy of a Link
A Link is a function that receives:
- `operation` (query, variables, context)
- `forward` (passes the operation to the next link)

```js
import { ApolloLink } from "@apollo/client";

const myCustomLink = new ApolloLink((operation, forward) => {
  // 1) Modify request (downstream)
  console.log(`Starting request: ${operation.operationName}`);

  // Forward to the next link
  return forward(operation).map((response) => {
    // 2) Modify response (upstream)
    console.log(`Finished request: ${operation.operationName}`);
    return response;
  });
});
```

### A) Building a custom auth link

#### Method 1: Helper way (setContext)
Best for async tokens and production code.

```js
import { setContext } from "@apollo/client/link/context";

const authLink = setContext((_, { headers }) => {
  const token = localStorage.getItem("jwt_token");

  return {
    headers: {
      ...headers,
      authorization: token ? `Bearer ${token}` : "",
    },
  };
});
```

#### Method 2: Raw ApolloLink
Useful for deep understanding of how context flows to `HttpLink`.

```js
import { ApolloLink } from "@apollo/client";

const authLink = new ApolloLink((operation, forward) => {
  const token = localStorage.getItem("jwt_token");

  operation.setContext(({ headers = {} }) => ({
    headers: {
      ...headers,
      authorization: token ? `Bearer ${token}` : "",
    },
  }));

  return forward(operation);
});
```

### B) Chaining links together
Use `ApolloLink.from([])` and keep the terminating link last.

```js
import { ApolloClient, InMemoryCache, HttpLink, ApolloLink } from "@apollo/client";
import { onError } from "@apollo/client/link/error";

const httpLink = new HttpLink({ uri: "https://my-api.com/graphql" });

const errorLink = onError(({ graphQLErrors, networkError }) => {
  if (graphQLErrors) console.log("GraphQL Error", graphQLErrors);
  if (networkError) console.log("Network Error", networkError);
});

const linkChain = ApolloLink.from([
  errorLink,
  authLink,
  httpLink, // terminating link must be last
]);

const client = new ApolloClient({
  link: linkChain,
  cache: new InMemoryCache(),
});
```
## GraphQL vs. REST: Why use Apollo Client and GraphQL?

Choosing between REST APIs and GraphQL (with Apollo Client) is a major architectural decision. Here is how GraphQL addresses key data-loading inefficiencies in REST, plus the pros and cons of using Apollo Client.

### A) Over-fetching (too much data)
**Problem (REST):** endpoints return fixed data structures.  
**Scenario:** you need only user names.

- REST request: `GET /users`
- REST response: `id`, `name`, `email`, `address`, `phone`, `dob`, etc.
- Issue: you download 50 fields to use 1

**Solution (GraphQL):** request only what you need.

```graphql
query {
  users {
    name
  }
}
```

### B) Under-fetching (not enough data) and the N+1 problem
**Problem (REST):** related data often requires multiple round trips.  
**Scenario:** show posts with each author’s name.

- Step 1: `GET /posts` (returns `author_id`)
- Step 2: `GET /users/1`, `GET /users/2`, ... `GET /users/10`
- Issue: 1 + N requests causes latency and waterfalls

**Solution (GraphQL):** nested queries fetch related data in one request.

```graphql
query {
  posts {
    title
    author {
      name
    }
  }
}
```

## Pros of Apollo Client + GraphQL
- **Precise data fetching** (no over/under-fetching)
- **Single request for related data** (reduces round trips)
- **Normalized cache** (fast UI updates and deduping)
- **Declarative UI** (components declare their data needs)
- **Powerful tooling** (devtools, cache inspection, schema awareness)

## Cons / trade-offs
- **Learning curve** (schema, caching, normalized data)
- **More setup** (links, cache policies, error handling)
- **Complexity on the server** (resolvers, authorization per field)
- **Potential for expensive queries** if not limited
- **Caching is non-trivial** compared to simple REST responses