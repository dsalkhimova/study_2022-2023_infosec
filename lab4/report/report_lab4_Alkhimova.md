
---
## Front matter
title: "Отчет по лаборатной работе №4"
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

Целью данной работы является получение практических навыков работы в консоли с расширенными атрибутами файлов.

# Задание 

1. От имени пользователя guest определите расширенные атрибуты файла `/home/guest/dir1/file1` командой `lsattr /home/guest/dir1/file1`.  
2. Установите командой `chmod 600 file1` на файл `file1` права, разрешающие чтение и запись для владельца файла.  
3. Попробуйте установить на файл `/home/guest/dir1/file1` расширенный атрибут `a` от имени пользователя `guest`:  `chattr +a /home/guest/dir1/file1`.  
    В ответ вы должны получить отказ от выполнения операции.  
4. Зайдите на третью консоль с правами администратора либо повысьте свои права с помощью команды `su`. Попробуйте установить расширенный атрибут `a` на файл `/home/guest/dir1/file1` от имени суперпользователя:  `chattr +a /home/guest/dir1/file1`.  
5. От пользователя guest проверьте правильность установления атрибута:
  `lsattr /home/guest/dir1/file1`.  
6. Выполните дозапись в файл file1 слова «test» командой `echo "test" /home/guest/dir1/file1`.  
    После этого выполните чтение файла `file1` командой `cat /home/guest/dir1/file1`.  
    Убедитесь, что слово `test` было успешно записано в `file1`.  
7. Попробуйте удалить файл `file1` либо стереть имеющуюся в нём информацию командой `echo "abcd" > /home/guest/dirl/file1`.  
    Попробуйте переименовать файл.  
8. Попробуйте с помощью команды `chmod 000 file1` установить на файл `file1` права, например, запрещающие чтение и запись для владельца файла. Удалось ли вам успешно выполнить указанные команды?
    Снимите расширенный атрибутa с файла `/home/guest/dirl/file1` от имени суперпользователя командой `chattr -a /home/guest/dir1/file1`. Повторите операции, которые вам ранее не удавалось выполнить. Ваши наблюдения занесите в отчёт. Повторите ваши действия по шагам, заменив атрибут «a» атрибутом «i». Удалось ли вам дозаписать информацию в файл? Ваши наблюдения занесите в отчёт.

# Теоретическое введение 
Утилита `chattr` позволяет устанавливать и отключать атрибуты файлов, на уровне файловой системы не зависимо от стандартных (чтение, запись, выполнение). Для просмотра текущих аттрибутов можно использовать `lsattr`. Изначально атрибуты управляемые `chattr` и `lsattr` поддерживались только файловыми системами семейства ext (ext2,ext3,ext4), но теперь эта возможность доступна и в других популярных файловых системах таких как XFS, Btrfs, ReiserFS, и т д.  

Утилиты  `chattr` и `lsattr` входят в пакет e2fsprogs и предустановлены во всех современных дистрибутивах. Базовый синтаксис `chattr` выглядит следующим образом:  

`$ chattr опции [оператор][атрибуты] файлы`  

Вот основные опции утилиты, которые вы можете использовать:  

+ R - рекурсивная обработка каталога;
+ V - максимально подробный вывод;
+ f - игнорировать сообщения об ошибках;
+ v - вывести версию.  

Оператор может принимать значения:  

+ `+` - включить выбранные атрибуты;
+ `-` - отключить выбранные атрибуты;
+ `=` - оставить значение атрибута таким, каким оно было у файла.  

Доступные атрибуты:

+ a - файл может быть открыт только в режиме добавления;
+ A - не обновлять время перезаписи;
+ c - автоматически сжимать при записи на диск;
+ C - отключить копирование при записи;
+ D - работает только для папки, когда установлен, все изменения синхронно записываются на диск сразу же;
+ e - использовать extent'ы блоков для хранения файла;
+ i - сделать неизменяемым;
+ j - все данные перед записью в файл будут записаны в журнал;
+ s - безопасное удаление с последующей перезаписью нулями;
+ S - синхронное обновление, изменения файлов с этим атрибутом будут сразу же записаны на диск;
+ t - файлы с этим атрибутом не будут хранится в отдельных блоках;
+ u - содержимое файлов с этим атрибутом не будет удалено при удалении самого файла и потом может быть восстановлено.

