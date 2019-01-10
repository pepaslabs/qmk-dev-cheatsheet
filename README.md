# qmk-dev-cheatsheet
Some notes about QMK internals

## Some important types



### `union action_t`

[From `action_code.h`](https://github.com/qmk/qmk_firmware/blob/84c24188176c3235b7064a9a77855f042ada2314/tmk_core/common/action_code.h#L137):

```c
typedef union {
    uint16_t code;
    struct action_kind {
        uint16_t param  :12;
        uint8_t  id     :4;
    } kind;
    struct action_key {
        uint8_t  code   :8;
        uint8_t  mods   :4;
        uint8_t  kind   :4;
    } key;
    struct action_layer_bitop {
        uint8_t  bits   :4;
        uint8_t  xbit   :1;
        uint8_t  part   :3;
        uint8_t  on     :2;
        uint8_t  op     :2;
        uint8_t  kind   :4;
    } layer_bitop;
    struct action_layer_tap {
        uint8_t  code   :8;
        uint8_t  val    :5;
        uint8_t  kind   :3;
    } layer_tap;
    struct action_usage {
        uint16_t code   :10;
        uint8_t  page   :2;
        uint8_t  kind   :4;
    } usage;
    struct action_backlight {
        uint8_t  level  :8;
        uint8_t  opt    :4;
        uint8_t  kind   :4;
    } backlight;
    struct action_command {
        uint8_t  id     :8;
        uint8_t  opt    :4;
        uint8_t  kind   :4;
    } command;
    struct action_function {
        uint8_t  id     :8;
        uint8_t  opt    :4;
        uint8_t  kind   :4;
    } func;
    struct action_swap {
        uint8_t  code   :8;
        uint8_t  opt    :4;
        uint8_t  kind   :4;
    } swap;
} action_t;
```

## Manually sending keycodes

[From `quantum.h`](https://github.com/qmk/qmk_firmware/blob/9c2d77612391c1c762dc53e53aab4f91c50d22f8/quantum/quantum.h#L244):

```c
void register_code16(uint16_t code);
void unregister_code16(uint16_t code);
void tap_code16(uint16_t code);
```

## The loop


- [`process_record_quantum()`](https://github.com/qmk/qmk_firmware/blob/9c2d77612391c1c762dc53e53aab4f91c50d22f8/quantum/quantum.c#L206)
  - layer is determined via `layer_switch_get_layer()`
  - keycode is determined via `keymap_key_to_keycode()`
  - calls `process_action_kb()`


## Mods

- Check for a mod: `if (get_mods() & MOD_BIT(KC_LSHIFT))`
- Activate mods: `register_mods(MOD_BIT(KC_RSFT))`
- Deactivate mods: `unregister_mods(MOD_BIT(KC_RSFT))`


## Adding / removing keys from the current report

[See action_util.h](https://github.com/qmk/qmk_firmware/blob/9c2d77612391c1c762dc53e53aab4f91c50d22f8/tmk_core/common/action_util.h#L32)

- `add_key()`
- `del_key()`
- `clear_keys()`

- `add_mods()`
- `del_mods()`
- `set_mods()`
- `clear_mods()`


## Transmissing key events to the computer

- `send_keyboard_report()`
