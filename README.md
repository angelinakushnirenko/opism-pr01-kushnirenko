# Практична робота № 1

**Дисципліна:** Основи побудови інформаційних систем та мереж  
**Тема:** Спостереження за процесом звернення до вебресурсу. Побудова власної моделі рівнів взаємодії

| | |
|---|---|
| **Прізвище, ім'я** | Кушніренко Ангеліна |
| **Група** | ЗКБЗІ-21 |
| **Номер варіанта** | 3 |
| **Домен варіанта** | example.net |
| **Середовище виконання** | Windows (PowerShell 5.1) |
| **Версія curl** | `curl 8.22.0 (Windows) libcurl/8.22.0 Schannel zlib/1.3.2 WinIDN WinLDAP` |
| **Дата виконання** | 20.11.2026 |

---

## Частина A. Збір експериментальних даних

### A.1. Запит із діагностичним виводом

**Команда:**
```powershell
curl.exe -v [https://example.net](https://example.net)
```

**Вивід:**
```text
* Host example.net:443 was resolved.
* IPv6: (none)
* IPv4: 104.20.21.8, 172.66.175.59
*   Trying 104.20.21.8:443...
*   Trying 172.66.175.59:443...
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* ALPN: server accepted http/1.1
* Established connection to example.net (104.20.21.8 port 443) from 192.168.0.135 port 62048
* using HTTP/1.x
> GET / HTTP/1.1
> Host: example.net
> User-Agent: curl/8.22.0
> Accept: */*
>
* Request completely sent off
* schannel: remote party requests renegotiation
* schannel: renegotiating SSL/TLS connection
* schannel: SSL/TLS connection renegotiated
* schannel: remote party requests renegotiation
* schannel: renegotiating SSL/TLS connection
* schannel: SSL/TLS connection renegotiated
< HTTP/1.1 200 OK
< Date: Fri, 20 Nov 2026 17:43:16 GMT
< Content-Type: text/html; charset=utf-8
< Transfer-Encoding: chunked
< Connection: keep-alive
< Server: cloudflare
< last-modified: Thu, 19 Nov 2026 22:11:52 GMT
< allow: GET, HEAD
< Accept-Ranges: bytes
< Age: 12041
< Cache-Control: max-age=14400
< cf-cache-status: HIT
< CF-RAY: a5d94186fce4e199-WAW
< alt-svc: h3=":443"; ma=86400
<
<!doctype html><html lang=en><head><meta charset=utf-8><link rel=icon href=data:,><meta name=viewport content="width=device-width,initial-scale=1"><title>Example Domain</title><style>html{color-scheme:light dark;background:light-dark(#eee,#222)}body{font:16px/1.6 system-ui,sans-serif;max-width:26em;margin:auto;padding:25vh 2em 2em;text-align:center}</style></head><body><p>This domain is for use in documentation examples without needing permission. This is not a service; avoid relying on it for testing and monitoring purposes.</p><script src=/s.js></script></body></html>
* Connection #0 to host example.net:443 left intact
```

---

### A.2. Запит без захисту з'єднання

**Команда:**
```powershell
curl.exe -v [http://neverssl.com](http://neverssl.com)
```

**Вивід:**
```text
* Host neverssl.com:80 was resolved.
* IPv6: (none)
* IPv4: 34.223.124.45
*   Trying 34.223.124.45:80...
* connect to 34.223.124.45 port 80 from 0.0.0.0 port 53378 failed: Timed out
* Failed to connect to neverssl.com:80 after 22997 ms: Could not connect to server
* closing connection #0
curl: (28) Failed to connect to neverssl.com:80 after 22997 ms: Could not connect to server
```

---

### A.3. Запит до служби доменних імен

**Команда (перше виконання):**
```powershell
Resolve-DnsName example.net
```

**Вивід:**
```text
Name                                           Type   TTL   Section    IPAddress
----                                           ----   ---   -------    ---------
example.net                                    AAAA   600   Answer     2606:4700:10::ac42:af3b
example.net                                    AAAA   600   Answer     2606:4700:10::6814:1508
example.net                                    A      600   Answer     104.20.21.8
example.net                                    A      600   Answer     172.66.175.59
```

**Команда (повторне виконання через ~8–9 хвилин):**
```powershell
Resolve-DnsName example.net
```