# Выполнение лабораторной работы 

1. Проверила наличие файла file в директории dir1. Создала file1 в директории dir1 ([рис. 1](images4/1.png)).

![Создание файла](images4/1.png){ #fig:001 width=70% }
	
2. От имени пользователя guest определила расширенные атрибуты файла с помощью команды `lsattr file1`. Расширенных атрибутов не установлено ([рис. 2](images4/2.png)).
 
![Проверка расширенных атрибутов файла](images4/2.png){ #fig:002 width=70% }


3. Установила командой `chmod 600 file1` на файл file1 права, разрешающие чтение и запись для владельца файла ([рис. 3](images4/3.png)).

![Установка прав для владельца файла](images4/3.png){ #fig:003 width=70% }

4. Попробовала установить на file1 расширенный атрибут `a` от имени пользователя `guest` командой `chattr +a /home/guest/dir1/file1`. В ответ получила отказ на выполнение операции ([рис. 4](images4/4.png)).

![Установка расширенных прав от имени guest](images4/4.png){ #fig:004 width=70% }

5. Зашла в консоль от имени суперпользователя и попробовала установить расширенный атрибут `a` на файл командой `chattr +a /home/guest/dir1/file1` ([рис. 5](images4/5.png)).

![Установка расширенных прав от имени суперпользователя](images4/5.png){ #fig:005 width=70% }

6. От пользователя guest проверила правильность установления атрибута с помощью команды `lsattr /home/guest/dir1/file1`. Атрибут `a` установился ([рис. 6](images4/6.png)).

![Проверка установки расширенного атрибута](images4/6.png){ #fig:006 width=70% }

7. Выполнила дозапись в файл file1 слова «test» командой `echo "test" /home/guest/dir1/file1`.
После этого выполнила чтение файла file1 командой `cat /home/guest/dir1/file1`.
Убедилась, что слово test было успешно записано в file1 ([рис. 7](images4/7.png)).

![Дозапись в файл](images4/7.png){ #fig:008 width=70% }

8. Попробовала стереть имеющуюся в file1 информацию командой `echo "abcd" > /home/guest/dirl/file1`, переименовать файл командой `mv` - доступ запрещен. ([рис. 8](images4/8.png))

![Изменение файла](images4/8.png){ #fig:010 width=70% }

9. От имени пользователя guest попробовала снять с файла file1 все атрибуты командой `chmod 000 file1` - доступ запрещен ([рис. 9](images4/9.png)).

![Редактрирование прав файла от имени guest](images4/9.png){ #fig:011 width=70% }

10. Сняла расширенный атрибут `a` с файла file1 от имени суперпользователя командой `chattr -a /home/guest/dir1/file1` ([рис. 10](images4/10.png)).

![Снятие атрибута а](images4/10.png){ #fig:012 width=70% }

Повторила операции, которые ранее не удавалось выполнить. Операции выполнены успешно ([рис. 11](images4/11.png)).

![Повтор операций без атрибута а](images4/11.png){ #fig:012 width=70% }

11. Вернула владельцу права на чтение и запись для файла file1. Заменила атрибут «`a`» атрибутом «`i`» ([рис. 12](images4/12.png)). 

![Смена атрибута а на i](images4/12.png){ #fig:012 width=70% }

Повторила предыдущие действия с файлом. Операции выполнить не удалось, так как доступ запрещен - атрибут `i` делает файл неизменяемым ([рис. 13](images4/13.png)).

![Действия с файлом при наличии атрибута i](images4/13.png){ #fig:012 width=70% }

# Выводы 

В процессе выполнения данной лабораторной работы я повысила навыки использования интерфейса командой строки (CLI), познакомилась на примерах с тем, как используются основные и расширенные атрибуты при разграничении доступа. Связала теорию дискреционного разделения доступа (дискреционная политика безопасности) с её реализацией на практике в ОС Linux, а также опробовала действие на практике расширенных атрибутов «`a`» и «`i`».


# Список литературы 

1. Команда CHATTR в LINUX — URL: <https://losst.ru/neizmenyaemye-fajly-v-linux?ysclid=l8k3gdovtw184374249>
2. File attribute. — URL: <https://en.wikipedia.org/wiki/File_attribute>
