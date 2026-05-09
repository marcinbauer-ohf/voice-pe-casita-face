# Voice PE: Casita LED mod

A mod for the Voice PE from the Open Home Foundation, fitted with a 16×8 LED matrix. Faces react to voice assistant state in real time. Also supports scrolling text and custom sprites.

![Voice PE LED Matrix Face showing the default blue face](images/IMG_7704.jpeg)

---

> [!WARNING]
> **This mod will void your Voice PE warranty.** Disassembling the device and soldering to its internals can permanently brick it. Proceed at your own risk. Start by making a backup of your current ESPHome configuration.
> If you run into issues and want to revert, flash the [official factory firmware](https://esphome.github.io/home-assistant-voice-pe/).

> [!NOTE]
> This is **not** an official Open Home Foundation (OHF) or Nabu Casa product. It was made by me as a personal project. No support is provided beyond this documentation.

---

## Capabilities

| Display mode | Description |
|---|---|
| Faces | Animated: default, happy, grin, sad, angry, surprised, neutral, listening, thinking, talking, sleeping, and more |
| Scrolling text | Send any message from Home Assistant to scroll across the matrix |
| Custom sprites | Upload pixel-art animations via the included sprite editor |
| Solid colour | Fill all LEDs with a colour — controlled via the LED Matrix light entity in HA |
| Equalizer | Animated music visualizer bars |
| Progress bar | Generic 0–100% horizontal bar, green→yellow→red |
| Boot animation | Home Assistant logo fade-in on startup |
| Disconnected | Automatically shown when HA API is offline for more than 15 seconds after boot |

The display integrates directly with the voice assistant — faces switch automatically when you speak, ask a question, get a response, or the device goes idle. You can also control what is displayed via a custom dashboard card.

---

## What you need

### Hardware

| Item | Notes |
|---|---|
| Home Assistant Voice PE | The device this mod targets |
| 2× [8×8 WS2812B LED matrix](https://allegro.pl/oferta/led-rgb-2812-matryca-8x8-64-diody-adresowalne-18292956151) | Chainable; these form the 16×8 display |
| 330 Ω resistor | On the data line between Voice PE and the matrix |
| 5 V power source, min 2 A | It's not recommended to use the VPE 5V pin. It may cause brownouts or your VPE to fail. |
| 2-pin JST or similar connector *(optional)* | Only needed with an external PSU |
| 20 AWG stranded wire | Power feed to the matrix (VCC + GND) — heavier gauge handles the current |
| Dupont / Arduino wire connectors | For signal wiring |
| Heat shrink tubing | To insulate exposed solder joints and wires |
| Small screwdriver set | To open the Voice PE enclosure |
| Soldering iron + solder | For making connections to the Voice PE board |
| 3D printer or a way to print the parts | To print the modified case that fits the matrix |
| 5× M4 20 mm screws *or* super glue | To assemble the printed enclosure |
| Double-sided sticky tape | To secure the LED matrix panels inside the frame |

### Software

| Item | Notes |
|---|---|
| [Home Assistant](https://www.home-assistant.io/) | Required — the device connects to HA via ESPHome API |
| [ESPHome](https://esphome.io/) | For compiling and flashing the firmware |
| An AI coding assistant *(optional)* | Useful for editing `led_faces.h` to create new faces |

---

## 3D printed parts

All files are in the `3d-parts/` folder. Print all parts from the same version — mixing versions may cause fit issues.

**Material:** Matte white PLA recommended (PETG also works); black for `LED-MASK`  
**Nozzle:** 0.4 mm (model optimised for this size)  
**Recommended printer:** Bambu A1

| Preview | File | Description | Supports | Notes |
| --- | --- | --- | --- | --- |
| ![CABLE-PLUG](images/parts/cable-plug.png) | `CABLE-PLUG.3mf` | Plug for the cable opening | None | |
| ![CASE-BACK](images/parts/case-back.png) | `CASE-BACK.3mf` | Rear enclosure panel | Minimal (flat orientation) | Can also be printed same as CASE-FRONT with full supports |
| ![CASE-FRONT](images/parts/case-front.png) | `CASE-FRONT.3mf` | Front enclosure panel | Full model | Needed for clean rounded shape |
| ![CASE-MIDDLE](images/parts/case-middle.png) | `CASE-MIDDLE.3mf` | Middle section joining front and back | Outer edge only | |
| ![LED-FRAME](images/parts/led-frame.png) | `LED-FRAME.3mf` | Frame that holds the LED matrix panels in place | None | |
| ![LED-MASK](images/parts/led-mask.png) | `LED-MASK.3mf` | Diffuser mask over the matrix | None | **Print in black** |
| ![MUTE-SWITCH](images/parts/mute-switch.png) | `MUTE-SWITCH.3mf` | Replacement mute button cap | None | |

### Version compatibility

| Part | V1 |
|---|---|
| CABLE-PLUG | ✅ |
| CASE-BACK | ✅ |
| CASE-FRONT | ✅ |
| CASE-MIDDLE | ✅ |
| LED-FRAME | ✅ |
| LED-MASK | ✅ |
| MUTE-SWITCH | ✅ |

> All V1 parts are compatible with each other. When future versions are released, this table will show which parts can be mixed.

---

## Build guide

### 1. Print the parts

Print all parts listed in the [3D printed parts](#3d-printed-parts) section above. Follow the material, colour, and support guidance in that section. Allow prints to finish before moving on — the case assembly is needed before you can solder in position.

### 2. Solder and assemble the hardware

#### Back up your current Voice PE config

Before opening the device, save your existing ESPHome configuration. If something goes wrong you can restore the original firmware wirelessly.

#### Open and disassemble the Voice PE

Parts needed: main board, speaker, all screws

#### LED face assembly and wiring

- 2× LED panels
- 3× black 24 AWG wires (10 cm)
- 2× red 24 AWG wires (10 cm)
- 3× white 24 AWG wires (10 cm)
- 2× Dupont connectors
- 3× heat shrink tubing
- 2-pin JST connectors, or any other connector to supply min. 1A via an additional cable

##### 2.1 Stick LED panels to LED-FRAME with double-sided tape

Printed part rotation doesn't matter. Just make sure the LED panel alignment is the same on both. The 3-pin connector is optional and can be used to daisy-chain additional internal LEDs.

Front:

![](images/IMG_7613.jpeg)

Backside:

![](images/IMG_7615.jpeg)

##### 2.2 Solder data line between the two panels

![](images/IMG_7616.jpeg)

##### 2.3 Solder and secure GND and VCC cable

The 2-pin JST male connector is just my way to connect external power. You may cut and solder a USB cables VCC and GND that is capable of delivering 2A max, that is connected to a USB charger (2A).

![](images/IMG_7674.jpg)

##### 2.4 Solder and secure data cable with 330 Ω resistor

![](images/IMG_7619.jpeg)

##### 2.5 Solder the data cable to the IN of the first LED panel

![](images/IMG_7621.jpeg)

##### 2.6 Solder the VCC/GND cable to the panels GND and VCC IN pads

![](images/IMG_7697.jpeg)

##### 2.7 Solder 2-pin JST female connector to wire for external PSU *(optional)*
As I mentioned, you can use whatever power cable you like, as long as it can provide 1A at minimum. I connect mine to a Meanwell PSU.

![](images/IMG_7648.jpeg)


#### Case assembly

##### 2.8 Secure the soldered LED contanct points with hot glue where needed

##### 2.9 Assemble CASE-BACK, MUTE-SWITCH and mainboard

Put the mute switch in the case then align the mainboard's toggle tip through the mute-switch hole, use original mainboard screws to secure it to the frame. Insert 5× M4 20 mm screws, or super-glue the front, middle and back parts at the end if you don't have the required screws.

![](images/IMG_7653.jpeg)

##### 2.10 Mount the speaker on CASE-MIDDLE

Align the speaker with the printed holes and secure with the original screws.

![](images/IMG_7651.jpeg)

##### 2.11 Join CASE-BACK and CASE-MIDDLE

Align the middle part with the back and screw in. Make sure the outer edges of the case align. Be careful not to screw the part too tightly. Connect the speaker to main board.

![](images/IMG_7654.jpeg)

##### 2.12 Join and screw together CASE-FRONT

When using screws, don't overtighten.
![](images/IMG_7699.jpeg)


##### 2.13 Connect DATA and GND cables
![](images/IMG_7698.jpeg)

##### 2.14 Guide through Voice PE USB-C power cable, LED panels external cable, and any audio cables. Close with CABLE-PLUG

![](images/IMG_7700.jpeg)

### 3. Flash the firmware

There are two options — use the pre-built binary for the fastest setup, or compile your own for full control.

---

#### Option A — Flash the pre-built binary (easiest)

1. Download `casita-led-face-v1.0.0.bin` from the [latest release](https://github.com/marcinbauer-ohf/voice-pe-casita-face/releases/latest)
2. Connect the Voice PE via USB
3. Open **[https://web.esphome.io](https://web.esphome.io)** in **Chrome or Edge** — Firefox not supported (no Web Serial API)
4. Click **Connect** → select the device serial port
5. Click **Install** → select the downloaded `.bin` file
6. After flashing, the device creates a Wi-Fi hotspot — connect to it and enter your Wi-Fi credentials
7. Home Assistant will discover the device automatically via mDNS

> Pre-built binary uses HA-managed API key negotiation. No secrets file needed.

---

#### Option B — Compile your own

Clone this repository:

```bash
git clone https://github.com/marcinbauer-ohf/voice-pe-casita-face.git
cd voice-pe-casita-face
```

Copy `home-assistant-voice.yaml`, `led_faces.h`, and the `components/` folder into your ESPHome configuration directory (usually `/config/esphome/` on Home Assistant OS).

> The `components/const/__init__.py` local override is required to fix a compatibility issue between ESPHome 2026.x and the Voice PE external components. Without it the firmware will not compile.

In your ESPHome `secrets.yaml`:

```yaml
wifi_ssid: "YourNetwork"
wifi_password: "YourPassword"
api_encryption_key: "<generate in ESPHome dashboard — 32 bytes base64>"
```

To generate an API key: in the ESPHome dashboard click **Secrets** → **Generate** next to a new key, or run `openssl rand -base64 32` in a terminal.

Open `home-assistant-voice.yaml` and change the `name` and `friendly_name` substitutions, then flash via ESPHome dashboard:

1. Open **Settings → Add-ons → ESPHome** in Home Assistant
2. Find your device and click **Install**
3. Choose **Wirelessly** if already online, or **Plug into this computer** for first-time flash

The device will reboot and show the boot animation on the matrix.

---

## Using the sprite editor

The sprite editor is a self-contained HTML file — no server needed.

1. Open `sprite-editor.html` in any modern browser
2. Draw your sprite on the 16×8 pixel grid
3. Add multiple frames for animation; set frame delay in milliseconds
4. Enable **Crossfade** *(optionally)* for smooth transitions between frames
5. Click **Copy to clipboard** (or use the export textarea)
6. In Home Assistant, call the ESPHome service:

```yaml
service: esphome.<your_device>_display_sprite
data:
  frames: "<paste output here>"
  frame_count: 3       # number of frames you drew
  frame_ms: 200        # delay per frame in ms
  fade: true           # crossfade between frames
```

### Sprite editor features

| Feature | Description |
|---|---|
| Paint / Eraser / Eyedropper / Fill | Drawing tools |
| Quick palette | Pre-set colors matching the built-in face palette |
| Onion skin | Shows previous frame ghosted beneath current for animation reference |
| Glow effect | Adds a 50% brightness halo around lit pixels |
| Frame management | Add, duplicate, delete, reorder frames |
| Undo / Redo | Full history |
| Sprite library | Save and reload named sprites locally in the browser |
| Preview | Animated real-time preview at matrix scale |

---

## Dashboard card

A custom Lovelace card (`led-matrix-card.js`) provides a live pixel preview of the matrix, a face gallery, and buttons to control every display mode.

### 1. Install the card

Copy `led-matrix-card.js` from this repo to your Home Assistant `/config/www/` folder.

Then register it as a Lovelace resource:

1. Go to **Settings → Dashboards → ⋮ (menu) → Resources**
2. Click **+ Add resource**
3. URL: `/local/led-matrix-card.js`
4. Type: **JavaScript module**
5. Save and reload the browser

### 2. Create a dashboard

Create a new dashboard (or use an existing one) and add a card in YAML mode. Replace `home_assistant_voice` with your device's name (the `name` substitution from `home-assistant-voice.yaml`, with hyphens replaced by underscores):

```yaml
type: vertical-stack
cards:
  - type: custom:led-matrix-card
    status_entity: sensor.home_assistant_voice_matrix_display_status
    satellite_entity: assist_satellite.voice_assist_satellite
    esphome_device: home_assistant_voice
    pixel_size: 30
    show_label: true
  - type: entity
    entity: sensor.home_assistant_voice_matrix_display_status
    name: Current Display
  - type: markdown
    content: "## Faces"
  - type: grid
    columns: 4
    cards:
      - type: button
        name: Default
        icon: mdi:emoticon-neutral
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: default
      - type: button
        name: Neutral
        icon: mdi:emoticon-neutral-outline
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: neutral
      - type: button
        name: Happy
        icon: mdi:emoticon-happy
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: happy
      - type: button
        name: Grin
        icon: mdi:emoticon-cool
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: grin
      - type: button
        name: Sad
        icon: mdi:emoticon-sad
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: sad
      - type: button
        name: Angry
        icon: mdi:emoticon-angry
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: angry
      - type: button
        name: Thinking
        icon: mdi:emoticon-confused
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: thinking
      - type: button
        name: Surprised
        icon: mdi:emoticon-excited
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: surprised
      - type: button
        name: Listening
        icon: mdi:ear-hearing
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: listening
      - type: button
        name: Talking
        icon: mdi:message-text
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: talking
      - type: button
        name: Sleeping
        icon: mdi:sleep
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: sleeping
      - type: button
        name: Music
        icon: mdi:music-note
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: music
      - type: button
        name: Giggle
        icon: mdi:emoticon-lol
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: giggle
      - type: button
        name: Heart
        icon: mdi:heart
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: heart
      - type: button
        name: Wink
        icon: mdi:emoticon-wink
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: wink
      - type: button
        name: Joy
        icon: mdi:emoticon-excited
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: joy
      - type: button
        name: Laugh
        icon: mdi:emoticon-happy
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: laugh
      - type: button
        name: Error
        icon: mdi:emoticon-dead
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: error
      - type: button
        name: Cry
        icon: mdi:emoticon-cry
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: cry
      - type: button
        name: Disconnected
        icon: mdi:wifi-off
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: disconnected
  - type: markdown
    content: "## Feedback Icons"
  - type: grid
    columns: 4
    cards:
      - type: button
        name: Light On
        icon: mdi:lightbulb-on
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: light_on
      - type: button
        name: Light Off
        icon: mdi:lightbulb-off
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: light_off
      - type: button
        name: Switch On
        icon: mdi:toggle-switch
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: switch_on
      - type: button
        name: Switch Off
        icon: mdi:toggle-switch-off
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: switch_off
      - type: button
        name: Locked
        icon: mdi:lock
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: lock_closed
      - type: button
        name: Unlocked
        icon: mdi:lock-open
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: lock_open
      - type: button
        name: Cover Up
        icon: mdi:arrow-up-box
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: cover_up
      - type: button
        name: Cover Down
        icon: mdi:arrow-down-box
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: cover_down
      - type: button
        name: Scene
        icon: mdi:star-shooting
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: scene
      - type: button
        name: Recording
        icon: mdi:record-circle
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: recording
  - type: markdown
    content: "## Animations"
  - type: grid
    columns: 3
    cards:
      - type: button
        name: Logo
        icon: mdi:diamond
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: logo
      - type: button
        name: DVD
        icon: mdi:television-play
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_face
          data:
            name: dvd
      - type: button
        name: Equalizer
        icon: mdi:equalizer
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_equalizer
      - type: button
        name: Rick
        icon: mdi:dance-ballroom
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_sprite
          data:
            frames: "00000000000000000000000000000000000050280A50280A50280A50280A000000000000000000000000000000000000000000000000000000000000000000FFC88C50280AFFC88CFFC88C50280AFFC88C000000000000000000000000000000000000000000000000000000000000FFC88CFFC88CFFC88CFFC88CFFC88CFFC88C000000000000000000000000000000000000000000000000000000000000000000DC5050DCDCDCDCDCDCDC5050000000000000000000000000000000000000000000000000000000FFC88CFFC88C1E1E50DCDCDCC81E1EC81E1EDCDCDC1E1E50FFC88CFFC88CFFC88CFFC88C0000000000000000000000000000001E1E501E1E501E1E501E1E501E1E501E1E501E1E501E1E500000000000000000000000000000000000000000000000000000000000001E1E501E1E501E1E501E1E500000000000000000000000000000000000000000000000000000000000000000000000001E1E500000000000001E1E50000000000000000000000000000000000000|00000000000000000000000000000050280A50280A50280A50280A50280A000000000000000000000000000000000000000000000000000000000000000000FFC88C50280AFFC88CFFC88CFFC88CFFC88C000000000000000000000000000000000000000000000000000000000000FFC88CFFC88CFFC88CFFC88CFFC88CFFC88C000000000000000000000000000000000000000000000000000000000000DC5050DC5050DCDCDCDCDCDCDC5050DC5050000000000000000000000000000000000000FFC88CFFC88CFFC88CFFC88C1E1E50DCDCDCC81E1EC81E1EDCDCDC1E1E50FFC88CFFC88C0000000000000000000000000000000000000000001E1E501E1E501E1E501E1E501E1E501E1E501E1E501E1E500000000000000000000000000000000000000000000000000000000000001E1E501E1E501E1E501E1E500000000000000000000000000000000000000000000000000000000000000000001E1E500000000000001E1E50000000000000000000000000000000000000000000"
            frame_count: 2
            frame_ms: 400
            fade: false
      - type: button
        name: Solid
        icon: mdi:palette
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_solid
          data:
            r: 255
            g: 255
            b: 255
  - type: markdown
    content: "## Controls"
  - type: entities
    entities:
      - entity: text.home_assistant_voice_matrix_message
        name: Message
  - type: grid
    columns: 3
    cards:
      - type: button
        name: Wake Display
        icon: mdi:weather-sunny
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_wake
      - type: button
        name: Send Text
        icon: mdi:send
        tap_action:
          action: call-service
          service: button.press
          target:
            entity_id: button.home_assistant_voice_send_matrix_message
      - type: button
        name: "Off"
        icon: mdi:power
        tap_action:
          action: call-service
          service: esphome.home_assistant_voice_display_off
  - type: entities
    entities:
      - entity: number.home_assistant_voice_matrix_brightness
        name: Brightness
      - entity: number.home_assistant_voice_matrix_dim_brightness
        name: Dim Brightness
      - entity: number.home_assistant_voice_scroll_speed
        name: Scroll Speed
```

> The card auto-follows the voice assistant state — faces switch in the preview as the device reacts to speech. The **Gallery** button shows all available faces.

---

## Home Assistant services

Once flashed, the device exposes these services under **Developer Tools → Services**:

| Service | Parameters | Description |
|---|---|---|
| `display_face` | `name: string` | Show a named face (see list below) |
| `display_face_briefly` | `name`, `duration` (seconds) | Show face temporarily, then return |
| `display_text` | `message: string` | Scroll text across the matrix |
| `display_sprite` | `frames`, `frame_count`, `frame_ms`, `fade` | Show custom sprite animation |
| `display_wake` | — | Return to default face |
| `display_equalizer` | — | Show equalizer animation |
| `display_progress` | `value: float` (0.0–1.0) | Show horizontal progress bar, green→yellow→red |
| `display_solid` | `r`, `g`, `b` (int 0–255) | Fill all LEDs with a solid colour — or set colour via the LED Matrix light entity in HA |
| `display_off` | — | Turn off the matrix |

### Available faces

`default` · `neutral` · `happy` · `grin` · `sad` · `angry` · `surprised` · `thinking` · `talking` · `listening` · `sleeping` · `music` · `giggle` · `heart` · `wink` · `joy` · `laugh` · `error` · `cry` · `disconnected`

> `rick` is available as a sprite — see the Animations section of the dashboard card.

Face colors follow a consistent scheme:
- **Blue** — idle / calm
- **Green** — positive / talking / happy
- **Orange** — attention / listening / thinking
- **Red** — error / sad / angry

### Device entities

After flashing, the device exposes these entities in Home Assistant:

| Entity | Type | Description |
|---|---|---|
| `Matrix Brightness` | Number (slider) | Overall display brightness 5–100% |
| `Matrix Dim Brightness` | Number (slider) | Stored dim level used by dim blueprints 1–100% |
| `Scroll Speed` | Number (slider) | Text scroll speed in ms per step |
| `Solid Red` / `Solid Green` / `Solid Blue` | Number (slider) | RGB values for solid colour mode 0–255 |
| `Matrix Message` | Text | Message to send to the scrolling display |
| `Send Matrix Message` | Button | Sends current `Matrix Message` value to the display |
| `Matrix Display Status` | Sensor | Current display mode/face as a string |

---

## Automation blueprints

Ready-made blueprints are in the `blueprints/` folder. Copy them to `/config/blueprints/automation/casita/` on your Home Assistant instance, then import via **Settings → Automations → Blueprints**.

| Blueprint | Description |
|---|---|
| `notify-face.yaml` | Show a named face for N seconds when any entity changes state |
| `dim-on-time.yaml` | Dim the display at a fixed time and restore at another |
| `dim-on-sun.yaml` | Dim the display at sunset and restore at sunrise |

Dim blueprints use entity pickers for **Matrix Brightness** and **Matrix Dim Brightness** — set your dim level in the `Matrix Dim Brightness` slider on the device, then pick it in the blueprint. Normal/restore brightness is a number input in the blueprint itself. Copy blueprints to `/config/blueprints/automation/voice-pe-casita-face/` and reload via **Settings → Automations → Blueprints**.

---

## Face gallery

### Expressions

| Col 1 | Col 2 | Col 3 | Col 4 |
|---|---|---|---|
| ![default](images/faces/default.gif) default | ![neutral](images/faces/neutral.gif) neutral | ![happy](images/faces/happy.gif) happy | ![grin](images/faces/grin.gif) grin |
| ![sad](images/faces/sad.gif) sad | ![angry](images/faces/angry.gif) angry | ![thinking](images/faces/thinking.gif) thinking | ![surprised](images/faces/surprised.gif) surprised |
| ![listening](images/faces/listening.gif) listening | ![talking](images/faces/talking.gif) talking | ![sleeping](images/faces/sleeping.gif) sleeping | ![music](images/faces/music.gif) music |
| ![giggle](images/faces/giggle.gif) giggle | ![heart](images/faces/heart.gif) heart | ![wink](images/faces/wink.gif) wink | ![joy](images/faces/joy.gif) joy |
| ![laugh](images/faces/laugh.gif) laugh | ![error](images/faces/error.gif) error | ![cry](images/faces/cry.gif) cry | ![disconnected](images/faces/disconnected.gif) disconnected |

### Feedback icons

| Col 1 | Col 2 | Col 3 | Col 4 | Col 5 |
|---|---|---|---|---|
| ![light_on](images/faces/light_on.gif) light_on | ![light_off](images/faces/light_off.gif) light_off | ![switch_on](images/faces/switch_on.gif) switch_on | ![switch_off](images/faces/switch_off.gif) switch_off | ![scene](images/faces/scene.gif) scene |
| ![lock_closed](images/faces/lock_closed.gif) lock_closed | ![lock_open](images/faces/lock_open.gif) lock_open | ![cover_up](images/faces/cover_up.gif) cover_up | ![cover_down](images/faces/cover_down.gif) cover_down | ![recording](images/faces/recording.gif) recording |

### Animations

| Col 1 | Col 2 |
| --- | --- |
| ![logo](images/faces/logo.gif) logo | ![dvd](images/faces/dvd.gif) dvd |

---

## Adding custom faces

Faces are defined in `led_faces.h` as sparse pixel arrays in `{x, y, R, G, B}` format. Each face can have multiple frames for animation. An AI coding assistant works well for generating face data from a description.

---

## Troubleshooting

| Symptom | Likely cause |
|---|---|
| Matrix stays dark | Check data line wiring and the 330 Ω resistor |
| Garbled / wrong pixels | Panels connected in wrong order or wrong data-out tap |
| Device offline after flash | Wi-Fi credentials wrong in `secrets.yaml` |
| Faces show but dim | Check `Matrix Brightness` slider in HA entity |
| Device reboots randomly | Power supply underpowered for matrix current draw |
| `Cannot import KEY_METADATA` compile error | `components/` folder missing — copy it to your ESPHome config directory alongside the YAML |
| Default face not showing after boot | Re-flash — this was a known bug fixed in the current firmware |
| Wake word not working after flash | Check that the `components/const/__init__.py` override is present — a wrong version breaks the voice kit |

---

## Contributing

Pull requests welcome — new faces, display modes, case designs, wiring guides with photos.

Please open an issue before starting large changes.

---

## License

MIT — see [LICENSE](LICENSE).

---

## Acknowledgements

Built on top of the official [Home Assistant Voice PE ESPHome firmware](https://github.com/esphome/home-assistant-voice-pe) by Nabu Casa.
