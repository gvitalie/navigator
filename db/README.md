import requests
import subprocess
import sqlite3

request = requests.get('https://pypi.org/simple')

with open('pypkgs.html', 'w') as file:
    file.write(request.text)

cmd = r"""grep -o 'href="/simple/[^/]*' pypkgs.html | sed 's|href="/simple/||' | tee pypkgs.txt"""
subprocess.run(cmd, shell=True, check=True)


with sqlite3.connect('pypkgs.db') as db:
    cursor = db.cursor()
    cursor.execute('CREATE TABLE pypkgs (id integer primary key, pypkg text)')
    cursor.execute('CREATE UNIQUE INDEX idx_pypkgs_pypkg ON pypkgs(pypkg)')

with sqlite3.connect('pypkgs.db') as db:
    cursor = db.cursor()
    query = 'insert into pypkgs (pypkg) values (?)'
    with open('pypkgs.txt', 'r') as file:
        for line in file:
            cursor.execute(query, (line.strip(),))
print('Done')
