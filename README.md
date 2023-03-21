# This is the project to Extracting Business Card Data with OCR 

### This project was done through google colab

# Installing required packages
```python
pip install streamlit
pip install streamlit_option_menu
pip install easyocr
```
# Installing the required libraries
```python
import easyocr
import cv2
import pandas as pd
import re
import sqlite3
import base64
import streamlit as st
from streamlit_option_menu import option_menu
```

# creating a table in sqlserver by connecting python with with sql database using sqlite3

```python
conn = sqlite3.connect('data.db', check_same_thread=False)
cursor = conn.cursor()
mytable='CREATE TABLE IF NOT EXISTS Business_cards_data(ID INTEGER PRIMARY KEY AUTOINCREMENT,COMAPANY_NAME TEXT,EMPLOYEE_NAME TEXT,DISIGNATION Text,EMAIL_ID TEXT,CONTACT TEXT,ALTERNATE_CONTACT TEXT,WEBSITE TEXT,ADDRESS TEXT,IMAGE BLOB)'
cursor.execute(mytable)
```

# Image processing like resizing,converting color to gray scale image and setting threshold value brefore passing to OCR Engine

```python
img = cv2.imread(image)
orig_img = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
rect,thresh_image = cv2.threshold(orig_img,70,255,cv2.THRESH_TOZERO)
```
# Extracting the data from image

```python
reader = easyocr.Reader(['en'], gpu=False)
res=reader.readtext(thresh_image,detail=0,paragraph=True)
```
# Extracting pertcular data from text like email,phone number,address etc, by using regular expressions
```python
emails = re.findall(r'[A-Za-z0-9\.\-+_]+@[A-Za-z0-9\.\-+_]+\.[a-z]+', text)
```
# Convering image to binaryform of data
```python
with open(image, 'rb') as f:
     img = f.read()
image=base64.b64encode(img)
```
# Insering extracted data to sql table
```python
mydata='INSERT INTO Business_cards_data(COMAPANY_NAME,EMPLOYEE_NAME,DISIGNATION,EMAIL_ID,CONTACT,ALTERNATE_CONTACT,WEBSITE,ADDRESS,IMAGE)values(?,?,?,?,?,?,?,?,?)'
cursor.execute(mydata,(company_name,name,designation,email_id,contact,alter_contact,link,address,image))
conn.commit()
```
# I hope this projects helps leads to data uploading is easy with the images 
