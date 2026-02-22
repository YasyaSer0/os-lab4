# МІНІСТЕРСТВО ОСВІТИ І НАУКИ УКРАЇНИ  
## КИЇВСЬКИЙ ФАХОВИЙ КОЛЕДЖ ЗВ’ЯЗКУ  

---

# ЗВІТ  
## про виконання лабораторної роботи №4
### з дисципліни «Операційні системи»

---

**Тема:**  
Команди Linux для управління процесами

---

**Виконала:**  
студентка групи **БІКС-33**  
**Сербіна Ярослава Вячеславівна**

---

**Перевірила:**  
**Сушанова Вікторія Сергіївна**

---

**Київ - 2026**

---

# Лабораторна робота №4
## Операційні системи

---

## Тема
Команди Linux для управління процесами

---

## Мета роботи
1. Отримання практичних навиків роботи з командною оболонкою Bash.
2. Знайомство з базовими командами для управління процесами.

---

## Матеріальне забезпечення занять:
1. ЕОМ типу IBM PC.  
2. ОС сімейства Windows та віртуальна машина VirtualBox (Oracle).  
3. ОС GNU/Linux (будь-який дистрибутив).  
4. Сайт мережевої академії Cisco netacad.com та його онлайн курси по Linux.

---

## Завдання для попередньої підготовки
### Glossary of Basic English Terms 

**Process** - A program that is currently running on the system.

**Process ID (PID)** - A unique identification number assigned to each running process.

**ps Command** - A command used to display information about processes running on the system.

**top Command** - A real-time monitoring tool that displays dynamic information about running processes.

**Unix-style Parameters** - Command options preceded by a single dash (-).

**GNU Long Parameters** - Command options preceded by a double dash (--).

**Full Format Listing (-f)** - An option of the ps command that displays extended process information.

**Long Listing (-l)** - An option of the ps command that displays detailed process information with additional columns.

**All Processes (-e)** - An option that shows all processes running on the system.

**TTY** - The terminal device from which a process was started.

**CPU Utilization (%CPU)** - The percentage of processor time used by a process.

**Memory Utilization (%MEM)** - The percentage of physical memory used by a process.

**Virtual Memory (VIRT)** - The total amount of virtual memory used by a process.

**Resident Memory (RES)** - The amount of physical memory currently used by a process.

**Process State** - The current status of a process (running, sleeping, stopped, or zombie).

**Signal** - A predefined message sent to a process to control its behavior.

**TERM Signal (15)** - A signal that requests a process to terminate if possible.

**KILL Signal (9)** - A signal that forcefully and unconditionally terminates a process.

**STOP Signal (17)** - A signal that stops a process without terminating it.

**CONT Signal (19)** - A signal that resumes a stopped process.

**kill Command** - A command used to send signals to processes using their PID.

**killall Command** - A command used to terminate processes by their name instead of PID.

**Real-Time Monitoring** - The ability to observe system processes continuously as they change.


## Відповіді на теоретичні питання

### Які команди для моніторингу стану процесів ви знаєте? Як переглянути їх можливі параметри?

Для моніторингу процесів у Linux використовуються команди **ps** та **top**.

Команда **ps** дозволяє переглянути інформацію про процеси, які виконуються в системі (PID, користувач, час роботи, стан тощо).

Команда **top** використовується для моніторингу процесів у реальному часі. Вона показує завантаження процесора, використання пам’яті та список активних процесів.

Переглянути можливі параметри команд можна за допомогою:
- `man ps`
- `man top`
- або `ps --help`

### Чи може команда ps у реальному часі відслідковувати стан процесів?

Ні, команда **ps** не відслідковує процеси у реальному часі.  
Вона показує стан процесів лише на конкретний момент виконання команди.

Для моніторингу процесів у реальному часі використовується команда **top**.

### За якими параметрами можливе сортування процесів в команді top? Як переключатись між ними?

За замовчуванням команда **top** сортує процеси за параметром **%CPU** (відсоток використання процесора).

Також можна змінити сортування за іншими полями, наприклад:
- %CPU
- %MEM
- PID
- TIME

