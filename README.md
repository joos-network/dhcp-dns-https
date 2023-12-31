# dhcp-dns-https
DHCP, PXE, DNS, HTTP, HTTPS

# DHCP

**Dynamic Host Configuration Protocol** - Протокол динамической конфигурации узла - сетевой протокол прикладного уровня модели TCP/IP
позволяющий сетевым устройствам автоматически получать IP-адрес и другие параметры, необходимые для работы в сети. Работает на 67/68 портах поверх UDP.

**DHCP** является расширением и дополнением протокола **BOOTP**

**BOOTSTRAP PROTOCOL** - Протокол начальной загрузки - сетевой протокол, используемый для автоматического получения клиентом IP-адреса (обычно во время загрузки компьютера)

**Клиент** - Отправляет широковещательное сообщение UDP с запросом загрузочной информации

**Сервер** - Возвращает клиенту его IP-адрес, и при необходимости, местоположение файла загрузки

С помощью **TFTP** (протокол пересылки файлов) клиент загружает необходимое программное обеспечение и начинает работу

**Trivial File Transfer Protocol** - Простой протокол передачи файлов - используется главным образом для первоначальной загрузки бездисковых рабочих станций. TFTP, в отличие от FTP, не содержит возможностей аутентификации (хотя возможна фильтрация по IP-адресу) и основан на транспортном протоколе UDP

Термины DHCP:
- Scope (область) - диапазон IP-адресов, из которого сервер будет предлагать адреса клиенту в аренду
- Reservation (резервирование) - закрепление IP адреса за конкретным устройством
- Lease (аренда) - период, в течение которого клиент может использовать IP-адрес
- Exclusion range (исключаемый диапазон) - диапазон IP-адресов, которые не могут быть назначены клиенту
- Address pool (пул адресов) -  свободные IP-адреса, готовые к выдачи клиентам

Механизмы выделения IP-адресов сервером DHCP:
- Динамическое присвоение - IP- адрес выдается сервером по общим правилам на ограниченное время
- Ручной режим работы DHCP-сервера -  IP-адрес выдается вручную системным администратором
- Автоматическое назначение статистических адресов -  IP-адрес выдается сервером на основании MAC-адреса клиента. База соответствий ведется в конфигурационных файлах сервера системным администратором

Формат кадра DHCP:
-  Opcode (op) – тип DHCP-сообщения
- - 0×01 запрос от клиента к серверу BOOTREQUEST
- - 0×02 ответ DHCP-сервера или BOOTREPLY
-  Hardware Type (htype) – тип адреса на канальном уровне. 0x01 для протокола Ethernet и MAC-адресов.
- Hardware Length (hlen) – длина аппаратного адреса в байтах. Для Ethernet значение 0×06
- Hops – количество промежуточных маршрутизаторов, которые находятся на пути между клиентом и сервером. DHCP-клиенты всегда ставят значение 0×00
- Transaction ID (xid) – случайное значение для идентификации диалога
- Seconds Elapsed (secs) – время в секундах с момента начала процесса получения IP-адреса
- Flags – поле для флагов или специальных параметров DHCP
- Client IP Address (ciaddr) – IP-адрес клиента. Клиент заполняет его в случае, если клиент хочет продлить аренду IP-адреса
- Your ID Address (yiaddr) – IP-адрес, который DHCP-сервер предлагает клиенту.
- Server IP Address (siaddr) – IP-адрес сервера
- Gateway IP Address (address) – IP-адрес промежуточного DHCP Relay Agent.
- Client Hardware Address (chaddr) – Если используется протокол Ethernet, то в это поле записывается MAC-адрес клиента
- Server Host Name (sname) – доменное имя сервера, поле не является обязательным.
- Boot File (file) – указатель файла для загрузки бездисковым станциям, не является обязательным.
- Options - опции для динамической конфигурации хоста

Виды DHCP сообщений:
- DHCPDISCOVER - обнаружение DHCP (Широковещательный запрос для обнаружения DHCP-сервера)
- DHCPOFFER - предложение DHCP (Сервер определяет конфигурацию в соответствие с серверными настройками, резервирует IP адрес и отправляет конфигурацию)
- DHCPREQUEST - запрос DHCP (Клиент выбирает одну из конфигураций, предложенных серверами и отправляет запрос на эту конфигурацию широковещательно, чтобы другие сервера знали, что их предложение отклонено)
- DHCPACK - подтверждение DHCP (Обмен сообщениями закончен, и клиент должен применить полученные настройки)

