# breeze
Linux auto config (https://habrahabr.ru/post/309628/)

Администрирование → Скрипт для тех, кому лень разбираться в Linux из песочницы
Настройка Linux*
Сфер применения Linux может быть очень много. Особенно, когда арендовать VPS стало можно от $1 в месяц. Кроме стандартного использования под хостинг сайтов, его используют в качестве сервера для игр (CS:GO, Terraria, Minecraft), в качестве Proxy-сервера и VPN-сервера. Под майнинг криптовалют. Под резервное хранилище бэкапов. Под домашнюю торренто-качалку. А также для тестирования, разработки и просто различных экспериментов. Именно доступность VPS на базе Linux с огромным спектром возможного его применения привела к популяризации Linux. Но желающих использовать Linux значительно больше, чем людей, которые умеют его использовать. И часто именно слабые познания администрирования Linux останавливают людей от его использования. Ну или просто усложняют таким людям жизнь — им приходится часами ковыряться в мануалах, форумах и «статьях для новичков».

Да мне и самому надоело лазить по специализированным форумам, каждый раз, когда приходится сделать шаг влево или шаг вправо относительно того, что я уже научился делать. Именно поэтому, со временем, все типовые вещи я свёл в один скрипт с дружелюбным интерфейсом, который умеет делать всё сам. Начиналась всё с малого. Скрипт просто автоматизировал установку нужного мне софта. Но за полгода он превратился уже в весьма серьёзную утилиту весом 85 Кб, в которой более 2100 строк кода. Скрипт ранее нигде не выкладывался. Использовался только в личных целях мной и несколькими моими товарищами. Пришло время им поделиться с публикой. Уверен, многим людям он способен сэкономить кучу времени.

Чтобы понять, что он умеет, проще всего глянуть на заглавный скриншот:



Далее подробнее опишу, что и как он делает.

Информация о системе

Этот раздел в дружелюбной форме выводит информацию об этом сервере. Что за железо, какая ОС, какой IP-адрес. Причём, IP адрес он сначала пытается определить по интерфейсу, но т.к. бывают VPS без выделенного IP-адреса (за NAT), далее скрипт лезет в интернет и с помощью сторонних сайтов смотрит с какого IP пришли запросы и показывает реальный внешний IP-адрес (на это тратится пара секунд). Выглядит это информационное окно так:



Работа с ОС

В этом разделе собрано несколько как мелких утилит, типа смены пароля root, установки часового пояса, обновление ОС, добавление репозитория, установка популярных приложений (типа midinght commander), так и несколько более серьезных, на которых остановлюсь подробнее:

Антивирус. Да, на Линуксе не бывает вирусов. Но бывают бэкдоры, которые через сайт открывают доступ ко всем вашим файлам злоумышленникам. Через эти же бэкдоры спамеры потом могут делать спам-рассылки используя ваш сервер. У меня в своё время за это забанили VPS. Поэтому, я предусмотрел установку антивируса. Разбираться в нём не нужно. Сам всё ставит, обновляет. Далее, не разбираясь в синтаксисе антивируса, через скрипт можно проверить весь диск или конкретную папку (например, где хостятся сайты).

Firewall. Не все умеют настраивать правила Firewall (iptables). А в некоторых арендуемых VPS он преднастроен и включен, причём так, что устанавливаемые вами сервисы не будут доступны. И вам придётся копаться в настройках и открывать порты. А неправильная и необдуманная настройка firewall может вообще привести к тому, что вы после этого в принципе не сможете подключиться по SSH к своему серверу и вам придется либо переустанавливать ОС, либо как-то лезть на сервер через web-терминал, но эта функция есть не у всех хостеров. В случае использования моего скрипта такие проблемы исключены, а настройка Firewall происходит через «Помощник», где нужно просто ответить на простые вопросы.

Планировщик задач (cron). Бывает, что нужно периодически выполнять какие-то типовые действия: проверка на вирусы, очистка логов, выкладывание бэкапов и т.д. Многие знают, что в Linux есть планировщик, который может выполнять задания по расписанию, но не всё умеют им пользоваться. Мой скрипт позволяет установить, включить, выключить планировщик и добавить в него задание, выбрав интервал запуска.

Установка панели управления хостингом

Очень часто Linux используется именно для хостинга, но руками настраивать там все сервисы типа: Apache, Nginx, PHP, MySQL, почтовый демон и так далее — совсем непросто для новичков. Большинство предпочитает установить какую-либо панель управления. Но даже её нужно сначала как-то установить. В своём скрипте я собрал пять бесплатных панелей управления сайтом (Vesta CP, Webuzo, CWP, ZPanel, Ajenti) и платную ISPmanager (которая является, пожалуй, самой распространенной панелью в России). Про каждую панель есть небольшое описание, системные требования. Выбираем нужную панель, скрипт сам её скачает с официального сайта (свежую версию) и установит.

Работа с VPN

В последнее время многие заводят себе VPN для того, чтобы получить преимущества пользователей других географических зон. Например, обход запретов РосКомНадзора, использование каких-то внутренних американских или европейских сервисов и так далее. Многие для этого покупают готовые VPN-сервисы. Но значительно дешевле купить себе VPS в нужной стране и поднять свой собственный VPN. Вот только не все умеют его настраивать. С помощью этого скрипта вам нужно просто отвечать на вопросы и всё. Весь нужный софт установится сам, в firewall пропишутся нужные правила. Вы сможете просматривать, добавлять и удалять пользователей, которым разрешен доступ. Причём скрипт проанализирует какая у вас ОС и сделает всё, учитывая особенности конкретно этой ОС.

Работа с Proxy

Некоторым привычнее использовать прокси вместо VPN. Ну и это зачастую дешевле, потому что Прокси, в отличии от VPN, можно использовать на серверах, у которых нет своего выделенного IP-адреса (которые находятся за NAT), а такие сервера стоят в несколько раз дешевле (их можно приобрести за $2 в год). Поднимать свой прокси-сервер на Линукс — тоже не самая простая задача для новичков. Но этот скрипт всё сделает за вас. Причём там очень много настроек. «Помощник» при установке спросит на каком порту нам нужен Прокси (или же предложит стандартный), спросит нужна ли авторизация по логину/паролю (или пускать всех, кто знает адрес и порт). Скрипт даже учтёт потребности тех пользователей, кто любит загонять сторонний трафик в Прокси (через программы типа Proxifier) и настроит конфиг нужным образом. Ну и, естественно, сам внесёт все нужные правила в Firewall (iptables). Настройка прокси ещё никогда не была такой простой. Руками в конфиг лазить вообще не нужно.

Работа с файлами и программами

В этом разделе можно установить нужную программу (пакет) или удалить. Причём самое полезное там — это именно правильное удаление. Дело в том, что при установке какой-либо программы очень часто к ней устанавливается несколько сопутствующих пакетов, необходимых для её работы. И зачастую, размер этих дополнительных пакетов превышает размер собственно той программы, которую вы устанавливали. Но вот при удалении вашей программы удалится только сама программа, а все пакеты, которые были нужны для её (и только её) работы — останутся и продолжат занимать место. В моём скрипте я предусмотрел полное удаление программы — со всем дополнительным софтом, который ставился вместе с ней. В скрипте просто указываем название программы, дальше скрипт всё сделает сам.

Очистка системы

В случае активного использования сервера под хостинг, часто копится огромное количества логов доступа к сайтам. Объём всего этого мусора иногда может достигать гигабайт. Не все знают где и как это удалять. В этом разделе можно почистить эти логи. Удаляются логи Apache и Nginx, как целиком, так и конкретного пользователя. Кроме этого, из этого раздела можно удалить старые установочные пакеты, которые по умолчанию остаются на диске после установки софта и продолжают занимать место.


На этом, собственно, пока всё. В планах, конечно, много что ещё хочется добавить. Например, бенчмарк, для оценки производительности различных компонентов сервера. Расширить поддержку других ОС и так далее. По мере сил и времени, я собственно этим и занимаюсь. Но, уже сейчас, он выполняет довольно много функций, востребованных новичками и способен сильно упростить им жизнь.

Теперь собственно, очень важный вопрос. А на каких дистрибутивах Linux всё это работает? На всех версиях CentOS. А также, на всех прочих производных от Red Hat Enterprise Linux дистрибутивах (например, Scientific Linux). Почему именно RHEL? Ну скрипт я писал для себя, а мне CentOS ближе. Ну и если говорить о целевой аудитории данного скрипта (новички), то им, как правило, без разницы на каком дистрибутиве работать. Ведь обычно они одинаково плохо знают любой из них. А RHEL весьма неплох, стабилен, нетребователен к ресурсам, ну и, самое главное, присутствует почти у каждого хостера VPS. Версию CentOS при этом можно выбрать любую (5, 6, 7), как и разрядность. Для всех действий скрипт анализирует какая версия дистрибутива и учитывает специфику именно этой версии. Я, обычно, выбираю дистрибутив CentOS 6.x (можно Minimal) с разрядностью 64 бита.

Ну и самое главное. Как собственно обзавестись этим скриптом? Ниже прилагаю полный листинг кода скрипта со всеми потрохами и подробными комментариями (для тех, кто хочет что-то своё дописать):

Исходный код скрипта
Всё что вам нужно, чтобы его использовать — это создать файл [название].sh и засунуть в него это содержимое. После чего запустить командой «sh [название].sh». Создать его можно как на сервере, так и на своем компьютере, а потом скопировать на сервер. Я, например, закинул его себе на домашний сервер и на каждом новом VPS вбиваю команду:

cd /root/
wget http://evtikhov.ru/breeze.sh -r -N -nd
cat >> /root/.bashrc <<END
alias breeze='cd /root/
sh breeze.sh'
END
exit

После этого под рутом в терминале одной командой «breeze» запускаю его.

P.S. Вообще, частенько возникает вопрос доверия использования чужих скриптов и это правильно. Но прелесть скриптов на bash в том, что перед их запуском можно открыть и посмотреть его. И убедиться в том, что он не делает ничего плохого. Функцию обновления скрипта вас использовать никто не заставляет. В общем-то, вас вообще никто не заставляет его использовать. Всегда можно просто посмотреть как настраивается VPN, Proxy и прочие вещи и вручную вколотить пару десятков команд, разобравшись в них. Обилие комментариев в коде даже поможет в них разобраться.