**Вивід:**
```text
Name                                           Type   TTL   Section    IPAddress
----                                           ----   ---   -------    ---------
example.net                                    AAAA   78    Answer     2606:4700:10::ac42:af3b
example.net                                    AAAA   78    Answer     2606:4700:10::6814:1508
example.net                                    A      78    Answer     172.66.175.59
example.net                                    A      78    Answer     104.20.21.8
```

**Зафіксовані значення:**

| Параметр | Перше виконання | Повторне виконання |
|---|---|---|
| Час виконання (год:хв) | 19:43 | 19:52 |
| IP-адреса | IPv4: `104.20.21.8`, `172.66.175.59`; IPv6: `2606:4700:10::ac42:af3b`, `2606:4700:10::6814:1508` | IPv4: `172.66.175.59`, `104.20.21.8`; IPv6: `2606:4700:10::ac42:af3b`, `2606:4700:10::6814:1508` |
| Значення TTL | 600 | 78 |

---

### A.4. Контрольний ресурс

**Команда:**
```powershell
curl.exe -v [https://google.com](https://google.com)
```

**Вивід:**
```text
* Host google.com:443 was resolved.
* IPv6: (none)
* IPv4: 142.250.109.113, 142.250.109.139, 142.250.109.101, 142.250.109.138, 142.250.109.100, 142.250.109.102
*   Trying 142.250.109.113:443...
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* ALPN: server accepted http/1.1
* Established connection to google.com (142.250.109.113 port 443) from 192.168.0.135 port 51004
* using HTTP/1.x
> GET / HTTP/1.1
> Host: google.com
> User-Agent: curl/8.22.0
> Accept: */*
>
* Request completely sent off
* schannel: remote party requests renegotiation
* schannel: renegotiating SSL/TLS connection
* schannel: SSL/TLS connection renegotiated
* schannel: remote party requests renegotiation
* schannel: renegotiating SSL/TLS connection
* schannel: SSL/TLS connection renegotiated
< HTTP/1.1 301 Moved Permanently
< Location: [https://www.google.com/](https://www.google.com/)
< Content-Type: text/html; charset=UTF-8
< Content-Security-Policy-Report-Only: object-src 'none';base-uri 'self';script-src 'nonce-gcwHs9iztdiLWwy3iL60uA' 'strict-dynamic' 'report-sample' 'unsafe-eval' 'unsafe-inline' https: http:;report-uri [https://csp.withgoogle.com/csp/gws/other-hp](https://csp.withgoogle.com/csp/gws/other-hp)
< Date: Fri, 20 Nov 2026 17:45:00 GMT
< Expires: Sun, 20 Dec 2026 17:45:00 GMT
< Cache-Control: public, max-age=2592000
< Server: gws
< Content-Length: 220
< X-XSS-Protection: 0
< X-Frame-Options: SAMEORIGIN
< Alt-Svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000
<
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="[https://www.google.com/](https://www.google.com/)">here</A>.
</BODY></HTML>
* Connection #0 to host google.com:443 left intact
```

---

### A.5. Ресурси з некоректною конфігурацією сертифіката

**Випадок 1**

```powershell
curl.exe -v [https://expired.badssl.com](https://expired.badssl.com)
```

```text
*   Trying 104.154.89.105:443...
* Host expired.badssl.com:443 was resolved.
* IPv6: (none)
* IPv4: 104.154.89.105
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* schannel: next InitializeSecurityContext failed: SEC_E_CERT_EXPIRED (0x80090328) - The received certificate has expired.
* closing connection #0
curl: (35) schannel: next InitializeSecurityContext failed: SEC_E_CERT_EXPIRED (0x80090328) - The received certificate has expired.
```

**Випадок 2**

```powershell
curl.exe -v [https://wrong.host.badssl.com](https://wrong.host.badssl.com)
```

```text
*   Trying 104.154.89.105:443...
* Host wrong.host.badssl.com:443 was resolved.
* IPv6: (none)
* IPv4: 104.154.89.105
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* schannel: SNI or certificate check failed: SEC_E_WRONG_PRINCIPAL (0x80090322) - The target principal name is incorrect.
* closing connection #0
curl: (60) schannel: SNI or certificate check failed: SEC_E_WRONG_PRINCIPAL (0x80090322) - The target principal name is incorrect.
More details here: [https://curl.se/docs/sslcerts.html](https://curl.se/docs/sslcerts.html)

curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the webpage mentioned above.
```

**Випадок 3**

```powershell
curl.exe -v [https://self-signed.badssl.com](https://self-signed.badssl.com)
```

