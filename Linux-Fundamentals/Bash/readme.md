## HTB-ACADEMY (Bash)
## Ivan Medina
---

### ``` #!/bin/bash ```

### Bash operators

- ``` -z ```: If empty.
- ``` -n ```: If not null.
- ``` -eq ```: equals.
- ``` -ne ```: not equals.
- ``` -lt ```: less than.
- ``` -gt ```: greater than.
- ``` -e ```: file exists.
- ``` -f ```: is file.
- ``` -d ```: is directory.
- ``` -L ```: is link.
- ``` -N ```: is modified.
- ``` -O ```: owns actual user.
- ``` -G ```: is any group.
- ``` -S ```: greater than 0.
- ``` -S ```: greater than 0.


### Global variables

- ``` name binary ```: $0.
- ``` $1 ```: argument 1.
- ``` $2 ```: argument 2.
- ``` ... ```
- ``` $9 ```: argument 9.
- ``` $# ```: number args.
- ``` $$ ```: proc id.
- ``` $? ```: exit status.

### Brackets
```[[]]```: used for bool and strings values.

### lists

```
domains = (1 ,2,3)
echo ${doamins[0]} 
```

### variables

```
d = 1
echo $1
```

### count length

```
${#varname}
```

###  operations

```
echo "$((10/10))"
```

###  read 

```
echo Username
read user
echo Bienvenido, $user
```

```
read -p "Username: " user
echo Bienvenido, $user 
```

- ``` -s ```: hidden input (for passwords)

### if

```
if [$# -eq 0]
then
    echo -e "x"
    exit 1
elif []
then
    ...
else 
    ...
fi
```

### for

```
for i in {1..5}
do
  if [[ "$i" == '4' ]]
  then
    continue
  fi
  echo "Hello $i4"
done
```

```
for i in {0. .8. .2}
do
  echo "Hello $i"
done
```

```
for i in $(seq 1,10))
do
  echo "Hello $i"
done
```

### while

```
while[$x -eq 1]
```

### switch
```
case EXPRESSION in

  PATTERN_1)
    STATEMENTS
    ;;

  PATTERN_2)
    STATEMENTS
    ;;

  PATTERN_N)
    STATEMENTS
    ;;

  *)
    STATEMENTS
    ;;
esac
```