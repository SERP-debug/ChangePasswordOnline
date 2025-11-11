Описание работы 
ChangePasswordOnline – это сервис по смене пароля в AD, 
подключение к сервису осуществляется по web – интерфейсу без VPN. 
http://localhost/ChangePassword 
- путь по умолчанию внутри сети, где localhost – имя веб сервера, где опубликована программа.  
Работа программы
Первый шаг
Вводится адрес электронной почты, на который будет выслан код аутентификации 
<img width="791" height="529" alt="image" src="https://github.com/user-attachments/assets/942e5195-a2fc-4cb1-b76f-d6fe478d4f32" />
Второй шаг
Вводится код подтверждения из письма 
 























Третий шаг 
При корректном вводе кода проверки из письма, вводится нужный пароль














При соблюдении требований парольной политики - пароль меняется

















Настройка программы 
Управление настройками программы осуществляется с помощью http запросов, для этого должен быть опубликован http-сервис config. 








Поддерживаемые http методы.
Требование: каждый запрос в заголовке (Header) должен содержать обязательный параметр – token. 
Токен нужно запросить через http метод gentoken – и в дальнейшем использовать для других запросов.  
Стартовый токен 
token = 00000000-0000-0000-0000-000000000000
После запроса рабочего токена, стартовый токен перестает работать. 
Сгенерировать токен
GET/gentoken/
URL 
http://localhost/ChangePassword/hs/config_api/v1/gentoken
Headers
token = 00000000-0000-0000-0000-000000000000
Код состояния
200
Результат в формате JSON




Проверка подключения с ChangePasswordOnline
GET/checkauth/
URL 
http://localhost/ChangePassword/hs/config_api/v1/checkauth
Headers
token =   - токен полученный методом gentoken
Код состояния
200
Результат в формате JSON




Получение текущих настроек
GET/getconfig/
URL 
http://localhost/ChangePassword/hs/config_api/v1/getconfig
Headers
token =   - токен полученный методом gentoken
Код состояния
200
Результат в формате JSON
 
Передать необходимые параметры для работы сервиса
POST/postconfig/
URL 
http://localhost/ChangePassword/hs/config_api/v1/postconfig
Headers
token =   - токен полученный методом gentoken
Тело запроса

 
Код состояния
200
Результат в формате JSON







Проверка доступности IDM из ChangePasswordOnline 
GET/checkidm/
URL 
http://localhost/ChangePassword/hs/config_api/v1/checkidm 
Headers
token =   - токен полученный методом gentoken
Код состояния
200
Результат в формате JSON






Включить/Выключить сервис
POST/serviceon/
URL 
http://localhost/ChangePassword/hs/config_api/v1/serviceon
Headers
token =   - токен полученный методом gentoken
Тело запроса
Включить






Выключить






Код состояния
200
Результат в формате JSON
Включен							Выключен









Управление REST запросами из PowerShell
https://morgantechspace.com/2024/01/working-with-rest-api-in-powershell-using-invoke-restmethod.html
Проверка подключения с ChangePasswordOnline;
$APIUri = "http://localhost/ChangePassword/hs/config_api/v1/checkauth"
$AccessToken = "6aa1da53-3543-4a8a-bb57-e401feae3437" #Replace your OAuth Access Token
$Headers = @{token = "$AccessToken"}
$Response = Invoke-RestMethod -Method Get -Uri $APIUri -Headers $Headers
$Response

Включение сервиса: 
$APIUri = "http://localhost/ChangePassword/hs/config_api/v1/serviceon"
$AccessToken = "6aa1da53-3543-4a8a-bb57-e401feae3437" #Replace your OAuth Access Token
$Headers = @{token = "$AccessToken"}
 
$Data = @{
    "ServiceOn" = "true"
} 
 
$JsonBody = $Data | ConvertTo-Json
 
$Response = Invoke-RestMethod -Method POST -Uri $APIUri -Headers $Headers -Body $JsonBody  -ContentType "application/json"
$Response

Выключение сервиса:
$APIUri = "http://localhost/ChangePassword/hs/config_api/v1/serviceon"
$AccessToken = "6aa1da53-3543-4a8a-bb57-e401feae3437" #Replace your OAuth Access Token
$Headers = @{token = "$AccessToken"}
 
 
$Data = @{
    "ServiceOn" = "false"
} 
 
$JsonBody = $Data | ConvertTo-Json
 
$Response = Invoke-RestMethod -Method POST -Uri $APIUri -Headers $Headers -Body $JsonBody  -ContentType "application/json"
$Response








Подключение Telegram
Для работы бота ChangePasswordOnline_bot: 
Token: 7562668473:AAF5Zjl8g9Thckq2BLMteAOIoqU0QxwFXaU
Id_chat: -4759492177

Как эти данные были получены?
Вызов настройки чат-бота telegram








Бот предоставляет команды управления 
1.	Создание чат бота 
Называем имя бота t.me/ChangePasswordOnline_bot
Use this token to access the HTTP API:
7562668473:AAF5Zjl8g9Thckq2BLMteAOIoqU0QxwFXaU





2.	Создание группы куда будет отправлять сообщения TelegramBot.
Available commands:
» /admins to get a list of all current admins.
» /json in reply to a message to get it's bot api json variant.
» /user in reply to a message for info about that user.

This chat
 ├ id: -4759492177
 ├ title: Логирование ChangePasswordOnline
 ├ type: group
 └ all_members_are_administrators: true

Для присоединения к группе нужно перейти
invite line: https://t.me/+3C70s9IsXwJjZjQy 

Сброс настроек (RESET)
В случае если рабочий токен утерян, настройки можно сбросить следующим образом: 
1.	Нужно разрешить вход в тонкий клиент, закомментировав отказ запуска 






2.	Настройки хранятся в ХранилищеЗначений РС НастройкиПараметров для очистки достаточно зайти в тонкий клиент и удалить строку настроек. Затем нужно запретить запуск тонкого клиента см. п.1  


 







3.	После сброса все настройки нужно настроить заново. 
