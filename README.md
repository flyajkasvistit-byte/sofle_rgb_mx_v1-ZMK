# Sofle RGB MX v1 — ZMK

Конфигурация ZMK для split-клавиатуры **Sofle RGB MX v1** на
**nice!nano v2**. Поддерживаются два отдельных режима:

- обычный — левая половина central, правая peripheral;
- с dongle — обе половины peripheral, третий nice!nano является central.

## Что включено

- официальный shield `sofle` из ZMK v0.3;
- обычный и dongle-вариант в одной сборочной матрице;
- два энкодера;
- RGB: 36 WS2812 на каждой половине, 72 светодиода суммарно;
- пять Bluetooth-профилей;
- SH1106 128×64 на dongle;
- экран со статусом подключения, WPM, слоем, модификаторами, lock-индикаторами
  и уровнем заряда обеих половин;
- анимированный Bongo Cat, включая overdrive-анимацию от 100 WPM;
- отдельная прошивка `settings_reset` для сброса сохранённых привязок.

OLED на самих половинах по умолчанию выключены. Экран включён только в
отдельной прошивке dongle.

## Сборка

После каждого push GitHub Actions собирает архив `firmware`.

Обычный режим:

- `sofle_rgb_mx_v1_left.uf2` — прошить в левый nice!nano v2;
- `sofle_rgb_mx_v1_right.uf2` — прошить в правый nice!nano v2.

Режим с dongle:

- `sofle_rgb_mx_v1_dongle_left.uf2` — левая половина как peripheral;
- `sofle_rgb_mx_v1_dongle_right.uf2` — правая половина как peripheral;
- `sofle_rgb_mx_v1_dongle.uf2` — отдельный nice!nano с экраном, central.

Общий файл:

- `settings_reset_nice_nano_v2.uf2` — очистка настроек и BLE-привязок
  любого из трёх контроллеров.

Для прошивки дважды быстро нажмите Reset на нужном контроллере. Появится
USB-диск `NICENANO`; скопируйте на него соответствующий `.uf2`.

### Обычный режим

1. Прошить `settings_reset_nice_nano_v2.uf2` в обе половины.
2. Прошить `sofle_rgb_mx_v1_left.uf2` в левую половину.
3. Прошить `sofle_rgb_mx_v1_right.uf2` в правую половину.
4. Включить обе половины и заново выполнить Bluetooth-сопряжение.

### Режим с dongle

1. Прошить `settings_reset_nice_nano_v2.uf2` во все три контроллера.
2. Прошить `sofle_rgb_mx_v1_dongle_left.uf2` в левую половину.
3. Прошить `sofle_rgb_mx_v1_dongle_right.uf2` в правую половину.
4. Прошить `sofle_rgb_mx_v1_dongle.uf2` в контроллер dongle.
5. Включить dongle, затем левую половину, дождаться соединения и включить
   правую половину.

При возврате из одного режима в другой снова выполните `settings_reset` на
всех участвующих контроллерах: peripheral хранит только одну split-привязку.

## Подключение экрана dongle

Рабочая схема повторяет dongle от Dactyl:

| SH1106 128×64 | nice!nano v2 |
| --- | --- |
| VCC | 3V3 |
| GND | GND |
| SDA | D2 |
| SCL | D3 |

Адрес дисплея — `0x3C`. Используется I²C 400 кГц.

## Управление

- `Lower + Raise` включает слой `Adjust`.
- На `Adjust`: RGB toggle, яркость, оттенок, насыщенность и эффект.
- На `Raise`: выбор Bluetooth-профиля и очистка текущего профиля.
- Левый энкодер регулирует громкость, правый — Page Up/Page Down.

Раскладка находится в `config/sofle.keymap` и совместима с
[ZMK Keymap Editor](https://nickcoutsos.github.io/keymap-editor/).

Экранные виджеты основаны на MIT-проекте
[zmk-dongle-display](https://github.com/englmaxi/zmk-dongle-display) и
адаптированы из рабочего конфига Dactyl.

Документация: [ZMK](https://zmk.dev/docs/).
