# This is the project to Extracting Business Card Data with EasyOCR 

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
# Image processing 
- read the image
- resize the image 
- converting color to gray scale image 
- set threshold value brefore passing to OCR Engine
```python
img = cv2.imread(image)
orig_img = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
rect,thresh_image = cv2.threshold(orig_img,70,255,cv2.THRESH_TOZERO)
```
# Extracting the data from image
- extraxt the data from image using easyocr using following command
```python
reader = easyocr.Reader(['en'], gpu=False)
res=reader.readtext(thresh_image,detail=0,paragraph=True)
```
# To store the data in to sql server
- creating a table in sqlserver by connecting python with with sql database using sqlite3
-  create string using result
- retrive the the pericular entity like,phone no,email-id,address etc by using regular expressions
```python
emails = re.findall(r'[A-Za-z0-9\.\-+_]+@[A-Za-z0-9\.\-+_]+\.[a-z]+', text)
```
- convert the image to binary form by using base64 to store the image in sql server
- store the retrieved data to table

# I hope this projects helps to store the business cards data  with the image
