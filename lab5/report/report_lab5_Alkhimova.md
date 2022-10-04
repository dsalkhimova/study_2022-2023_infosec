
---
## Front matter
title: "Отчет по лаборатной работе №5"
subtitle: "по предмету Информационная безопасность"
author: "Алхимова Дарья Сергеевна"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: false # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt

## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english

## I18n babel
babel-lang: russian
babel-otherlangs: english

## Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9

## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric

## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"

## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
  
  - \linepenalty=10 # the penalty added to the badness of each line within a paragraph (no associated penalty node) Increasing the value makes tex try to have fewer lines in the paragraph.

  - \interlinepenalty=0 # value of the penalty (node) added after each line of a paragraph.

  - \hyphenpenalty=50 # the penalty for line breaking at an automatically inserted hyphen

  - \exhyphenpenalty=50 # the penalty for line breaking at an explicit hyphen

  - \binoppenalty=700 # the penalty for breaking a line at a binary operator

  - \relpenalty=500 # the penalty for breaking a line at a relation

  - \clubpenalty=150 # extra penalty for breaking after first line of a paragraph

  - \widowpenalty=150 # extra penalty for breaking before last line of a paragraph

  - \displaywidowpenalty=50 # extra penalty for breaking before last line before a display math

  - \brokenpenalty=100 # extra penalty for page breaking after a hyphenated line

  - \predisplaypenalty=10000 # penalty for breaking before a display

  - \postdisplaypenalty=0 # penalty for breaking after a display

  - \floatingpenalty = 20000 # penalty for splitting an insertion (can only be split footnote in standard LaTeX)

  - \raggedbottom # or \flushbottom


---

# Цель работы 

Целью данной работы является изучение механизмов изменения идентификаторов, применения SetUID- и Sticky-битов. Получение практических навыков работы в кон- соли с дополнительными атрибутами. Рассмотрение работы механизма смены идентификатора процессов пользователей, а также влияние бита Sticky на запись и удаление файлов.

# Задание 
## Часть 1
1. Войдите в систему от имени пользователя guest. 
2. Создайте программу simpleid.c: 
	
	`#include <sys/types.h>`  
  	`#include <unistd.h>`   
 	`#include <stdio.h>`  
	`int`  
	`main () `  
	`{`  
		`uid_t uid = geteuid ();`  
		`gid_t gid = getegid ();`  
		`printf ("uid=%d, gid=%d\n", uid, gid);`  
		`return 0;`  
	`}`
	
3. Скомплилируйте программу и убедитесь, что файл программы создан: `gcc simpleid.c -o simpleid`
4. Выполните программу simpleid: `./simpleid`
5. Выполните системную программу id (`id`) и сравните полученный вами результат с данными предыдущего пункта задания.
6. Усложните программу, добавив вывод действительных идентификато-
ров:

   `#include <sys/types.h>`  
   `#include <unistd.h>`  
   `#include <stdio.h>`  
	`int`  
	`main () `  
	`{`  
		`uid_t real_uid = getuid (); uid_t e_uid = geteuid ();`  
		`gid_t real_gid = getgid (); gid_t e_gid = getegid ();`  
		`printf ("e_uid=%d, e_gid=%d\n", e_uid, e_gid); printf ("real_uid=%d, 		real_gid=%d\n", real_uid, real_gid);`   
		`return 0;`  
	`}`

   Получившуюся программу назовите simpleid2.c.
7. Скомпилируйте и запустите simpleid2.c:  
	`gcc simpleid2.c -o simpleid2`   
	`./simpleid2`
8. От имени суперпользователя выполните команды:
	```
	chown root:guest /home/guest/simpleid2
	chmod u+s /home/guest/simpleid2
   ```
9. Используйте sudo или повысьте временно свои права с помощью su. Поясните, что делают эти команды.
10. Выполните проверку правильности установки новых атрибутов и смены владельца файла simpleid2: `ls -l simpleid2`
11. Запустите simpleid2 и id:
	```
	./simpleid2
	id
	```
	Сравните результаты.
