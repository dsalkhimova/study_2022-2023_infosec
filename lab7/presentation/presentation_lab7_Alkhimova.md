---
## Front matter
lang: ru-RU
title: Элементы криптографии. Однократное гаммирование.
author: |
	 Алхимова Дарья Сергеевна НБИ-01-19\inst{1}

institute: |
	\inst{1}Российский Университет Дружбы Народов

date: 18 октября, 2022

## Formatting
mainfont: PT Serif
toc: false
slide_level: 2
theme: metropolis
header-includes: 
 - \metroset{progressbar=frametitle,sectionpage=progressbar,numbering=fraction}
 - '\makeatletter'
 - '\beamer@ignorenonframefalse'
 - '\makeatother'
aspectratio: 43
section-titles: true
---

# Информация

## Докладчик


  * Алхимова Дарья Сергеевна
  * студентка 4 курса
  * группы НБИбд-01-19
  * Российский университет дружбы народов
  * [1032191645@rudn.ru](mailto:1032191645@rudn.ru)


# Цель работы

Освоить на практике применение режима однократного гаммирования.

# Теоретическое введение

## Определение гаммирования

Гаммы — это сложение её элементов с элементами открытого (закрытого) текста по некоторому фиксированному модулю, значение которого представляет собой известную часть алгоритма шифрования.

Открытый текст имеет символьный вид, а ключ — шестнадцатеричное представление. Ключ также можно представить в символьном виде, вос- пользовавшись таблицей ASCII-кодов.

# Теоретическое введение

## Алгоритм гаммирования

1. Сгенерировать сегмент гаммы H(1) и наложить его на соответствующий участок шифруемых данных.  
2. Подсчитать контрольную сумму участка, соответствующего сегменту гаммы H(1).  
3. Сгенерировать следующий сегмент гамм H(2) с учетом контрольной суммы уже зашифрованного участка данных.  
4. Подсчитать контрольную сумму участка данных, соответствующего сегменту данных H(2) и т.д.

# Выполнение лабораторной работы

## Создала файл программы

Заполнила файл кодом программы гаммирования.

![Фрагмент программы гаммирования](images7/2.png){ #fig:001 width=70% }


# Выполнение лабораторной работы

##  Проверила правильность выполнения программы

Сначала по первому сценарию:  
необходимо определить вид шифротекста при известном ключе и известном открытом тексте. 

![Запуск программы по сценарию 1](images7/10.png){ #fig:002 width=70% }

# Выполнение лабораторной работы

##  Проверила правильность выполнения программы

Затем по второму сценарию:  
необходимо определить ключ, по которому шифротекст может быть преобразован во фрагмент текста (один из возможных вариантов прочтения открытого текста).

![Запуск программы по сценарию 2](images7/11.png){ #fig:003 width=70% }

# Вывод

В процессе выполнения данной лабораторной работы я приобрела навыки применения режима однократного гаммирования.