- DHCPDECLINE – отправляется клиентом, если он обнаруживает что адрес, предложенный сервером уже используется в сети
- DHCPNACK – отправляется сервером; после такого сообщения клиент должен повторить процедуру инициализации
- DHCPRELEASE – отправляется клиентом, если он по какой-то причине хочет прекратить аренду
- DHCPINFORM – отправляется клиентом, в случае если ему нужны только опции и не нужен IP адрес

IP-адреса выдаются сервером DHCP на время, заданное в настройках сервера (от минут до месяца). После завершения половины времени аренды, клиент пытается обновить аренду

- RENEWING – клиент отправляет запрос на продление аренды
- Сервер отвечает DHCPACK с подтверждением продолжения аренды и обновленными параметрами
- В случае отказа от продолжения аренды сервер отправляет DHCPNACK и клиент начинает инициализацию заново

- REBINDING - при неполучении ответа, клиент пытается продлить аренду через широковещательные запросы Если и это не выходит, клиент заново ищет DHCP-сервер

DHCP relay agent - позволяет отправить DHCP пакеты в другие сети через маршрутизатор

DHCP сервер может сообщать клиенту дополнительные параметры для работы в сети, количество опций зависит от реализации сервера
- domain-name-servers - настраивает на клиенте к какому серверу dns-имен обращаться
- next-server - сервер, для загрузки ПО на клиента
- smtp-server - список доступных клиенту почтовых серверов

Безопасность DHCP:
- DHCP Starvation (истощение ресурсов) - Адреса могут быть исчерпаны злонамеренно при достаточном количестве запросов - Легитимные клиенты не могут получить настройки и подключиться к сети
- DHCP Snooping (отслеживание) - Злоумышленник может установить свой DHCP сервер - Порты помечаются как доверенные и нет. Если на недоверенном порту появиться DHCP сервер – коммутатор заблокирует этот порт - точнее коммутатор не пропустить DHCP ответ к сторону клиента.
- Rogue DHCP Server (мошеннический DHCP-сервер) -  DHCP сервер может быть подменен - Клиента могут заставить работать через сетевые устройства злоумышленника, весь трафик может быть перехвачен

**Preboot eXecution Environment** - Cреда предварительного исполнения - технология, которая позволяет компьютеру загружаться и работать используя сетевую карту

Для запуска компьютера достаточно иметь:
- Клиент, поддерживающий PXE - Большинство современных компьютеров поддерживает PXE
- DHCP-сервер - Экземпляр сервера, который поддерживает необходимые опции и сконфигурированный для отправки ответов
- TFTP-сервер - Сервер, на котором размещены файлы загрузки

Варианты использования PXE:
- Установка - Можно использовать для установки операционной системы на компьютеры через сеть
- Загрузка - Для работы с операционной системой или с программным обеспечением через сеть


# DNS

**DNS (Domain Name System, система доменных имён)** – это распределённая иерархическая система доменных имён, которая связывает буквенные названия (имена) доменов с IP- адресами компьютеров, соответствующих этим доменам.

**Файл hosts** выполняет то же функцию что и DNS-сервер, но делает это локально. По умолчанию это первый источник, где Linux будет пытаться разрешить имя в IP адрес. Если в вашей сети нет DNS сервера или он неисправен – временным решением может служить внесение записей в файл /etc/hosts.

**FQDN (Fully Qualifed Domain Name, полностью определённое имя домена)** – это имя домена, однозначно определяющее доменное имя и включающее в себя имена всех родительских доменов иерархии DNS, в том числе и корневого. Ближайший аналог абсолютный путь в файловой системе.

**Архитектура DNS** - Архитектурно система DNS строится на иерархической распределенной модели.

**Корневые сервера** -  В верхней части иерархической системы доменных имен находится 13 корневых серверов. До 30% обращений приводит к обращению к корневому серверу. У корневых серверов множество реплик. Имена корневых серверов выглядят {a-m}.root-servers.net. В РФ также есть несколько реплик корневых DNS серверов.

