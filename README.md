# Apogee Keyboard - ZMK Firmware

Custom ZMK firmware configuration for the Apogee 65% keyboard.

## Hardware Features

- **MCU**: Nice!Nano V2 (nRF52840)
- **Connectivity**: USB-C and Bluetooth
- **Matrix**: 5 rows × 16 columns via MCP23017 I2C GPIO expander
- **Encoders**: 3 rotary encoders with push buttons
- **Display**: 128×64 OLED (SSD1306)
- **Indicator**: Analog RGB LED (accent lighting)

## Layout

```
┌─────────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────┬─────────┬─────┐
│  ENC 1  │ ESC │  1  │  2  │  3  │  4  │  5  │  6  │  7  │  8  │  9  │  0  │  -  │  =  │BACKSPACE│ DEL │
├─────────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────────┼─────┤
│  ENC 2  │ TAB │  Q  │  W  │  E  │  R  │  T  │  Y  │  U  │  I  │  O  │  P  │  [  │  ]  │    \    │ M1  │
├─────────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┴─────────┼─────┤
│  ENC 3  │CAPS │  A  │  S  │  D  │  F  │  G  │  H  │  J  │  K  │  L  │  ;  │  '  │     ENTER     │ M2  │
├─────────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┼─────┴───────┬───────┼─────┤
│NAV LAYER│SHIFT│  Z  │  X  │  C  │  V  │  B  │  N  │  M  │  ,  │  .  │  /  │    SHIFT    │   ↑   │ M3  │
├─────────┼─────┼─────┼─────┼─────┴─────┴─────┴─────┴─────┼─────┼─────┼─────┼─────┬───────┼───────┤─────┤
│   RGB   │CTRL │ WIN │ ALT │            SPACE            │ FN  │ WIN │CTRL │  ←  │   ↓   │   →   │     │
└─────────┴─────┴─────┴─────┴─────────────────────────────┴─────┴─────┴─────┴─────┴───────┴───────┴─────┘
```

## Layers

### Layer 0 - Base (Default)
Standard QWERTY layout with:
- Encoder 1: Volume control (rotate), Play/Pause (press)
- Encoder 2: Volume control (rotate), Mute (press)
- Encoder 3: Volume control (rotate), Next Track (press)
- Macro keys (M1-M3): F13-F15 (can be remapped in OS or keymap)
- Layer Sel: Tap-dance to cycle through layers

### Layer 1 - Function (FN)
- Number row → F1-F12
- Grave/tilde on ESC position
- Delete → Insert
- Arrow keys → Page Up/Down, Home/End
- Print Screen, Scroll Lock, Pause

### Layer 2 - Navigation (Nav)
- Bluetooth profile selection (1-5)
- Bluetooth clear
- Output toggle (USB/BLE)
- Bootloader/Reset access
- Encoder 1: Brightness control

## Building

The firmware is automatically built via GitHub Actions when you push to main/master.

### Manual Build

1. Set up ZMK development environment
2. Clone this repo
3. Run: `west build -b nice_nano_v2 -- -DSHIELD=apogee`

## Files Structure

```
apogee-zmk-config/
├── .github/
│   └── workflows/
│       └── build.yml          # GitHub Actions workflow
├── config/
│   ├── boards/
│   │   └── shields/
│   │       └── apogee/
│   │           ├── Kconfig.defconfig
│   │           ├── Kconfig.shield
│   │           └── apogee.overlay
│   ├── apogee.conf            # Kconfig options
│   ├── apogee.keymap          # Keymap definition
│   └── west.yml               # ZMK module manifest
└── build.yaml                 # Build matrix definition
```

## Customization

### Modifying the Keymap

Edit `config/apogee.keymap` to change key bindings. Refer to the [ZMK documentation](https://zmk.dev/docs/codes/) for available keycodes.

### Encoder Behavior

Encoder bindings are defined per-layer in the `sensor-bindings` section of each layer.

### RGB LED

The analog RGB LED is currently set up for layer indication. Customization requires modifying the overlay and potentially adding custom behaviors.

## Troubleshooting

### Firmware won't build
- Check GitHub Actions logs for specific errors
- Ensure all files are in the correct locations

### Keys not registering
- Verify MCP23017 wiring and I2C address (should be 0x20)
- Check diode direction matches `col2row` setting

### OLED not working
- Verify I2C wiring (SDA/SCL)
- Check OLED I2C address (typically 0x3C)

### Encoders not working
- Verify A/B pin connections
- Try swapping A and B pins if direction is reversed

## License

MIT License - See LICENSE file for details.
