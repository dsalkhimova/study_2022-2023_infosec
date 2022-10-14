
---
## Front matter
title: "Отчет по лаборатной работе №6"
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

Развить навыки администрирования ОС Linux. Получить первое практическое знакомство с технологией SELinux1. Проверить работу SELinx на практике совместно с веб-сервером Apache.

# Задание 
1. Войдите в систему с полученными учётными данными и убедитесь, что SELinux работает в режиме enforcing политики targeted с помощью команд `getenforce` и `sestatus`.
2. Обратитесь с помощью браузера к веб-серверу, запущенному на вашем компьютере, и убедитесь, что последний работает: `service httpd status` или `/etc/rc.d/init.d/httpd status`.
Если не работает, запустите его так же, но с параметром start.
3. Найдите веб-сервер Apache в списке процессов,определите его контекст безопасности и занесите эту информацию в отчёт. Например, можно использовать команду `ps auxZ | grep httpd` или `ps -eZ | grep httpd`.
4. Посмотрите текущее состояние переключателей SELinux для Apache с помощью команды `sestatus -bigrep httpd`. Обратите внимание, что многие из них находятся в положении «off».
5. Посмотрите статистику по политике с помощью команды `seinfo`, также определите множество пользователей, ролей, типов.
6. Определите тип файлов и поддиректорий, находящихся в директории /var/www, с помощью команды `ls -lZ /var/www`.
7. Определите тип файлов, находящихся в директории /var/www/html: ls -lZ /var/www/html.
8. Определите круг пользователей, которым разрешено создание файлов в директории /var/www/html.
9. Создайте от имени суперпользователя (так как в дистрибутиве после установки только ему разрешена запись в директорию) html-файл /var/www/html/test.html следующего содержания:  
`<html>`  
`<body>test</body>`  
`</html>`
10. Проверьте контекст созданного вами файла. Занесите в отчёт контекст, присваиваемый по умолчанию вновь созданным файлам в директории /var/www/html.
11. Обратитесь к файлу через веб-сервер, введя в браузере адрес `http://127.0.0.1/test.html`. Убедитесь, что файл был успешно отображён.
12. Изучите справку `man httpd_selinux` и выясните, какие контексты файлов определены для httpd. Сопоставьте их с типом файла test.html. Проверить контекст файла можно командой `ls -Z` (`ls -Z /var/www/html/test.html`)
13. Измените контекст файла /var/www/html/test.html с httpd_sys_content_t на любой другой, к которому процесс httpd не должен иметь доступа, например, на samba_share_t:  
`chcon -t samba_share_t /var/www/html/test.html`  
`ls -Z /var/www/html/test.html`. 
После этого проверьте, что контекст поменялся.
14. Попробуйте ещё раз получить доступ к файлу через веб-сервер, введя в
браузере адрес `http://127.0.0.1/test.html`. Вы должны получить сообщение об ошибке:  
`Forbidden
You don't have permission to access /test.html on this server.`
15. Проанализируйте ситуацию. Почему файл не был отображён, если права доступа позволяют читать этот файл любому пользователю?  
`ls -l /var/www/html/test.html`
Просмотрите log-файлы веб-сервера Apache. Также просмотрите системный лог-файл: `tail /var/log/messages`
Если в системе окажутся запущенными процессы setroubleshootd и audtd, то вы также сможете увидеть ошибки, аналогичные указанным выше, в файле /var/log/audit/audit.log. Проверьте это утверждение самостоятельно.
16. Попробуйте запустить веб-сервер Apache на прослушивание ТСР-порта 81 (а не 80, как рекомендует IANA и прописано в /etc/services). Для этого в файле /etc/httpd/httpd.conf найдите строчку Listen 80 и замените её на Listen 81.
17. Выполните перезапуск веб-сервера Apache. Произошёл сбой? Поясните почему?
18. Проанализируйте лог-файлы: `tail -nl /var/log/messages`.  
Просмотрите файлы /var/log/http/error_log, /var/log/http/access_log и /var/log/audit/audit.log и выясните, в каких файлах появились записи.
19. Выполните команду `semanage port -a -t http_port_t -р tcp 81`
После этого проверьте список портов командой `semanage port -l | grep http_port_t`
Убедитесь, что порт 81 появился в списке.
20. Попробуйте запустить веб-сервер Apache ещё раз. Поняли ли вы, почему
он сейчас запустился, а в предыдущем случае не смог?
21. Верните контекст httpd_sys_cоntent__t к файлу /var/www/html/test.html: `chcon -t httpd_sys_content_t /var/www/html/test.html`
После этого попробуйте получить доступ к файлу через веб-сервер, введя в браузере адрес `http://127.0.0.1:81/test.html`. Вы должны увидеть содержимое файла — слово «test».
22. Исправьте обратно конфигурационный файл apache, вернув Listen80.
23. Удалите привязку http_port_t к 81 порту: `semanage port -d -t http_port_t -p tcp 81` и проверьте, что порт 81 удалён.
24. Удалитефайл/var/www/html/test.html: `rm /var/www/html/test.html`


