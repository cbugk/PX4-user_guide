# Модуль WiFi ESP32

ESP32 є легкодоступними модулями WiFi з присвяченими інтерфейсами UART, SPI та I2C, повним стеком TCP/IP та можливістю мікроконтролера. Вони поставляються без прошивки, але _DroneBridge для ESP32_ може бути встановлений для їх активації як прозорий та двосторонній послідовний міст WiFi. Потім вони можуть бути використані в якості модуля WiFi телеметрій, для будь-якого контролера серій Pixhawk.

Зазвичай не потрібна конфігурація, якщо підключено до `TELEM2`. Типовий діапазон складає приблизно 50 м-200м (залежно від використаної антени).

![DroneBridge for ESP32 connection concept](../../assets/peripherals/telemetry/esp32/db_ESP32_setup.png)

## Рекомендоване обладнання

_DroneBridge для ESP32_ може працювати майже на кожній дошці розробки ESP32. Рекомендується використовувати плати та модулі з зовнішнім роз'ємом антени, оскільки вони забезпечать більший діапазон.

:::warning
Багато модулів ESP32 підтримують введення живлення 3,3 В та 5 В, тоді як деякі контролери польоту (наприклад, Pixhawk 4) видають напругу на рівні 5 В.
Вам потрібно буде перевірити сумісність та знизити напругу, якщо це потрібно.
:::

Модулі та DevKit-и, які приймають живлення 3,3 В або 5 В:

