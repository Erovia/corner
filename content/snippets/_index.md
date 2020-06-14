---
title: "Small code snippets"
menuTitle: "Snippets"
---

Here I collected some code snippets.  
You can use these as pointers to implement your desired functionality.

---

### Multi-function rotary encoder
Change the encoder's behavour when it is pressed as a key.

```c
bool secondary_function = false;

enum custom_keycodes {
    ENC_MODE = SAFE_RANGE
};

// Define what encoder does depending on direction flag.
void encoder_update_user(uint8_t index, bool clockwise) {
    if (clockwise) {
        if (secondary_function) {
            tap_code(KC_DOWN);
        } else {
            tap_code(KC_RIGHT);
        }
    } else {
        if (secondary_function) {
            tap_code(KC_UP);
        } else {
            tap_code(KC_LEFT);
        }
    }
}

// When custom keycode ENC_MODE is clicked, switch encoder direction
bool process_record_user(uint16_t keycode, keyrecord_t *record) {
  switch (keycode) {
    case ENC_MODE:
      if (record->event.pressed) {
          secondary_function = !secondary_function;
      }
      return false; // Skip all further processing of this key
    default:
      return true; // Process all other keycodes normally
  }
}
```

---

### Turn on Numlock for a specific layer
And turn it off on other layers

```c
layer_state_t layer_state_set_user(layer_state_t state) {
    switch (get_highest_layer(state)) {
    case _NUM:
        if (!host_keyboard_led_state().num_lock) {
             tap_code16(KC_NLCK);
        }
        break;
    default: //  for any other layers, or the default layer
        if (host_keyboard_led_state().num_lock) {
             tap_code16(KC_NLCK);
        }
        break;
    }
  return state;
}
```

### Change the behaviour of one key by another
This is really similar to the [encoder one](#multi-function-rotary-encoder)

```c
bool switcheroo = false;

bool process_record_user(uint16_t keycode, keyrecord_t *record) {
  switch (keycode) {
    case CL_BS:
      if (record->event.pressed) {
       switcheroo = !switcheroo; // Change CapsLock into BackSpace
      }
      return false; // Skip all further processing of this key
    case KC_CAPS:
      if (record->event.pressed) {
        if (switcheroo) {
            tap_code(KC_CAPS);
        } else {
            tap_code(KC_BSPC);
      } 
      return true; // Let QMK send the enter press/release events
    default:
      return true; // Process all other keycodes normally
  }
}
```
