# FABot
![Иллюстрация к проекту](https://github.com/pixlll/FABot/raw/master/files/BHoteUJQ8cA.jpg)  
Бот telegram для показа расписания занятий всех факультетов Финуниверситета.  

# Принцип работы

Приложение работает на **Python 3** и **PostgreSQL** (>= 9.5). Для работы необходим доступ к порталу ФУ (логин и пароль учетной записи студента).  
Регулярно (раз в сутки) бот забирает актуальное расписание ФУ по всем факультетам в собственную базу.  
Telegram-бот взаимодействует с БД бота (postgres), для этого был реализован простой ORM к БД из бэкэнда бота (для коннекта к БД используется **psycopg2**).  
Для упрощения взаимодействия с telegram bot api была использована библиотека **pyTelegramBotAPI** (https://github.com/eternnoir/pyTelegramBotAPI).  
При первом взаимодействии пользователя с ботом, бот запоминает user_id пользователя телеграм (в таблицу users) и указанную им группу (и факультет), для того, чтобы формировать расписание по нажатию одной кнопке в интерфейсе.   
Бот может работать в двух режимах bot-polling и webhook. Для режима webhook необходим веб-сервер и валидный SSL-сертификат (подробнее о webhook можно прочитать в документации к bot api telegram).   
Для реализации функций веб-сервера был использован python-фреймворк werkzeug.
  
Бот рассчитан под размещение на linux-хостах (для Windows могут потребоваться дополнительные правки кодировки и скриптов для запуска).

# Структура проекта

* **DDL/SCHEDULE_DB.sql**	- файл, описывающий структуру БД postgres (DDL-скрипты) т.е. модель данных бэкэнда приложения.
* **config.py** - модуль, который содержить необходимую конфигурационную информацию бота (API KEY, коннекты к БД, логин к порталу ФУ, пути к сертификату и т.п.)   	
* **files/** - каталог для хранения файлов и ключей сертификатов (для режима webhook)  
* **loader/** - каталог, содержит модули для парсинга html-структур портала ФУ. Эти модули необходимы для выгрузки расписания, разбора структур, и загрузки данных в БД.  
* **planner.py** - модуль для регламентого запуска выгрузки расписания по всем группам университета
* **bot_app.py** - модуль для запуска бэкэнд приложения бота.
* **bot_handler.py** - модуль, который содержит функции клиентской части Бота и отвечает за обработку и взаимодействие с кнопками интерфейса бота.  
* **common.py** - модуль, содержит основные классы для обработки данных бэкэнда приложения
* **message.py** - модуль, содержит основные функции для формирования представления информации о расписании (в виде тестовых сообщений)
* **database.py** - модуль, который отвечает за коммуникацию с БД postgres (простой ORM)

# Зависимости проекта

Общее:   
* **Python 3**
* **Postgres >= 9.5**     

Библиотеки python:   

* **pyTelegramBotAPI**
* **requests**
* **BeautifulSoup**
* **lxml**
* **psycopg2**
* **werkzeug**

