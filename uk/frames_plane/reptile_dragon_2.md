# Reptile Dragon 2 (RD2) Збірка

Reptile Dragon 2 - це двомоторний літак RC, спеціально розроблений для ефективного польоту FPV [(перегляд з першої особи)](https://en.wikipedia.org/wiki/First-person_view_(radio_control)). Будучи специфічним для FPV, RD2 оптимізований для легкого монтажу камер, сенсорів, логічної електроніки, великих батарей, антен та інших компонентів навантаження, які можуть бути знайдені на типовому літаку FPV. Цей акцент на корисне навантаження робить цей літак ідеальним кандидатом для установки PX4.

![Finished Reptile Dragon 2 airframe front](../../assets/airframes/fw/reptile_dragon_2/airframe_front.jpg)

![Finished Reptile Dragon 2 airframe rear](../../assets/airframes/fw/reptile_dragon_2/airframe_rear.jpg)


## Загальний огляд

Метою цієї побудови було створити ефективну, довготривалу платформу FPV для загального тестування та розвитку PX4.

Основні особливості конструкції фюзеляжу:

- Просторий салон
- Easy access to the entire fuselage cavity with large top hatch
- Rear hatch
- Removable V tail or conventional tail options included
- Threaded inserts in the wings and fuselage top for external mounting
- Numerous mounting features
  - Top antenna hole
  - Top GPS cover
  - Side "T" antenna mounts
  - Rear electronics tray
  - Front facing "action cam" cutout
  - Front facing FPV camera cutout
- Removable wings
- Low stall speed
- Gentle handling

Key build features

- Easy overall build
- Easy access to Pixhawk and all peripherals
- FPV with camera pan mount
- Air data from pitot/static probe
- ~40 minute long flight times

## Список деталей

- [Reptile Dragon 2 kit](https://usa.banggood.com/REPTILE-DRAGON-2-1200mm-Wingspan-Twin-Motor-Double-Tail-EPP-FPV-RC-Airplane-KIT-or-PNP-p-1805237.html?cur_warehouse=CN&ID=531466)

- [ARK6X FMU](https://arkelectron.com/product/arkv6x/)
- [ARK6X carrier](https://arkelectron.com/product/ark-pixhawk-autopilot-bus-carrier/)
- [Alternative FMU carrier: Holybro Pixhawk 5x Carrier board](https://holybro.com/products/pixhawk-baseboards)
- [Holybro power module](https://holybro.com/products/pm02d-power-module)
- [Holybro M9N GPS module](https://holybro.com/products/m9n-gps)
- Holybro PWM breakout board
- MS4525DO differential pressure module and pitot tube
- [Caddx Vista FPV air unit](https://caddxfpv.com/products/caddx-vista-kit)
- [Emax ES08MA ii](https://emaxmodel.com/products/emax-es08ma-ii-12g-mini-metal-gear-analog-servo-for-rc-model-robot-pwm-servo)
- [DJI FPV Goggles](https://www.dji.com/fpv)
- [ExpressLRS Matek Diversity RX](http://www.mateksys.com/?portfolio=elrs-r24)
- [5V BEC](https://www.readymaderc.com/products/details/rmrc-3a-power-regulator-5-to-6-volt-ubec)
- [4s2p 18650 LiIon flight battery](https://www.upgradeenergytech.com/product-page/6s-22-2v-5200mah-30c-dark-lithium-xt60)
- [Custom designed 3D printed parts](https://github.com/PX4/PX4-user_guide/raw/main/assets/airframes/fw/reptile_dragon_2/rd2_3d_printed_parts.zip)
  - ARK6X carrier mount
  - Holybro Pixhawk 5x carrier mount
  - FPV pod and camera mount
  - Pitot static probe "plug" adapter
- [Custom designed power distribution PCB](https://github.com/PX4/PX4-user_guide/raw/main/assets/airframes/fw/reptile_dragon_2/xt30_power_distro_pcb.zip)
- Misc hardware: M3 hardware (standoffs, washers, O-rings, bolts), M2.5 nylon standoffs and screws, XT30 connectors, hot glue, heatshrink, Molex Microfit connectors
- Silicone wiring (14awg for high current, 16awg for low current, 22awg for low power and signals)

## Інструменти

Наступні інструменти використовувалися у цій збірці.

- Тестер сервопривода (з кнопкою центрування)
- Набір викруток
- 3D-принтер
- Набір гаєчних ключів
- Клей: гарячий клей, клей CA (цианакрилат), клей "Foamtac"
- Sandpaper

## Побудова фюзеляжа

Літак потребує певної збірки з коробки. Сервоприводи, крила та хвіст потрібно встановити.

:::info

Для цієї частини збірки інструкція, включена в комплект, повинна бути достатньою, але нижче наведено деякі корисні поради.
:::


### Наклейка піни

При склеюванні пінних деталей RD2 разом використовуйте шкурку для шліфування монтажної поверхні, а потім використовуйте клей CA. Якщо піну не шліфувати шкуркою, клей не матиме поверхні, за яку можна було б "захопити" піну, і з'єднання буде погане.

Foamtac не здається добре прилипає до цієї піни, тому я використовував клей CA для всіх пар піни-піни.

### Захисна пластина

Підкладка, яка поставляється з RD2, потребує підгонки під розмір.

![Skid plate installed on the bottom of the RD2 airframe](../../assets/airframes/fw/reptile_dragon_2/skid_plate.jpg)

Відріжте формування плісняви з плоского боку піддону. Використовуйте грубий шліфувальний папір, щоб згрубіти внутрішню поверхню бронепластини, а також поверхню з'єднання знизу каркасу. Після перевірки на герметичність використовуйте клей CA для склеювання підкладки до нижньої частини RD2.

### Встановлення сервоприводу

:::info

Перед встановленням сервоприводу рекомендується використовувати шліфувальний папір, щоб зробити грубою сторону сервоприводу, яка спрямована на кришку сервоприводу. Під час остаточного встановлення покладіть краплю Foamtac між сервоприводом та кришкою. Це запобіжить сервоприводу рухатися після встановлення.
:::


![Correctly adjusted servo linkage installation](../../assets/airframes/fw/reptile_dragon_2/servo_linkage.jpg)

Сервоприводи на RD2 підключені до поверхонь керування з регульованими зчепленнями сервоприводів. Інструкції RD2 відзначать, що кожна керуюча поверхня використовує конкретну довжину зв'язки (включено в комплект). Переконайтеся, що виміряли кожне з'єднання перед установкою, щоб бути впевненими, що це правильна довжина з'єднання для цієї поверхні керування. Дуже важливо вирівнювати сервоприводи так, щоб механічний діапазон сервопривода був добре вирівняний з механічним діапазоном поверхні керування. Коли серводвигун знаходиться в центрі, рука серводвигуна повинна знаходитися під кутом 90 градусів до серводвигуна, а поверхня керування повинна бути приблизно в центрі. Можливо, не вдасться досягти ідеального вирівнювання, тому будь-який залишковий зсув буде виправлено в програмному забезпеченні.

The following steps can be used to perform servo alignment:

1. Begin with the servo outside of the airplane
2. Use the servo tester to move the servo to its center point
3. Install the servo horn with the included retaining screw, taking care to align the horn to extend as close as possible to 90 degrees out on the correct side of the servo
4. Install the servo in the servo pocket on the airplane
5. Install the linkage, and twist to adjust it such that the control surface is as close to centered as possible

:::info

Рогова важіль ймовірно не буде точно знаходитися під кутом 90 градусів до сервоприводу через зубці на валу сервоприводу. Ви можете побачити це на зображенні налаштування, показаному вище. Просто наблизьте його достатньо до 90 градусів, і залишкове зміщення буде видалено або зв'язкою, або пізніше у програмному забезпеченні.
:::


## Монтаж модуля GPS/Компасу

GPS/Компас повинен бути встановлений на задній полиці електроніки, що постачається з RD2. Це місце далеко ззаду від електропроводки (і будь-чого іншого, що може спричиняти магнітні перешкоди), що робить його ідеальним місцем для модуля GPS/компасу

![GPS tray installed in the RD2 airframe](../../assets/airframes/fw/reptile_dragon_2/gps_tray.jpg)

Модуль GPS може бути видалений зі свого пластикового корпусу для можливості використання отворів для монтажу. Then use the nylon M3 hardware to attach it to the rear electronics shelf.

Дві з трьох необхідних отворів вже випадково розташовані в лотку для електроніки, тому я використовував маркер і дриль, щоб позначити і просвердлити третій отвір.


## FPV Підставка

### FPV Pod Assembly

Спочатку встановіть сервопривід ES08MA ii в кишеню сервопривода FPV-поду. Сервопривід просто повинен вкладатися, з кабелем, що виходить з FPV-підсистеми через отвір у кишені сервопривода. Використовуйте краплю клею Foamtac, щоб закріпити сервопривід.

![Camera carrier with servo horn installed](../../assets/airframes/fw/reptile_dragon_2/camera_carrier.jpg)

Використовуйте один з кілець керування, включених в комплект ES08ma ii. Виріжте ріг так, щоб він вліз у паз у камери FPV-підвіски для камери. Воно повинно лежати рівно на дні слоту. Закріпіть ріг з клеєм CA.

Використовуйте тестер сервоприводу, щоб вирівняти сервопривод. Прикріпіть рогатку кар'єра камери безпосередньо до верхушки сервоприводу та закріпіть її за допомогою включеної гвинта. Закріпіть камеру DJI FPV в кар'єрі за допомогою двох бічних гвинтів.

Для завершення зборки FPV-поду встановіть Caddx Vista на задню частину поду за допомогою довгих болтів M2, стійок 1 мм та гайок з фіксацією.

![FPV pod close up mounted on the RD2 airframe](../../assets/airframes/fw/reptile_dragon_2/fpv_pod.jpg)

### Встановлення корпусу FPV Pod

FPV-под був встановлений зверху на кришку акумулятора за допомогою нейлонових болтів M3 з двома ущільнювальними кільцями для відстані FPV-под від кришки акумулятора.

## Встановлення польотного комп'ютера

:::info

Ця збірка сумісна як з перевізником ARK6X, так і з перевізником Holybro 5X. Інструкції надано для обох.
:::


![ARK Carrier assembled to mount](../../assets/airframes/fw/reptile_dragon_2/base_plate.jpg) RD2 поставляється з дерев'яною базовою платою електроніки, попередньо приклеєною в корпус. На цьому зображенні використовуються два набори вказівок маркерів, щоб показати, де повинні розташовуватися кріплення для кожного кріплення кар'єра; один маркер для кріплення кар'єра Holybro 5X і два маркера для кріплення кар'єра ARK5X.

### ARK6X Carrier (Рекомендовано)

A custom 3D printed mount was made for the ARK6X carrier. Для кріплення перевізника ARK6X до кріплення використовувалися деталі з нейлону M2.5.

![ARK6X carrier parts](../../assets/airframes/fw/reptile_dragon_2/ark_carrier_parts.jpg) ![ARK6X carrier assembled to mount](../../assets/airframes/fw/reptile_dragon_2/ark_carrier_assembled.jpg)

Кар'єр ARK6X не має звичайних роз'ємів виводу сервоприводів. Замість цього в ньому є один роз'єм JST GH, який містить 8 виходів сервоприводу FMU. A Holybro PWM breakout board was used to split the single JST GH PWM output connector into 8 individual servo channels.

![ARK6X carrier with PWM breakout](../../assets/airframes/fw/reptile_dragon_2/ark_carrier_pwm.jpg)

Носій ARK6X показаний тут, встановлений на базову пластину. Зверніть увагу на тилову частину носія, вирівняну проти двох позначок.

![ARK6X carrier installed](../../assets/airframes/fw/reptile_dragon_2/ark_carrier_mount.jpg)

Нарешті, ARK6X був встановлений угорі на гірському вершині.

![ARK6X carrier installed](../../assets/airframes/fw/reptile_dragon_2/ark_carrier_installed.jpg)


### Holybro 5X Carrier (Додатково)

Альтернативною платою перевізник є платформа Holybro Pixhawk 5X.

Оператор встановлений у пластиковому кейсі. Хоча справа виглядає гарно, це додаткова вага, тому переноска була видалена з чохла. Після видалення з корпусу ARK6X був встановлений, а захисна кришка встановлена зверху.

![Flight computer carrier board](../../assets/airframes/fw/reptile_dragon_2/holybro_5x.jpg)

Спеціальний кріплення для плати-носія Pixhawk 5X було розроблено та надруковано в 3D. Ця кріплення адаптує внутрішню плиту кріплення RD2 до отворів кріплення на платі розподільника Pixhawk 5X.

![Flight computer mount](../../assets/airframes/fw/reptile_dragon_2/holybro_5x_carrier_mount.jpg)

Важливо встановити цей кріплення в правильному місці всередині RD2; якомога далі назад. З великою батареєю та FPV капсулою спереду літак буде схильний до переваги ваги в ніс. Встановлення бортового комп'ютера далеко назад допоможе зберегти центр ваги повітряної рами (CG) у правильному місці.

![Flight computer mount](../../assets/airframes/fw/reptile_dragon_2/holybro_5x_carrier_mount_installed.jpg)

Зображення вище показують повністю завершену та підключену установку переносного пристрою Holybro 5X.

![Flight computer mount](../../assets/airframes/fw/reptile_dragon_2/holybro_electronics_0.jpg) ![Flight computer mount](../../assets/airframes/fw/reptile_dragon_2/holybro_electronics_1.jpg)


## Електрика

### Розподіл електроживлення батареї

Живлення батареї подається через модуль живлення Holybro, а потім на спеціально розроблену плату розподілу живлення PCB (друкована плата). З розподільної дошки живлення живлення від батареї розподіляється до BEC, обох ESCs та Caddx Vista через окремі роз'єми XT30.

![Power wiring in the RD2 airframe](../../assets/airframes/fw/reptile_dragon_2/power_0.jpg)

Без спеціальної плати PCB все ще легко розподілити живлення всім компонентам у літаку. This image shows an alternative solution constructed from an XT60 connecter wired to several XT30 connectors. The servo power BEC is also shown in this image.

![Alternative power distribution harness](../../assets/airframes/fw/reptile_dragon_2/alt_harness.jpg)


### Servo Power

Оскільки носій Holybro не містить вбудованого джерела живлення для сервоприводів, для подачі живлення на сервоприводи використовується зовнішній ["BEC"](https://en.wikipedia.org/wiki/Battery_eliminator_circuit). Вводні виводи EC були припаяні до роз'єму XT30, який був підключений до плати розподілу потужності. Вихід BEC можна підключити до будь-якого невикористаного виходу сервоприводу (я вибрав вихід IO 8).

### ESCs & Двигуни

![Esc and motor](../../assets/airframes/fw/reptile_dragon_2/esc_motor.jpg)

До 16awg проводів були припаяні кульові роз'єми, які потім були припаяні до кожного виходу фази на кожному РКШ. Термоусадка була зменшена на готових ESCs, а булетні роз'єми з ESCs були підключені до відповідних моторів.

Напрямок руху мотора залежить від порядку підключення витків мотора до ESC. Наразі припустіться з обох сторін. Якщо будь-який з двигунів обертається у неправильному напрямку, напрям можна змінити, помінявши місцями будь-які дві з'єднання. Правильна напрямок руху буде перевірено під час останньої перевірки перед польотом.

### Лінії сигналів Servos & ESC

Сервоприводи були підключені до порту виходу FMU в такому порядку: лівий елерон, правий елерон, лівий ESC, правий ESC, елеватор, руль, FPV панорамування.

:::info

[DSHOT ESC](../peripherals/dshot.md#wiring-connections) були використані (не PWM, як для сервоприводів). Для ефективного використання обмежень виходного порту DSHOT, два ESC мають бути підключені до вихідних каналів FMU 3 та 4.
:::


### Датчик швидкості повітря та пітот-трубка

Датчик швидкості повітря був підключений до порту I2C на платі-адаптері FMU за допомогою постачального кабелю з роз'ємом JST GH I2C.

![RD2 pitot plug](../../assets/airframes/fw/reptile_dragon_2/pitot_plug.jpg)

Пітот-трубка була протиснута через кріплення пітот-трубки та встановлена в виріз для Fpv-камери спереду.

Шланги пітоту/статики були вирізані на потрібну довжину та встановлені для з'єднання пітот-статичної зонди з датчиком швидкості повітря. Нарешті, датчик пітот-статики був приклеєний до бічної стінки корпусу літального апарата (за допомогою двостороннього скотчу).

### ELRS RX

Був виготовлений спеціальний кабель для підключення ELRS RX до порту `TELEM2` з роз'ємом JST GH на платі-адаптері FMU.

![ExpressLRS to telem port cable](../../assets/airframes/fw/reptile_dragon_2/elrs_cable.jpg)

Інший кінець кабелю був завершений роз'ємом Dupont для підключення до стандартно розміщених роз'ємів на ELRS RX. ELRS RX був підключений до кабелю, після чого термоусадка використовувалася для їх фіксації разом.

![ExpressLRS RX attached to telem port cable](../../assets/airframes/fw/reptile_dragon_2/elrs_rx_cable.jpg)

![ExpressLRS RX installed in the RD2 airframe](../../assets/airframes/fw/reptile_dragon_2/elrs_pitotstatic.jpg)

Тонка антенна трубка була протиснута через верх корпусу літального апарата, щоб установити одну з двох антен ELRS в вертикальне положення. Друга антена для різноманітності була приклеєна до бічної стінки корпусу літального апарата під кутом 90 градусів від положення першої антени. ELRS RX був прикріплений до бічної стінки корпусу літального апарата поруч з датчиком пітот-статики за допомогою двостороннього скотчу.

### USB

Був використаний кабель з USB-портом типу C з прямим кутом, щоб забезпечити легкий доступ до USB-порту типу C на FMU.

![Rear USB cable hatch](../../assets/airframes/fw/reptile_dragon_2/usb_hatch.jpg)

Кабель був встановлений таким чином, що він виходить з Pixhawk, спрямований до тильної частини літального апарата. Кабель продовжується до задньої кришки, де зайва довжина може бути безпечно змотана в вузол. Доступ до цього кабелю можна здійснити, просто вийшовши задню кришку і розплутавши кабель.

## Збірка прошивки

Ви не можете використовувати заздалегідь побудовану прошивку PX4 (або основну), оскільки вона залежить від модулів PX4 [crsf_rc](../modules/modules_driver.md#crsf-rc) та [msp_osd](../modules/modules_driver.md#msp-osd), які за замовчуванням не включені.

Для їх використання потрібна деяка налаштування.

Спочатку слід дотримуватися [цього посібника для налаштування середовища розробки](../dev_setup/dev_env.md) і [цього посібника для отримання вихідного коду PX4](../dev_setup/building_px4.md).

Після налаштування середовища збірки відкрийте термінал та виконайте команду `cd` в директорію `PX4-Autopilot`. Щоб запустити інструмент конфігурації [плати PX4 (`menuconfig`)](../hardware/porting_guide_config.md#px4-menuconfig-setup), виконайте:

```
make ark_fmu-v6x_default boardconfig
```

### `crsf_rc` Модуль

PX4 включає автономний модуль розбору CRSF, який підтримує телеметрію та CRSF LinkStatistics. Для використання цього модуля потрібно вимкнути модуль `rc_input` за замовчуванням та увімкнути модуль `crsf_rc`.

1. У інструменті конфігурації плати PX4 перейдіть до підменю `драйверів`, прокрутіть вниз, щоб виділити `rc_input`.
2. Використовуйте клавішу Enter, щоб видалити `*` з прапорця `rc_input`.
3. Прокрутіть вниз, щоб виділити підменю `RC`, а потім натисніть Enter, щоб відкрити його.
4. Прокрутіть вниз, щоб виділити `crsf_rc` і натисніть Enter, щоб увімкнути його.
5. Збережіть і вийдіть з інструменту конфігурації плати PX4.

Для отримання додаткової інформації див. [TBS Crossfire (CRSF) Telemetry](../telemetry/crsf_telemetry.md).

### Модуль `msp_osd`

Модуль `msp_osd` передає телеметрію MSP на вибраний серійний порт. Пристрій Caddx Vista Air підтримує прослуховування телеметрії MSP і відображає отримані значення телеметрії на своєму OSD (екрані).

1. У інструменті конфігурації плати PX4 перейдіть до підменю `драйверів`, прокрутіть вниз, щоб виділити `OSD`.
2. Використовуйте клавішу Enter, щоб відкрити підменю `OSD`
3. Прокрутіть вниз, щоб виділити `msp_osd` і натисніть Enter, щоб увімкнути його

### Побудова & Прошивання

Після увімкнення модулів `msp_osd` та `crsf_rc` та вимкнення модуля `rc_input`, потрібно скомпілювати вихідний код прошивки та збудувати образ, який потім буде прошитий в FMU.

Для компіляції та прошивання прошивки підключіть FMU/Carrier до ПК-хоста збудови через USB і виконайте:

```
make ark_fmu-v6x_default upload
```

## Конфігурація PX4

### Конфігурація параметрів

Цей файл параметрів містить настроювану конфігурацію параметрів PX4 для цієї збірки, включаючи налаштування радіо, налаштування і датчиків. Завантажте файл через QGC, використовуючи інструкції на [Parameters> Tools](https://docs.qgroundcontrol.com/master/en/qgc-user-guide/setup_view/parameters.html#tools) (Посібник користувача QGC).

- [Знімок параметрів аеродинамічної рами PX4](https://github.com/PX4/PX4-user_guide/raw/main/assets/airframes/fw/reptile_dragon_2/reptile_dragon_2_params.params)


Можливо, вам доведеться змінити деякі параметри для вашої збірки, зокрема вам слід перевірити:

- Параметр [MSP_OSD_CONFIG](../advanced_config/parameter_reference.md#MSP_OSD_CONFIG) повинен відповідати послідовному порту, який підключений до Caddx Vista (у цій збірці, `/dev/ttyS7`).
- Параметр [RC_CRSF_PRT_CFG](../advanced_config/parameter_reference.md#RC_CRSF_PRT_CFG) повинен відповідати послідовному порту, який підключений до ELRS RX (у цій збірці, `Telem 1`).

### Налаштування радіо

Вам слід активувати ручні, акро та позиційні режими на вашому контролері (принаймні для першого польоту). Інструкції дивіться у [Flight mode Configuration](../config/flight_mode.md)

Ми також рекомендуємо налаштувати [перемикач автоматичного](../config/autotune.md#enable-disable-autotune-switch-fixed-wing) налаштування для першого польоту, так як це полегшить включення / вимкнення автоматичного налаштування під час польоту.

Мапування каналів для цієї збірки включено в постачальний [файл параметрів](#parameter-config). Порядок каналів - це керування газом, крен, тангаж, рульове керування, (порожнє), і режим польоту

:::info

ExpressLRS потребує `AUX1` як "канал вибору режиму". Цей канал вибору режиму є окремим від механізму зброювання PX4 і використовується для повідомлення ELRS TX, що він може перемикатися в режим високої потужності передачі.

У мапуванні каналів PX4 я просто пропускаю цей канал. На моєму передавачі цей канал завжди встановлений на "високий", тому ELRS завжди в зброї.
:::


### Налаштування мотора & встановлення пропелера

Мотори та налаштування керування повітряними поверхнями виконуються в розділі [Actuator](../config/actuators.md). Постачений [файл параметрів](#parameter-config) відображає актуатори, як описано в цій збірці.

Комплект RD2 поставляється з годинними та проти годинною пропелерами для протиопорних двигунів. З протиопорними пропелерами літак може бути налаштований таким чином, що в нього не буде [критичних двигунів](https://en.wikipedia.org/wiki/Critical_engine).

З відсутністю критичних двигунів, керованість буде максимальною в разі виходу з ладу одного з двигунів. Напрямок обертання двигунів слід встановити таким чином, щоб пропелери оберталися до фюзеляжу зверху літака. Іншими словами, якщо ви дивитесь на лівий двигун, обертаючись від вас від літака, він повинен обертатися за годинниковою стрілкою, тоді як правий двигун повинен обертатися проти годинникової стрілки.

Після зняття пропелерів включіть живлення літака та використайте тест актуатора в QGC для запуску моторів. Якщо лівий або правий двигун не обертається у правильному напрямку, поміняйте місцями два з його кабелів ESC та перевірте ще раз. Наприкінці, коли обидва двигуни обертаються у правильному напрямку, скористайтеся ключем для закріплення пропелерів.

## Останні перевірки

Перед першим польотом необхідно провести всебічний попередній огляд.

Я рекомендую перевірити наступні елементи:

- Sensor calibration (QGC)
  - Mag calibration
  - Accelerometer calibration
  - Airspeed calibration
  - Level horizon calibration
- Check control surface deflection
 - Right stick -> Right aileron goes up, left aileron goes down
 - Left stick -> Left aileron goes up, right aileron goes down
 - Stick back -> elevator goes up -Stick forward -> elevator goes down
 - Left rudder -> Rudder goes left
 - Right rudder -> Rudder goes right
- Check Px4 inputs (in `stabilized mode`)
 - Roll right -> Right Aileron goes down
 - Roll left -> Left aileron goes down
 - Pitch up -> Elevator goes down
 - Pitch down -> Elevator goes up


## Перший політ

Рекомендую виконати перший зліт в ручному режимі. Тому що у цього літака немає шасі, вам доведеться кинути літак самостійно або, в ідеалі, мати помічника, який кине його. Під час кидання будь-якого літака кидайте з невеликим підняттям носа з повною потужністю двигуна.

Критично бути готовим дати введення задньої палиці, щоб запобігти літаку від удару по землі, якщо він випадково буде вирівняний носом вниз. Після того як літак успішно піднявся в повітря, вирушайте на висоту кількох сотень футів і переключіться на [Режим Акробатика](../flight_modes_fw/acro.md). Це гарний час для використання [Автоналагодження](../config/autotune.md) для налаштування конструкції повітряного каркасу.

Якщо літак гарно себе веде в режимі _Акро_, перемкніться на [Режим позиції](../flight_modes_fw/position.md).

## Результати збирання &  продуктивність

Загалом, ця збірка була успішною. RD2 добре літає в цій конфігурації та має достатньо місця на борту для сенсорів та додаткового обладнання.

### Продуктивність

- Швидкість зупинки: вказано 15 миль/год
- Круїзна швидкість: 35-50м/год
- Витривалість: ~40 хвилин о 28м/год

### Відео &  Журнали польотів

[Demo Flight log](https://review.px4.io/plot_app?log=6a1a279c-1df8-4736-9f55-70ec16656d1e)

FPV video of flight log:

@[youtube](https://www.youtube.com/watch?v=VqNWwIPWJb0&ab_channel=ChrisSeto)