# OverTheWire: Bandit Notes

This repo contains my notes as I solve the OverTheWire Bandit wargame, a CTF challenge designed to teach Linux fundamentals, terminal commands, file permissions, and basic networking.

🔗 [Bandit wargame link](https://overthewire.org/wargames/bandit/)

## Tools & Concepts Covered
- Linux shell commands (`ls`, `cd`, `cat`, `find`, etc.)
- File permissions and ownership
- Data encoding/decoding
- Network basics using `ssh` and `nc`

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
## LEVEL 6 - 7
**Goal:** The password for the next level is stored somewhere on the server and has all of the following properties:

owned by user bandit7
owned by group bandit6
33 bytes in size

**Command:**  
```bash
bandit6@bandit:/$ find . -type f -user bandit7 -group bandit6 -size 33c
2>/dev/null
```
2>/dev/null --> throws 'permission denied' files into a blackhole

## LEVEL 7 - 8
**Goal:** The password for the next level is stored in the file data.txt next to the word millionth

**Command:**  
```bash
bandit7@bandit:~$ grep millionth data.txt
```
## LEVEL 8 - 9
**Goal:** The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

**Command:**  
```bash
bandit8@bandit:~$ sort data.txt | uniq -u
```
Count Duplicates
- ```uniq -c file.txt```  
  Prefixes each line with the number of occurrences.

Show Only Repeated Lines
- ```uniq -d file.txt```  
  Displays only duplicate lines, showing each duplicate once.

Show Only Unique Lines
- ```uniq -u file.txt```
  Displays lines that are not repeated.

Ignore Case
- ```uniq -i file.txt```
  Treats lines as equal regardless of case.

Skip Fields
- ```uniq --skip-fields=1 file.txt```  
  Ignores the first field (space-separated) when comparing lines.

Skip Characters
- ```uniq --skip-chars=3 file.txt```  
  Ignores the first 3 characters of each line when comparing.

## LEVEL 9 - 10
**Goal:** The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

**Command:**  
```bash
bandit9@bandit:~$ strings data.txt | grep '==='
```

## LEVEL 10 - 11
**Goal:** The password for the next level is stored in the file data.txt, which contains base64 encoded data

**Command:**  
```bash
bandit10@bandit:~$ base64 -d data.txt
```

## LEVEL 11 - 12
**Goal:** The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

**Command:**  
```bash
bandit11@bandit:~$ cat data.txt|tr 'A-Za-z' 'N-ZA-Mn-za-m'
```

## LEVEL 12 - 13
**Goal:** The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work. Use mkdir with a hard to guess directory name. Or better, use the command “mktemp -d”. Then copy the datafile using cp, and rename it using mv (read the manpages!)

**Command:**  
```bash
# 1. Identify file type
file copy.txt

# 2. Rename gzip file and decompress
mv copy.txt copy.gz
gzip -d copy.gz

# 3. Check resulting file type and extract hex dump
file copy
xxd -r copy > copy.bin
file copy.bin

# 4. Rename and decompress bzip2 file
mv copy.bin copy.bz2
bzip2 -d copy.bz2

# 5. Check resulting file type and decompress gzip again
file copy
mv copy copy.gz
gzip -d copy.gz

# 6. Check for tar archive and extract
file copy
mv copy copy.tar
tar -xf copy.tar

# 7. Check and process the next file in the chain
file data5.bin
mv data5.bin data5.tar
tar -xf data5.tar

file data6.bin
mv data6.bin data6.bz2
bzip2 -d data6.bz2

file data6
mv data6 data6.tar
tar -xf data6.tar

file data8.bin
mv data8.bin data8.gz
gzip -d data8.gz

# 8. Final step – password revealed
file data8
cat data8
```
