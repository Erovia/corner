---
title: "Frequently Asked Questions"
menuTitle: "FAQ"
weight: 5
---

Quick answers to frequent questions.

---

### Q: I'd like to learn QMK, but don't know where to start  
**A**: Follow the [Syllabus](https://docs.qmk.fm/#/syllabus)

---

### Q: Do I need to code to be able to use QMK?
**A**: Nope. A lot of people only use the [Configurator](https://config.qmk.fm). Even if someone decides to write code, usually they do it by adapting the examples in the documentation or other's code.  
(All roads lead to ~~Rome~~ [drashna's userspace](https://github.com/qmk/qmk_firmware/tree/master/users/drashna).)

---

### Q: What's the relationship between VIA and QMK?
**A**: [VIA](https://caniusevia.com) is a tool that allows you modify the keymap of your keyboard on the fly. The firmware on your keyboard will still be QMK.

---

### Q: How do I add VIA support for my board?
**A**: If the board is already supported by QMK, the steps are the following:

1. Create a new keymap for your board, called `via`
1. Create a `rules.mk` file in this new keymap's directory with `VIA_ENABLE=yes` in it
1. Create the [json file necessary for VIA](https://caniusevia.com/docs/specification) to know how to handle the board 

{{% notice tip %}}
You can also enable the VIA functionality for your already existing keymap.
{{% /notice %}}

---

### Q: I get `bash: python3: command not found` on Windows, what to do?
**A**: Restart the MinGW terminal and make sure to start "MinGW 64bit" and **not** "MSYS".

---

### Q: I ran `qmk setup` on Windows, but still get errors, what to do?
**A**: Restart "MinGW 64bit" and run `qmk doctor`. If you still get errors ask for help.

---

### Q: QMK Configurator - where is the keymap.c?
**A**: the Configurator no longer offers `keymap.c`, instead it provides a `keymap.json`. You have multiple possibilies with this file:

* convert it to `keymap.c`
```shell
qmk json2c /path/to/keymap.json -o keymap.c
```

* compile it directly from the json
```shell
qmk compile /path/to/keymap.json
```

* use it in place of keymap.c (see [the Minivan VIA keymap](https://github.com/qmk/qmk_firmware/tree/master/keyboards/thevankeyboards/minivan/keymaps/via) for example)

---

### Q: How do I use accented characters in QMK Configurator?
**A**: It's important to understand, that your keyboard does not send characters, but keycodes (actually [scancodes](https://en.wikipedia.org/wiki/Scancode), but for the sake of simplicity, let's say they are the same thing). What this keycodes will represent, depends on the locale settings of your OS.  
Let's say you'd like to type `ß`. The keycode for this letter in with German locale is the same is the `-`'s in the US English locale.  
Since Configurator uses the latter's keycodes, if you assign `KC_MINS` to a key, it will output `ß` with your German locale.  

{{% notice tip %}}
Use the content of the [extra keymaps](https://github.com/qmk/qmk_firmware/tree/master/quantum/keymap_extras) to find the necessary US English keycodes.
{{% /notice %}}

---

### Q: I've flashed the wrong firmware and my keyboard is unresponsive. Did I kill it?
**A**: Likely not. The bootloader should be intact on your keyboard, allowing you to flash the correct firmware. Resetting the board might be harder, though.  
Look for:

* a button on the underside of the PCB  
* a pair of pins (usually labelled RESET) that can be shorted with a tweezer  
* if none of the above is an option, you can also short the RST and GND pins on your board's microcontroller (*not recommended*)

{{% notice warning %}}
Shorting the pins on the microcontroller is potentially **dangerous**. Use it only as a last resort.
{{% /notice %}}

---

### Q: Help, Control and CapsLock (or Alt and GUI) are swapped!
**A**: You most likely activated a [bootmagic](https://docs.qmk.fm/#/feature_bootmagic) functionality. Check the docs for the key that can swap them back.
