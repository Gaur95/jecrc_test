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
### main.c
```
#include <stdio.h>
int main() {
 int i = 1;
 while (i <= 5){
 printf("%d\n" , i);
 ++i;
 }
 return 0;
}
```
### Dockerfile for c
```
FROM gcc:4.9
COPY . /usr/src/myapp
WORKDIR /usr/src/myapp
RUN gcc -o myapp main.c
CMD ["./myapp"]
```