Щоб переключатися між параметрами сортування, потрібно під час роботи `top` натиснути клавішу **f** та вибрати потрібне поле.

Вийти з команди `top` можна натиснувши **q**.

### Які команди для завершення роботи процесів ви знаєте?

Для завершення роботи процесів у Linux використовуються команди **kill** та **killall**.

Команда `kill` використовується для надсилання сигналу процесу за його PID (Process ID). 
За замовчуванням вона надсилає сигнал **TERM (15)**, який коректно завершує процес, якщо це можливо.

Приклад:
`kill 1234`

Якщо процес не завершується, можна використати примусовий сигнал **KILL (9)**:
`kill -9 1234`

Команда `killall` дозволяє завершувати процеси за їх назвою, а не за PID. 
Це зручно, якщо потрібно завершити одразу декілька процесів з однаковою назвою.

Приклад:
`killall sleep`

Таким чином, основними командами для завершення процесів є `kill` та `killall`, які працюють через систему сигналів Linux.

---

## Хід роботи

**1. Запуск системи**

Я запустила віртуальну машину **VM2_Linux_CLI** у VirtualBox та виконала вхід у систему.  
Після авторизації відкрився командний рядок (CLI), у якому виконувалась лабораторна робота.

<img width="967" height="926" alt="image" src="https://github.com/user-attachments/assets/b1e39eee-7477-44bd-85e9-7fadbd27ee17" />

Скрін 1 - Запущена віртуальна машина та вхід у систему

**2. Робота з директорією /proc**

Команди:
```bash
ls /proc | head
cat /proc/cpuinfo | head
cat /proc/meminfo | head
```
Пояснення:

Директорія /proc знаходиться у кореневому каталозі /.
Вона є віртуальною файловою системою, яка створюється ядром під час роботи системи.

У ній міститься:
- інформація про всі запущені процеси (каталоги з PID),
- дані про процесор,
- інформація про оперативну пам’ять,
- параметри ядра.

Файли у /proc не зберігаються фізично на диску - вони формуються динамічно.

<img width="549" height="422" alt="image" src="https://github.com/user-attachments/assets/dd7cfdd5-8e54-439f-bb70-86681e3ccbce" />

<img width="730" height="245" alt="image" src="https://github.com/user-attachments/assets/b6029dd3-e3e4-4579-b253-4c5b636c9c5f" />

<img width="751" height="243" alt="image" src="https://github.com/user-attachments/assets/6f6830bd-32df-473b-be05-ec83ea788748" />

Скрін 2 - Вміст директорії /proc та системна інформація

**3. Інформація про поточні сеанси користувачів**

Команда:
```bash
w
```
або
```bash
who
```

Пояснення:
Команда w показує:
- активних користувачів,
- час входу в систему,
- з якого терміналу виконано вхід,
- навантаження системи.

Команда who виводить список користувачів, які наразі працюють у системі.

<img width="814" height="157" alt="image" src="https://github.com/user-attachments/assets/395ea868-8c43-49cc-aeb9-a6346fb76ce1" />

Скрін 3 - Інформація про активні сеанси користувачів

**4. Комбінації клавіш у терміналі**

4.1 Використання Ctrl + C

Команда для демонстрації:
```bash
sleep 100
```
Після запуску команди було натиснуто комбінацію клавіш:
```bash
Ctrl + C
```
Пояснення:

Комбінація Ctrl + C використовується для примусового завершення виконання команди.
Вона надсилає сигнал SIGINT (Interrupt) процесу.

У результаті:
- виконання команди зупиняється,
- процес завершується,
- з’являється символ ^C,
- командний рядок повертається.
  
<img width="459" height="86" alt="image" src="https://github.com/user-attachments/assets/e3140758-39ce-498a-b75e-d0b5cafc6cb3" />

Скрін 4 - Демонстрація Ctrl + C
  
4.2 Використання Ctrl + Z

Команда для демонстрації:
```bash
sleep 200
```
Після запуску було натиснуто:
```bash
Ctrl + Z
```
Потім виконано:
```bash
jobs
```
Пояснення:

