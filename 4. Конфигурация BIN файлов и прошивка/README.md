# Конфигурация BIN файлов и прошивка
<br />

## Конфигурация BIN файлов
* Устанавливаем на наш хост клиппер (инструкций в интернете полно) и переходим в папку конфигурации клиппера:
```shell
~cd
cd klipper
```
<br />

* Вбиваем вот такие параметры для генерации прошивки платы extruder board (плата головы)
```shell
make menuconfig
```
![eboard](https://i.ibb.co/pz9dkrV/Screenshot-5.png)
```shell
make clean
make
```
Cкачиваем получившийся бинарник.
<br />

* Вот такие для прошивки платы mainboard (основной платы)
```shell
make menuconfig
``` 
![mainboard](https://i.ibb.co/xGCpB4f/Screenshot-6.png)
```shell
make clean
make
```
Скачиваем получившийся бинарник.
<br />
<br />
p.s Разница между ними не велика, но для PB9 у mainboard нужно установить инверсию сразу, что бы при старте системы стол не грелся.
Компилируем, скачиваем, и с помощью программатора ST-Link и программного обеспечения stlink utility или stm32cubeprog  зашиваем наши микроконтроллеры.
<br />
Переходим к следующему пункту, прошивка
<br />
<br />
## Прошивка <br />
### Распиновка extruder board <br />
Распиновка eboard:
- vcc - 3.3V
- GND-GND
- DIO - SWDIO
- CLK-SWDCLK

### Распиновка mainboard <br />
Распиновка mainboard:
- GND-GND
- DIO - SWDIO
- CLK-SWDCLK
- vcc -  не подключаем, стлинк не потянет питание всей платы mainboard, просто включите питание принтера.

### Прошивка
Подключаем к STLINK поочередно платы, запускаем stm32cubeprog:
![cubeprog](https://i.ibb.co/1Q8fNgw/Screenshot-8.png)
<br />
<br />
Настройки выставляйте как у меня, и жмем connect.
- В случае если камень не подключился, понизьте частоту до 25 кгц, а reset mode переведите в режим core reset и переподключите программатор.
- В случае успеха вы увидите следующую картинку:
![cubeprog_1](https://i.ibb.co/zmN4hg7/Screenshot-9.png)
<br />
Нажмите кнопку open file и выберите скомпилированную и скаченную ранее прошивку для вашего микроконтроллера и нажмите кнопку download.
<br />
![cubeprog2](https://i.ibb.co/6bL6HKh/Screenshot-10.png)
После чего начнется загрузка прошивки в MCU. По завершению stm32cubeprog оповестит вас об успешной прошивке.
<br />
