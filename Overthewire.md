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

- The password for the next level is stored in the file data.txt and is the only line of text that occurs only once:
```sh
  sort data.txt | uniq -u 
```

- The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters:
```sh
  strings data.txt | grep "=="
```

- The password for the next level is stored in the file data.txt, which contains base64 encoded data:
```sh
  base64 -d data.txt # The -d flag decodes the data
```

- The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions:
```sh
  cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

- The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!):
```sh
  ls
  cd /tmp
  mkdir directory_name or mktemp -d # Creates a secure temporary folder
  cd directory_name; cp ~/data.txt .
  mv data.txt data
  xxd -r data > binary # Convert from data form to another in a reverse format
  file binary # Confirm the data type stored in the file
  mv binary binary.gz
  gunzip binary.gz
  ls
  file binary
  bunzip2 binary
  ls
  file binary.out
  mv binary.out binary.gz
  gunzip binary.gz
  ls
  file binary
  tar -xvf binary
  rm binary data
  file data5.bin
  tar -xvf data5.bin
  ls
  rm data5.bin
  bunzip2 data6.bin
  ls
  file data6.bin.out
  tar -xvf data6.bin.out
  ls
  rm data6.bin.out
  file data8.bin
  mv data8.bin data8.gz
  gunzip data8.gz
  ls
  file data8
  cat data8
```



