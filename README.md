# Gamepad Menu

Gamepad button mapper that lives as a menu in your OS X status bar.

When trying to play Broforce (yes, [Broforce](http://www.broforcegame.com)) on a Mac with my SteelSeries Nimbus controller, I noticed most buttons were not supported by the game's gamepad configuration, probably because (for some weird reason) every single button on the Nimbus is pressure sensitive, except the "Menu" button. Existing Mac software for joystick mapping was either paid or crappy. More specifically, they had no configurable threshold for the pressure sensitive buttons, meaning even the simple action buttons had to be pressed down very firmly to trigger a keypress.

Of course, with those limitations, it's hard to get your bro on, so I decided to make my own button mapper. It is aimed exclusively at gamepads and, while focussed on getting Nimbus' pressure sensitive buttons working, supports other gamepads as well.

## Updated FORK
update for more keys and building towards all controller keys integration and support for [WoW consoleport](http://consoleport.net/home/) addon.
* profile support for newer PlayStation DualShock 4 Controllers with signature: Vendor=0x54c & Product=0x9cc
* presets for wow consoleport addon
* added more infos regarding AppleScript button keycodes. Validate buttons via https://keycode.info

## Features

* Uses device profiles to map each supported gamepad device to a common control set, which a preset maps to specific keyboard presses.

* Adjustable button threshold for pressure sensitive buttons and triggers.

* Smart thresholding stickiness filter to prevent double key presses on a jittery value.

* Kept simple on purpose. Only maps to keyboard, not mouse. Currently has no UI for creating device profiles or presets; this is done by editing plist files instead.

* To facilitate "Start at Login", the `ServiceManagement.framework` is used together with a helper app, which should work in a sandboxed environment as well (as long as the app lives in /Applications).

## Installation

Just fetch the latest ZIP from the [release section](https://github.com/robbertkl/GamepadMenu/releases) and put the extracted app into your Applications folder.

You might need to disable OS X Gatekeeper to run it: *System Preferences* > *Security & Privacy* > *General* tab > *Allow apps downloaded from: Anywhere*.

## Device profiles

Device profiles map known gamepad devices to "standardised" controls commonly found on gamepads. See [the Device Profiles folder](Resources/Device%20Profiles/) for all currently supported devices. A device profile plist contains 2 sections:

* `Identifier` contains a vendor ID + product ID used to identify newly connected devices. If the same device uses different identifiers (e.g. one for USB and another for bluetooth), you can make this an array of dictionaries (see [SteelSeries Nimbus](Resources/Device%20Profiles/SteelSeries%20Nimbus.plist) for an example).

* `Elements` contains a mapping from all device elements to the common gamepad controls. They are identified by `<usage-page>:<usage>`. Supported element types are:
    * `Button` - Regular button or trigger, either binary (0..1) or pressure sensitive (0..255). Has 1 key binding.
    * `Axis` - X or Y axis of an analog stick. Has 2 key bindings (left+right or up+down).
    * `Hat Switch` - Single element which maps an entire D-Pad. Has 4 key bindings. See [PlayStation 4 Controller](Resources/Device%20Profiles/PlayStation%204%20Controller.plist) element `1:57` for an example.

A useful tool to check out HID devices and their elements is [Apple's HID Calibrator code sample](https://developer.apple.com/library/mac/samplecode/HID_Calibrator/).

Please note the Xbox 360 controller requires a driver to work on OS X. I'm using [360Controller](https://github.com/360Controller/360Controller), which works perfectly on El Capitan. I only have a wired Xbox 360 controller, so I'm not sure how well it works with the wireless controllers. It would be nice if someone could test it and send me its vendor and product IDs, or a custom device profile if it differs from the wired version.

## Presets

Preset plist files contain the key bindings for each of the mapped common gamepad controls. Use a string value to bind it to a keyboard character, or a number to bind it to a `CGKeyCode`.

## Buttons Table Overview

Look here: https://eastmanreference.com/complete-list-of-applescript-key-codes
Or use this list where the first numeric value is the integer in the presets to map the equivalent button:
(for example 100 is triggering button F8)
    
0 = 0x00 = ANSI_A
1 = 0x01 = ANSI_S
2 = 0x02 = ANSI_D
3 = 0x03 = ANSI_F
4 = 0x04 = ANSI_H
5 = 0x05 = ANSI_G
6 = 0x06 = ANSI_Z
7 = 0x07 = ANSI_X
8 = 0x08 = ANSI_C
9 = 0x09 = ANSI_V
10 = 0x0A = ISO_Section
11 = 0x0B = ANSI_B
12 = 0x0C = ANSI_Q
13 = 0x0D = ANSI_W
14 = 0x0E = ANSI_E
15 = 0x0F = ANSI_R
16 = 0x10 = ANSI_Y
17 = 0x11 = ANSI_T
18 = 0x12 = ANSI_1
19 = 0x13 = ANSI_2
20 = 0x14 = ANSI_3
21 = 0x15 = ANSI_4
22 = 0x16 = ANSI_6
23 = 0x17 = ANSI_5
24 = 0x18 = ANSI_Equal
25 = 0x19 = ANSI_9
26 = 0x1A = ANSI_7
27 = 0x1B = ANSI_Minus
28 = 0x1C = ANSI_8
29 = 0x1D = ANSI_0
30 = 0x1E = ANSI_RightBracket
31 = 0x1F = ANSI_O
32 = 0x20 = ANSI_U
33 = 0x21 = ANSI_LeftBracket
34 = 0x22 = ANSI_I
35 = 0x23 = ANSI_P
36 = 0x24 = Return
37 = 0x25 = ANSI_L
38 = 0x26 = ANSI_J
39 = 0x27 = ANSI_Quote
40 = 0x28 = ANSI_K
41 = 0x29 = ANSI_Semicolon
42 = 0x2A = ANSI_Backslash
43 = 0x2B = ANSI_Comma
44 = 0x2C = ANSI_Slash
45 = 0x2D = ANSI_N
46 = 0x2E = ANSI_M
47 = 0x2F = ANSI_Period
48 = 0x30 = Tab
49 = 0x31 = Space
50 = 0x32 = ANSI_Grave
51 = 0x33 = Delete
53 = 0x35 = Escape
55 = 0x37 = Command
56 = 0x38 = Shift
57 = 0x39 = CapsLock
58 = 0x3A = Option
59 = 0x3B = Control
60 = 0x3C = RightShift
61 = 0x3D = RightOption
62 = 0x3E = RightControl
63 = 0x3F = Function
64 = 0x40 = F17
65 = 0x41 = ANSI_KeypadDecimal
67 = 0x43 = ANSI_KeypadMultiply
69 = 0x45 = ANSI_KeypadPlus
71 = 0x47 = ANSI_KeypadClear
72 = 0x48 = VolumeUp
73 = 0x49 = VolumeDown
74 = 0x4A = Mute
75 = 0x4B = ANSI_KeypadDivide
76 = 0x4C = ANSI_KeypadEnter
78 = 0x4E = ANSI_KeypadMinus
79 = 0x4F = F18
80 = 0x50 = F19
81 = 0x51 = ANSI_KeypadEquals
82 = 0x52 = ANSI_Keypad0
83 = 0x53 = ANSI_Keypad1
84 = 0x54 = ANSI_Keypad2
85 = 0x55 = ANSI_Keypad3
86 = 0x56 = ANSI_Keypad4
87 = 0x57 = ANSI_Keypad5
88 = 0x58 = ANSI_Keypad6
89 = 0x59 = ANSI_Keypad7
90 = 0x5A = F20
91 = 0x5B = ANSI_Keypad8
92 = 0x5C = ANSI_Keypad9
93 = 0x5D = JIS_Yen
94 = 0x5E = JIS_Underscore
95 = 0x5F = JIS_KeypadComma
96 = 0x60 = F5
97 = 0x61 = F6
98 = 0x62 = F7
99 = 0x63 = F3
100 = 0x64 = F8
101 = 0x65 = F9
102 = 0x66 = JIS_Eisu
103 = 0x67 = F11
104 = 0x68 = JIS_Kana
105 = 0x69 = F13
106 = 0x6A = F16
107 = 0x6B = F14
109 = 0x6D = F10
111 = 0x6F = F12
113 = 0x71 = F15
114 = 0x72 = Help
115 = 0x73 = Home
116 = 0x74 = PageUp
117 = 0x75 = ForwardDelete
118 = 0x76 = F4
119 = 0x77 = End
120 = 0x78 = F2
121 = 0x79 = PageDown
122 = 0x7A = F1
123 = 0x7B = LeftArrow
124 = 0x7C = RightArrow
125 = 0x7D = DownArrow
126 = 0x7E = UpArrow
    

## Wishlist

* More supported devices (PR if you have new ones!)
* Storing device profiles and presets in `~/Library/Application Support` for easy manipulation
* UI for creating / editing device profiles
* UI for creating / editing presets
* Auto update device profiles from an online (GitHub?) repository
* Automatic activation of presets when games are launched

## Authors

* Robbert Klarenbeek, <robbertkl@renbeek.nl>

## Credits

* Thanks to [Alex Zielenski](https://twitter.com/#!/alexzielenski) for [StartAtLoginController](https://github.com/alexzielenski/StartAtLoginController), which ties together the ServiceManagement stuff without even a single line of code (gotta love KVO).

* The [vector drawing used for the app icon](Resources/Graphics/AppIcon.ai) was made by [sebi01](http://www.vecteezy.com/members/sebi01) from [Vecteezy.com](http://www.vecteezy.com).

* The [vector drawing used for the status menu icon](Resources/Graphics/StatusMenuTemplate.eps) was made by [Freepik](http://www.freepik.com) from [Flaticon](http://www.flaticon.com).

## License

Gamepad Menu is published under the [MIT License](http://www.opensource.org/licenses/mit-license.php).