Типы DNS-серверов:
- корневой – это DNS-сервер, который хранит в себе адреса всех TLD-серверов (TLD – top-level domain, домен верхнего уровня);
- TLD-серверы – эти серверы связаны с доменами верхнего уровня (TLD), обычно они идут после корневых DNS-серверов;
- авторитативный DNS-сервер – это серверы, которые ответственны за зону. Они хранят фактические записи типа A, NS, CNAME, TXT, и т. п. Авторитативные DNS-серверы по возможности возвращают IP-адреса хостов. Если сервер этого сделать не может — он выдаёт ошибку, и на этом поиск IP-адреса по серверам заканчивается.

Авторитативные DNS-сервера:
- первичный (primary master) – вносить изменения в описание зоны может только администратор данного сервера. Все остальные серверы только копируют информацию с master- сервера.
- вторичный (secondary master) – также является ответственным (authoritative) за зону. Его основное назначение заключается в том, чтобы подстраховать работу основного сервера доменных имен (master server), ответственного за зону, на случай его выхода из строя, а также для того, чтобы разгрузить основной сервер, приняв часть запросов на себя.

**Кэширующий DNS-сервер** - Кэширующий DNS-сервер занимается обработкой DNS запросов, которые выполняет ваша система, затем сохраняет результаты в памяти или кэширует их. В следующий раз, когда система посылает DNS запрос для того же адреса, то локальный сервер выдает результат быстрее чем если бы запрос был отправлен на DNS-сервер провайдера

**Делегирование домена** — это передача контроля над частью доменной зоны другой ответственной стороне. Делегирование осуществляется с помощью записи NS, в которой указывается адрес DNS-сервера , отвечающего за поддержание зоны и определяющего её содержимое

Виды запросов:
- рекурсивный – это первый запрос, который выполняется в процессе DNS-поиска и выполняется от пользователя к резолверу;
- нерекурсивные – резолвер сразу возвращает ответ без каких- либо дополнительных запросов на другие сервера имён; это случается, если в DNS-сервере был закэширован необходимый IP-адрес либо если запрос поступает напрямую на авторитативные сервер;
- итеративный – итеративный запрос выполняется, когда резолвер не может вернуть ответ, потому что он не закэширован и он выполняет запрос на корневой DNS-сервер.

Типы ресурсных записей:
- А Address record (запись адреса) указывает на соответствие между доменным именем и IP-адресом (IPv4).
- AAAA Аналогична записи A, но указывает соответствие доменного имени для IPv6.
- CNAME Canonical name – запись, которая позволяет присваивать домену каноническое имя для псевдонима (одноуровневая переадресация).
- DKIM DomainKeys Identified Mail – технология e-mail-аутентификации, которая позволяет подтвердить подлинность отправителя. DKIM добавляет в сообщение цифровую подпись, удостоверяющую, что письмо действительно поступило с ящика на указанном домене. Наличие DKIM повышает доверие серверов-получателей к письму и тем самым снижает его шансы оказаться в папке «Спам» или вовсе быть отклоненным принимающим сервером.
- MX Запись, указывающая на адрес почтового шлюза для домена. Состоит из двух частей: приоритета (чем число больше, тем ниже приоритет) и адреса узла.
- NS Указывает на DNS-сервер, обслуживающий данный домен, т.е. указывает серверы, на которые домен делегирован. Данный тип записи критически важен для функционирования самой системы доменных имён.
- PTR Pointer – запись, которая указывает на соответствие адреса имени — обратное соответствие для A и AAAA.
- SRV Server selection – этот тип записи указывает на серверы, обеспечивающие работу тех или иных служб в данном домене (например, Jabber, Active Directory).
- SOA Start of Authority (начальная запись зоны) – запись описывает основные/начальные настройки зоны, определяет зону ответственности данного сервера. Для каждой зоны должна существовать только одна запись SOA.
- TXT Запись содержит вспомогательную информацию о домене (запись произвольных данных). Записи TXT используются для различных целей: подтверждения права собственности на домен, обеспечения безопасности электронной почты, а также подтверждения SSL-сертификата. Можно прописывать неограниченное количество TXT-записей

