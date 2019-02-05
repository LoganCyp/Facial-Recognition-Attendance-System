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

For `record_face.py` I used cv2 for facial detection and haarcascades to detect faces to make a dataset.

```python
import cv2
import numpy as np
import sqlite3
import os
import time
conn = sqlite3.connect('database.db')
if not os.path.exists('./dataset'):
    os.makedirs('./dataset')
c = conn.cursor()
face_cascade = cv2.CascadeClassifier('haarcascade_frontalface_alt.xml')
cap = cv2.VideoCapture(0)
uname = input("Enter your name: ")
c.execute('INSERT INTO users (name) VALUES (?)', (uname,))
uid = c.lastrowid
sampleNum = 0
while True:
  ret, img = cap.read()
  gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
  faces = face_cascade.detectMultiScale(gray, 1.3, 5)
  for (x,y,w,h) in faces:
    sampleNum = sampleNum+1
    cv2.imwrite("dataset/User."+str(uid)+"."+str(sampleNum)+".jpg",gray[y:y+h,x:x+w])
    cv2.rectangle(img, (x,y), (x+w, y+h), (255,0,0), 2)
    cv2.waitKey(100)
  cv2.imshow('img',img)
  cv2.waitKey(1);
  if sampleNum > 60:
    break
cap.release()
conn.commit()
conn.close()
cv2.destroyAllWindows()
```
