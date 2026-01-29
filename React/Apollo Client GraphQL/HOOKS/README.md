
# Apollo Client Hooks (React)

These hooks are the main way React components talk to Apollo Client. They
connect your UI to the cache and the network.

## 1) `useQuery` (automatic fetching)
Runs immediately on mount and **subscribes to the cache**. If cached data
changes (even from another component), it re-renders.

**Returns:** `loading`, `error`, `data`, `refetch`, and more.

```jsx
import { useQuery, gql } from "@apollo/client";

const GET_DOGS = gql`
  query GetDogs {
    dogs {
      id
      breed
    }
  }
`;

function Dogs() {
  const { loading, error, data, refetch } = useQuery(GET_DOGS);

  if (loading) return <p>Loading...</p>;
  if (error) return <p>Error: {error.message}</p>;

  return (
    <div>
      {data.dogs.map((dog) => (
        <p key={dog.id}>{dog.breed}</p>
      ))}
      <button onClick={() => refetch()}>Refresh List</button>
    </div>
  );
}
```

## 2) `useMutation` (manual updates)
Does **not** run automatically. It returns a trigger function you call when
the user performs an action.

**Returns:** `[mutateFunction, stateObject]`  
**Common options:** `onCompleted`, `onError`, and `update` (for cache updates).

```jsx
import { useMutation, gql } from "@apollo/client";

const ADD_DOG = gql`
  mutation AddDog($breed: String!) {
    addDog(breed: $breed) {
      id
    }
  }
`;

function AddDogForm() {
  const [addDog, { loading, error }] = useMutation(ADD_DOG, {
    onCompleted: (data) => {
      console.log("Dog added!", data);
      alert("Success!");
    },
    onError: (err) => {
      console.error("Failed to add dog", err);
    },
  });

  return (
    <button onClick={() => addDog({ variables: { breed: "Bulldog" } })}>
      {loading ? "Adding..." : "Add Bulldog"}
    </button>
  );
}
```

## 3) `useLazyQuery` (manual fetching)
Behaves like `useQuery`, but waits for a manual trigger. Great for search
and drill‑down screens.

**Returns:** `[triggerFunction, resultObject]`

```jsx
import { useLazyQuery, gql } from "@apollo/client";

const GET_DOG_BY_BREED = gql`
  query GetDogByBreed($breed: String!) {
    dog(breed: $breed) {
      id
      name
    }
  }
`;

function DogSearch() {
  const [getDog, { loading, data }] = useLazyQuery(GET_DOG_BY_BREED);

  return (
    <div>
      <button onClick={() => getDog({ variables: { breed: "Poodle" } })}>
        Search Poodle
      </button>
      {loading && <p>Searching...</p>}
      {data && <p>Found: {data.dog.name}</p>}
    </div>
  );
}
```

## 4) `useSubscription` (real‑time data)
Keeps a live connection (usually WebSocket). When the server pushes data,
your component updates instantly.

**Use cases:** chat, live dashboards, stock tickers.

```jsx
import { useSubscription, gql } from "@apollo/client";

const COMMENTS_SUB = gql`
  subscription OnCommentAdded {
    commentAdded {
      id
      content
    }
  }
`;

function LiveComments() {
  const { data, loading } = useSubscription(COMMENTS_SUB);

  if (loading) return <p>Waiting...</p>;
  if (!data) return null;

  return <h4>Newest Comment: {data.commentAdded.content}</h4>;
}
```

## Quick comparison
- `useQuery` -> automatic fetch + cache subscription
- `useMutation` -> manual trigger for writes
- `useLazyQuery` -> manual trigger for reads
- `useSubscription` -> real-time push updates