Реализации DNS-сервера:
- BIND (Berkeley Internet Name Domain) – наиболее распространенная реализация DNS-сервера (10 из 13 корневых серверов работают на BIND);
- NSD (NLnet Labs Name Server Daemon) – быстрый DNS сервер, выступающий только в роли мастера (не реализует рекурсивные запросы и кэширование);
- PowerDNS – open source продукт, поставляется в 3 версиях, позиционируется как быстрый;
- Microsoft DNS Server – роль на Microsoft Server, прекрасно интегрируется в экосистему добавляя много функционала.

**BIND (Berkeley Internet Name Domain)** – разработанная в 1980-х годах в Университете Berkley реализация DNS-сервера. Является самым популярной в интернете и входит в поставку почти всех дистрибутивов LInux. BIND обладает огромным количеством настроек и интеграций.

Настройка DNS клиента
- vim /etc/hosts - прямое указание адрес - имя
- vim /etc/resolv.conf - dns сервера которые опрашиваем - можно добавить search ru - зона по умолчанию, если не нашли netology, то ищем netology.ru
- vim /etc/nsswitch.conf - строчка hosts - порядок опроса - файл, сервер - можно менять
- Автоматическая через опции DHCP - параметр с dns сервером
- resolvconf
- systemctl status systemd-resolved
- systemd-resolve --flush-caches
- systemctl status dnsmasq

Утилиты для диагностики:
- nslookup – утилита для отправки запросов DNS серверам;
- dig (Domain information groper) – утилита для отправки запросов DNS серверам; входит в пакет bind-utils.
- whois – утилита, выводящая информацию о домене:
- - ns сервера;
- - регистратора;
- - дату регистрации;
- - дату истечения срока регистрации;
- - информацию о владельце.
- systemd-resolve – демон для настроки / диагностики настроек DNS на стороне клиента.
- dnstop – программа позволяет отслеживать трафик от/к DNS серверу.

Атаки на DNS:
- DNS-флуд – на DNS-сервер отправляется множество запросов, которые потребляют ресурсы сервера / сети, тем самым не давая возможности легитимным клиентам получить ответы на их запросы;
- Атака посредством отраженных DNS-запросов – на DNS сервер отправляется множество запросов, при этом адрес отправителя меняется на адрес сервера-жертвы. Т.к. ответ больше запроса, то канал до сервера жертвы забивается паразитным трафиком, ем самым не давая возможности легитимным клиентам получить ответы на их запросы.
- Атака при помощи рекурсивных DNS-запросов – отправляется множество рекурсивных запросов к DNS серверу. Т.к. такие запросы потребляют много ресурсов - производительность сервера падает;
- Garbage DNS – суть данной атаки в переполнении канала до сервера путем отправки пакетов большого размера(1500 байт и больше) на сервер-жертву;
- Подмена DNS сервера - перехватив пакеты или иным способом заставить поверить клиента что атакующая машина и есть легитимный DNS сервер;

# HTTP / HTTPS

**HTTP (HyperText Transfer Protocol, протокол передачи гипертекста)** – протокол изначально созданный для передачи документов HTML, но использующийся в данный момент для передачи любых данных. В основе протокола лежит технология клиент-сервер.
```
curl -v netology.ru
curl -v https://netology.ru
```
Структура протокола:
- Стартовая строка – содержит параметры типа сообщения, имеет различную структуру для клиента и сервера. Имеет вид:
- Для клиента: - Метод URI HTTP/Версия - Пример: GET / HTTP/1.1
- Для сервера: - HTTP/Версия КодСостояния Пояснение - Пример: HTTP/1.1 200 OK
- Заголовки – используются для передачи любых дополнительных параметров. ➡ Например, о типе данных в сообщении или о наличии. Начиная с версии 1.1 заголовок Host является обязательным.
- Тело – непосредственно сами данные.

