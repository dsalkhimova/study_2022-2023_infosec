
---
## Front matter
title: "Отчет по лаборатной работе №7"
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

Освоить на практике применение режима однократного гаммирования.

# Задание 
Нужно подобрать ключ, чтобы получить сообщение «С Новым Годом, друзья!». Требуется разработать приложение, позволяющее шифровать и дешифровать данные в режиме однократного гаммирования. Приложение должно:  
1. Определить вид шифротекста при известном ключе и известном открытом тексте.  
2. Определить ключ, с помощью которого шифротекст может быть преобразован в некоторый фрагмент текста, представляющий собой один из возможных вариантов прочтения открытого текста.


# Теоретическое введение 
Гаммирование представляет собой наложение (снятие) на открытые (зашифрованные) данные последовательности элементов других данных, полученной с помощью некоторого криптографического алгоритма, для получения зашифрованных (открытых) данных. Иными словами, наложение гаммы — это сложение её элементов с элементами открытого (закрытого) текста по некоторому фиксированному модулю, значение которого представляет собой известную часть алгоритма шифрования.

Наложение гаммы по сути представляет собой выполнение операции сложения по модулю 2 (XOR) (обозначаемая знаком `+`) между элементами гаммы и элементами подлежащего сокрытию текста. Напомним, как работает операция XOR над битами: 0 `+` 0 = 0, 0 `+` 1 = 1, 1 `+` 0 = 1, 1 `+` 1 = 0.
Такой метод шифрования является симметричным, так как двойное прибавление одной и той же величины по модулю 2 восстанавливает исходное значение, а шифрование и расшифрование выполняется одной и той же программой.

Открытый текст имеет символьный вид, а ключ — шестнадцатеричное представление. Ключ также можно представить в символьном виде, вос- пользовавшись таблицей ASCII-кодов.

Необходимые и достаточные условия абсолютной стойкости шифра:  
+ полная случайность ключа;  
+ равенство длин ключа и открытого текста;  
+ однократное использование ключа.

Метод гаммирования становится бессильным, если известен фрагмент исход- ного текста и соответствующая ему шифрограмма. В этом случае простым вычи- танием по модулю 2 получается отрезок псевдослучайной последовательности и по нему восстанавливается вся эта последовательность.  

Алгоритм гаммирования:  
1. Генерация сегмента гаммы H(1) и наложение его на соответствующий участок шифруемых данных.  
2. Подсчет контрольной суммы участка, соответствующего сегменту гаммы H(1).  
3. Генерация с учетом контрольной суммы уже зашифрованного участка данных следующего сегмента гамм H(2).  
4. Подсчет контрольной суммы участка данных, соответствующего сегменту данных H(2) и т.д.

# Выполнение лабораторной работы 
	
1. Создала файл программы и открыла его для заполнения кодом. ([рис. 1](images7/1.png)).

	![Создание файла программы](images7/1.png){ #fig:001 width=70% }

2. Заполнила файл кодом программы гаммирования ([рис. 2](images7/2.png)).

	![Фрагмент программы гаммирования](images7/2.png){ #fig:002 width=70% }
	
	Полный текст программы ([рис. 3](images7/3.png), [рис. 4](images7/4.png), [рис. 5](images7/5.png), [рис. 6](images7/6.png), [рис. 7](images7/7.png), [рис. 8](images7/8.png), [рис. 9](images7/9.png)):  
	  
	  ![Листинг программы 1/7](images7/3.png){ #fig:003 width=70% }
	  
	  ![Листинг программы 2/7](images7/4.png){ #fig:004 width=70% }
	  
	  ![Листинг программы 3/7](images7/5.png){ #fig:005 width=70% }
	  
	  ![Листинг программы 4/7](images7/6.png){ #fig:006 width=70% }
	  
	  ![Листинг программы 5/7](images7/7.png){ #fig:007 width=70% }
	  
	  ![Листинг программы 6/7](images7/8.png){ #fig:008 width=70% }
	  
	  ![Листинг программы 7/7](images7/9.png){ #fig:009 width=70% }


3. Проверила правильность выполнения программы гаммирования по первому сценарию (см. раздел Задание) ([рис. 10](images7/10.png)).

	![Запуск программы по сценарию 1](images7/10.png){ #fig:010 width=70% }

4. Проверила правильность выполнения программы гаммирования по второму сценарию (см. раздел Задание) ([рис. 11](images7/11.png)).

	![Запуск программы по сценарию 2](images7/11.png){ #fig:011 width=70% }
	
# Выводы 

В процессе выполнения данной лабораторной работы я приобрела навыки применения режима однократного гаммирования.


# Список литературы 

1. Описание лабораторной работы 7 - URL: <https://esystem.rudn.ru/pluginfile.php/1652175/mod_resource/content/2/007-lab_crypto-gamma.pdf>
2. Установка javac  - URL: <https://stackoverflow.com/questions/5407703/javac-command-not-found>


