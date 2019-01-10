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
