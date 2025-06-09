## LEVEL 0 - 1
**Goal:** SSH into the server  
**Command:**  
```bash
ssh bandit0@bandit.labs.overthewire.org -p 2220
```

## LEVEL 1 - 2
**Goal:** open a file '-'  
**Command:**  
```bash
cat ./-
```

## LEVEL 2 - 3
**Goal:** open a file with spaces  
**Command:**  
```bash
cat 'file with spaces'
```
or
```bash
cat file\ with\ spaces
```

## LEVEL 3 - 4
**Goal:** open a hidden file   
**Command:**  
```bash
ls -a 
```
(shows a list of all files. hidden ones have a '.' before their name ex: .filehidden)
```bash
cat .filehidden
```
## LEVEL 4 - 5
**Goal:** open the human-readable file among many nr
**Command:**  
```bash
find . -type f -readable
```
## LEVEL 5 - 6
**Goal:** open the file with the properties:
human-readable
1033 bytes in size
not executable
**Command:**  
```bash
find . -type f -size 1033c ! -executable -readable
```
