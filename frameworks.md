# Frameworks

## JavaScript Frameworks
### React
- Create a new react app:

```sh
  npx create-react-app projectname
```

- Run a react app:

```sh
  npm run
```

- Import react in a react file in VS Code: `imr`
- Create basic code structure in a react file in VS Code: `rfce`
- Create basic code structure in a typescript file in VS Code: `rafce`

### Next
- Create a new next app:

```sh
  npx create-next-app projectname
```

- Run a next app:

```sh
  npm run dev
```

### Vite
- Create a new Vite & React project using npm's create-vite tool:
```sh
  npm create vite@latest . -- --template react # Creates in the current folder
  npm create vite@latest folder_name -- --template react
```

- Install the project dependencies:
```sh
  npm install
```

*`N\B:` Select `Eslint` as the safer default.*

- Update the `src/App.jsx`:
```sh
  import { useEffect, useState } from "react";

  const API_URL = import.meta.env.VITE_API_URL || "http://localhost:8000";
  
  function App() {
    const [orders, setOrders] = useState([]);
  
    useEffect(() => {
      fetch(`${API_URL}/api/orders`)
        .then((res) => res.json())
        .then(setOrders);
    }, []);
  
    return (
      <div>
        <h1>Orders</h1>
        <ul>
          {orders.map((o) => (
            <li key={o.id}>{o.item} — ${o.price}</li>
          ))}
        </ul>
      </div>
    );
  }
  
  export default App;
```

- Run the server using:
```sh
  npm run dev
```

## Python Frameworks
### FastAPI
- Create the virtual environment.
- Install `fastapi` and `uvicorn`:
```sh
  pip install fastapi uvicorn
```

- Create the `main.py` file with a sample code:
```sh
  from fastapi import FastAPI
  from fastapi.middleware.cors import CORSMiddleware
  
  app = FastAPI()
  
  # Allow the Vite dev server to call this API
  app.add_middleware(
      CORSMiddleware,
      allow_origins=["http://localhost:5173"],
      allow_methods=["*"],
      allow_headers=["*"],
  )
  
  # Fake "database"
  orders = [
      {"id": 1, "item": "Keyboard", "price": 49.99},
      {"id": 2, "item": "Monitor", "price": 199.99},
  ]
  
  @app.get("/api/orders")
  def get_orders():
      return orders
  
  @app.post("/api/orders")
  def add_order(order: dict):
      order["id"] = len(orders) + 1
      orders.append(order)
      return order
```

- Run the server using:
```sh
  uvicorn main:app --reload --port 8000
```


