# jecrc_test
## subheading
### title
### dockerfile
```
FROM python:3
WORKDIR /usr/src/app
COPY hello.py .
CMD ["python", "./hello.py"]
```
### hello.py
```
import time
fruits = ["apple", "banana", "cherry"]
for x in fruits:
    print(x)
    time.sleep(2)
```
