# Bandits

- Look for a file has all of the following properties: human-readable, 1033 bytes in size, not executable:
```sh
  find . -type f -size 1033c ! -executable
  find . -type f -size 1033c ! -executable -exec file {} + | grep ASCII # If result brings multiple files
```

- Look for a file somewhere on the server that has all of the following properties: owned by user bandit7, owned by group bandit6, 33 bytes in size:
```sh
  find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
```
