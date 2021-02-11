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

### Multi-function, layer-aware rotary encoder
Make the previous example work differently based on the active layer.

{{% notice note %}}
You will need the `process_record_user` function, the `custom_keycodes` enum and the `secondary_function` boolean from the previous example.
{{% /notice %}}

```c
enum layer_name {
    _BASE,
    _FN
};

// Define what encoder does depending on direction flag.
void encoder_update_user(uint8_t index, bool clockwise) {
  // the first encoder, in case you have more
  if (index == 0) {
    if (layer_state_is(_BASE)) {
      if (clockwise) {
        if (secondary_function) {
          tap_code(KC_RIGHT);
        }
        else {
          tap_code(KC_UP); 
        }
      }
      else {
        if (secondary_function) {
          tap_code(KC_LEFT);
        }
        else {
          tap_code(KC_DOWN);
        }
      }
    }
    else if (layer_state_is(_FN)) {
      if (clockwise) {
        if (secondary_function) {
          tap_code(KC_F12);
        }
        else {
          tap_code(KC_VOLU); 
        }
      }
      else {
        if (secondary_function) {
          tap_code(KC_F11);
        }
        else {
          tap_code(KC_VOLD);
        }
      }
    }
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

---

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
