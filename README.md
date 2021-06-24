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
оператор >>  <br/>
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
Vim обладает большим количеством горячих клавиш. Более того вся концепция vim состоит в том что его интерфейс подобен **языку программирования**. Нужно просто начать использовать его. Больше всего мне запомнилось из лекции что можно просто набрать 	*dw* и он удалит целое слово, а если набрать *4dw* он удалит 4 впереди идущих слова. Также не нужно отрывать руки от клавиатуры и двигать мышкой. Тратит в сотню раз меньше памяти чем VS code.<br/>
Для его изучения можно попробовать игры которые я нашел.

# Лекция № 5: Command-line Environment (2020)
- Ссылка на лекцию: https://youtu.be/e8BO_dYxk5c
- Теги: 

### Выполнить процесс и оставить его выполняться в фоне
```console
nohup sleep 20 &
>[2] 116
```
### Узнать все процессы связанные с текущим сеансом терминала
```console
jobs
>
[1]-  Running                 sleep 2000 &
[2]+  Done                    nohup sleep 20
```
### Остановить какой-то процесс
```console
kill -STOP process_number
>
```
### Завершить какой-то процесс
```console
kill process_number
>
```
### Создать свое имя для своей команды
```console
alias my_alias="ls"
my_alias
>
'New Folder'    nohup.out          some_1.py   some_4.py   test_1.txt   test_4.txt   test_7.txt
 cat            python_script.py   some_2.py   some_5.py   test_2.txt   test_5.txt   test_program
 list_txt.txt   readme.md          some_3.py   test        test_3.txt   test_6.txt
```
```console
alias rm_txt="rm *.txt"
ls
>
'New Folder'   nohup.out          readme.md   some_2.py   some_4.py   test
 cat           python_script.py   some_1.py   some_3.py   some_5.py   test_program
```
### Создать указатель на файл или папку, чтобы когда другая программа обращалась к этому пути она шла в исходный путь, symlink
```console
ln -s path symlink_path
>
```
# Лекция № 7: Debugging and Profiling (2020)
- Ссылка на лекцию: https://youtu.be/l812pUnKxME
- Теги: 

### Дебагер для python https://docs.python.org/3/library/pdb.html
### Дебагер для с++ https://www.gnu.org/software/gdb/
### Показать вермя выполнения какой-то задачи
- real - реальное время выполнения, в данном случае скачивалось
- user - сколько заняло выполнение в системе
- sys - сколько выполнялся этот процесс на CPU
```console
time curl https://missing.csail.mit.edu &> /dev/null
>
real    0m0.495s
user    0m0.014s
sys     0m0.041s
```
### Python time profiler
- тут видно что больше всего занимает метод open
```console
python -m cProfile -s tottime grep.py 1000 '^(import|\s*def)[^,]*$' *.py
>
ncalls  tottime  percall  cumtime  percall filename:lineno(function)
     8000    0.266    0.000    0.292    0.000 {built-in method io.open}
     8000    0.153    0.000    0.894    0.000 grep.py:5(grep)
    17000    0.101    0.000    0.101    0.000 {built-in method builtins.print}
     8000    0.100    0.000    0.129    0.000 {method 'readlines' of '_io._IOBase' objects}
    93000    0.097    0.000    0.111    0.000 re.py:286(_compile)
    93000    0.069    0.000    0.069    0.000 {method 'search' of '_sre.SRE_Pattern' objects}
    93000    0.030    0.000    0.141    0.000 re.py:231(compile)
    17000    0.019    0.000    0.029    0.000 codecs.py:318(decode)
        1    0.017    0.017    0.911    0.911 grep.py:3(<module>)
```
### Python memory profiler
```python
@profile
def my_func():
    a = [1] * (10 ** 6)
    b = [2] * (2 * 10 ** 7)
    del b
    return a
if __name__ == '__main__':
    my_func()
```
```console
python -m memory_profiler example.py
Line #    Mem usage  Increment   Line Contents
==============================================
     3                           @profile
     4      5.97 MB    0.00 MB   def my_func():
     5     13.61 MB    7.64 MB       a = [1] * (10 ** 6)
     6    166.20 MB  152.59 MB       b = [2] * (2 * 10 ** 7)
     7     13.61 MB -152.59 MB       del b
     8     13.61 MB    0.00 MB       return a
```

### Общая информация о системе
```console
htop
>
```
### Информация о I/O operations 
```console
iotop
>
```
### Информация о использовании диска
```console
df
>
Filesystem     1K-blocks      Used Available Use% Mounted on
/dev/sdb       263174212   1752920 247983136   1% /
tmpfs            6494692         0   6494692   0% /mnt/wsl
tools          379954172 330583144  49371028  88% /init
none             6492404         0   6492404   0% /dev
none             6494692         8   6494684   1% /run
none             6494692         0   6494692   0% /run/lock
none             6494692         0   6494692   0% /run/shm
none             6494692         0   6494692   0% /run/user
tmpfs            6494692         0   6494692   0% /sys/fs/cgroup
C:\            379954172 330583144  49371028  88% /mnt/c
D:\            976759804 804463696 172296108  83% /mnt/d
```
### Информация о использовании памяти
```console
free
>
              total        used        free      shared  buff/cache   available
Mem:       12989388       91504    12688252          72      209632    12651616
Swap:       4194304           0     4194304
```