```text
*   Trying 104.154.89.105:443...
* Host self-signed.badssl.com:443 was resolved.
* IPv6: (none)
* IPv4: 104.154.89.105
* schannel: disabled automatic use of client certificate
* ALPN: curl offers http/1.1
* schannel: SEC_E_UNTRUSTED_ROOT (0x80090325) - The certificate chain was issued by an authority that is not trusted.
* closing connection #0
curl: (60) schannel: SEC_E_UNTRUSTED_ROOT (0x80090325) - The certificate chain was issued by an authority that is not trusted.
More details here: [https://curl.se/docs/sslcerts.html](https://curl.se/docs/sslcerts.html)

curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the webpage mentioned above.
```

---

## Частина B. Власна модель рівнів

**Кількість виділених груп:** 5

| № | Назва групи (власне формулювання) | Рядки виводу, віднесені до групи | Обґрунтування |
|---|---|---|---|
| 1 | Вміст вебсторінки (HTML-документ для користувача) | `<!doctype html><html lang=en>...<title>Example Domain</title>...</html>` (A.1); `<HTML><HEAD>...<TITLE>301 Moved</TITLE>...<A HREF="https://www.google.com/">here</A>.</BODY></HTML>` (A.4) | Це кінцеві корисні дані (розмітка і текст), заради яких виконується звернення до ресурсу і які відображаються користувачеві. |
| 2 | Прикладний обмін запитом і відповіддю (HTTP-заголовки та метадані) | `> GET / HTTP/1.1`, `> Host: example.net`, `> User-Agent: curl/8.22.0`, `> Accept: */*`, `* Request completely sent off`, `< HTTP/1.1 200 OK`, `< Content-Type: text/html; charset=utf-8`, `< Server: cloudflare`, `< cf-cache-status: HIT`, `< CF-RAY: a5d94186fce4e199-WAW`, `< HTTP/1.1 301 Moved Permanently`, `< Location: https://www.google.com/`, `< Server: gws` (та інші рядки з `>` і `<` у A.1, A.4) | Описують змістовий діалог клієнта й сервера: який ресурс запитується, код результату, тип вмісту, параметри кешування та перенаправлення. |
| 3 | Захист каналу, перевірка сертифіката та узгодження версії протоколу | `* schannel: disabled automatic use of client certificate`, `* ALPN: curl offers http/1.1`, `* ALPN: server accepted http/1.1`, `* using HTTP/1.x`, `* schannel: remote party requests renegotiation`, `* schannel: renegotiating SSL/TLS connection`, `* schannel: SSL/TLS connection renegotiated` (A.1, A.4); помилки `SEC_E_CERT_EXPIRED`, `SEC_E_WRONG_PRINCIPAL`, `SEC_E_UNTRUSTED_ROOT` (A.5) | Відповідають за перевірку справжності сервера та встановлення шифрованого каналу. У незахищеному запиті A.2 цих рядків немає, а в A.5 саме на цьому етапі з'єднання переривається. |
| 4 | Пошук мережевої адреси за доменним іменем (DNS) | `* Host example.net:443 was resolved.`, `* IPv6: (none)`, `* IPv4: 104.20.21.8, 172.66.175.59` (A.1); `* Host neverssl.com:80 was resolved.`, `* IPv4: 34.223.124.45` (A.2); записи `AAAA` (`2606:4700:10::ac42:af3b`, `2606:4700:10::6814:1508`) та `A` (`104.20.21.8`, `172.66.175.59`) із `TTL 600` і `78` (A.3) | Перетворюють зрозуміле людині текстове ім'я сайту на числові IP-адреси, без яких клієнт не може ініціювати підключення. |
| 5 | Встановлення, утримання та закриття мережевого з'єднання (IP і порт) | `* Trying 104.20.21.8:443...`, `* Trying 172.66.175.59:443...`, `* Established connection to example.net (104.20.21.8 port 443) from 192.168.0.135 port 62048`, `* Connection #0 to host example.net:443 left intact` (A.1); `* connect to 34.223.124.45 port 80 from 0.0.0.0 port 53378 failed: Timed out`, `* closing connection #0` (A.2) | Найближчий до мережевого обладнання етап: відкриття й розрив каналу зв'язку між локальним інтерфейсом (`192.168.0.135`) та віддаленим сервером за IP-адресою й номером порту. |

*Групи впорядковано від найближчої до користувача (№ 1) до найближчої до апаратного забезпечення.*

**Рядки, які не вдалося віднести до жодної групи:** 2

