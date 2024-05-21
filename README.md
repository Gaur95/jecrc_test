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

### Main.java
```
public class Main {
	public static void main(String[] args){
		for (int i = 0; i < 5; i++){
			System.out.println(i);
		}
	}
}

```

### Dockerfile for java
```
FROM openjdk:11
COPY . /usr/src/myapp
WORKDIR /usr/src/myapp
RUN javac Main.java
CMD ["java", "Main"]
```

### Dockerfile for apache
```
#base images like ubuntu
FROM ubuntu 
# when check details of image ist show mainainer name
maintainer akash 
# to run command 
RUN apt update ; apt install apache2 -y  
# cd  in /var/www/html 
workdir /var/www/html
# copy <source_local>  <dest_continer>
copy index.html    .
# when run container   , to run apache service 
CMD ["apachectl","-D", "FORGROUND"] 
```
### index.html 
```
<h1>HELLLLLOOOOOOOOOOOOOOOOOOOOOO</h1>
```

# portaner
```
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```