12. Проделайте тоже самое относительно SetGID-бита.
13. Создайте программу readfile.c:
    
    `#include <fcntl.h>`  
    `#include <stdio.h>`  
    `#include <sys/stat.h>`  
    `#include <sys/types.h>`  
    `#include <unistd.h>`  
	`int`  
	`main (int argc, char* argv[])`   
	`{`  
		`unsigned char buffer[16]; size_t bytes_read;`  
		`int i;`  
		`int fd = open (argv[1], O_RDONLY);`   
		`do`  
		`{`  
			`bytes_read = read (fd, buffer, sizeof (buffer));`  
			`for (i =0; i < bytes_read; ++i) printf("%c", buffer[i]); `  
		`}`  
		`while (bytes_read == sizeof (buffer)); `  
		`close (fd);`  
		`return 0;`  
	`}`

14. Откомпилируйте её: `gcc readfile.c -o readfile`.
15. Смените владельца у файла readfile.c (или любого другого текстового файла в системе) и измените права так, чтобы только суперпользователь (root) мог прочитать его, a guest не мог.
16. Проверьте, что пользователь guest не может прочитать файл readfile.c.
17. Смените у программы readfile владельца и установите SetU’D-бит.
18. Проверьте, может ли программа readfile прочитать файл readfile.c?
19. Проверьте, может ли программа readfile прочитать файл /etc/shadow?
Отразите полученный результат и ваши объяснения в отчёте.

## Часть 2
1. Выясните, установлен ли атрибут Sticky на директории /tmp, для чего выполните команду `ls -l / | grep tmp`
2. От имени пользователя guest создайте файл file01.txt в директории /tmp со словом test: `echo "test" > /tmp/file01.txt`
3. Просмотрите атрибуты у только что созданного файла и разрешите чтение и запись для категории пользователей «все остальные»:
	```
	ls -l /tmp/file01.txt
	chmod o+rw /tmp/file01.txt
   ls -l /tmp/file01.txt
   ```
4. От пользователя guest2 (не являющегося владельцем) попробуйте прочитать файл /tmp/file01.txt: `cat /tmp/file01.txt`
5. От пользователя guest2 попробуйте дозаписать в файл /tmp/file01.txt слово test2 командой `echo "test2" > /tmp/file01.txt`
Удалось ли вам выполнить операцию?
6. Проверьте содержимое файла командой `cat /tmp/file01.txt`
7. От пользователя guest2 попробуйте записать в файл /tmp/file01.txt
слово test3, стерев при этом всю имеющуюся в файле информацию командой `echo "test3" > /tmp/file01.txt`
Удалось ли вам выполнить операцию?
8. Проверьте содержимое файла командой `cat /tmp/file01.txt`
9. От пользователя guest2 попробуйте удалить файл /tmp/file01.txt командой `rm /tmp/fileOl.txt`
Удалось ли вам удалить файл?
10. Повысьте свои права до суперпользователя командой `su -` и выполните после этого команду, снимающую атрибут `t` (Sticky-бит) с директории /tmp: `chmod -t /tmp`
11. Покиньте режим суперпользователя командой `exit`
12. От пользователя guest2 проверьте, что атрибута t у директории /tmp
нет: `ls -l / | grep tmp`
13. Повторите предыдущие шаги. Какие наблюдаются изменения?
14. Удалось ли вам удалить файл от имени пользователя, не являющегося
его владельцем? Ваши наблюдения занесите в отчёт.
15. Повысьте свои права до суперпользователя и верните атрибут t на ди- ректорию /tmp:
	```
   su -
   chmod +t /tmp
   exit
   ```