| Рядок виводу | Причина утруднення |
|---|---|
| `< Connection: keep-alive` (A.1) | Передається як заголовок HTTP-відповіді (група 2), але фактично керує станом мережевого з'єднання — не закривати його після відповіді (група 5). |
| `< alt-svc: h3=":443"; ma=86400` (A.1) та `< Alt-Svc: h3=":443"; ma=2592000,h3-29=":443"; ma=2592000` (A.4) | За формою це HTTP-заголовок (група 2), проте за змістом він пропонує змінити протокол обміну на HTTP/3 (група 3) та вказує мережевий порт `:443` (група 5). |

---

## Контрольні питання

**1. Скільки рядків діагностичного виводу передує отриманню даних сторінки (завдання A.1)?**

> Отриманню HTML-коду сторінки (`<!doctype html>...`) передує **37 рядків** діагностичного виводу:
> - **17 службових рядків із префіксом `*`** (розв'язання імені, спроби підключення, налаштування `schannel` та `ALPN`, підтвердження встановлення з'єднання, відправлення запиту та два повторні узгодження SSL/TLS);
> - **5 рядків із префіксом `>`** (4 рядки HTTP-запиту клієнта та 1 порожній рядок `>`, що завершує заголовки запиту);
> - **15 рядків із префіксом `<`** (14 рядків статусу й HTTP-заголовків відповіді сервера та 1 порожній рядок `<`, що відділяє заголовки від тіла сторінки).

**2. Які рядки наявні у виводі A.1 і відсутні у виводі A.2? Чим це зумовлено?**

> У виводі A.1 є дві групи рядків, яких немає в A.2:
> 1. **Рядки захищеного з'єднання та узгодження протоколу** (`* schannel: disabled automatic use of client certificate`, `* ALPN: curl offers http/1.1`, `* ALPN: server accepted http/1.1`, `* schannel: remote party requests renegotiation` тощо). Це зумовлено тим, що в A.1 використовується схема `https://`, яка вимагає перевірки сертифіката й шифрування, тоді як у A.2 — незахищена схема `http://`.
> 2. **Рядки успішного підключення, HTTP-запиту та HTTP-відповіді** (`* Established connection...`, `> GET / HTTP/1.1...`, `< HTTP/1.1 200 OK...`). У моєму виводі A.2 вони відсутні, оскільки спроба підключення до `34.223.124.45:80` завершилася тайм-аутом через 22997 мс (`* connect to 34.223.124.45 port 80 from 0.0.0.0 port 53378 failed: Timed out`), тому до обміну HTTP-повідомленнями справа не дійшла.

**3. Звідки у виводі з'явилося значення `443`, якщо його не було вказано в адресі?**

> Номер порту `443` утиліта `curl.exe` підставила автоматично як стандартний порт за замовчуванням для схеми `https://` (аналогічно для схеми `http://` у завданні A.2 автоматично підставлено стандартний порт `80`).

**4. Як змінилося значення TTL між двома запитами (A.3)? Що означає це число?**

> У першому запиті значення TTL для всіх записів (`AAAA` та `A`) домену `example.net` становило **600 секунд**, а у повторному запиті — зменшилося до **78 секунд** (різниця — 522 секунди). TTL (Time To Live) означає залишок часу в секундах, протягом якого DNS-резолвер зберігає отриманий запис у своєму кеші як актуальний і не звертається повторно до авторитативного сервера.

**5. Чим відрізняються між собою три причини помилок із завдання A.5? Сформулювати кожну однією фразою.**

| Випадок | Причина недовіри |
|---|---|
| `expired` | Строк дії сертифіката сервера вже сплив (`SEC_E_CERT_EXPIRED`). |
| `wrong.host` | Доменне ім'я в адресі запиту не збігається з іменем, на яке видано сертифікат (`SEC_E_WRONG_PRINCIPAL`). |
| `self-signed` | Сертифікат підписаний невідомим видавцем (самим сервером), якого немає в переліку довірених кореневих центрів сертифікації ОС (`SEC_E_UNTRUSTED_ROOT`). |

**6. Три рядки з власних виводів, про які не йшлося на лекції 1:**

| № | Рядок виводу | Джерело (номер завдання) |
|---|---|---|
| 1 | `* schannel: remote party requests renegotiation` | A.1 |
| 2 | `< CF-RAY: a5d94186fce4e199-WAW` | A.1 |
| 3 | `< alt-svc: h3=":443"; ma=86400` | A.1 |

---

## Висновки

**D.1. Що виявилося неочевидним або несподіваним**

