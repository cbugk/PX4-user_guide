# VTOL без датчика повітряної швидкості

<Badge type="warning" text="Experimental" />

:::warning

Підтримка для VTOL-апаратів без датчика швидкості повітря вважається експериментальною і повинна використовуватися лише досвідченими пілотами.
Рекомендується використання датчика швидкості повітря.
:::

Фіксовані крила використовують [датчики швидкості](../sensor/airspeed.md) повітря для визначення швидкості руху літака повітрям. Залежно від вітру це може відрізнятися від швидкості руху на землі. У кожного літака є мінімальна швидкість повітря, нижче якої літак заходить в пікетаж. В умовах сприятливої погоди та з налаштуваннями, значно вищими за мінімальну швидкість пікетажу, VTOL може працювати без використання датчика швидкості повітря.

Цей посібник описує параметри, необхідні для обходу датчика швидкості повітря для VTOL-літаків.

::: info 
Більшість налаштувань, описаних тут, також можуть бути застосовані до фіксованих крил, які не є VTOL, але це наразі не тестувалося.
Поворот під час переходу та квадро-парашут - це специфічні для VTOL параметри.
:::

## Підготовка

Перш ніж спробувати вилучити датчик швидкості повітря, вам слід спочатку визначити безпечний рівень газу. Також необхідно знати тривалість переднього переходу. Для цього ви можете виконати референтний польот з датчиком швидкості повітря або пілотувати транспортний засіб вручну. У обох випадках референтний польот слід виконати в умовах дуже слабкого вітру.

Польот повинен відбуватися зі швидкістю, достатньою для польоту в умовах сильного вітру, і повинен складатися з таких етапів:

- Успішний передній перехід
- Прямий та рівний польот
- Агресивний поворот
- Швидке підняття на вищу висоту

## Огляд журналу

Після референтного польоту завантажте журнал та скористайтеся програмою [FlightPlot](../log/flight_log_analysis.md#flightplot) (або іншим інструментом аналізу) для його огляду. Побудуйте на графіку висоту (`GPOS.Alt`), тягу (`ATC1.Thrust`), швидкість (вираз: `sqrt(GPS.VelN\^2 + GPS.VelE\) ^2)`), подача (`ATT.Pitch`) і крен (`AT.Roll`).

Перевірте рівень дросельної заслінки (тягу), коли транспортний засіб стоїть рівно (відсутній або невеликий нахил і крен), під час підйому (збільшення висоти) і коли автомобіль накрениться (більший крен). Початковим значенням для використання в якості крейсерської швидкості має бути найвища тяга, застосована під час крену або підйому, тяга під час горизонтального польоту повинна вважатися мінімальним значенням, якщо ви вирішите надалі зменшити свою швидкість.

Також зверніть увагу на час, який знадобився для завершення переднього переходу. Це буде використано для встановлення мінімального часу переходу. З міркувань безпеки вам слід додати до цього часу +- 30%.

Нарешті, зверніть увагу на швидкість руху під час крейсерського польоту. Це можна використовувати для налаштування дросельної заслінки після першого польоту без датчика швидкості.

## Встановлення параметрів

Для обхідного передпольотного перевірки швидкості повітря потрібно встановити [SYS_HAS_NUM_ASPD](../advanced_config/parameter_reference.md#SYS_HAS_NUM_ASPD) на 0.

Для того щоб уникнути використання встановленого датчика швидкості повітря для зворотного керування, встановіть [FW_USE_AIRSPD](../advanced_config/parameter_reference.md#FW_USE_AIRSPD) на `False`. Це дозволить вам перевірити поведінку системи в умовах відсутності датчика швидкості повітря, зберігаючи при цьому фактичне значення швидкості повітря для перевірки безпеки відносно межі безпеки від відмивання та ін.

Встановіть тримання газу ([FW_THR_TRIM](../advanced_config/parameter_reference.md#FW_THR_TRIM)) в відсотках, як визначено з журналу референтного польоту. Зауважте, що QGC масштабує це від `1..100`, а значення тяги з журналу масштабується від `0..1`. Таким чином, значення тяги 0.65 слід ввести як 65. З міркувань безпеки рекомендується додати +- 10% газу до визначеного значення для тестування першого польоту.

Встановіть мінімальний час переднього переходу ([VT_TRANS_MIN_TM](../advanced_config/parameter_reference.md#VT_TRANS_MIN_TM)) у кількості секунд, визначеної з референтного польоту і додайте +- 30% для безпеки.

### Додаткові рекомендовані параметри

Оскільки ризик звалювання є реальним, рекомендується встановлювати 'мінімальну висоту для фіксованого крила' (він же. 'quad-chute') поріг ([VT_FW_MIN_ALT](../advanced_config/parameter_reference.md#VT_FW_MIN_ALT)).

Це призведе до того, що VTOL повернеться в режим мультикоптера та запустить [режим повернення](../flight_modes_vtol/return.md) нижче певної висоти. Ви можете встановити значення 15 або 20 метрів, щоб дати мультикоптеру час відновитися після зупинки.

Перевіреним для цього режиму оцінювачем позиції є EKF2, його можна встановити, змінивши [SYS_MC_EST_GROUP](../advanced_config/parameter_reference.md#SYS_MC_EST_GROUP).

## Перший польот без датчика швидкості повітря

Ці значення застосовуються до польоту з контролем позиції (наприклад, в [режимі утримання](../flight_modes_fw/hold.md), або [режимі місії](../flight_modes_vtol/mission.md)). Тому рекомендується налаштувати місію на безпечній висоті, приблизно на 10 метрів вище порогу квадратного парашуту.

Подібно до референтного польоту, цей польот слід виконувати в умовах дуже слабкого вітру. Для першого польоту рекомендується:

- Залишайтеся на одній висоті
- Встановіть точки маршруту достатньо широко і так, щоб не потрібні були гострі повороти
- Зберіть місію достатньо малою, щоб вона залишалася на виду, якщо знадобиться ручне керування.
- Якщо швидкість повітря дуже велика, розгляньте можливість виконання ручного переходу назад, перемикаючи в режим висоти.

Якщо місія завершилася успішно, вам слід перевірити журнал наступного:

- Швидкість на землі повинна бути значно вище, ніж швидкість на землі під час референтного польоту.
- Висота не повинна бути значно нижчою, ніж під час референтного польоту.
- Кут крена не повинен постійно відрізнятися від референтного польоту.

Якщо всі ці умови виконані, ви можете почати поступово знижувати рівень газу круїзу до тих пір, поки швидкість на землі не відповідає тій, що була під час референтного польоту.

## Огляд параметрів

Відповідними параметрами є:

- [FW_USE_AIRSPD](../advanced_config/parameter_reference.md#FW_USE_AIRSPD)
- [SYS_HAS_NUM_ASPD](../advanced_config/parameter_reference.md#SYS_HAS_NUM_ASPD)
- [SYS_MC_EST_GROUP](../advanced_config/parameter_reference.md#SYS_MC_EST_GROUP): EKF2 (2)
- [FW_THR_TRIM](../advanced_config/parameter_reference.md#FW_THR_TRIM): визначено (наприклад, 70%)
- [VT_TRANS_MIN_TM](../advanced_config/parameter_reference.md#VT_TRANS_MIN_TM): визначено (наприклад, 10 секунд)
- [VT_FW_MIN_ALT](../advanced_config/parameter_reference.md#VT_FW_MIN_ALT): 15