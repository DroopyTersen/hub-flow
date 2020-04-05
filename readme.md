# Hub Flow

Hub Flow is a pattern for managing state in React.js applications. It steals the great principles from Flux & Redux (one-way data flow), but leaves out the ceremonious complexity. Hub Flow comes with built in immutability, which means a huge performance boost and an end to accidental state mutation bugs.
Hub Flow is powered by Freezer.js

```
npm install --save hub-flow
```

## Getting Started

_hub.ts_

```typescript
import createHub, { Hub } from "hub-flow";
export * from "hub-flow";
import { registerActions: registerCounter } from "models/counter/counter.actions";
import { registerActions: registerUsers } as users from "models/users/users.actions";
import { User, Counter } from "models";

// Describe the Shape of your App State
export interface AppState {
  users: User[];
  counter: Counter;
}

// Setup a default App State
const DEFAULT_STATE: AppState = {
  users: [],
  counter: new Counter(),
};

// Create the HUB
let hub = createHub(DEFAULT_STATE);

// Register the actions
registerCounter(hub);
registerUsers(hub);

// Expose a few different ways to get the hub
export const getHub = () => hub;

declare global {
  interface Window {
    hub: Hub<AppState>;
  }
}
window.hub = hub;
export default hub;
```

_counter.models.ts_

```typescript
import hub from "global/hub";
import { Model } from "hub-flow";

export class Counter extends Model<Counter> {
  constructor() {
    super();
    this.listener = hub.state.counter.getListener();
  }
  get count() {
    return hub.state.counter.count;
  }
  increment() {
    hub.state.counter.set({ count: hub.state.counter.count + 1 });
  }
  decrement() {
    hub.state.counter.set({ count: hub.state.counter.count - 1 });
  }
  reset() {
    hub.state.counter.set({ count: 0 });
  }
}

// To be stored in the App State
export interface CounterState {
  count: number;
}
```

![Hub Flow](https://cdn-images-1.medium.com/max/1000/1*fQCprFj929rurkPYllpbUw.png)

[Overview](https://medium.com/@droopytersen/hub-flow-introduction-13260c90c893)

[Getting Started](https://medium.com/@droopytersen/hub-flow-getting-started-27b4168cbaa9)

[Modifying State](https://medium.com/@droopytersen/hub-flow-modifying-state-f0426e110c7d)

[Demo](https://stackblitz.com/edit/hub-flow-demo)
