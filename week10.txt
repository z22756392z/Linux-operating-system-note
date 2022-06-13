```
tar cvfz backup02.tar.gz `find . -cnewer ./timebase -type f`
		等於
find . -cnewer ./timebase -type f | tar cvfz backup02.tar.gz - 
// - 代表把管道裡面拿到的東西 丟到這個指令後面(不是每個指令可以這樣寫)
```

 ````
 `find . -cnewer ./timebase -type f`
 
 代表先執行這個指令 == $(find . -cnewer ./timebase -type f)
 ````



run ``ls /tmp`` oput(``1>``) push to dev/null  and also standard error(``2>``) push into dev/null

```
ls /tmp 1>dev/null 2>&1
```

is last command run correctly (output => 0 mean run correct , else run uncorrectly);

```echo$?
echo $?
```

whether is account exist in the system ``id tom``

if output == 0 tom exist else none

```id
id tom 1> /dev/null 2>&1
echo $?
```



change the password of user tom

```
echo "tom" | passwd --stdin password
```



try to find the files  which have been modify 7 day ago

```
find . -mtime 7
```

try to find the files which size is 50 

```
find . -size +50M -size -100M
```



## incremental backup



```
tar cvfz backup01.tar.gz a
touch timebase
..modify some file inside a
tar cvfz backup02.tar.gz `find . -cnewer ./timebase -type f`
```





## sh

``which bash`` where is the bash 

> output = #! /usr/bin/bash

because in different linux distrubtion bash located differently



#### **script Example in linux** 

```
./checkuser.sh peter 123 222^C
```

inside checkuser.sh

```
//check all the user we type as paraemeter is exist
#! /usr/bin/bash

id $1 1>/dev/null 2>&1 //$1 mean the first paraemeter we type , $2 second ... and so on
result = $(echo $?)

if [$result -eq 0]
then
	echo "$1 exists"
else
	echo "$1 doesn't exist"
if

```

