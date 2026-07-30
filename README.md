# Sofle RGB MX v1 — ZMK

Конфигурация ZMK для split-клавиатуры **Sofle RGB MX v1** на двух
контроллерах **nice!nano v2**.

## Что включено

- официальный shield `sofle` из ZMK v0.3;
- беспроводная связь между половинами;
- левая половина — central (USB и Bluetooth);
- правая половина — peripheral;
- два энкодера;
- RGB: 36 WS2812 на каждой половине, 72 светодиода суммарно;
- пять Bluetooth-профилей;
- отдельная прошивка `settings_reset` для сброса сохранённых привязок.

OLED по умолчанию выключены для экономии энергии. Если SSD1306-дисплеи
установлены, раскомментируйте `CONFIG_ZMK_DISPLAY=y` в
`config/sofle.conf`.

## Сборка

После каждого push GitHub Actions собирает архив `firmware`. В нём:

- `sofle_rgb_mx_v1_left.uf2` — прошить в левый nice!nano v2;
- `sofle_rgb_mx_v1_right.uf2` — прошить в правый nice!nano v2;
- `sofle_rgb_mx_v1_right_rc_oscillator.uf2` — диагностический вариант
  правой половины с внутренним RC-генератором вместо кварца 32,768 кГц;
- `settings_reset_nice_nano_v2.uf2` — очистка настроек и BLE-привязок.

Для прошивки дважды быстро нажмите Reset на нужном контроллере. Появится
USB-диск `NICENANO`; скопируйте на него соответствующий `.uf2`.

При первой установке рекомендуется:

1. Прошить `settings_reset_nice_nano_v2.uf2` в обе половины.
2. Затем прошить левую и правую половины соответствующими файлами.
3. Включить обе половины и заново выполнить Bluetooth-сопряжение.

## Управление

- `Lower + Raise` включает слой `Adjust`.
- На `Adjust`: RGB toggle, яркость, оттенок, насыщенность и эффект.
- На `Raise`: выбор Bluetooth-профиля и очистка текущего профиля.
- Левый энкодер регулирует громкость, правый — Page Up/Page Down.

Раскладка находится в `config/sofle.keymap` и совместима с
[ZMK Keymap Editor](https://nickcoutsos.github.io/keymap-editor/).

Документация: [ZMK](https://zmk.dev/docs/).
