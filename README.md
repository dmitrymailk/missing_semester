## Missing Semester

- Created: 2021-05-09 19:05
- Tags: #git #shell #productivity
- Link: https://missing.csail.mit.edu/
- Момент появления:
- Summary:
- youtube: https://www.youtube.com/playlist?list=PLyzOVJj3bHQuloKGG59rS43e29ro7I57J

# Лекция № 1: Course Overview + The Shell (2020)

- Ссылка на лекцию: https://youtu.be/Z56Jmr9Z34Q
- Теги: #shell
- ссылка на официальный конспект https://missing.csail.mit.edu/2020/course-shell/

Сначала рассказывается что такое shell и как туда можно вбивать команды.

### Вывести дату

```console
date
> Sun Jun 20 19:00:09 MSK 2021
```

### Вывести что-то в консоль

```console
echo hello
>hello

echo "hello world"
>hello world
```

### Вывести где лежит файл или программа

```console
which echo
>/usr/bin/echo
```

### Вывести рабочую дирректорию

print work dirrectory

```console
pwd
>/mnt/d/computer_science_essentials/missing_semester
```

### Изменить рабочую дирректорию

change dirrectory

```console
pwd
>/mnt/d/computer_science_essentials
cd missing_semester/
pwd
>/mnt/d/computer_science_essentials/missing_semester
```

### Вывести содержимое текущей дирректории

```console
ls
>git  missing_semester
```

### Вернуться обратно в ту дирректорию в которой работал ранее

```console
cd -
>/mnt/d/computer_science_essentials/missing_semester
cd -
>/mnt/d/computer_science_essentials
```

### Узнать более подробную информацию о файлах и папках в дирректории

Первая d обозначает что это дирректория.<br/>
Дальше идут последовательности прав для этого файла.

```console
ls -l
>
-rwxrwxrwx 1 root root    0 Jun 20 19:25 readme.md
drwxrwxrwx 1 root root 4096 Jun 20 19:25 test_folder
```

### Переименовать файл

```console
mv readme.md readme_please.md
ls
>readme_please.md  test_folder
```

### Удалить файл

```console
rm readme_please.md
ls
>test_folder
```

### Удалить пустую папку

```console
rmdir test_folder
ls
>
```

### Создать новую папку

```console
mkdir "New Folder"
ls
>'New Folder'
```

### Показать мануал для какой-то команды

```console
man ls
>ТУТ ТИПА МАНУАЛ
```

### Очистить консоль = нажать ctrl+l

### Перенаправить output какой-то программы в какой-то файл

оператор > <br/>
перезаписывает содержимое <br/>
оператор >> <br/>
добавляет в конец

```console
echo hello > hello.txt
cat hello.txt
>hello
```

### Перенаправить содержимое файла в программу, а потом, то что она вывела в другой файл

```console
cat < hello.txt > hello2.txt
ls
>'New Folder'   hello.txt   hello2.txt
cat hello2.txt
>hello
```

### Взять output одной программы и перехватить его при помощи другой

для этого используется оператор **|** <br/>
Тут видно что сначала мы вывели в консоль, а потом записали это в файл.

```console
ls - / | tail -n1
>-rwxrwxrwx 1 root root    6 Jun 20 19:45 hello2.txt
ls -l . | tail -n1 > hello2.txt
cat hello 2.txt
>-rwxrwxrwx 1 root root    0 Jun 20 19:54 hello2.txt
```

# Лекция № 2: Shell Tools and Scripting (2020)

- Ссылка на лекцию: https://youtu.be/kgII-YWo3Zw
- Теги: #shell

### Повторить только что написанную команду

```console
touch some.txt
>!!
```

### Специальные переменные для написания скриптов

$? - содержит код, с которым завершилась предыдущая программа <br/>
0 - успешно <br/>
1 - ошибка

```console
echo hello
>hello
echo $?
>0
grep some hello.txt
echo $?
>1
```

### Логические операторы для работы в консоли

