## Python
- View the Python version:
```sh
  python --version
```

- Create a virtual environment:
```sh
  py -m venv .venv
```

- Activate the virtual environment:
```sh
  .venv\Scripts\Activate.ps1

  Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass # Run this if activation is blocked
```

- Update pip:
```sh
  py -m pip install --upgrade pip
```

- Install Jupyter:
```sh
  pip install notebook ipykernel
```

- Start Jupyter:
```sh
  jupyter notebook
```

- Install CHromaDB:
```sh
  pip install chromadb
```

- Install langchain and langgraph:
```sh
  pip install -U langchain langgraph
```

- Check if multiple python versions are installed in the machine:
```sh
  py -0
```

- Install the python version you want:
```sh
  py install 3.12
```

- Recreate the virtual environment:
```sh
  py -3.12 -m venv .venv
```

# Programming Terms
`1) Class`
- Refers to a blueprint for an object/ instance.
```sh
  class Calculator:
    def __init__(self, name):
        self.name = name
    
    def add(self, a, b): # Method
        return a + b
    
    def display_result(self, result):  # Method 2
        print(f"{self.name} says: {result}")
```

`2) Object`
- Refers to the actual thing from the blueprint (Instance of a class).
```sh
  calc = Calculator("My Calculator")
```

`3) Method`
- Refers to the function inside of a class. It is called using the object name:
```sh
  calc.add(5, 3)
  calc.display_result(result)
```

`4) Function`
- Refers to a standalone block of code performing a certain task/ action (Outside of the Class). 
```sh
  def add(a, b):
    return a + b

  result = add(5, 3)
```
