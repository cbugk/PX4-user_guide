# Репозиторій моделей Gazebo (PX4-gazebo-models)

Репозиторій [PX4-gazebo-models](https://github.com/PX4/PX4-gazebo-models) використовується для зберігання моделей [Gazebo](../sim_gazebo_gz/index.md) та світів, що підтримуються PX4.

- Моделі зберігаються в директорії `/models`, а світи - в директорії `/worlds`.
- Python скрипт [simulation-gazebo](https://github.com/PX4/PX4-gazebo-models/blob/main/simulation-gazebo) використовується для [запуску Gazebo в автономному режимі](../sim_gazebo_gz/index.md#standalone-mode).

Репозиторій `PX4-gazebo-models` включений у PX4 як підмодуль, а усі моделі доступні за замовчуванням при використанні "звичайної" цілі збірки для `make`, наприклад `make px4_sitl gz_x500`.

Для [автономних симуляцій](../sim_gazebo_gz/index.md#standalone-mode) Gazebo спочатку потрібно отримати Python скрипт [simulation-gazebo](https://github.com/PX4/PX4-gazebo-models/blob/main/simulation-gazebo), а потім він отримає моделі та світи у `~/.simulation-gazebo` якщо ця директорія відсутня.

## simulation-gazebo (скрипт запуску автономної симуляції)

Python скрипт [simulation-gazebo](https://github.com/PX4/PX4-gazebo-models/blob/main/simulation-gazebo) використовується для запуску Gazebo у [автономному режимі](../sim_gazebo_gz/index.md#standalone-mode).
Скрипт може спілкуватися з екземпляром PX4 SITL на тому ж комп'ютері за замовчуванням.
Якщо аргументи скрипту встановлені правильно, він може також спілкуватися з будь-яким екземпляром PX4 на будь-якій машині в одній мережі.

Скрипт `simulation-gazebo` не потребує додаткових бібліотек і має працювати як є.

### Основне використання

Скрипт за замовчуванням `simulation-script` може бути запущений як:

```sh
python simulation-gazebo
```

Він отримає моделі та світи з [PX4 репозиторію моделей gazebo](https://github.com/PX4/PX4-gazebo-models) у підкаталоги каталогу `~/.simulation-gazebo` при першому запуску (або точніше, якщо ця директорія не виявлена).
Екземпляр _gz-server_ буде запущено з використанням світу за замовчуванням "сіра рівнина".

Система збірки автоматично не оновлюватиме локальну копію, якщо виявлено директорію `.simulation-gazebo`, але ви можете змусити її оновитися до останніх моделей та рухомих засобів передавши скрипту прапорець `overwrite`.
В результаті команда виглядатиме приблизно так:

```sh
python simulation-gazebo --overwrite
```

Ви можете під'єднати рухомий засіб з підтримкою PX4 до екземпляра _gz-server_ використовуючи кілька підходів:

- Запустіть PX4 використовуючи `PX4_GZ_STANDALONE=1 make px4_sitl gz_<vehicle>` в новому терміналі й побачите як засіб з'являється у Gazebo.

- Gazebo також має власний вбудований "відтворювач ресурсів".
  Він може бути викликаний натисканням на три крапки у верхньому правому куті графічного інтерфейсу Gazebo.
  Там введіть "resource spawner" та у "Fuel resources" додайте власника "px4".
  Ви можете перетягнути будь-яку PX4 модель у вашу симуляцію.

  ::: info
  These models are taken from an web-server called [Gazebo Fuel](https://app.gazebosim.org/dashboard), which essentially acts as an online database for all types of models and worlds that can be launched in Gazebo.

:::

  Після перетягування рухомого засобу за вашим вибором в Gazebo запустіть PX4 SITL:

  ```sh
  PX4_SYS_AUTOSTART=<airframe-number-of-choice> PX4_GZ_MODEL_NAME=<vehicle-of-choice> ./build/px4_sitl_default/bin/px4`
  ```

  Що з'єднає PX4 SITL із запущеним екземпляром Gazebo.

Весь наявний функціонал та гнучкість PX4 приданий та працює безпосередньо в екземплярі Gazebo.

### Аргументи командного рядка

Скрипту `simulation-gazebo` можна передати наступні аргументи:

- `--world`
  Рядкова змінна яка позначає файл sdf що запускає світ симуляції.
  Аргумент за замовчуванням - "default", який посилається на світ за замовчуванням.

- `--gz_partition`
  Рядкова змінна яка встановлює розділ gazebo в якому запускатись (додаткова інформація [тут](https://gazebosim.org/api/transport/13/envvars.html))

- `--gz_ip`
  Рядкова змінна, яка встановлює IP адресу вихідного мережевого інтерфейсу (більше інформації [тут](https://gazebosim.org/api/transport/13/envvars.html))

- `--interactive` Логічна змінна яка вимагає можливості запуску коду в інтерактивному режимі, дозволяючи користувацькі шляхи для `--model_download_source`.
  Якщо це не вказано, `--model_download_source` буде завантажувати тільки з Github репозиторію за замовчуванням.

- `--model_download_source`
  Рядкова змінна, яка встановлює шлях до директорії звідки будуть імпортуватися моделі.
  На цей момент це може бути лише локальний файловий каталог або http адреса.
  Джерело має закінчуватися файлом zip архіву моделі (наприклад: https://path/to/simulation/models/models.zip).

- `--model_store`
  Рядкова змінна яка встановлює шлях до директорії зберігання моделі.
  Тут буде розміщений zip-файл, вказаний у `model_download_source`.

- `--overwrite`
  Логічна змінна, яка вказує, що у `.simulation-gazebo` слід оновити дані світів та рухомих засобів з репозиторію моделей gazebo.

- `--dryrun`
  Логічна змінна, яку можна вказати під час запуску тестів.
  Вона не надасть жодної взаємодії, і не запустить екземпляр gz-server.

Жоден із цих аргументів не є необхідним для роботи `simulation-gazebo`.
Вони необхідні, коли ви хочете завантажити власні моделі, інші світи або хочете запустити Gazebo і PX4 на окремих комп'ютерах.

## Приклад: Запуск на одному комп'ютері з кількома терміналами

Цей приклад пояснює, як ви можете запустити автономний режим PX4 за допомогою двох терміналів на одному комп'ютері.

1. В одному терміналі запустіть

   ```sh
   PX4_GZ_STANDALONE=1 PX4_SYS_AUTOSTART=4001 PX4_SIM_MODEL=gz_x500 PX4_GZ_WORLD=windy ./build/px4_sitl_default/bin/px4
   ```

2. У вікні другого терміналу запустіть:

   ```sh
   python3 /path/to/simulation-gazebo --world windy
   ```

Не потрібно передавати додаткових параметрів скрипту simulation-gazebo щоб цей приклад працював, оскільки усі вузли Gazebo виконуються на одному комп'ютері.
Дивіться приклад складнішого сценарію з різними комп'ютерами нижче.

## Приклад: Запуск на декількох комп'ютерах

Наступний приклад покаже як можна створити розподілену систему, що виконує PX4 на одному комп'ютері в мережі (що називається "PX4-host") та Gazebo на іншому (що називається "Gazebo-host").
Це призведе до того, що два вузли Gazebo будуть працювати на двох різних комп'ютерах в одній мережі та спілкуватися з використанням бібліотеки `gz-transport`.

Спочатку ми повинні зрозуміти, яку IP-адресу ми можемо використовувати для відправлення повідомлень на обох комп'ютерах.

Запустіть наступні команди на PX4-host:

```sh
sudo apt update
sudo apt install iproute2
```

Потім введіть:

```sh
ip a
```

Якщо ви підключені до мережі через WiFi, то потрібна адреса зазвичай буде мати назву щось на зразок "wlp12345".
Запишіть адресу IPv4 для цього інтерфейсу відмітивши числа, вказані поруч з `inet`.
Це має бути чотири числа, розділені крапками, за якими йде скісна риска та інше число.
Правильна IPv4 адреса наприклад буде: `192.168.24.89/24`.

Вам потрібно лише перші чотири числа.
Тому в цьому прикладі IP-адреса PX4-host буде: "192.168.24.89".
Зверніть увагу, якщо ви підключені через Ethernet, мережевий інтерфейс може починатися з `eth` або `en`, однак це не стандартизовано.

Повторіть ту саму процедуру на Gazebo-host та занотуйте другу адресу IPv4.
Для цього прикладу ми візьмемо "192.168.24.107".

Тепер ми можемо почати налаштовування обох комп'ютерів.
Спочатку налаштуємо PX4-host:

```sh
GZ_PARTITION=relay GZ_RELAY=192.168.24.107 GZ_IP=192.168.24.89 PX4_GZ_STANDALONE=1 PX4_SYS_AUTOSTART=4001 PX4_SIM_MODEL=gz_x500 PX4_GZ_WORLD=baylands ./build/px4_sitl_default/bin/px4
```

Опис змінних середовища:

- `GZ_PARTITION` декларує ім'я розділу в якому два вузли gazebo будуть запущені.
  Він **має** бути тим самим для всіх під'єднаних вузлів.
- `GZ_RELAY` вказує на цільову IP-адресу на іншому комп'ютері (в цьому випадку - Gazebo-host).
  Ця змінна середовища необхідна для під'єднання двох вузлів один до одного.
  Зверніть увагу, що це з'єднання двостороннє, тому `GZ_RELAY` потрібно налаштувати тільки на один комп'ютер.
- `GZ_IP` каже поточному комп'ютеру який мережевий інтерфейс використовувати для відправлення повідомлень.
  Це потрібно при оголошенні рубрик або сервісів.

Тепер ми можемо налаштувати Gazebo-host.
Зверніть увагу, що фактичний порядок налаштування (спочатку PX4-host або спочатку Gazebo-host first) не важливий.
Обидва будуть постійно шукати інші вузли Gazebo, поки вони не знайдуть одне одного.
На Gazebo-host запустіть в терміналі:

```sh
python3 /path/to/simulation-gazebo --gz_partition relay --gz_ip 192.168.24.107 --world baylands
```

Тут ми передаємо ті ж змінні середовища як аргументи.
Зверніть увагу, що значення `--world` повинні бути однаковими для того, щоб мати можливість під'єднатися.
Якщо ви випадково встановили "baylands" на одному комп'ютері та скажемо "default" на іншому, тоді два вузли не зможуть під'єднатися.

Якщо все спрацювало правильно, тоді два комп'ютери повинні бути з'єднані й ви зможете запустити свій засіб у командному рядку на PX4-host.

Крім того, ви можете також налаштувати QGC і запускати свій рухомий засіб таким чином.
На додаток можна також під'єднати кілька PX4-host до одного Gazebo-host встановивши прапорець "-i", як показано на сторінці [симуляції декількох засобів](. multi_vehicle_simulation.md).
Для отримання додаткової інформації що стосується змінних середовища, ви також можете подивитися на [документацію gz-transport](https://gazebosim.org/api/transport/12/envvars.html).

З'єднання через VPN (а отже кількох мереж) також можливо, але на цей час не задокументовано.

## Робочий процес

Коли гілка вихідного коду об'єднується з головною (це може бути додавання або видалення моделі, або щось на зразок зміни параметра), усі моделі, які отримали будь-які зміни будуть автоматично оновлені та завантажені в обліковий запис PX4 на [Gazebo Fuel](https://app.gazebosim.org/PX4).
Крім того, є процес, який можна використовувати для перевірки правильності будь-якого наданого файлу sdf.