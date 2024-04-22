# Інструкція з налаштування мультикоптера PID (Manual/Basic)

Цей посібник пояснює, як _вручну_ налаштувати петлі PID на PX4 для всіх [багтроторних налаштувань](../airframes/airframe_reference.md#copter) (Квадрокоптери, Гексакоптери, Октокоптери тощо).

:::tip
[Автопідстроювання](../config/autotune.md) рекомендується для більшості користувачів, оскільки воно набагато швидше, простіше і забезпечує хороше налаштування для більшості кадрів. Рекомендується ручна настройка для кадрів, де автоналаштування не працює, або де важлива дотюнінг.
:::

Загалом, якщо ви використовуєте відповідну [підтримувану конфігурацію рами](../airframes/airframe_reference.md#copter), налаштування за замовчуванням повинні дозволити вам безпечно керувати транспортним засобом. Налаштування рекомендується для всіх нових налаштувань транспортних засобів, щоб отримати _найкращу продуктивність_, оскільки відносно невеликі зміни у апаратурі та збірці можуть впливати на необхідні вигоди налаштування для оптимального польоту. Наприклад, різні ESC або двигуни змінюють оптимальні налаштування коефіцієнтів настройки.

## Вступ

PX4 використовує **П**ропорційні, **I**нтегральні, **П**иференційні (PID) контролери (це найбільш поширена техніка керування).

Настройка _QGroundControl_ **PID Tuning** забезпечує реальні графіки установки транспортного засобу та кривих відгуку. Метою налаштування є встановлення значень P/I/D таким чином, щоб крива _Відповідь_ максимально точно відповідала криві _Setpoint_ (тобто швидка відповідь без перевищень).

![QGC Rate Controller Tuning UI](../../assets/mc_pid_tuning/qgc_mc_pid_tuning_rate_controller.png)

Контролери рівні, що означає, що контролер більш високого рівня передає свої результати контролеру нижчого рівня. Найнижчий контролер - це **контролер рівня**, за яким слідує **контролер напрямку**, і, нарешті, **контролер швидкості & положення**. Налаштування PID потрібно виконати в тому ж порядку, починаючи з регулятора швидкості, оскільки воно вплине на всі інші регулятори.

Процедура тестування для кожного контролера (швидкість, ставлення, швидкість/позиція) та осі (імовірність, кочення, тангаж) завжди однакова: створіть швидку зміну заданої точки, швидко рухаючи палицями, та спостерігайте за реакцією. Потім налаштуйте слайдери (як обговорено нижче), щоб покращити відстеження реакції на задане значення.

:::tip

- Налаштування регулятора швидкості є найважливішим, і якщо воно налаштовано добре, інші регулятори часто не потребують жодних або лише незначних корекцій
- Зазвичай для кочення і тангажу можна використовувати ті ж самі коефіцієнти налаштування.
- використовуйте режим Acro/Stabilized/Altitude для налаштування контролера швидкості
- Використовуйте [Режим позиції](../flight_modes_mc/position.md), щоб налаштувати _Контролер швидкості_ та _Контролер позиції_. Переконайтеся, що ви перейшли в режим _Простого керування позицією_, щоб ви могли генерувати крокові входи. ![QGC PID tuning: Simple control selector](../../assets/mc_pid_tuning/qgc_mc_pid_tuning_simple_control.png)
:::

## Передумови

- Ви вибрали найближчу відповідність [конфігурації рами за замовчуванням](../config/airframe.md) для вашого транспортного засобу. Це повинно дати вам транспортний засіб, який вже літає.
- Ви повинні були зробити [Калібрування ESC](../advanced_config/esc_calibration.md).
- Якщо використовується вихід PWM, їх мінімальні значення повинні бути правильно встановлені в [Конфігурації приводів](../config/actuators.md). Ці значення повинні бути встановлені низькими, але такими, що **двигуни ніколи не зупиняються**, коли транспортний засіб зброєний.

  Це можна протестувати в [режимі Acro](../flight_modes_mc/acro.md) або в [режимі Ручного/Стабілізованого керування](../flight_modes_mc/manual_stabilized.md):

  - Видаліть пропелери
  - Збройте транспортний засіб і знизьте оберти до мінімуму
  - Нахиліть транспортний засіб у всі напрямки, близько 60 градусів
  - Перевірте, чи не вимикаються мотори

- Використовуйте високошвидкісне телеметричне з'єднання, таке як WiFi, якщо це взагалі можливо (типовий телеметричний радіо з невеликим діапазоном не є достатньо швидким для отримання реального часу зворотнього зв'язку та графіків). Це особливо важливо для регулятора швидкості.
- Вимкніть [MC_AIRMODE](../advanced_config/parameter_reference.md#MC_AIRMODE) перед налаштуванням транспортного засобу (є опції для цього на екрані налаштування PID).

:::warning
Погано налаштовані транспортні засоби ймовірно нестійкі, і легко можуть зіткнутися. Переконайтеся, що призначено [Вимикач аварійного вимкнення](../config/safety.md#emergency-switches).
:::

## Параметри налаштування

Параметри налаштування такі:

1. Озброїте транспортний засіб, злітайте та тримайтеся у повітрі (зазвичай у режимі [Режим позиції](../flight_modes_mc/position.md)).
1. Відкрийте _QGroundControl_ **Налаштування Транспортного Засобу > Налаштування PID** ![QGC Rate Controller Tuning UI](../../assets/mc_pid_tuning/qgc_mc_pid_tuning_rate_controller.png)
1. Виберіть вкладку **Контролер швидкості**.
1. Підтвердіть, що селектор режиму повітряних режимів встановлено на **Вимкнуто**
1. Встановіть _значення кривої тяги_ на: 0.3 (PWM, контролери на основі потужності) або 1 (RPM-контролери)

   ::: info Для ШІМ, контролерів швидкості на основі потужності та (деяких) контролерів швидкості UAVCAN, відношення сигналу керування до тяги може не бути лінійним. Як результат, оптимальне налаштування при потужності утримання може бути не ідеальним, коли транспортний засіб працює на вищій потужності.

   Значення кривої тяги може бути використане для компенсації цієї нелинійності:

   - Для PWM контролерів, 0.3 є хорошим значенням за замовчуванням (яке може бути корисним для [подальшої настройки](../config_mc/pid_tuning_guide_multicopter.md#thrust-curve)).
   - Для контролерів на основі RPM використовуйте 1 (додаткове налаштування не потрібно, оскільки вони мають квадратичну криву тяги).

   Для отримання додаткової інформації дивіться [детальний PID посібник з налаштування](../config_mc/pid_tuning_guide_multicopter.md#thrust-curve).
:::

1. Встановіть радіокнопку _Вибір налаштування_ на: **Roll**.
1. (Опціонально) Виберіть прапорець **Автоматичного Перемикання Режиму Польоту**. Це _автоматично_ перемкнеся з режиму [Режим позиції](../flight_modes_mc/position.md) на [Режим стабілізації](../flight_modes_mc/manual_stabilized.md), коли ви натискаєте кнопку **Start**
1. Для налаштування регулятора швидкості переключіться в режим _Acro_, _Stabilized_ або _Altitude_ (якщо не ввімкнено автоматичне перемикання).
1. Виберіть кнопку **Start**, щоб почати відстеження кривих задання та відповіді.
1. Швидко пересувайте _roll stick_ на повний діапазон і спостерігайте за відгуком кроку на графіках. :::tip Припиніть відстеження, щоб забезпечити більш зручний огляд графіків. Це відбувається автоматично, коли ви збільшуєте/панорамуєте. Використовуйте кнопку **Start**, щоб перезапустити графіки, а також **Clear**, щоб скинути їх.
:::
1. Змініть три значення PID, використовуючи повзунки (для налаштування швидкості кочання ці значення впливають на `MC_ROLLRATE_K`, `MC_ROLLRATE_I`, `MC_ROLLRATE_D`), і знову спостерігайте за відгуком на крок. Значення зберігаються на транспортний засіб, як тільки переміщуються слайдери. ::: info Метою є те, щоб крива _Відповідь_ максимально точно відповідала криві _Setpoint_ (тобто швидка відповідь без перевищень). ::: Дані PID можуть бути скориговані таким чином:
   - P (пропорційне) або К підсилення:
     - збільште це для більшої реакції
     - зменшити, якщо відповідь перевищує і / або коливається (до певної міри збільшення значення D також допомагає).
   - D (похідне) надходження:
     - це можна збільшити, щоб заглушити перевищення та коливання
     - збільшуйте це лише настільки, наскільки це потрібно, оскільки це підсилює шум (і може призвести до нагрітих моторів)
   - I (інтегральний) коефіцієнт отримання:
     - використовується для зменшення поміченої похибки стану рівноваги
     - якщо значення занадто низьке, відповідь може ніколи не досягти заданої точки (наприклад, у вітрових умовах)
     - якщо занадто високий, можуть виникнути повільні коливання
1. Повторіть процес налаштування вище для крена та курсу:
   - Використовуйте радіокнопку _Вибір налаштування_ для вибору вісі для налаштування
   - Перемістіть відповідні палички (тобто паличку крена для крена, паличку риштування для риштування).
   - Для налаштування крену почніть з тих самих значень, що й для крену. :::tip Використовуйте кнопки **Зберегти в буфер обміну** та **Скинути з буфера обміну** для копіювання налаштувань ролів для початкових налаштувань кроку.
:::
1. Повторіть процес налаштування контролера нахилу для всіх осей.
1. Повторіть процес налаштування контролерів швидкості та позицій (на всіх осях).

   - Використовуйте режим позиції при налаштуванні цих контролерів
   - Виберіть опцію **Простий контроль позиції** у селекторі _режиму керування позицією ..._ (це дозволяє пряме керування для генерації крокових входів)

     ![QGC PID tuning: Simple control selector](../../assets/mc_pid_tuning/qgc_mc_pid_tuning_simple_control.png)

Готово! Пам'ятайте увімкнути повітряний режим перед виходом з налаштувань.