Комбінація Ctrl + Z призупиняє виконання процесу.
Процес не завершується, а переходить у стан Stopped.

Команда jobs дозволяє переглянути список призупинених або фонових задач.

У результаті видно:
```bash
[1]+ Stopped sleep 200
```

 <img width="498" height="167" alt="image" src="https://github.com/user-attachments/assets/90c5aa49-8cbf-4a34-bcd7-f1704a4b83fc" />

 Скрін 5 - Демонстрація Ctrl + Z та команда jobs

4.3 Використання Ctrl + D
Пояснення:

Комбінація Ctrl + D використовується для:
- завершення сеансу терміналу,
- виходу з shell,
- передачі сигналу EOF (End Of File).

Якщо натиснути Ctrl + D у терміналі:
- відбудеться вихід із поточного сеансу,
- або завершиться введення даних.
  
**5. Фонові та звичайні процеси**

- Процес у режимі foreground займає термінал до завершення роботи.
- Процес у режимі background виконується у фоновому режимі та не блокує термінал.
- Фонові процеси використовуються для довготривалих задач, щоб можна було продовжувати роботу в терміналі.

**6. Команди jobs, fg, bg**

- jobs - показує список запущених або призупинених задач.
- fg - переводить процес на передній план.
- bg - відновлює виконання процесу у фоновому режимі.

 ### Практична частина
**7. Запуск команди top**

Команда:
```bash
top
```
Пояснення:

Команда top використовується для моніторингу процесів у реальному часі.

Вона показує:
- кількість процесів,
- завантаження процесора,
- використання оперативної пам’яті,
- список активних процесів із параметрами %CPU, %MEM, PID тощо.

За замовчуванням процеси сортуються за параметром %CPU.

Для призупинення команди top було використано комбінацію клавіш:
```bash
Ctrl + Z
```
<img width="829" height="509" alt="image" src="https://github.com/user-attachments/assets/b3910adf-56dc-483a-b054-882f407ff65a" />

Скрін 4 — Робота команди top

**8. Демонстрація роботи з фоновим процесом (sleep)**

Для демонстрації керування процесами було запущено команду:

```bash
sleep 200
```
Після запуску процесу його було призупинено за допомогою комбінації клавіш:
```bash
Ctrl + Z
```
Після цього виконано команду:
```bash
jobs
```
Пояснення:

Команда sleep 200 запускає процес, який виконується протягом 200 секунд.
Після натискання Ctrl + Z процес не завершується, а переходить у стан Stopped (призупинений).

Команда jobs дозволяє переглянути список фонових або призупинених задач поточного терміналу.
У результаті видно, що процес sleep 200 знаходиться у статусі Stopped.

<img width="515" height="120" alt="image" src="https://github.com/user-attachments/assets/83848ac2-aac7-4ce9-9cd2-becfe47dad6d" />

Скрін 5 — Запуск sleep та призупинення процесу

**9. Відновлення та завершення фонового процесу**

Для відновлення призупиненого процесу у фоновому режимі було виконано:

```bash
bg %1
```
Після цього перевірено стан процесу:
```bash
jobs
```
Пояснення:

Команда bg %1 відновлює процес №1 та переводить його у фоновий режим (background).
Процес продовжує виконання, але не блокує термінал.
Статус задачі змінюється на Running.

<img width="482" height="91" alt="image" src="https://github.com/user-attachments/assets/c507bba8-d103-450d-89c0-3c19d4502c9d" />

Скрін 7 — Відновлення sleep у background

Для завершення процесу спочатку було визначено його PID:
```bash
ps | grep sleep
```
Після цього виконано команду:
```bash
kill PID (3284)
```
Пояснення:
- Команда ps | grep sleep дозволяє знайти PID процесу.
- Команда kill надсилає сигнал завершення процесу.

Після завершення процес зникає зі списку задач (jobs).

<img width="660" height="152" alt="image" src="https://github.com/user-attachments/assets/691f3c31-f26a-41a9-a313-2e807b63e92f" />

Скрін 8 — Завершення процесу sleep
