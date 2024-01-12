# Bash(task1)



1.Написать скрипт, отображающий всю информацию о файле из текущего рабочего каталога (файл и каталог придумать самим)
```bash
#! /bin/bash

if  [ -n "$1" ]
then
ls -l "$(pwd)/$1"
else
echo "Isn`t current file!"
fi
```
2.Лесенка
```bash
#!/bin/bash

read -p "Enter the nubmer of lines for the ladder:" rows

for ((i = 1; i <=rows; i++)); do
    for ((j = 1; j <= i; j++)); do
        echo -m "$j"
    done
    echo
done
```

3.Лесенка с увеличением на единицу
```bash
#!/bin/bash

read -p "Enter the nubmer of lines for the ladder:" rows
counter=1

for ((i = 1; i <=rows; i++)); do
    for ((j = 1; j <= i; j++)); do
        echo -n "$counter"
        ((counter++))
    done
    echo
done
```
4.В этом задании вам нужно просто определить, является ли данный год високосным или нет. Если вы не знаете правил, вот они:

- Годы, кратные 4, являются високосными,
- но годы, кратные 100, не являются високосными,
- но годы, делящиеся на 400, являются високосными.

Годы находятся в пределах допустимого 1600 ≤ year ≤ 4000.

```bash
#!/bin/bash

read -p "Enter year: " year

if ((year % 4 == 0 && year % 100 != 0)) || ((year % 400 == 0)); then
    echo "$year - Leap year"
else
    echo "$year - No leap year"
fi
```

5.Печать шахматной доски заданной при помощи передаваемого параметра

Чтобы распечатать черный квадрат, echo -e -n “\\\\e[40m” ” “
Чтобы напечатать белое квадрат, echo -e -n “\\\\e[47m” ” “
Вызов команд происходит в цикле.
```bash
#!/bin/bash

if  [ -n "$1" ]
then
   for ((i=0; i < $1; ++i)); do
      for ((j=0; j < $1; ++j)); do
         if [ $(( (i+j) % 2 )) -eq 0 ]; then
            echo -n -e "\e[40m" " "
         else
            echo -n -e "\e[47m" " "
         fi
      done
      echo -n -e "\e[0m"
      echo
   done
else
   echo "Specify number!"
fi
```

6.Описание:

Ваша задача состоит в том, чтобы сложить буквы в одну букву.

Функции будет предоставлено переменное количество аргументов, каждый из которых представляет собой букву для добавления.

Notes:
Буквы всегда будут строчными.
Буквы могут переполняться (см. предпоследний пример описания)
Если буквы не указаны, функция должна вернуть 'z'


Примеры:

addLetters('a', 'b', 'c') = 'f'
addLetters('a', 'b') = 'c'
addLetters('z') = 'z'
addLetters('z', 'a') = 'a'
addLetters('y', 'c', 'b') = 'd' // notice the letters overflowing
addLetters() = 'z'

```bash
#! /bin/bash

function letSum {
   if [ $# -ne 0 ]; then
      local sum=0
      local ind=0
      local alph="abcdefghijklmnopqrstuvwxyz"
      for lt in $@; do
         ind=`expr index $alph $lt`
         sum=$(($sum + $ind))
         sum=$(($sum  % 26))
      done
      sum=$(($sum - 1))
      echo "${alph:sum:1}"
   else
      echo 'z'
   fi
}

letSum 'a' 'b' 'c'

letSum 'a' 'b'

letSum 'z'

letSum 'z' 'a'

letSum 'y' 'c' 'b'

letSum
```

7.Напишите алгоритм, который будет определять действительные адреса IPv4 в десятичном формате с точками. 
IP-адреса следует считать действительными, если они состоят из четырех октетов со значениями от 0 до 255 включительно.


Примеры допустимых входных данных:

1.2.3.4
123.45.67.89


Недопустимые примеры ввода:

1.2.3
1.2.3.4.5
123.456.78.90
123.045.067.089

Заметки: 
Начальные нули (например, 01.02.03.04) считаются недействительными. 
Входные данные гарантированно будут одной строкой

```bash
#! /bin/bash

if  [ -n "$1" ]; then
   ip=$1
   valid=true
   IFS='.' read -r -a octets <<< "$1"
   if [ ${#octets[@]} -ne 4 ]; then
      valid=false
   fi

   for octet in ${octets[@]}; do
      if [[ $octet == 0* && ${#octet} -gt 1 ]]; then
         valid=false
         break
      fi

      if (( $octet < 0 || $octet > 255 )); then
         valid=false
         break
      fi
   done

   if [ $valid == true ]; then
      echo "Incorrect IP adress"
   else
      echo "Incorrect IP adress"
   fi
else
   echo "Isn`t current file!"
fi
```


---
Bash(task2)
1.Суммирование
Напишите программу, которая находит суммирование каждого числа от 1 до num. Число всегда будет целым положительным числом, большим 0.
```bash
#!/bin/bash

if [ $# -ne 1];  then
echo "error"
         exit 1
fi

n=$1
sum=0
for ((i = 1; i <= n;  i++)); do
     ((sum += i))
done 

echo "sum 1 to $n: $sum"
```

2.Возьмите 2 строки s1 и s2, включающие только буквы от a до z.
Возвращает новую отсортированную строку, максимально длинную, содержащую различные буквы - каждая из которых берется только один раз - исходящие из s1 или s2.
a = "xyaabbbccccdefww"
b = "xxxxyyyyabklmopq"
longest(a, b) -> "abcdefklmopqwxy"
a = "abcdefghijklmnopqrstuvwxyz"
longest(a, a) -> "abcdefghijklmnopqrstuvwxyz"
```bash
#!/bin/bash

s1=$1
s2=$2
if [ "${#s2}" -gt 0 ]; then
    combined=$(echo "$s1s2" | grep -o . |  sort -u | tr -d '\n')
else
   combined=$(echo "$s1" | grep -0 . | sort -u | tr -d '\n\')
fi
echo "$combined"
```
3.Джон пригласил друзей. Его список:
S="Ann:Russel;John:Gates;Paul:Wahl;Alex:Tolkien;Ann:Bell;Lewis:Kern;Sarah:Rudd;Sydney:Korn;Madison:Meta";
Нужно написать программу, которая переводит эту строку в верхний регистр и сортирует ее в алфавитном порядке по фамилии.
Если фамилии совпадают, отсортируйте их по имени. Фамилия и имя гостя вводятся в результате в скобках через запятую.
Таким образом, результатом будет:
"(BELL, ANN)(GATES, JOHN)(KERN, LEWIS)(KORN, SYDNEY)(META, MADISON)(RUDD, SARAH)(RUSSEL, ANN)(TOLKIEN, ALEX)(WAHL, PAUL)"
Может случиться так, что в двух разных семьях с одинаковой фамилией два человека также имеют одинаковое имя.
```bash
#!/bin/bash

input="ann:Russel;John:Gates;Paul:Wahl;Alex:Tolkien;Ann:Bell;Lewis:Kern;Sarah:Rudd;Sydney:Korn;Madison:Meta";
sorted_input=$(echo "$input" | tr 'a-z' 'A-Z' | tr ';' '\n' | sort -t: -k2,2 -k1,1)
resultat=""

while DATA=':' read -r name surname; do
         resultat+="($surname, $name)"
done <<< "$sorted_input"

echo "$resultat"
```






