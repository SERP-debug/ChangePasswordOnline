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

<img width="798" height="536" alt="image" src="https://github.com/user-attachments/assets/59762a1a-cf0e-4842-95b7-063d386c080b" />

Третий шаг 
При корректном вводе кода проверки из письма, вводится нужный пароль

<img width="912" height="616" alt="image" src="https://github.com/user-attachments/assets/7a917919-f900-49d1-a30a-22d895e8939f" />

При соблюдении требований парольной политики - пароль меняется

<img width="1017" height="542" alt="image" src="https://github.com/user-attachments/assets/cf045390-fd93-4665-8bb5-7a0db42f6822" />

Настройка программы 
Управление настройками программы осуществляется с помощью http запросов, для этого должен быть опубликован http-сервис config. 

<img width="604" height="335" alt="image" src="https://github.com/user-attachments/assets/51efe87f-089b-4d86-a178-7ac432f1f546" />

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

<img width="514" height="168" alt="image" src="https://github.com/user-attachments/assets/65fba2e7-b4e0-4e24-9e98-dc8ec2ad46e5" />

Проверка подключения с ChangePasswordOnline
GET/checkauth/
URL 
http://localhost/ChangePassword/hs/config_api/v1/checkauth
Headers
token =   - токен полученный методом gentoken
Код состояния
200
Результат в формате JSON

<img width="496" height="179" alt="image" src="https://github.com/user-attachments/assets/0e054518-6bdd-4e99-80b8-637b075a5023" />

Получение текущих настроек
GET/getconfig/
URL 
http://localhost/ChangePassword/hs/config_api/v1/getconfig
Headers
token =   - токен полученный методом gentoken
Код состояния
200
Результат в формате JSON

<img width="442" height="290" alt="image" src="https://github.com/user-attachments/assets/a7b162db-77ca-4bb8-8ebc-3d29c67aba1e" />

Передать необходимые параметры для работы сервиса
POST/postconfig/
URL 
http://localhost/ChangePassword/hs/config_api/v1/postconfig
Headers
token =   - токен полученный методом gentoken
Тело запроса

<img width="1088" height="325" alt="image" src="https://github.com/user-attachments/assets/e548ca50-3113-465e-8b71-c748e6fb8428" />

Код состояния
200
Результат в формате JSON

<img width="480" height="234" alt="image" src="https://github.com/user-attachments/assets/705d66ab-7805-436d-adc8-f95ab48df5fa" />

Проверка доступности IDM из ChangePasswordOnline 
GET/checkidm/
URL 
http://localhost/ChangePassword/hs/config_api/v1/checkidm 
Headers
token =   - токен полученный методом gentoken
Код состояния
200
Результат в формате JSON

<img width="447" height="239" alt="image" src="https://github.com/user-attachments/assets/3382954a-978e-4916-b71c-662129a002b7" />

Включить/Выключить сервис
POST/serviceon/
URL 
http://localhost/ChangePassword/hs/config_api/v1/serviceon
Headers
token =   - токен полученный методом gentoken
Тело запроса
Включить

<img width="632" height="150" alt="image" src="https://github.com/user-attachments/assets/0db73603-f114-4b36-b2c9-f822297faf0b" />

Выключить

<img width="626" height="142" alt="image" src="https://github.com/user-attachments/assets/9b521b4b-4a7e-4525-9858-110520bef55d" />

Код состояния
200
Результат в формате JSON
Включен							

<img width="439" height="220" alt="image" src="https://github.com/user-attachments/assets/c817159c-3e1d-4382-aaaf-176fc2453646" />

Выключен

<img width="443" height="220" alt="image" src="https://github.com/user-attachments/assets/a89352a7-666a-476f-9bbe-39c541511b71" />

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

<img width="460" height="160" alt="image" src="https://github.com/user-attachments/assets/bbca4c48-7b51-4f69-954e-f62ff3506804" />

Бот предоставляет команды управления 
1.	Создание чат бота 
Называем имя бота t.me/ChangePasswordOnline_bot
Use this token to access the HTTP API:
7562668473:AAF5Zjl8g9Thckq2BLMteAOIoqU0QxwFXaU

<img width="552" height="585" alt="image" src="https://github.com/user-attachments/assets/134e0206-61db-40c0-99da-e884b8aad237" />

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

<img width="511" height="770" alt="image" src="https://github.com/user-attachments/assets/ea286adc-065e-4336-8b3d-4d8236247b38" />

<img width="535" height="451" alt="image" src="https://github.com/user-attachments/assets/4d10f2d8-5e11-4e31-af1c-7afe23eaf1d9" />

Сброс настроек (RESET)
В случае если рабочий токен утерян, настройки можно сбросить следующим образом: 
1.	Нужно разрешить вход в тонкий клиент, закомментировав отказ запуска 

<img width="773" height="242" alt="image" src="https://github.com/user-attachments/assets/fd75fc00-da62-486a-908e-76d80e0420d8" />

2.	Настройки хранятся в ХранилищеЗначений РС НастройкиПараметров для очистки достаточно зайти в тонкий клиент и удалить строку настроек. Затем нужно запретить запуск тонкого клиента см. п.1  

<img width="608" height="380" alt="image" src="https://github.com/user-attachments/assets/e083a2b2-bfb0-40b9-8939-dea77f316f8e" />

3.	После сброса все настройки нужно настроить заново. 
