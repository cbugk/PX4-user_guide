# Значення LED індикаторів (Pixhawk Series)

[Політні контролери серії Pixhawk](../flight_controller/pixhawk_series.md) використовують LED індикатори для індикації поточного стану апарату.
- [LED індикатори інтерфейсу користувача](#ui_led) надає користувачеві інформацію про стан, пов'язаний з *готовністю до польоту*.
- [Індикатори статусу](#status_led) показують стан PX4IO і FMU SoC. Вони показують заряд, режим і стан завантажувача, а також помилки.

<a id="ui_led"></a>

## LED індикатори інтерфейсу

RGB *індикатори інтерфейсу* вказують на поточний стан *готовності до польоту* апарату. Зазвичай це дуже яскравий I2C, який може бути встановлений або не встановлений на платі політного контролера (наприклад. FMUv4 не має такого на борту, і зазвичай використовує світлодіод, встановлений на GPS).

На зображенні нижче показано взаємозв'язок між світлодіодом і станом апарату.

:::warning
Ви можете мати GPS лок (зелений світлодіод), але все ще не мати можливості увімкнути апарат, оскільки PX4 ще не [пройшов передпольотні перевірки](../flying/pre_flight_checks.md). **Для зльоту потрібна коректна оцінка глобальної позиції!**
:::

:::tip
У разі виникнення помилки (блимає червоним кольором), або якщо апарат не може досягти GPS блоку (зміна кольору з синього на зелений), перегляньте більш детальну інформацію про статус у *QGroundControl*, включно зі статусом калібрування, а також повідомлення про помилки, які надаються [Передпольотні перевірки (внутрішні)](../flying/pre_flight_checks.md). Також перевірте, чи правильно підключений GPS-модуль, чи правильно Pixhawk зчитує ваш GPS, і чи правильно GPS передає дані про місцеперебування.
:::

![Значення світлодіодів](../../assets/flight_controller/pixhawk_led_meanings.gif)


* **[Безперервний синій] Стан Armed, відсутній GPS лок:** Вказує на те, що апарат був увімкнений і не має визначення позиції від GPS-пристрою. Коли апарат вмикається, PX4 розблоковує керування моторами, що дозволить вам керувати дроном. Як завжди, будьте обережні під час увімкнення, оскільки великі гвинти можуть бути небезпечними на високих обертах. У цьому режимі апарат не може виконувати керовані місії.

* **[Пульсуючий синій] Стан Disarmed, відсутній GPS лок:** Подібно до попереднього, але ваш апарат вимкнений. Це означає, що ви не зможете управляти моторами, але всі інші підсистеми працюють.

* **[Безперервний зелений] Стан Armed, наявний GPS лок:** Вказує на те, що апарат увімкнений та має валідний лок позиціювання від GPS-пристрою. Коли апарат вмикається, PX4 розблоковує управління моторами, що дозволить вам керувати дроном. Як завжди, будьте обережні під час увімкнення, оскільки великі гвинти можуть бути небезпечними на високих обертах. У цьому режимі апарат може виконувати керовані місії.

* **[Пульсуючий зелений] Стан Disarmed, наявний GPS лок:** Подібно до попереднього, але ваш апарат вимкнений. Це означає, що ви не зможете управляти моторами, але всі інші підсистеми, включаючи GPS-фіксацію положення, працюють.

* **[Постійний фіолетовий] Failsafe режим:** Цей режим активується щоразу, коли апарат стикається з проблемою під час польоту, наприклад, втрата ручного керування, критично низький заряд батареї або внутрішня помилка. Під час аварійного режиму апарат намагатиметься повернутися до місця зльоту або може просто знизитися там, де він зараз перебуває.

* **[Постійний помаранчевий] Попередження про низький рівень заряду акумулятора:** Вказує на небезпечний розряд акумулятора вашого апарату. Після певного моменту пристрій перейде у failsafe режим. Однак цей режим повинен сигналізувати про те, що настав час завершити політ.

* **[Блимаючий червоний] Помилка / необхідне налаштування:** Вказує на те, що автопілот потрібно налаштувати або відкалібрувати перед польотом. Під'єднайте автопілот до наземної станції керування, щоб перевірити, в чому проблема. Якщо ви завершили процес налаштування, а автопілот все ще сигналізує червоним і блимає, можливо, виникла інша помилка.


<a id="status_led"></a>

## Індикатор стану

Три *LED індикатори стану* відображають стан FMU, а ще три - стан PX4IO (за наявності). Вони показують заряд, режим і стан бутлоадера, а також помилки.

![Pixhawk 4](../../assets/flight_controller/pixhawk4/pixhawk4_status_leds.jpg)

Після ввімкнення живлення процесори FMU та PX4IO спочатку запускають завантажувач (BL), а потім додаток (APP). У таблиці нижче показано, як завантажувач, а потім додаток використовують світлодіоди для індикації стану.

| Колір                 | Позначка                        | Використання завантажувача                | Використання додатку |
| --------------------- | ------------------------------- | ----------------------------------------- | -------------------- |
| Синій                 | ACT (активність)                | Мерехтить, коли завантажувач отримує дані | Індикація стану ARM  |
| Червоний/помаранчевий | B/E (в завантажувачі / помилка) | Мерехтить, коли в завантажувачі           | Індикація ERROR      |
| Зелений               | PWR (потужність)                | Не використовується в завантажувачі       | Індикація стану ARM  |

:::info
Світлодіодні позначення, показані вище, є загальноприйнятими, але можуть відрізнятися на деяких платах.
:::

Більш детальна інформація про те, як інтерпретувати світлодіодні індикатори, наведені нижче (де "х" означає "будь-який стан")

| Червоний/помаранчевий | Синій | Зелений | Значення                                                     |
| --------------------- | ----- | ------- | ------------------------------------------------------------ |
| 10 Гц                 | x     | x       | Перевантаження CPU > 80%, або використання RAM > 98%         |
| ВИМК                  | x     | x       | Перевантаження CPU <= 80%, або використання RAM <= 98%       |
| NA                    | ВИМК  | 4 Гц    | actuator_armed->armed && failsafe                            |
| NA                    | УВІМК | 4 Гц    | actuator_armed->armed && !failsafe                           |
| NA                    | ВИМК  | 1 Гц    | !actuator_armed-> armed && actuator_armed->ready_to_arm  |
| NA                    | ВИМК  | 10 Гц   | !actuator_armed->armed  && !actuator_armed->ready_to_arm | 