- [AZ-Delivery — ESP-32 DevKit C](https://www.az-delivery.de/en/products/esp-32-dev-kit-c-v4)
- [TinyPICO — Плата розробки ESP32 - V2](https://www.adafruit.com/product/4335)
- [Плата Adafruit HUZZAH32 — ESP32 Feather](https://www.adafruit.com/product/3405)
- [Adafruit AirLift — Плата розбірного модуля WiFi ESP32](https://www.adafruit.com/product/4201) (потрібен адаптер FTDI для прошивки вбудованого програмного забезпечення)
- [Adafruit — HUZZAH32](https://www.adafruit.com/product/4172) (потрібен адаптер FTDI для прошивки ПЗ)

## Завантаження та Прошивання Програмного Забезпечення

[Завантажте прошивку з репозиторію GitHub](https://github.com/DroneBridge/ESP32/releases) і потім [дотримуйтесь цих інструкцій щодо прошивки](https://github.com/DroneBridge/ESP32#installationflashing-using-precompiled-binaries).

:::tip
Інструкції на Github рекомендовані, оскільки вони завжди актуальні. Зверніть увагу, що параметри можуть відрізнятися між релізами _DroneBridge для ESP32_.
:::

Основними кроками є:

1. [Завантажте скомпільовані бінарні файли прошивки](https://github.com/DroneBridge/ESP32/releases)
1. Підключіть свій DEVKit до комп'ютера за допомогою USB/серійного мосту (більшість DevKits вже мають USB-порт для прошивання та налагодження)
1. Очистіть флеш-пам'ять та прошейте прошивку DroneBridge для ESP32 на свій ESP32
   - Використання [Інструменту завантаження Flash від Espressif](https://www.espressif.com/en/support/download/other-tools) (лише для Windows)
   - Використання esp-idf/esptool (на всіх платформах)
1. Перезапустіть ESP32
1. [Підключіться до мережі WiFi "DroneBridge для ESP32" та налаштуйте прошивку для вашої програми](#configuring-dronebridge-for-esp32)

## Підключення

Проводка дуже проста, і схожа для всіх пристроїв при підключенні до портів Pixhawk TELEM1/2. Ви можете використовувати роз'єми заголовка з кроком 2,54 мм або припаяти кабелі телеметрії PX4 безпосередньо до плати.

![Example for wiring an ESP32 to the TELEM port](../../assets/peripherals/telemetry/esp32/pixhawk_wiring.png)

1. Підключіть UART ESP32 до UART вашого контролера польоту (наприклад, порту TELEM 1 або TELEM 2). Переконайтеся, що рівні напруги відповідають: більшість ESP32 DevKits можуть приймати лише 3,3 В!
   - TX до RX
   - RX до TX
   - GND до GND
   - Забезпечте стабільне живлення 3.3 В або 5 В для ESP32 (в залежності від доступних входів вашого DevKit)
1. Встановіть порт керування польотом на потрібний протокол.
1. Плати з роз'ємом IPEX для зовнішньої антени часто також мають вбудовану антену, яка активується за замовчуванням. Можливо, вам знадобиться перепаяти резистор, щоб активувати зовнішній антенний порт.

::: info

- Відповідно до ESP32 для виробників на енергопостачанні. Деякі плати можуть мати проблеми, якщо вони одночасно підключені до джерела живлення 5 В та мають підключений кабель USB до USB/серійного мосту (роз'єм USB плати розробника ESP32).
- Деякі виробники ESP32 DevKits використовують неправильні мітки для контактів на своїх продуктах. Переконайтеся, що PIN-коди на вашому платі відмічені правильно при зіткненні з проблемами.

:::

## Налаштування QGroundControl

QGroundControl повинен автоматично виявляти підключення і жодні подальші дії не повинні бути необхідними (_DroneBridge для ESP32_ автоматично пересилає дані з усіх підключених WiFi пристроїв через UDP для порту 14550).

Доступні наступні параметри модулів:

- UDP унікаст на порт `14550` до всіх підключених пристроїв.
- TCP на порту `5760`

## Налаштування DroneBridge для ESP32

Стандартна конфігурація _DroneBridge для ESP32_ повинна працювати для підключення до PX4 "з коробки". Єдине налаштування, яке може бути потрібним, - це забезпечення відповідності швидкостей передачі ESP32 та контролера польоту.

Вам слід змінити ці налаштування, якщо ви хочете використовувати різні контакти на ESP32, іншу конфігурацію WiFi або налаштувати розмір пакета. Менший розмір пакету означає більше накладних витрат і навантаження на систему, але також менше затримки і швидше відновлення після втраченого пакету.

### Конфігурація за замовчуванням

- SSID: `DroneBridge для ESP32`
- Пароль: `dronebridge`
- Transparent/MAVLink
- UART baud rate `115200`
- UART TX pin `17`
- UART RX pin `16`
- Gateway IP: `192.168.2.1`

### Налаштування за замовчуванням та веб-інтерфейс

Ви можете змінити конфігурацію за замовчуванням через веб-інтерфейс. Підключіться до ESP32 через WiFi та введіть `dronebridge.local`, `http://dronebridge.local` або `192.168.2.1` у адресну стрічку вашого браузера.

![DroneBridge for ESP32 Webinterface](../../assets/peripherals/telemetry/esp32/dbesp32_webinterface.png)

:::tip
Деякі налаштування потребують перезавантаження ESP32, щоб набути чинності.
:::

### API

DroneBridge для ESP32 пропонує REST:API, яке дозволяє вам читати та записувати параметри конфігурації. Ви не обмежені варіантами, які пропонуються веб-інтерфейсом (наприклад, швидкості передачі даних). Ви можете використовувати API для встановлення власних швидкостей передачі даних або для інтеграції системи у власне налаштування.

**Запит налаштувань**

```http request
http://dronebridge.local/api/settings/request
```

**Для запиту статистики**

```http request
http://dronebridge.local/api/system/stats
```

**Запустіть перезавантаження**

```http request
http://dronebridge.local/api/system/reboot
```

**Тригер зміни в налаштуваннях:** Відправити дійсний JSON

```json
{
  "wifi_ssid": "DroneBridge ESP32",
  "wifi_pass": "dronebridge",
  "ap_channel": 6,
  "tx_pin": 17,
  "rx_pin": 16,
  "telem_proto": 4,
  "baud": 115200,
  "msp_ltm_port": 0,
  "ltm_pp": 2,
  "trans_pack_size": 64,
  "ap_ip": "192.168.2.1"
}
```

до

```http request
http://dronebridge.local/api/settings/change
```

## Усунення проблем

- Завжди стерти спалах ESP32 перед прошивкою нового релізу/прошивки
- Перевірте, чи піни на вашій платі ESP правильно позначені.
- Уведіть IP-адресу в адресний рядок вашого браузера `http://192.168.2.1`. Немає підтримки https! Можливо, вам доведеться відключитися від мобільної мережі при використанні телефону, щоб мати доступ до веб-інтерфейсу.
- Якщо ваша мережа працює в тому ж діапазоні IP, що й БД для ESP32, вам потрібно змінити IP-адресу шлюзу в веб-інтерфейсі на щось подібне до `192.168.5.1`.