|| - значит **or**, работает также как и во всех языках программирования, если первая ложь тогда работает второе, если первое истина до второго не доходит <br/>
&& - значит **and** работает если только оба истина <br/>
; заканчивает высказывание

```console
false || echo hello
>hello
true || echo hello
>
true && echo hello
>hello
false && echo hello
>
false ; echo hello
>hello
```

### Запись результата функции в переменную

```console
foo=$(pwd)
echo $foo
>/mnt/d/computer_science_essentials/missing_semester
echo "Working dir is $foo"
>Working dir is /mnt/d/computer_science_essentials/missing_semester
```

### Создание первого скрипта на shell

```shell
#!/bin/bash
#означает что файл исполняемый
echo "Starting program at $(date)" # выводит дату
echo "Running program $0 with $# arguments with pid $$"
# $@ - список всех аргументов
# $? - код завершения последней программы, 0 - корректно, 1 - ошибка
for file in "$@"; do
  echo "Some file -> $file"
  if [[ $? -ne 1 ]]; then
    echo "OK!"
  fi
done
```

```console
./test_program hello.txt hellooooo.txt
>
Starting program at Sun Jun 20 21:07:13 MSK 2021
Running program ./test_program with 2 arguments with pid 291
Some file -> hello.txt
OK!
Some file -> hellooooo.txt
OK!
```

### Создать или удалить в цикле несколько файлов, которые немного отличаются

```console
touch file{1,2,3}.txt
ls
>'New Folder'   file1.txt   file2.txt   file3.txt   hello.txt   hello2.txt   some.txt test   test_program

rm file{1,2,3}.txt
ls
>'New Folder'   hello.txt   hello2.txt   some.txt   test   test_program
```

### Сделать питоновский скрипт запускаемым в консоли

```python
#!/usr/bin/python3
import sys
for arg in reversed(sys.argv[1:]):
    print(arg)
```

```console
./python_script.py 1 2 3
>
1
2
3
```

### Найти что-то в папках

```console
# Find all directories named src
find . -name src -type d
# Find all python files that have a folder named test in their path
find . -path '*/test/*.py' -type f
# Find all files modified in the last day
find . -mtime -1
# Find all zip files with size in range 500k to 10M
find . -size +500k -size -10M -name '*.tar.gz'
```

### Найти что-то а потом для этих файлов что-то исполнить

```console
# Delete all files with .tmp extension
find . -name '*.tmp' -exec rm {} \;
# Find all PNG files and convert them to JPG
find . -name '*.png' -exec convert {} {}.jpg \;
```

### Вывести историю комманд

```console
history
>
199  rg
200  apt install ripgrep
201  dpkg --configure -a
202  type updatedb updatedb
203  rpm -qf /usr/bin/updatedb
204  apt-get remove mlocate
205  fzf
206  apt install fzf
207  history
208  cat README.md | fzf
209  history
```

### Поиск по тексту в реальном времени

```console
cat README.md | fzf
>
```

### Вывести красивое дерево файлов и папок

```console
tree
>
.
├── New Folder
├── README.md
├── list_txt.txt
├── python_script.py
├── some_1.py
├── some_2.py
├── some_3.py
├── some_4.py
├── some_5.py
├── test
│   ├── 1
│   ├── 2
│   └── 3
├── test_1.txt
├── test_2.txt
├── test_3.txt
├── test_4.txt
├── test_5.txt
├── test_6.txt
├── test_7.txt
└── test_program
```

# Лекция № 3: Editors (vim) (2020)

- Ссылка на лекцию: https://youtu.be/a6Q8Na575qc
- Теги: #vim
  Vim обладает большим количеством горячих клавиш. Более того вся концепция vim состоит в том что его интерфейс подобен **языку программирования**. Нужно просто начать использовать его. Больше всего мне запомнилось из лекции что можно просто набрать _dw_ и он удалит целое слово, а если набрать _4dw_ он удалит 4 впереди идущих слова. Также не нужно отрывать руки от клавиатуры и двигать мышкой. Тратит в сотню раз меньше памяти чем VS code.<br/>
  Для его изучения можно попробовать игры которые я нашел
