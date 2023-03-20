# ConsoleChatFP11
Итоговый проект по Блоку «Основные конструкции C++» 11.8

На этом этапе реализовано: регистрация пользователя и вход в чат

**Project\ChatRun.h**
Является точкой входа в программу
В нем объявляется:

Указатель на базу данных:
```cpp
std::unique_ptr<DB> db = std::make_unique<DB>();
```

Создание сервисного админа и добавление его в базу:
```cpp
User serviceAdmin("admin", "admin", "1234");
db->addUser(serviceAdmin);
```
Далее объявляются дочерние классы, в которые передается указатель на базу данных:
```cpp
ChatUserInterface userInterface();
ChatAdminInterface adminInterface();
```

Далее идет запрос данных пользователя куда он хочет пойти, в чат или раздел администратора.

В зависимости от выбора пользователя вызываются методы классов
```cpp
userInterface.run(std::move(db)); 
```
или 
```cpp
adminInterface.run(std::move(db));
```
Так как база данных чата является единой для всех интерфейсов, нужно не забывать ее перемещать из класса в класс.

С вызова этих методов методов стартует вся дальнейшая логика интерфейса. Из них мы уже вызываем все методы и функции которые будут выводить на экран сообщения показываемые пользователю. Внутри этих методов мы можем использовать любые обращения к базе данных.

userInterface.run  и adminInterface.run должны возвратить элемент перечислителя enum Results. То-есть login_success, register_cancel, app_exit итп.

**Project\IChatInterface**
Это абстрактный базовый класс. В нем определены и реализованы переменные и методы которые мы можем использовать одновременно в классах ChatUserInterface и ChatAdminInterface

В нем реализовано:

Перечисляемый тип. Удобно для возврата методами которые работают непосредственно с пользователями.
```cpp
enum Input
{
    login_success,
    login_cancel,
    register_success,
    register_cancel,
    yes,
    no,
    app_exit,
    back,
    empty
};
```
Нам будет удобно если после обработки запроса пользователя вернуть вызывающему методу значения ok cancel close итп.

Поля:
```cpp
std::unique_ptr<DB> db; //база данных
std::unique_ptr<User> user;
```
user это активный авторизованный пользователь чата.

```cpp
Result login();
```
Запрашивает логин пароль и при успешной авторизации присваивает полю user умный указатель на пользователя.
Это внутренний метод класса не виртуальный. Он реализован и является общим для всех дочерних классов.

```cpp
 virtual Results run(std::unique_ptr<DB> _db) = 0; 
 ``` 
 Этот метод должен быть реализован в дочерних классах. С него стартуют все другие методы осуществляющие взаимодействие пользователя с программой.