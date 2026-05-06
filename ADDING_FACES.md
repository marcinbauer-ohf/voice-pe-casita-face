# Adding or removing faces — checklist

When adding, renaming, or removing a face, update **all 5 places**:

- [ ] **`led_faces.h`** — pixel arrays (`F_NAME_0[]`, `F_NAME_1[]`), registry entry `{"name", nf, {F_NAME_0, F_NAME_1}}`, and `LED_FACES_COUNT`
- [ ] **`blueprints/notify-face.yaml`** — `options:` list under the `face_name` input
- [ ] **Dashboard card** — button entry in the relevant grid section (Faces / Feedback Icons / Animations)
- [ ] **`README.md`** — available faces list in the Home Assistant services section
- [ ] **`www/led-matrix-card.js`** — pixel array constants (`F_NAME_0`, `F_NAME_1`) and `FACES` registry entry

> Only update `home-assistant-voice-0900f7.yaml` if the face is used for a voice assistant phase (surprised/listening/thinking/talking/neutral/sad). All other faces don't need a FW change beyond `led_faces.h`.
