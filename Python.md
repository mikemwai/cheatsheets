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