Методы HTTP запроса:
- GET - Используется для получения данных (заголовок, тело) с сервера.
- POST - Применяется при передаче данных от клиента на сервер.
- PUT - Используется для загрузки данных на указанный ресурс.
- OPTIONS - Получение информации о сервере. Обычно используется для получения возможностей сервера.
- HEAD - Аналогично GET но без получения данных (тело) с сервера. Ранее использовался для получения метаданных, но без загрузки самих данных. (узнать есть ли файл, его размер)
- PATCH - Аналогичен PUT, но используется для правки только части данных. (PUT если не указать все данные - перезапишет и удалит то чего нет, PATCH - оставт что есть и перепишет новое)
- DELETE - Удаляет ресурс с сервера.

Коды состояния HTTP:
- 1ХХ Информационные ответы.
- 2ХХ Успех. Сообщает об успехе того или иного запроса.
- 3ХХ Перенаправление. Сообщает о том что ресурс доступен по другому адресу.
- 4ХХ Сообщает о наличии ошибки со стороны клиента.
- 5ХХ Сообщает о наличии ошибки на стороне сервера.

В header передается Content-type - Позволяет клиенту понять содержимое ответа и по разному обрабатывать различный контент.
- text/plain – является типом по умолчанию для текстовых файлов;
- application/octet-stream – является типом по умолчанию для всех остальных случаев.
- text/html – HTML содержимое;
- image/jpeg – JPEG изображения;
- audio/mpeg – mp3 аудио.

Запуск сервера Nginx
- Установка из репозитория: - apt-get install nginx
- Конфигурация: - /etc/nginx/nginx.conf
- Запуск | остановка | статус: - service nginx start | stop | status

ss -t  / netstat -t - проверка tco портов 
cat /etc/protocols - какие поротоклы бывают

**HTTPS (HyperText Transfer Protocol Secure)** — расширение протокола HTTP, поддерживающее шифрование. Данные, передаваемые по протоколу HTTP, «упаковываются» в криптографический протокол SSL или TLS. В отличие от HTTP, для HTTPS по умолчанию используется TCP-порт 443.

**Создание самоподписанного сертификата**

- openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 — сгенерировать новый сертификат
- openssl x509 - nodes -outform der -in cert.pem -out cert.crt - перевести сертификат из pem в crt
```
user root;
worker_processes auto;
pid /run/nginx.pid;
events {
  worker_connections 1024;
}
http {
  gzip on;
  server {
    listen 443 ssl;
    ssl_certificate /etc/nginx/cert.crt;
    ssl_certificate_key /etc/nginx/cert.key;
    ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    ssl_ciphers HIGH:!aNULL:!MD5;
    location / {
      proxy_pass https://google.com;
    }
  }
}
```
**Server Name Indication (SNI)** — расширение компьютерного протокола TLS, которое позволяет клиентам сообщать имя хоста, с которым он желает соединиться во время процесса «рукопожатия». Это позволяет серверу иметь несколько сертификатов на одном IP-адресе и TCP- порту и предоставлять нужный в момент рукопожатия.


Первый ответ, который клиент получает на запрос GET, часто содержит не всю страницу, а ссылки на дополнительные ресурсы, необходимые для запрашиваемой страницы. Только после загрузки страницы клиент обнаруживает, что для полной визуализации от сервера требуются эти дополнительные ресурсы. Потому клиенту приходится делать дополнительные запросы для извлечения этих ресурсов. В HTTP/1.0 клиент должен был разрывать и снова создавать TCP-соединение для каждого нового запроса, а это довольно затратно с точки зрения времени и ресурсов.

HTTP/1.1 устраняет эту проблему через постоянные соединения и конвейерную обработку (pipelining).

HTTP2:
- возможность выбирать протокол, например, HTTP/1.1, HTTP/2 или другой;
- высокая совместимость с HTTP/1.1 — методы, коды статусов, поля хедеров;
- улучшение скорости загрузки благодаря сжатию хедеров запросов, бинарному протоколу, отправке данных по инициативе сервера, блокировке пакетов и запросу многократной передачи данных.

**HTTP2 Мультиплексирование** - Использование одного TCP соединения для загрузки нескольких ресурсов с одного серверa. Включить http2 можно только на сервере на котором настроен SSL

**RPC** - это удаленный вызов процедур. Стек технологий для выполнения каких либо операций на удаленном сервере. Обычно состоит из сетевого протокола и языка описания структур.


