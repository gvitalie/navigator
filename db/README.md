Here is pypkgs.db

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
