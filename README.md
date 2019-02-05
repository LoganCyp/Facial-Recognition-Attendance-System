# Facial Recognition Attendance System
This projects uses XLSXWriter and OpenCV3 to create a basic Facial Recognition based Attendance system. 

For `create_database.py` SQLite3 is used to create a database linked to images of the faces.

```python
import sqlite3
conn = sqlite3.connect('database.db')
c = conn.cursor()
sql = """
DROP TABLE IF EXISTS users;
CREATE TABLE users (
           id integer unique primary key autoincrement,
           name text,
           date 
           
);
"""
c.executescript(sql)
conn.commit()
conn.close()
```
