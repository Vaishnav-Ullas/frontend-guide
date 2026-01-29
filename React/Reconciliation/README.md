# Reconciliation

## What is Reconciliation?
The actual process of syncing the **Virtual DOM** with the **Real DOM** is called **Reconciliation**.

Comparing two full trees is mathematically expensive. To keep React fast ($O(n)$ speed), it makes two "heuristic" assumptions:

### 1. Elements of Different Types
If React sees that a tag has changed (e.g., going from a `<div>` to a `<span>`, or from a `<Navbar>` to a `<Header>`), it doesn't try to find similarities. It assumes the entire sub-tree is different.

**The Action**: React tears down the old tree, destroying all state in those components, and mounts the new one.

### 2. The "Key" Prop for Lists
This is the most famous part of reconciliation. When React looks at a list of children, it just iterates over both lists at the same time and generates a mutation whenever there’s a difference.

- **The Problem**: If you insert an item at the beginning of a list, React compares the first items, sees they are different, and might re-render every single item below it because it thinks everything shifted.
- **The Solution**: **Keys**. By giving each item a key, React uses that ID to "match" children across renders.

**Reconciliation Logic**: "I see a new item at the top, but I recognize keys A, B, and C from the last render. I'll just move them down and insert the new one."

## The Lifecycle of Reconciliation: "Render" vs. "Commit"
Reconciliation happens in two distinct phases. Understanding this is key to debugging performance.

### Phase 1: Render (The Calculation)
React calls your functions (components) to figure out what the UI should look like.
- This phase is **pure and fast**.
- React can **pause, restart, or abandon** this phase if more important updates come in.
- This is where the Virtual DOM is "Diffed."

### Phase 2: Commit (The Application)
Once React has the "List of Changes," it enters the Commit phase.
- React applies these changes to the Real DOM using browser APIs like `appendChild()` or `setAttribute()`.
- This phase is **fast and uninterruptible** to ensure the user doesn't see a partially updated screen.

## Fibers: The Modern Reconciler
Since React 16, the engine behind reconciliation is called **React Fiber**.

In the old days (**Stack Reconciler**), reconciliation was like a heavy train—once it started, it couldn't stop until it reached the end of the tree. This caused "jank" in animations.

Fiber changed this by breaking reconciliation into tiny chunks of work. It can:
- **Pause work** and come back to it later.
- **Assign priority** to different types of updates (e.g., a button click is high priority; a background data fetch is low priority).