# Теоретическое введение 
Так как по умолчанию пользователи CentOS являются свободными от типа (unconfined в переводе с англ. означает свободный), созданным фвйлам по умолчанию сопоставляется SELinux, пользователь unconfined_u. Это первая часть контекста.  

Далее политика ролевого разделения доступа RBAC используется процессами, но не файлами, поэтому роли не имеют никакого значения для файлов. Роль `object_r` используется по умолчанию для файлов на «постоянных» носителях и на сетевых файловых системах. (В директории /ргос файлы, относящиеся к процессам, могут иметь роль `system_r`. Если активна политика MLS, то могут использоваться и другие роли, например, `secadm_r`. 
Тип `httpd_sys_content_t` позволяет процессу httpd получить доступ к файлу. Благодаря наличию последнего типа можно получить доступ к файлу при обращении к нему через браузер.


# Выполнение лабораторной работы 
	
1. Вошла в систему с полученными учётными данными и убедилась, что SELinux работает в режиме enforcing политики targeted с помощью команд `getenforce` и `sestatus` ([рис. 1](images6/1.png)).

	![Проверка работы SELinux](images6/1.png){ #fig:001 width=70% }

2. Обратилась с помощью браузера к веб-серверу, запущенному на моем компьютере, и убедилась, что последний работает: `service httpd status` или `/etc/rc.d/init.d/httpd status` ([рис. 2](images6/2.png)).

	![Проверка работы веб-сервера](images6/2.png){ #fig:002 width=70% }

3. Нашла веб-сервер Apache в списке процессов, определила его контекст безопасности (unconfined_t). Использовала команду `ps auxZ | grep httpd` ([рис. 3](images6/3.png)).

	![Контекст Apache](images6/3.png){ #fig:003 width=70% }

4. Посмотрела текущее состояние переключателей SELinux для Apache с помощью команды `sestatus -bigrep httpd` ([рис. 4](images6/4.png)). 

	![Состояние переключателей SELinux для Apache](images6/4.png){ #fig:004 width=70% }

5. Посмотрела статистику по политике с помощью команды `seinfo`, также определила множество пользователей, ролей, типов ([рис. 5](images6/5.png)).

	![Статистика по политике](images6/5.png){ #fig:005 width=70% }
	
6. Определила тип файлов и поддиректорий, находящихся в директории /var/www, с помощью команды `ls -lZ /var/www` ([рис. 6](images6/6.png)).
7. Определила тип файлов, находящихся в директории /var/www/html: `ls -lZ /var/www/html` ([рис. 6](images6/6.png)) - нет файлов и поддиректорий в данной директории.
8. Определила круг пользователей, которым разрешено создание файлов в директории /var/www/html ([рис. 6](images6/6.png)) - только владельцу.

	![Типы файлов, поддиректорий и права для /www и /html](images6/6.png){ #fig:006 width=70% }

9. Создала от имени суперпользователя html-файл /var/www/html/test.html следующего содержания:  
`<html>`  
`<body>test</body>`  
`</html>` ([рис. 7](images6/7.png))

	![Создание файла test.html](images6/7.png){ #fig:007 width=70% }
	
10. Проверила контекст созданного файла командой `ls -Z /var/www/html/test.html`. Контекст, присваиваемый по умолчанию вновь созданным файлам в директории /var/www/html - httpd_sys_content_t ([рис. 8](images6/8.png)).

	![Контекст файла test.html](images6/8.png){ #fig:008 width=70% }
	
11. Обратилась к файлу через веб-сервер, введя в браузере адрес `http://127.0.0.1/test.html`. Убедилась, что файл был успешно отображён ([рис. 9](images6/9.png)).

	![Открытие test.html через браузер](images6/9.png){ #fig:009 width=70% }
	
12. Справку по команде `man httpd_selinux` не удалось получить, поэтому я изучила, какие контексты файлов определены для httpd с помощью интернета ([рис. 10](images6/10.png)).

	![Вызов справки httpd](images6/10.png){ #fig:010 width=70% }
	
13. Изменила контекст файла /var/www/html/test.html с httpd_sys_content_t на samba_share_t:   
`chcon -t samba_share_t /var/www/html/test.html`  
`ls -Z /var/www/html/test.html`. 
После этого проверила, что контекст поменялся ([рис. 11](images6/11.png)).

	![Смена контекста файла test.html](images6/11.png){ #fig:011 width=70% }
	
14. Попробуйте ещё раз получить доступ к файлу через веб-сервер, введя в
браузере адрес `http://127.0.0.1/test.html`. Получила сообщение об ошибке ([рис. 12](images6/12.png)).

	![Повторное открытие test.html через браузер](images6/12.png){ #fig:012 width=70% }
	
15. Файл не был отображён, т.к. политика ролевого разделения доступа RBAC используется процессами, но не файлами, поэтому роли не имеют никакого значения для файлов. Тип `httpd_sys_content_t` позволял процессу httpd получить доступ к файлу, а тип `samba_share_t` - нет.
Просмотрела системный лог-файл: `tail /var/log/messages` ([рис. 13](images6/13.png)).

	![Просмотр системного лога](images6/13.png){ #fig:013 width=70% }
	
	В системе были запущенными процессы setroubleshootd и audtd. Посмотрела ошибки, аналогичные указанным выше, в файле /var/log/audit/audit.log.

16. Попробовала запустить веб-сервер Apache на прослушивание ТСР-порта 81 (а не 80, как рекомендует IANA и прописано в /etc/services). Для этого в файле /etc/httpd/httpd.conf нашла строчку Listen 80 и заменила её на Listen 81 ([рис. 14](images6/14.png), [рис. 15](images6/15.png), [рис. 16](images6/16.png)).

	![Открытие httpd.conf](images6/14.png){ #fig:014 width=70% }

	![httpd.conf до исправления](images6/15.png){ #fig:015 width=70% }

	![httpd.conf после исправления](images6/16.png){ #fig:016 width=70% }

17. Выполнила перезапуск веб-сервера Apache ([рис. 17](images6/17.png)).

	![Перезапуск Apache](images6/17.png){ #fig:017 width=70% }
	
18. Проанализировала лог-файлы: `tail -nl /var/log/messages` ([рис. 18](images6/18.png)).  

	![Просмотр логов](images6/18.png){ #fig:018 width=70% }

	Просмотрела файлы /var/log/http/error_log ([рис. 20](images6/20.png)), /var/log/http/access_log ([рис. 19](images6/19.png)) и /var/log/audit/audit.log. Записи появились в error_log и access_log ([рис. 21](images6/21.png)).
	
	![Просмотр access_log](images6/20.png){ #fig:020 width=70% }

	![Просмотр error_log](images6/19.png){ #fig:019 width=70% }

	![Просмотр audit.log](images6/21.png){ #fig:021 width=70% }
	
19. Выполнила команду `semanage port -a -t http_port_t -р tcp 81` и проверила список портов командой `semanage port -l | grep http_port_t` ([рис. 22](images6/22.png)). Порт 81 присутствует в списке.

	![Добавление и просмотр наличия 81 порта](images6/22.png){ #fig:022 width=70% }

20. Попробовала запустить веб-сервер Apache ещё раз ([рис. 23](images6/23.png)). Запуск прошел успешно.

	![Запуск Apache](images6/23.png){ #fig:023 width=70% }
	
21. Вернула контекст httpd_sys_cоntent__t к файлу /var/www/html/test.html: `chcon -t httpd_sys_content_t /var/www/html/test.html` ([рис. 24](images6/24.png)).

	![Возвращение контекста файлу test.html](images6/24.png){ #fig:024 width=70% }
	
	После этого попробовала получить доступ к файлу через веб-сервер, введя в браузере адрес `http://127.0.0.1:81/test.html`. Открылось содержимое файла — слово «test» ([рис. 25](images6/25.png)).

	![Открытие файла test.html через браузер](images6/25.png){ #fig:025 width=70% }
22. Исправила обратно конфигурационный файл apache, вернув Listen80 ([рис. 26](images6/26.png)).

	![Возвращение настроек на 80 порт httpd.conf](images6/26.png){ #fig:026 width=70% }
	
23. Пропробовала удалить привязку `http_port_t` к 81 порту: `semanage port -d -t http_port_t -p tcp 81` - операция запрещена  ([рис. 27](images6/27.png)). Порт 81 остался в списке.

	![Попытка удаления 81 порта](images6/27.png){ #fig:027 width=70% }
	
24. Удалила файл /var/www/html/test.html: `rm /var/www/html/test.html`  ([рис. 28](images6/28.png)).

	![Удаление файла test.html](images6/28.png){ #fig:028 width=70% }


# Выводы 

В процессе выполнения данной лабораторной работы я приобрела навыки администрирования ОС Linux. Получила первое практическое знакомство с технологией SELinux и проверила работу SELinx на практике совместно с веб-сервером Apache.


# Список литературы 

1. Описание лабораторной работы 6 - URL: <https://esystem.rudn.ru/pluginfile.php/1652173/mod_resource/content/2/006-lab_selinux.pdf>
2. Активация Apache - URL: <https://stackoverflow.com/questions/51108495/fresh-install-httpd-service-unit-not-found>


