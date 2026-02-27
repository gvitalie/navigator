Here is pypkgs.db if You download it.  
You can also create it, with updated information from pypi.org.  
See the next how to.

```
me@amadeus:~$ cd Tutor/assets/
me@amadeus:~/Tutor/assets$ sqlite3 pypkgs.db 
SQLite version 3.51.2 2026-01-09 17:27:48
Enter ".help" for usage hints.
sqlite> .tables
pypkgs
sqlite> .schema pypkgs
CREATE TABLE pypkgs (id integer primary key, pypkg text);
sqlite> .quit
me@amadeus:~/Tutor/assets$
```

> Hints how to create an updated pypkgs.db database.

```bash
me@amadeus:~/Tutor/Om$ python3 -q
>>> import requests
>>> 
>>> request = requests.get('https://pypi.org/simple')
>>> request.status_code
200
>>> with open('pypkgs.html', 'w') as file:
...     file.write(request.text)
... 
38841504
>>> 
me@amadeus:~/Tutor/Om$ ll 
total 37944
drwxrwxr-x  2 me me     4096 Feb 27 09:51 ./
drwxrwxr-x 15 me me     4096 Feb 27 09:45 ../
-rw-rw-r--  1 me me 38841504 Feb 27 09:51 pypkgs.html
me@amadeus:~/Tutor/Om$ head -n 10 pypkgs.html 
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta name="pypi:repository-version" content="1.4">
    <title>Simple index</title>
  </head>
  <body>
<a href="/simple/0/">0</a>
<a href="/simple/0-0/">0-._.-._.-._.-._.-._.-._.-0</a>
<a href="/simple/000/">000</a>
me@amadeus:~/Tutor/Om$ tail -n 10 pypkgs.html 
<a href="/simple/zzz-web/">zzz-web</a>
<a href="/simple/zzzymobbe/">zzzymobbe</a>
<a href="/simple/zzzz/">zzzz</a>
<a href="/simple/zzzz-mylib-23-4/">zzzz-mylib-23-4</a>
<a href="/simple/zzzzztest/">zzzzztest</a>
<a href="/simple/zzzzzzzzz/">zzzZZZzzz</a>
<a href="/simple/zzzzzzzzzz-the-end-of-pip/">zzzzzzzzzz-the-end-of-pip</a>
<a href="/simple/zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz...</a>
</body>
me@amadeus:~/Tutor/Om$
```
 > Extracting just projects names and saving to file, pykgs.txt.
```bash
me@amadeus:~/Tutor/Om$ grep -o 'href="/simple/[^/]*' pypkgs.html | sed 's|href="/simple/||' | tee pypkgs.txt
```
> Checking content of pypkgs.txt
```bash
me@amadeus:~/Tutor/Om$ head -n 10 pypkgs.txt 
0
0-0
000
0-0-1
00101s
001-hello-uv
00-merlin-hu-mcpdemo-pipy
00print-lol
00smalinux
0101
me@amadeus:~/Tutor/Om$ tail -n 10 pypkgs.txt 
zzzpypitest2
zzzutils
zzz-web
zzzymobbe
zzzz
zzzz-mylib-23-4
zzzzztest
zzzzzzzzz
zzzzzzzzzz-the-end-of-pip
zzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzzz...
me@amadeus:~/Tutor/Om$
```
> Creating pypkgs.db. Creating table pypkgs.
```bash
me@amadeus:~/Tutor/Om$ sqlite3 pypkgs.db
SQLite version 3.51.2 2026-01-09 17:27:48
Enter ".help" for usage hints.
sqlite> create table if not exists pypkgs (id integer primary key, pypkg text);
sqlite> 
sqlite> .tables
pypkgs
sqlite> .schema pypkgs
CREATE TABLE pypkgs (id integer primary key, pypkg text);
sqlite> 
sqlite> .quit
me@amadeus:~/Tutor/Om$ ll pypkgs.db 
-rw-r--r-- 1 me me 8192 Feb 27 10:04 pypkgs.db
me@amadeus:~/Tutor/Om$ ll
total 48500
drwxrwxr-x  2 me me     4096 Feb 27 10:04 ./
drwxrwxr-x 15 me me     4096 Feb 27 09:45 ../
-rw-r--r--  1 me me     8192 Feb 27 10:04 pypkgs.db
-rw-rw-r--  1 me me 38841504 Feb 27 09:51 pypkgs.html
-rw-rw-r--  1 me me 10798867 Feb 27 09:56 pypkgs.txt
me@amadeus:~/Tutor/Om$
```
> To do, show how to insert projects name from file pypkgs.txt to new created database pypkgs.db table pypkgs.
```python
me@amadeus:~$ cd Tutor/Om/
me@amadeus:~/Tutor/Om$ ll
total 48500
drwxrwxr-x  2 me me     4096 Feb 27 10:04 ./
drwxrwxr-x 15 me me     4096 Feb 27 09:45 ../
-rw-r--r--  1 me me     8192 Feb 27 10:04 pypkgs.db
-rw-rw-r--  1 me me 38841504 Feb 27 09:51 pypkgs.html
-rw-rw-r--  1 me me 10798867 Feb 27 09:56 pypkgs.txt
me@amadeus:~/Tutor/Om$ python3 -q
>>> import sqlite3
>>> 
>>> with sqlite3.connect('pypkgs.db') as db:
...     cursor = db.cursor()
...     query = 'insert into pypkgs (pypkg) values (?)'
...     with open('pypkgs.txt', 'r') as file:
...             for line in file:
...                     cursor.execute(query, (line.strip(),))
... 
<sqlite3.Cursor object at 0x784fa10a08c0>
<sqlite3.Cursor object at 0x784fa10a08c0>
<sqlite3.Cursor object at 0x784fa10a08c0>
<sqlite3.Cursor object at 0x784fa10a08c0>
<sqlite3.Cursor object at 0x784fa10a08c0>
<sqlite3.Cursor object at 0x784fa10a08c0>
<sqlite3.Cursor object at 0x784fa10a08c0>
>>> # 🫜
>>> 
me@amadeus:~/Tutor/Om$ 
```
> Final step, checking integrity.
```bash
me@amadeus:~/Tutor/Om$ sqlite3 pypkgs.db 
SQLite version 3.51.2 2026-01-09 17:27:48
Enter ".help" for usage hints.
sqlite> .tables
pypkgs
sqlite> .schema pypkgs
CREATE TABLE pypkgs (id integer primary key, pypkg text);
sqlite> 
sqlite> select * from pypkgs limit 1, 5;
2|0-0
3|000
4|0-0-1
5|00101s
6|001-hello-uv
sqlite> select count(*) from pypkgs;
749719
sqlite> .quit
me@amadeus:~/Tutor/Om$ 
```
> Done.
