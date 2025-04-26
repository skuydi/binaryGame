# Arduino Binary Game

A fast and educational **binary conversion game** for Arduino, using switches as binary inputs, a TM1637 7-segment display for the number to guess, a MAX7219 8-digit display for the binary representation, a NeoPixel ring for score animation, and a buzzer for feedback.  
Convert random decimal numbers to binary as quickly as possible, within a time limit and with multiple difficulty levels!

---

## 🎲 Features

- **Random decimal number** to convert to binary
- **8 input switches** to enter binary numbers (0–255)
- **Difficulty selection** (8 modes: Easy, Normal, Medium, Hard, Extreme, Insane, Nightmare, Impossible)
- **7-segment display** (TM1637) to show the target number and timer
- **MAX7219 8-digit display** to show your binary input in real time
- **NeoPixel ring** for score progress and colorful feedback
- **Buzzer** and **red/green LEDs** for correct/wrong answers and end-of-game feedback
- **Countdown timer** to increase challenge and dynamism
- **Rainbow victory animation** when max score is reached
- **Victory/defeat melodies** via the buzzer
- **Serial output** for debugging

---

## 🛠️ Hardware

- 1x Arduino Mega
- 8x switches or buttons (for binary input, connected to A0–A7)
- 1x TM1637 7-segment display (number display, pins: CLK1, DIO1)
- 1x MAX7219 8-digit 7-segment display (shows binary entry, DataIn, CLK, LOAD/CS)
- 1x TM1637 7-segment display (timer, pins: CLK4, DIO4)
- 1x NeoPixel ring (8 LEDs) or WS2812 LED strip (set `NUM_LEDS` accordingly, pin 9)
- 1x Buzzer (pin 13)
- 1x Pushbutton (answer validation, pin 2)
- 2x LEDs (green: pin 22, red: pin 23) — optional for feedback
- 3x Mode select switches (pins 3, 4, 5)
- Jumper wires, breadboard, or PCB

---

## 🔌 Wiring

| Function         | Arduino Pin | Description                           |
|------------------|-------------|---------------------------------------|
| Binary Inputs    | A0–A7       | 8 switches/buttons for bits 0–7       |
| Mode Select      | 3, 4, 5     | Difficulty selection switches         |
| TM1637 Display 1 | 7, 8        | Target number (CLK1, DIO1)            |
| MAX7219 Display  | 12, 11, 10  | Binary entry (DIN, CLK, LOAD)         |
| TM1637 Display 2 | 15, 16      | Timer display (CLK4, DIO4)            |
| NeoPixel Ring    | 9           | Score and animation                   |
| Green LED        | 22          | Correct answer indicator (optional)   |
| Red LED          | 23          | Wrong answer indicator (optional)     |
| Push Button      | 2           | Validate answer                       |
| Buzzer           | 13          | Audio feedback                        |

*(Update the table if you change the wiring!)*

---

## 🚦 How To Play

1. **Set the difficulty** using the 3 mode switches before power-up.
2. The TM1637 display shows a **random decimal number**.
3. Enter its **binary value** using the 8 switches (A0–A7).
4. Your binary entry is shown live on the MAX7219 8-digit display.
5. Press the **Validate** button to check your answer:
   - Green LED/buzzer: correct answer, score increases!
   - Red LED/buzzer: wrong answer or timeout, score decreases.
   - Score is visualized on the NeoPixel ring.
6. **Beat the timer!** The timer is shown on the second TM1637 display.
7. **Game over** if you reach 0 or max score (rainbow animation & melody).

---

## 🧰 Dependencies

- [Adafruit_NeoPixel](https://github.com/adafruit/Adafruit_NeoPixel)
- [TM1637Display](https://github.com/avishorp/TM1637)
- [LedControl](https://github.com/wayoda/LedControl)

Install these libraries using the Arduino Library Manager or download from GitHub.

---

## 📄 Example

![Example wiring and display setup](images/binarygame_example.png) <!-- Add your own images/screenshots! -->

---

## 📝 Credits

- Original inspiration: [keebie81 - Instructables Binary Game](https://www.instructables.com/Binary-Game/)
- Modified, improved and expanded by [skuydi](https://github.com/skuydi)

---

## 📸 Pictures

* https://github.com/skuydi/binaryGame/blob/main/IMG_20240826_194823.jpg

---


## 🖥️ License

MIT License.  
See [LICENSE](LICENSE) file for details
