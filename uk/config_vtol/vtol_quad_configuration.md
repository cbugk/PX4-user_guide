# Загальна стандартна конфігурація VTOL (QuadPlane) & Тюнінг

Це документація конфігурації для [Загального Стандартного VTOL](../airframes/airframe_reference.md#vtol_standard_vtol_generic_standard_vtol), також відомого як "Квадрокоптерний VTOL". Це в основному літальний апарат з фіксованими крилами з додаванням квадрокоптерних двигунів.

Для документації та інструкцій щодо конструкції планера див. розділ [VTOL Framebuilds](../frames_vtol/index.md).

## Прошивка & Основні налаштування

1. Запустіть _QGroundControl_
2. Перезавантажте мікропрограму для вашого поточного випуску або master (запуск `основної` гілки PX4).
3. У розділі [Налаштування каркаса](../config/airframe.md) виберіть відповідний планер VTOL.

   Якщо вашого планера немає в списку, виберіть раму [Загальний стандартний VTOL](../airframes/airframe_reference.md#vtol_standard_vtol_generic_standard_vtol).

### Перемикач режимів польоту / переходу

Вам слід призначити перемикач на вашому пульті управління радіокерованої моделі для перемикання між режимами багатокоптера і фіксованим крилом.

::: info 
Хоча PX4 дозволяє польоти без радіопульту, вам обов'язково потрібно мати його при налаштуванні нового корпусу повітряного судна.
:::

Це робиться в налаштуваннях [режиму польоту](../config/flight_mode.md), де ви [призначаєте режими польоту та інші функції](../config/flight_mode.md#what-flight-modes-and-switches-should-i-set) для перемикачів на вашому радіокерованому пульті. Перемикач також можна призначити за допомогою параметра [RC_MAP_TRANS_SW](../advanced_config/parameter_reference.md#RC_MAP_TRANS_SW).

Перемикач у вимкненому положенні означає, що ви польотаєте в режимі багатокоптера.

### Настройка мультироторів / фіксованих крил

Перш ніж ви спробуєте перший перехід до польоту з фіксованим крилом, вам слід абсолютно впевнитися, що ваш VTOL налаштований належним чином у режимі багатокоптера. Одна з причин цього полягає в тому, що це режим, до якого ви повернетеся, якщо щось піде не так з переходом, і можливо, він буде рухатися досить швидко. Якщо він не налаштований належним чином, можуть трапитися погані речі.

Якщо у вас є доступна злітно-посадкова смуга і загальна вага не занадто велика, вам також слід налаштувати польот з фіксованим крилом. Якщо ні, то ви будете це намагатися зробити, коли він перейде в режим фіксованого крила. Якщо щось піде не так, вам потрібно буде бути готовим (і здатним) повернутися до режиму багатокоптера.

Дотримуйтесь відповідних посібників з налаштування та мультироторів, і фіксованих крил.

### Налаштування переходу

Хоча може здатися, що ви працюєте з транспортним засобом, який може літати у двох режимах (багатокоптер для вертикальних злітів та посадок та фіксоване крило для прямолінійного польоту), також існує додатковий стан, який вам також потрібно налаштувати: перехід.

Важливо належним чином налаштувати ваш перехід для отримання безпечного входу в режим фіксованого крила, наприклад, якщо ваша швидкість повітря буде занадто низькою при переході, вона може загальмуватися.

#### Параметр переходу газу

Параметр: [VT_F_TRANS_THR](../advanced_config/parameter_reference.md#VT_F_TRANS_THR)

Перехідний газ спереду визначає цільовий газ для тягового мотора (підтягуючий / тяговий) під час переднього переходу.

Це повинно бути встановлено достатньо високим, щоб забезпечити досягнення швидкості переходу. Якщо ваше повітряне судно обладнане датчиком швидкості повітря, ви можете збільшити цей параметр, щоб зробити передній перехід швидшим. Для вашого першого переходу краще встановити значення вище, ніж нижче.

#### Швидкість наростання тягового мотора вперед / назад

Параметр: [VT_PSHER_SLEW](../advanced_config/parameter_reference.md#VT_PSHER_SLEW)

Передній перехід відноситься до переходу з режиму багатокоптера в режим фіксованого крила. Швидкість наростання тягового мотора вперед / назад - це кількість часу в секундах, яку слід витратити на плавне збільшення газу до цільового значення (визначеного `VT_F_TRANS_THR`).

Значення 0 призведе до того, що команда встановлення газу переходу буде негайно встановлена. За замовчуванням швидкість наростання встановлена на 0,33, що означає, що для досягнення 100% газу знадобиться 3 с. Якщо ви хочете зробити збільшення гладкішим, ви можете зменшити це значення.

Зауважте, що після закінчення періоду збільшення газу газ буде встановлено на цільове значення і залишиться там, доки (сподіваємося) не буде досягнута швидкість переходу. Змішування швидкості повітря.

#### Blending Airspeed

Параметр: [VT_ARSP_BLEND](../advanced_config/parameter_reference.md#VT_ARSP_BLEND)

За замовчуванням, коли швидкість повітря наближається до швидкості переходу, керування орієнтацією багатокоптера буде зменшуватися, а керування фіксованим крилом почне безперервно збільшуватися до того моменту, поки не відбудеться перехід.

Вимкніть змішування, встановивши цей параметр на 0, що збереже повне керування багатокоптером і нульове керування фіксованим крилом до моменту переходу.

#### Швидкість переходу

Параметр: [VT_ARSP_TRANS](../advanced_config/parameter_reference.md#VT_ARSP_TRANS)

Це швидкість повітря, яка, коли досягнута, спричинить перехід з режиму багатокоптера в режим фіксованого крила. Дуже важливо правильно калібрувати ваш датчик швидкості повітря. Також важливо вибрати швидкість, яка комфортно перевищує мінімальну швидкість стрибка вашої рами (перевірте `FW_AIRSPD_MIN`), оскільки це наразі не перевіряється.

### Поради щодо переходу

Як вже зазначалося, переконайтеся, що у вас добре налаштований режим багатокоптера. Якщо під час переходу виникне проблема, ви перейдете назад до цього режиму, і це повинно бути досить плавно.

Перед польотом майте план, що ви будете робити в кожній з трьох фаз (багатокоптер, перехід, фіксоване крило), коли ви будете в будь-якому з них, і виникне проблема.

Рівень заряду батареї: залиште достатньо місця для переходу багатокоптера для посадки в кінці польоту. Не робіть батарею занадто розрядною, оскільки вам знадобиться більше потужності в режимі багатокоптера, щоб сісти. Будьте консервативними.

#### Перехід: підготовка

Переконайтеся, що ви перебуваєте принаймні на висоті 20 метрів над землею і маєте достатньо місця для завершення переходу. Можливо, вашому багатокоптеру VTOL доведеться втратити висоту, коли він перейде в режим фіксованого крила, особливо якщо швидкість повітря недостатня.

Перехід до вітру, коли це можливо, інакше він буде рухатися далі від вас, перш ніж перейти.

Перед початком переходу переконайтеся, що VTOL знаходиться в стабільному висінні.

#### Перехід: багатороторний на нерухоме крило (передній перехід)

Розпочніть перехід. Він повинен відбуватися на відстані від 50 до 100 метрів. Якщо цього не відбувається, або він не летить стабільно, перервіть перехід (див. нижче) і сідайте або повертайтеся у вихідне положення та сідайте. Спробуйте збільшити значення [переходового керування](#transition-throttle) (`VT_F_TRANS_THR`). Також розгляньте зменшення тривалості переходу (`VT_F_TRANS_DUR`), якщо ви не використовуєте датчик швидкості повітря. Якщо ви використовуєте датчик швидкості повітря, розгляньте зниження швидкості переходу, але залиште її значно вище мінімальної швидкості стрибка.

Як тільки ви помітите, що відбувається перехід, будьте готові взяти під контроль втрату висоти, яка може включати швидке збільшення обертів.

:::warning
Наступна функція обговорювалася, але ще не була реалізована:

- Після того, як відбудеться перехід, мотори багатокоптера зупиняться, а керування тягою тягового мотора залишиться на рівні `VT_F_TRANS_THR` до того моменту, поки ви не перемістите палицю керування обертів, припускаючи, що ви в ручному режимі.
:::

#### Перехід: Фіксовані крила на багатороторний вертоліт (Зворотний перехід)

Під час переходу назад до режиму багатороторного вертольота приведіть свій літак на прямий рівний захід та зменште його швидкість, перекиньте перемикач переходу і двигуни багатороторного вертольота почнуть роботу, а тяговий пропелер відразу зупиниться, що повинно призвести до досить плавного перехідного глайдування.

Пам'ятайте, що значення газу, яке ви маєте при переході, вказуватиме кількість тяги вашого багатороторного вертольота у момент перемикання. Оскільки крило все ще буде у повітрі, ви відчуєте, що у вас є достатньо часу, щоб відрегулювати газ для досягнення/утримання стійкого витримання. Для розширеного налаштування зворотного переходу дивіться Посібник з налаштування зворотного переходу.

Для розширеного налаштування зворотного переходу дивіться [Посібник з налаштування зворотного переходу](vtol_back_transition_tuning.md)

#### Скасування переходу

Важливо знати, що очікувати, коли ви відміняєте команду _переходу_ під час переходу.

Під час переходу від **мультиротора до фіксованого крила** (перемикач переходу ввімкнено/фіксоване крило), а потім повернення перемикача назад (вимкнено/положення мультиротора) _перед тим, як_ перехід відбудеться негайно повернутися до багатороторного режиму.

Під час переходу від **з нерухомим крилом до багатогвинтового вертольота** для цього типу VTOL перемикання відбувається миттєво, тому тут насправді немає можливості повернутися назад, на відміну від VTOL з нахиленим гвинтом. Якщо ви хочете, щоб він повернувся у режим фіксованих крил, вам потрібно буде пройти повний перехід. Якщо він все ще рухається швидко, це має відбутися швидко.

### Підтримка

Якщо у вас виникли запитання щодо перетворення або конфігурації VTOL, перегляньте [discuss.px4.io/c/px4/vtol](https://discuss.px4.io/c/px4/vtol).