# Теоретическое введение 
Компиляторы, доступные в Linux-системах, являются частью коллек- ции GNU-компиляторов, известной как GCC (GNU Compiller Collection, подробнее см. <http://gcc.gnu.org>). В неё входят компиляторы языков С, С++, Java, Objective-C, Fortran и Chill. Будем использовать лишь первые два.
Компилятор языка С называется gcc. Компилятор языка С++ называется g++ и запускается с параметрами почти так же, как gcc.
Проверить это можно следующими командами:
```
whereis gcc
whereis g++
```
Первый шаг заключается в превращении исходных файлов в объектный код:
`gcc -c file.с`
В случае успешного выполнения команды (отсутствие ошибок в коде) полученный объектный файл будет называться file.о.
Объектные файлы невозможно запускать и использовать, поэтому после компиляции для получения готовой программы объектные файлы необходимо скомпоновать. Компоновать можно один или несколько файлов. В случае использования хотя бы одного из файлов, написанных на С++, компоновка производится с помощью компилятора g++. Строго говоря, это тоже не вполне верно. Компоновка объектного кода, сгенерированного чем бы то ни было (хоть вручную), производится линкером ld, g++ его просто вызывает изнутри. Если же все файлы написаны на языке С, нужно использовать компилятор gcc.
Например, так:
`gcc -o program file.o`
В случае успешного выполнения команды будет создана программа program (исполняемый файл формата ELF с установленным атрибутом +х).

# Выполнение лабораторной работы 
## Часть 1

1. Вошла в систему от имени пользователя guest и создала программу simpleid.c. ([рис. 1](images5/1.png)). С помощью команды `nano` открыла файл для редактирования и внесла туда код из задания:

	`#include <sys/types.h>`  
  	`#include <unistd.h>`   
 	`#include <stdio.h>`  
	`int`  
	`main () `  
	`{`  
		`uid_t uid = geteuid ();`  
		`gid_t gid = getegid ();`  
		`printf ("uid=%d, gid=%d\n", uid, gid);`  
		`return 0;`  
	`}`

	![Создание файла программы](images5/1.png){ #fig:001 width=70% }
	
2. Скомплилировала программу и убедилась, что файл программы создан:
  `gcc simpleid.c -o simpleid` ([рис. 2](images5/2.png)).
 
	![Проверка наличия файла](images5/2.png){ #fig:002 width=70% }


3. Выполнила программу simpleid (`./simpleid`) и системную программу id (`id`). Вывод программ сходится - они передают одинаковые параметры пользователя и его группы. ([рис. 3](images5/3.png)).

	![Сравнения вывода программ simpleid и id](images5/3.png){ #fig:003 width=70% }

4. Усложнила программу, добавив вывод действительных идентификаторов:
`#include <sys/types.h>`  
   `#include <unistd.h>`  
   `#include <stdio.h>`  
	`int`  
	`main () `  
	`{`  
		`uid_t real_uid = getuid (); uid_t e_uid = geteuid ();`  
		`gid_t real_gid = getgid (); gid_t e_gid = getegid ();`  
		`printf ("e_uid=%d, e_gid=%d\n", e_uid, e_gid); printf ("real_uid=%d, 		real_gid=%d\n", real_uid, real_gid);`   
		`return 0;`  
	`}`
 Получившуюся программу назвала simpleid2.c. ([рис. 4](images5/4.png)).

	![Добавление в программу действительных индентификаторов](images5/4.png){ #fig:004 width=70% }

5. Скомпилировала и запустила simpleid2.c ([рис. 5](images5/5.png)).: 
`gcc simpleid2.c -o simpleid2`
`./simpleid2`

	![Компилирование и запуск программы simpleid2](images5/5.png){ #fig:005 width=70% }

6. От имени суперпользователя выполнила команды:  
`chown root:guest /home/guest/simpleid2` ([рис. 6](images5/6.png)) и 
`chmod u+s /home/guest/simpleid2` ([рис. 7](images5/7.png))

	![Смена владельца файла](images5/6.png){ #fig:006 width=70% }  
	![Смена прав файла](images5/7.png){ #fig:007 width=70% }

7. Повысила временно свои права с помощью su.
 + Самый частый пример использования sudo - выполнение программы от имени суперпользователя. Для этого достаточно написать sudo перед именем программы.
 + Если вызов команды su происходит без аргументов, то происходит смена пользователя оболочки shell на суперпользователя root. Программа выдаст приглашение ввода пароля, если пароль будет верным, то текущим пользователем станет root.

8. Выполнила проверку правильности установки новых атрибутов и смены
владельца файла simpleid2: `ls -l simpleid2` ([рис. 8](images5/8.png))

	![Проверка изменений файла](images5/8.png){ #fig:008 width=70% }

9. Запустила simpleid2 (`./simpleid2`) и id (`id`) ([рис. 9](images5/9.png)).

	![Сравнения вывода программ simpleid2 и id](images5/9.png){ #fig:009 width=70% }

Результаты вывода сходятся.

10. Проделала то же самое относительно SetGID-бита.([рис. 10](images5/10.png)).

	![Повтор операций для SetGID-бита](images5/10.png){ #fig:010 width=70% }

11. Создала и скомпилировала программу readfile.c ([рис. 11](images5/11.png)):
	`#include <fcntl.h>`  
	`#include <stdio.h>`  
    `#include <sys/stat.h>`  
    `#include <sys/types.h>`  
    `#include <unistd.h>`  
	`int`  
	`main (int argc, char* argv[])`   
	`{`  
		`unsigned char buffer[16]; size_t bytes_read;`  
		`int i;`  
		`int fd = open (argv[1], O_RDONLY);`   
		`do`  
		`{`  
			`bytes_read = read (fd, buffer, sizeof (buffer));`  
			`for (i =0; i < bytes_read; ++i) printf("%c", buffer[i]); `  
		`}`  
		`while (bytes_read == sizeof (buffer)); `  
		`close (fd);`  
		`return 0;`  
	`}`
	![Создание программы readfile](images5/11.png){ #fig:011 width=70% }

12. Сменила владельца у файла readfile.c на dsalkhimova ([рис. 12](images5/12.png)) и изменила права так, чтобы только суперпользователь (root) мог прочитать его, a guest не мог ([рис. 13](images5/13.png)).

	![Смена владельца readfile.c](images5/12.png){ #fig:012 width=70% }  
	![Смена прав readfile.c](images5/13.png){ #fig:013 width=70% }

13. Проверила, что пользователь guest не может прочитать файл readfile.c ([рис. 14](images5/14.png)).

	![Проверка доступа guest к readfile](images5/14.png){ #fig:014 width=70% }

14. Сменила у программы readfile владельца ([рис. 15](images5/15.png)) и установила SetU’D-бит ([рис. 16](images5/16.png)).

	![Смена владельца readfile](images5/15.png){ #fig:015 width=70% }  
	![Смена прав readfile](images5/16.png){ #fig:016 width=70% }

15. Проверила, что программа readfile может прочитать файл readfile.c  ([рис. 17](images5/17.png)):

	![Чтение readfile readfile.c](images5/17.png){ #fig:017 width=70% }

16. Проверила, что программа readfile может прочитать файл /etc/shadow  ([рис. 18](images5/18.png)):

	![Чтение readfile /etc/shadow](images5/18.png){ #fig:018 width=70% }

## Часть 2

1. Выяснила, установлен ли атрибут Sticky на директории /tmp: `ls -l / | grep tmp` ([рис. 19](images5/19.png)). Атрибут `t` установлен.

	![Проверка атрибута Sticky](images5/19.png){ #fig:019 width=70% }

2. От имени пользователя guest создала файл file01.txt в директории /tmp со словом test: `echo "test" > /tmp/file01.txt` ([рис. 20](images5/20.png), [рис. 21](images5/21.png))

	![Создание file01.txt](images5/20.png){ #fig:020 width=70% }  
	![Просмотр file01.txt](images5/21.png){ #fig:021 width=70% }

3. Просмотрела атрибуты у только что созданного файла и разрешила чтение и запись для категории пользователей «все остальные» ([рис. 22](images5/22.png)):

`ls -l /tmp/file01.txt`
`chmod o+rw /tmp/file01.txt`
`ls -l /tmp/file01.txt`

	![Просмотр и изменение атрибутов file01.txt](images5/22.png){ #fig:022 width=70% }

4. От пользователя guest2 (не являющегося владельцем) успешно прочитала файл /tmp/file01.txt: `cat /tmp/file01.txt` ([рис. 23](images5/23.png)):

	![Чение file01.txt пользователем guest2](images5/23.png){ #fig:023 width=70% }

5. От пользователя guest2 успешно дозаписала в файл /tmp/file01.txt слово test2 командой `echo "test2" >> /tmp/file01.txt` и проверила содержимое файла командой `cat /tmp/file01.txt` ([рис. 24](images5/24.png)):

	![Дозапись file01.txt пользователем guest2](images5/24.png){ #fig:024 width=70% }

6. От пользователя guest2 успешно дозаписала в файл /tmp/file01.txt слово test3 командой `echo "test3" > /tmp/file01.txt`, стерев при этом все предыдущее содержимое файла, и проверила содержимое файла командой `cat /tmp/file01.txt` ([рис. 25](images5/25.png)):

	![Дозапись с удалением file01.txt пользователем guest2](images5/25.png){ #fig:025 width=70% }

7. От пользователя guest2 попробовала удалить файл /tmp/file01.txt командой `rm /tmp/fileOl.txt` ([рис. 26](images5/26.png)). Операция запрещена.

	![Удаление file01.txt пользователем guest2](images5/26.png){ #fig:026 width=70% }

8. Повысила свои права до суперпользователя командой `su -` и выполнила после этого команду, снимающую атрибут t (Sticky-бит) с директории /tmp: `chmod -t /tmp`. Затем покинула режим суперпользователя командой `exit`. От пользователя guest2 проверила, что атрибута t у директории /tmp нет: `ls -l / | grep tmp ` ([рис. 27](images5/27.png)):

	![Снятие атрибута Sticky](images5/27.png){ #fig:027 width=70% }

9. Повторила предыдущие шаги ([рис. 28](images5/28.png), [рис. 29](images5/29.png)). Изменений нет, кроме того, что теперь удалось удалить файл file01.txt от имени guest2. 

	![Повтор действий с файлом без атрибута Sticky 1/2](images5/28.png){ #fig:028 width=70% }  
	![Повтор действий с файлом без атрибута Sticky 2/2](images5/29.png){ #fig:029 width=70% }

10. Повысила свои права до суперпользователя и вернула атрибут t на директорию /tmp ([рис. 30](images5/30.png)):

	![Возвращение атрибута Sticky на директрорию /tmp](images5/30.png){ #fig:030 width=70% }

# Выводы 

В процессе выполнения данной лабораторной работы я научилась работать с механизмами изменения идентификаторов, применением SetUID- и Sticky-битов. Получила практических навыков работы в консоли с дополнительными атрибутами. Рассмотрела работу механизма смены идентификатора процессов пользователей, а также влияние бита Sticky на запись и удаление файлов.


# Список литературы 

1. Описание лабораторной работы 5 - URL: <https://esystem.rudn.ru/pluginfile.php/1652171/mod_resource/content/2/005-lab_discret_sticky.pdf>
2. Команда SU в LINUX — URL: <https://losst.ru/komanda-su-v-linux?ysclid=l8u8lr4lj5312561951>
3. Команда SUDO в LINUX — URL: <https://losst.ru/komanda-sudo-v-linux?ysclid=l8uachqd98171389830>

