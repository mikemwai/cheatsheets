# Linux Cheatsheet

## Files and Folders

- Creating a new file:

```sh
  touch filename
```

- Creating a new directory:

```sh
  mkdir directoryname
```

- Viewing files and folders in a directory:

```sh
  ls
```

- Viewing files and folders in a directory with detailed information:

```sh
  ls -l
```

- Deleting directories with files in it:

```sh
  rm -rf directoryname
```

---- 

## Compilers

```sh
  df -h
```

```sh
  ls -lrt
```

- Compiler code for `main.c`:

```sh
  #include <stdio.h>

  int main(){
    print("Hello, World!\n");
    return 0;
  }
```

- Instructions to run:

```sh
 gcc -E main.c -o main_preprocessed.c
```

```sh
 gcc -S main_preprocessed.c -o main_assembly.s
```

```sh
 gcc -c main_assembly.s -o main_object.o
```

```sh
 gcc main_object.o -o my_program
```

```sh
 gcc main_object.o -o my_program
```

```sh
 ./my_program
```
