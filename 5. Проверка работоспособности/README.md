# Проверка работоспособности
После предыдущих пунктов у вас на руках будет принтер с установленным клиппером, прошитыми микроконтроллерами, но без рабочего файла конфигурации, можете взять мой - printer.cfg, и скопировать его содержимое к себе.

## Чуть более подробно
Узнаем к какому UART интерфейсам подключены ваши платы. В моем случае это UART 5,6,7 - эти UART будут соответствовать физическим ttyS* портам, однако нужно помнить, что они займут номер  ttySn, по порядку от 0 до максимального числа юартов поддерживаемых вашим одноплатынм компьютер. Занимая более низкий номер в случае аппаратной не активации приведущего.<br /><br />
К примеру у Olimex A20 есть 7 UART интерфейсов uart0, uart2, uart3, uart4, uart5, uart6, uart7. Я активировал uart5, uart6, uart7 дял подклчюения всех плат, uart0 - активирован изначально, на него выводится отладочная информация. Так у меня для подключения плат будут соответствия : uart0 - ttyS0 , uart5 - ttyS1, uart6 - ttyS2, uart7 - ttyS3.<br /><br />
В случае использования usb-uart преобразователей найдите подключенные платы командой find /dev/serial/by-id . Занесли эти данные в конфиг, принтер готов к работе.<br /><br />
## Проведите :<br />
1) Пид калибровку 
```shell
PID_CALIBRATE HEATER=extruder TARGET=255
PID_CALIBRATE HEATER=heater_bed TARGET=80
```
2) Калибровку тензисторов на нужной вам температуре
   Нагрейте стол до нужной температуры, введите команду
    ```shell
    H7
    H1
    ```
   Установите 500 грам по середине стола (бутылка воды 0.5 литра) введите 
    ```shell
    H2
    H7
    ```
    Сохраните калибровку
     ```shell
     H3
     ```
4) Калибровку стола
```shell
BED_MESH_CALIBRATE PROFILE="default"
 ```
5) Калибровку шейперов
  ```shell
   G28
   TEST_RESONANCES AXIS=X
   TEST_RESONANCES AXIS=Y
  ```
  По ssh введите 
 ```shell
~/klipper/scripts/calibrate_shaper.py /tmp/resonances_x_*.csv -o /tmp/shaper_calibrate_x.png
~/klipper/scripts/calibrate_shaper.py /tmp/resonances_y_*.csv -o /tmp/shaper_calibrate_y.png
  ```
6) Отпечатайте тестовую модель<br /><br />

# На этом переход на внешний хост закончен.


если при попытке снятия шейперво вылетает ошибка <br /><br />
 ```shell
Failed to import `numpy` module, make sure it was installed via `~/klippy-env/bin/pip install` (refer to docs/Measuring_Resonances.md for more details).

  ```
<br /><br />
То откройте консоль вашего линукс компьютера и введите следующее, это установит недостающие пакеты
<br /><br />
 ```shell
sudo apt update
sudo apt install python3-numpy python3-matplotlib libatlas-base-dev libopenblas-dev

~/klippy-env/bin/pip install -v numpy
  ```