Під час експерименту неочевидними виявилися три факти. По-перше, `Resolve-DnsName example.net` (A.3) повернув дві IPv6-адреси (`AAAA`), але `curl.exe` у A.1 вивів `* IPv6: (none)` і використав лише IPv4, причому паралельно ініціював спроби підключення одразу до обох адрес (`* Trying 104.20.21.8:443...` та `* Trying 172.66.175.59:443...`). По-друге, у завданні A.2 (`http://neverssl.com`) домен успішно розв'язався в IP-адресу `34.223.124.45`, проте саме підключення до порту 80 обірвалося через тайм-аут (`* connect to 34.223.124.45 port 80 from 0.0.0.0 port 53378 failed: Timed out`). По-третє, у запитах A.1 і A.4 уже після повного відправлення запиту (`* Request completely sent off`) сервер двічі ініціював повторне узгодження шифрування (`* schannel: remote party requests renegotiation`), перш ніж надіслати HTTP-відповідь.

**D.2. Чому саме така кількість груп у частині B**

Я виділила 5 груп, оскільки у власних виводах чітко простежуються п'ять різних за призначенням функцій: отримання HTML-вмісту, обмін HTTP-заголовками, захист і перевірка сертифіката (TLS/ALPN), перетворення імені на адресу (DNS) та встановлення з'єднання за IP і портом. Кількість груп довелося б змінити, якби до аналізу додалися дані нижчих рівнів (наприклад, MAC-адреси чи IP-пакети з аналізатора трафіку) або якби узгодження версії протоколу (ALPN) розглядалося окремо від криптографічної перевірки сертифіката.

**D.3. Питання, яке залишилося без відповіді**

Чому в моїх виводах A.1 (`example.net`) та A.4 (`google.com`) одразу після рядка `* Request completely sent off` бібліотека `schannel` двічі поспіль виконувала повторне узгодження SSL/TLS (`* schannel: remote party requests renegotiation`), і чи пов'язано це саме з версією TLS 1.3 у Windows?

---

## Використання штучного інтелекту

**Факт використання:** використано

**Установлений рівень для цієї роботи:** Р3 — ШІ як співвиконавець

**Фактичний рівень використання:** Р3

### Використані системи

| Система | Версія або модель | Період використання |
|---|---|---|
| Google Gemini | Gemini (вебінтерфейс) | 20.11.2026 |

### Промпти

| № | Розділ роботи | Текст промпта |
|---|---|---|
| 1 | Частина A (підготовка команд) | Перевір перелік команд для Windows PowerShell (`curl.exe`, `Resolve-DnsName`) за методичними вказівками. |
| 2 | Шапка звіту та вибір варіанта | Звір таблицю А.1 (Група 1) для варіанта № 3 (`example.net`) та структуру оформлення бланка. |
| 3 | Оформлення звіту (`README.md`) | Упорядкуй фінальний текст `README.md` згідно з бланком звіту. |

### Дії з отриманим результатом

| № промпта | Що перевірено | Що змінено | Що відхилено і чому |
|---|---|---|---|
| 1 | Відповідність команд синтаксису Windows PowerShell (`curl.exe` замість `curl`, `Resolve-DnsName` замість `dig`). | Усі команди Частини A виконано особисто у власному PowerShell на домені варіанта № 3 (`example.net`). | Нічого не відхилено. |
| 2 | Відповідність індивідуального домену `example.net` варіанту № 3 та наявність усіх обов'язкових розділів бланка. | Заповнено шапку звіту та перевірено заповнення всіх пунктів висновків (D.1–D.3). | Нічого не відхилено. |
| 3 | Вручну звірено підрахунок 37 рядків діагностичного виводу в A.1, значення TTL (600 → 78), коди помилок `schannel` у A.5 та точність цитування власних логів. | Відформатовано блоки коду, таблиці Markdown та фінальну структуру файлу `README.md`. | Нічого не відхилено. |

### Підтвердження

Підтверджую, що всі наведені в цьому звіті виводи команд отримано мною особисто внаслідок фактичного виконання відповідних дій, а відомості цього розділу є повними та достовірними.

> Виводи `curl`, `dig` та інші артефакти не можуть бути згенеровані. Це стосується будь-якого рівня використання ШІ.

---

## Примітки виконавця

У середовищі Windows PowerShell стандартне ім'я `curl` є псевдонімом `Invoke-WebRequest`, а утиліта `dig` відсутня, тому відповідно до п. 3.1 методичних вказівок використано `curl.exe` та `Resolve-DnsName`.
