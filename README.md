# Консольный чат
Итоговый проект по Блоку «Основные конструкции C++»

Версия для Linux и MacOS

[Версия для Windows](https://github.com/MaksMN/ConsoleChatFP11/tree/Windows-version)

1. Участники команды: [Максим](https://github.com/MaksMN/ConsoleChatFP11), [Иван](https://github.com/ivanKispaj/ConsoleChatFP11), [Ирина](https://github.com/Fineralla/ConsoleChatFP11)
2. Тимлид: [Максим](https://github.com/MaksMN/ConsoleChatFP11)
3. Идеи и решения:
   - Функционал консольного чата:
     - регистрация пользователей - логин, пароль, имя;
     - вход в чат по логину/паролю;
     - отправка сообщений конкретному пользователю;
     - обмен сообщениями между всеми пользователями чата одновременно;
     - Дополнительный функционал:
       - система администрирования и модерации;
       - система жалоб на сообщения в публичном чате;
       - блокировка пользователей;
       - Редактирование профиля пользователя. Смена логина, пароля, имени.
   - Структура приложения состоит из двух основных компонентов: база данных(backend) и пользовательский интерфейс(frontend).
     - База данных (backend) состоит из классов и структур, осуществляющих хранение сообщений и данных пользователей. Классы базы данных реализуют методы взаимодействия с классами пользовательского интерфейса. Обеспечивает поиск и выборку данных необходимых для отображения пользователю программы.
     - Пользовательский интерфейс (frontend) обрабатывает ввод пользователя и на основе введенных данных запрашивает в базе отображает необходимую информацию и выводит ее на экран.
    - Чат имеет два основных раздела: общий чат и раздел администратора. Основные разделы ветвятся на подразделы: публичная переписка, личные сообщения, профиль пользователя и прочие вспомогательные меню.
    - На страницах сообщений можно выводить все сообщения разом и только ограниченное количество. Реализован постраничный вывод и навигация по спискам. Выбор опции "навигация по чату" актуальна если количество сообщений превышает количество выводимое на страницу.
    - Списки сообщений на экране выводятся в формате:
      - №сообщения дата Имя\[логин\]\[userID x\]\[banned\]\[admin\]\[messageID n\]
    - Для операций над сообщениями или пользователями нужно вводить userID или messageID.
    - Для использования чата надо выполнять инструкции на экране и вводить предлагаемые значения. Есть защита от неверного ввода.
    - При разработке приложения применялись классы и шаблоны. Обработка исключений реализована в Project/IChatInterface.cpp в методе login() при вводе несуществующего логина пользователя.

4. Пользовательские типы и функции.
   - **ChatRun.h** - точка входа в процессы обеспечивающие работу чата. Здесь задаются умные указатели на базу данных, создаются сервисный администратор и бот для жалоб. Реализовано интерактивное меню выбора раздела чата. Обеспечивает передачу unique_ptr базы данных при переходе пользователя из ChatUserInterface в ChatAdminInterface и обратно.
   - **UserInput** - шаблонный класс. Реализует обработку ввода пользователя. Обрабатывает неверный ввод. Позволяет задать возвращаемые значения отличные от типа входных данных. Например если пользователю надо ввести строки "да" или "нет", в качестве возвращаемых значений можно задать элементы перечислителя chat::yes и chat::no, что очень сильно упрощает процессы ветвления в коде программы. В аргументах конструктора передаются строковые переменные которые надо вывести на экран перед запросом ввода. Методы класса обеспечивают множественный и сквозной ввод. Когда пользователь должен ввести конкретные значения или когда output = input.
     - Методы IOcin, IOcinThrough обеспечивают множественный и сквозной ввод с использованием std::cin. Методы IOgetline, IOgetlineThrough обрабатывают ввод с использованием std::getline.
   - **IChatInterface** - базовый абстрактный класс обеспечивающий интерфейсы разделов пользователей и а администраторов. Содержит базовые методы общего назначения необходимые для всех классов-наследников. Метод login(),вывод списка пользователей, сообщений, pagination, timestamp в дату/время. Код подробно задокументирован в Project/IChatInterface.h.
   - **ChatUserInterface** - наследует IChatInterface. Обеспечивает интерактивный интерфейс пользователя в разделе общего чата. Реализованы методы интерактивных меню: регистрации, входа, общего чата, профиль пользователя, личные сообщения. Код подробно задокументирован в Project/ChatUserInterface.h.
   - **ChatAdminInterface** - наследует IChatInterface. Обеспечивает интерактивный интерфейс раздела администратора. Реализованы методы интерактивных меню: работы с жалобами, блокировкой пользователей, выдачи администраторских прав другим пользователям, удаление сообщений. Код подробно задокументирован в Project/ChatAdminInterface.h.
   - **EnumOptions** - контейнер для хранения перечислителей, которые обеспечивают гибкое ветвление в коде программы.
   - **DBCore.h** - предназначен для обеспечения работы классов базы данных.
   - **DB** - обеспечивает основную работу с базой данных. Реализовано более двадцати методов добавления сообщений и пользователей в базу данных, получение всех данных из базы, обновление данных и их удаление. Позволяет добавлять получать из базы данные пользователей и сообщений по ID и логину, выбирать публичные и приватные сообщения. Проверка уникальности логина. При добавлении данных в базу проверяет и автоматически добавляет timestamp, уникальные ID. обеспечивает корректность и безопасность входных данных. Код подробно задокументирован в Project/DBCore.h.
   - **Message.h** - структура для хранения информации о сообщении в базе данных. В ней хранятся: текст сообщений, отправитель, получатель, timestamp, уникальный id. Реализованы геттеры и сеттеры для работы со всеми необходимыми данными. Код подробно задокументирован в Project/Message.h
   - **User** - структура для хранения информации о пользователе. В ней хранятся: имя, логин, пароль, количество сообщений, уникальный id. Код подробно задокументирован в Project/User.h

5. Распределение задач в команде.
   - [Максим](https://github.com/MaksMN/ConsoleChatFP11). Руководство проектом. Разработчик пользовательского интерфейса публичного раздела чата. Конвертация проекта для работы в [Windows](https://github.com/MaksMN/ConsoleChatFP11/tree/Windows-version).
   - [Иван](https://github.com/ivanKispaj/ConsoleChatFP11). Разработчик ядра базы данных и методов, обеспечивающих взаимосвязь базы с пользовательским интерфейсом. Оптимизация и код-ревью.   
   - [Ирина](https://github.com/Fineralla/ConsoleChatFP11). Разработчик интерфейса раздела администратора чата.