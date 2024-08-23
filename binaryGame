  /*
   * 24/08/23, B. Fasel
   * Like all Arduino code - copied from somewhere else :)
   * So don't claim it as your own
   */

#include <Arduino.h>
#include <TM1637Display.h>
#include <Adafruit_NeoPixel.h>
#include <LedControl.h>

// Display 1 - Define unique CLK and DIO pins for each display
#define CLK1 7
#define DIO1 8
TM1637Display display_1(CLK1, DIO1); // Create an instance of the TM1637Display 1

// Display 2 - Initialisation de l'objet LedControl - Button state
// Paramètres : DataIn, CLK, LOAD/CS, nombre de drivers MAX7219 connectés
LedControl display_2 = LedControl(12, 11, 10, 1); // Create an instance of the TM1637Display 2

// Display 3 - Define unique CLK and DIO pins for each display
#define CLK4 15
#define DIO4 16
TM1637Display display_3(CLK4, DIO4); // Create an instance of the TM1637Display 3

// Define the data pin for the LEDs
#define PIN 9

// Define the number of LEDs in the ring
#define NUM_LEDS 8

// Define the green and red LEDs PIN
#define greenLedPin 22
#define redLedPin 23

#define DURATION 5000 // Durée de l'animation en millisecondes

// Create an instance of the NeoPixel library
Adafruit_NeoPixel strip = Adafruit_NeoPixel(NUM_LEDS, PIN, NEO_GRB + NEO_KHZ800);

// Rainbow colors array
uint32_t rainbowColors[] = {
  strip.Color(255, 0, 0),    // Red
  strip.Color(255, 127, 0),  // Orange
  strip.Color(255, 255, 0),  // Yellow
  strip.Color(0, 255, 0),    // Green
  strip.Color(0, 0, 255),    // Blue
  strip.Color(75, 0, 130),   // Indigo
  strip.Color(148, 0, 211)   // Violet
};

int lastColorIndex = -1;

const int Pin0 = A0;  // Binary number 2^0 or 1
const int Pin1 = A1;  // Binary number 2^1 or 2
const int Pin2 = A2;  // Binary number 2^2 or 4
const int Pin3 = A3;  // Binary number 2^3 or 8
const int Pin4 = A4;  // Binary number 2^4 or 16
const int Pin5 = A5;  // Binary number 2^5 or 32
const int Pin6 = A6;  // Binary number 2^6 or 64
const int Pin7 = A7;  // Binary number 2^7 or 128

const int ModePin_1 = 5; // Mode selector 1
const int ModePin_2 = 4; // Mode selector 2
const int ModePin_3 = 3; // Mode selector 3

int binaryValue;        // Value for adding up numbers to compare to random number
long binaryToDisplay;

int correctNumber = 0;  // Flag to see if the number was correct
int wrongNumber = 0;    // Flag to see if the number was wrong

const int buzzer = 13;  // Buzzer pin
int freq; // Frequency out

const int buttonPin = 2;  // The number of the pushbutton pin

int buttonState;          // Variable for reading the pushbutton status
long randNumber;        // Variable for the random number

// Range pour les nombres aléatoires selon les différents modes
// Setup
int randEasy_1        = 15;   // Easy
int randNormal_1      = 25;   // Normal
int randMedium_1      = 35;   // Medium
int randHard_1        = 55;   // Hard
int randExtreme_1     = 85;   // Hard
int randInsane_1      = 105;  // Hard
int randNightmare_1   = 128;  // Medium
int randImpossible_1  = 255;  // Medium

// En cas de time's
int randEasy_2        = 15;   // Easy
int randNormal_2      = 25;   // Easy
int randMedium_2      = 35;   // Medium
int randHard_2        = 55;   // Hard
int randExtreme_2     = 85;   // Hard
int randInsane_2      = 105;  // Hard
int randNightmare_2   = 128;  // Medium
int randImpossible_2  = 255;  // Medium

// En cas de victoire
int randEasy_3        = 15;   // Easy
int randNormal_3      = 25;   // Easy
int randMedium_3      = 35;   // Medium
int randHard_3        = 55;   // Hard
int randExtreme_3     = 85;   // Hard
int randInsane_3      = 105;  // Hard
int randNightmare_3   = 128;  // Medium
int randImpossible_3  = 255;  // Medium

// En cas de défaite
int randEasy_4        = 15;   // Easy
int randNormal_4      = 25;   // Easy
int randMedium_4      = 35;   // Medium
int randHard_4        = 55;   // Hard
int randExtreme_4     = 85;   // Hard
int randInsane_4      = 105;  // Hard
int randNightmare_4   = 128;  // Medium
int randImpossible_4  = 255;  // Medium

// Limite de temps pour les différents modes
const unsigned long  timeLimitEasy = 10000;
const unsigned long  timeLimitNormal = 15000;
const unsigned long  timeLimitMedium = 20000;
const unsigned long  timeLimitHard = 35000;
const unsigned long  timeLimitExtreme = 45000;
const unsigned long  timeLimitInsane = 45000;
const unsigned long  timeLimitNightmare = 45000;
const unsigned long  timeLimitImpossible = 45000;

// Score variables
int score = 3;
int maxScore = NUM_LEDS;

// Timer variables
unsigned long startTime;
unsigned long timeLimit = 15000; // 15 seconds
bool timeisup;

void setup() {
  Serial.begin(9600);

  // Set the brightness to 5 (0=dimmest 7=brightest)
  display_1.setBrightness(5);  // Set the display brightness (0-7)
  display_1.showNumberDec(0, false); // Display the number
  display_1.clear();
  
  // Initialisation des drivers MAX7219
  display_2.shutdown(0, false);       // Allumer le driver 0
  display_2.setIntensity(0, 8);       // Régler la luminosité à un niveau moyen (0-15)
  display_2.clearDisplay(0);          // Effacer l'afficheur

  // Initialize the fourth display for the timer
  display_3.setBrightness(5);
  display_3.showNumberDec(0, false);
  display_3.clear();
  
  delay(1000);

  pinMode(Pin0, INPUT_PULLUP);
  pinMode(Pin1, INPUT_PULLUP);
  pinMode(Pin2, INPUT_PULLUP);
  pinMode(Pin3, INPUT_PULLUP);
  pinMode(Pin4, INPUT_PULLUP);
  pinMode(Pin5, INPUT_PULLUP);
  pinMode(Pin6, INPUT_PULLUP);
  pinMode(Pin7, INPUT_PULLUP);
  pinMode(ModePin_1, INPUT_PULLUP);
  pinMode(ModePin_2, INPUT_PULLUP);
  pinMode(ModePin_3, INPUT_PULLUP);
  pinMode(buttonPin, INPUT_PULLUP);
  pinMode(buzzer, OUTPUT); // Set buzzer pin as OUTPUT
  pinMode(greenLedPin, OUTPUT); // Set Green Led pin as OUTPUT
  pinMode(redLedPin, OUTPUT); // Set Red LED pin as OUTPUT

  // Initialize the NeoPixel strip
  strip.begin();
  strip.show(); // Initialize all pixels to 'off'
  strip.setBrightness(100);
  
  // Seed the random number generator with analog noise
  // randomSeed(analogRead(15));
  long seed = analogRead(A8) + analogRead(A9) + analogRead(A10) + analogRead(A11);
  randomSeed(seed);

  // Set the played mode
if (digitalRead(ModePin_1) == LOW && digitalRead(ModePin_2) == LOW && digitalRead(ModePin_3) == LOW) {
    randNumber = random(0, randEasy_1);
    timeLimit = timeLimitEasy;
    Serial.println("Easy Mode");
} else if (digitalRead(ModePin_1) == LOW && digitalRead(ModePin_2) == LOW && digitalRead(ModePin_3) == HIGH) {
    randNumber = random(0, randNormal_1);
    timeLimit = timeLimitNormal;
    Serial.println("Normal Mode");
} else if (digitalRead(ModePin_1) == LOW && digitalRead(ModePin_2) == HIGH && digitalRead(ModePin_3) == LOW) {
    randNumber = random(0, randMedium_1);
    timeLimit = timeLimitMedium;
    Serial.println("Medium Mode");
} else if (digitalRead(ModePin_1) == LOW && digitalRead(ModePin_2) == HIGH && digitalRead(ModePin_3) == HIGH) {
    randNumber = random(0, randHard_1);
    timeLimit = timeLimitHard;
    Serial.println("Hard Mode");
} else if (digitalRead(ModePin_1) == HIGH && digitalRead(ModePin_2) == LOW && digitalRead(ModePin_3) == LOW) {
    randNumber = random(0, randExtreme_1);
    timeLimit = timeLimitExtreme;
    Serial.println("Extreme Mode");
} else if (digitalRead(ModePin_1) == HIGH && digitalRead(ModePin_2) == LOW && digitalRead(ModePin_3) == HIGH) {
    randNumber = random(0, randInsane_1);
    timeLimit = timeLimitInsane;
    Serial.println("Insane Mode");
} else if (digitalRead(ModePin_1) == HIGH && digitalRead(ModePin_2) == HIGH && digitalRead(ModePin_3) == LOW) {
    randNumber = random(0, randNightmare_1);
    timeLimit = timeLimitNightmare;
    Serial.println("Nightmare Mode");
} else if (digitalRead(ModePin_1) == HIGH && digitalRead(ModePin_2) == HIGH && digitalRead(ModePin_3) == HIGH) {
    randNumber = random(0, randImpossible_1);
    timeLimit = timeLimitImpossible;
    Serial.println("Impossible Mode");
}

  // Display the number
  display_1.showNumberDec(randNumber, false); 
  Serial.print("BinaryValue : ");
  Serial.println(binaryValue);
  Serial.print("randNumber : ");
  Serial.println(randNumber);

  // Set up the interrupt
  attachInterrupt(digitalPinToInterrupt(buttonPin), checkNumber, FALLING);

  // Initialize the start time for the timer
  startTime = millis();

  Serial.print("Score : ");
  Serial.println(score);

  updateScoreDisplay();
}

void loop() {

  unsigned long currentTime = millis();
  int timeRemaining = (timeLimit - (currentTime - startTime)) / 1000;

  digitalWrite(redLedPin, LOW);   // sets the redLedPin pin off
  digitalWrite(greenLedPin, LOW); // sets the greenLedPin pin on
  
   // Si les délai est dépassé
  if (timeRemaining < 0) timeRemaining = 0;
    display_3.showNumberDec(timeRemaining, false);
    if (currentTime - startTime >= timeLimit) {
      timeisup = true;
      updateScore();
      updateScoreDisplay();

      digitalWrite(redLedPin, HIGH);  // sets the redLedPin pin off
      digitalWrite(greenLedPin, LOW); // sets the greenLedPin pin on
    
      Serial.println("Time's up!");
      Serial.print("Score : ");
      Serial.println(score);
      tone(buzzer, 200); // Play wrong answer tone
      delay(400);
      noTone(buzzer);     // Stop sound...

      // Affiche le nombre à trouver
      display_1.showNumberDec(randNumber, false);
      startTime = millis(); // Reset the timer
    }

    // Si le nombre entré est correct
    if (correctNumber == 1) {
      updateScore();
      updateScoreDisplay();
      Serial.println("Correct");
      Serial.print("Score : ");
      Serial.println(score);
    
      digitalWrite(redLedPin, LOW); // sets the redLedPin pin off
      digitalWrite(greenLedPin, HIGH); // sets the greenLedPin pin on
    
      tone(buzzer, 600); // Play correct answer tone
      delay(100);
      tone(buzzer, 1000);
      delay(100);
      tone(buzzer, 800);
      delay(100);
      noTone(buzzer);     // Stop sound...

      // Reset timer
      startTime = millis();

   // Set the played mode en cas de victoire
  if (digitalRead(ModePin_1) == LOW && digitalRead(ModePin_2) == LOW && digitalRead(ModePin_3) == LOW) {
    randNumber = random(0, randEasy_3);
    timeLimit = timeLimitEasy;
    Serial.println("Easy Mode");
} else if (digitalRead(ModePin_1) == LOW && digitalRead(ModePin_2) == LOW && digitalRead(ModePin_3) == HIGH) {
    randNumber = random(0, randNormal_3);
    timeLimit = timeLimitNormal;
    Serial.println("Normal Mode");
} else if (digitalRead(ModePin_1) == LOW && digitalRead(ModePin_2) == HIGH && digitalRead(ModePin_3) == LOW) {
    randNumber = random(0, randMedium_3);
    timeLimit = timeLimitMedium;
    Serial.println("Medium Mode");
} else if (digitalRead(ModePin_1) == LOW && digitalRead(ModePin_2) == HIGH && digitalRead(ModePin_3) == HIGH) {
    randNumber = random(0, randHard_3);
    timeLimit = timeLimitHard;
    Serial.println("Hard Mode");
} else if (digitalRead(ModePin_1) == HIGH && digitalRead(ModePin_2) == LOW && digitalRead(ModePin_3) == LOW) {
    randNumber = random(0, randExtreme_3);
    timeLimit = timeLimitExtreme;
    Serial.println("Extreme Mode");
} else if (digitalRead(ModePin_1) == HIGH && digitalRead(ModePin_2) == LOW && digitalRead(ModePin_3) == HIGH) {
    randNumber = random(0, randInsane_3);
    timeLimit = timeLimitInsane;
    Serial.println("Insane Mode");
} else if (digitalRead(ModePin_1) == HIGH && digitalRead(ModePin_2) == HIGH && digitalRead(ModePin_3) == LOW) {
    randNumber = random(0, randNightmare_3);
    timeLimit = timeLimitNightmare;
    Serial.println("Nightmare Mode");
} else if (digitalRead(ModePin_1) == HIGH && digitalRead(ModePin_2) == HIGH && digitalRead(ModePin_3) == HIGH) {
    randNumber = random(0, randImpossible_3);
    timeLimit = timeLimitImpossible;
    Serial.println("Impossible Mode");
}
    // Affiche le nombre à trouver
    display_1.showNumberDec(randNumber, false);
    correctNumber = 0;
    wrongNumber = 0;
  }

  // Si le nombre entré est incorrect
  if (wrongNumber == 1) {
    updateScore();
    updateScoreDisplay();
    
    digitalWrite(redLedPin, HIGH);  // sets the redLedPin pin off
    digitalWrite(greenLedPin, LOW); // sets the greenLedPin pin on

    Serial.println("Wrong");
    Serial.print("Score : ");
    Serial.println(score);
    Serial.print("randNumber : ");
    Serial.println(randNumber);
    Serial.print("BinaryValue : ");
    Serial.println(binaryValue);

    tone(buzzer, 200);  // Play wrong answer tone
    delay(400);
    noTone(buzzer);     // Stop sound...

    // Reset timer
    startTime = millis();

   // Set the played mode - En cas de défaite
if (digitalRead(ModePin_1) == LOW && digitalRead(ModePin_2) == LOW && digitalRead(ModePin_3) == LOW) {
    randNumber = random(0, randEasy_4);
    timeLimit = timeLimitEasy;
    Serial.println("Easy Mode");
} else if (digitalRead(ModePin_1) == LOW && digitalRead(ModePin_2) == LOW && digitalRead(ModePin_3) == HIGH) {
    randNumber = random(0, randNormal_4);
    timeLimit = timeLimitNormal;
    Serial.println("Normal Mode");
} else if (digitalRead(ModePin_1) == LOW && digitalRead(ModePin_2) == HIGH && digitalRead(ModePin_3) == LOW) {
    randNumber = random(0, randMedium_4);
    timeLimit = timeLimitMedium;
    Serial.println("Medium Mode");
} else if (digitalRead(ModePin_1) == LOW && digitalRead(ModePin_2) == HIGH && digitalRead(ModePin_3) == HIGH) {
    randNumber = random(0, randHard_4);
    timeLimit = timeLimitHard;
    Serial.println("Hard Mode");
} else if (digitalRead(ModePin_1) == HIGH && digitalRead(ModePin_2) == LOW && digitalRead(ModePin_3) == LOW) {
    randNumber = random(0, randExtreme_4);
    timeLimit = timeLimitExtreme;
    Serial.println("Extreme Mode");
} else if (digitalRead(ModePin_1) == HIGH && digitalRead(ModePin_2) == LOW && digitalRead(ModePin_3) == HIGH) {
    randNumber = random(0, randInsane_4);
    timeLimit = timeLimitInsane;
    Serial.println("Insane Mode");
} else if (digitalRead(ModePin_1) == HIGH && digitalRead(ModePin_2) == HIGH && digitalRead(ModePin_3) == LOW) {
    randNumber = random(0, randNightmare_4);
    timeLimit = timeLimitNightmare;
    Serial.println("Nightmare Mode");
} else if (digitalRead(ModePin_1) == HIGH && digitalRead(ModePin_2) == HIGH && digitalRead(ModePin_3) == HIGH) {
    randNumber = random(0, randImpossible_4);
    timeLimit = timeLimitImpossible;
    Serial.println("Impossible Mode");
}
    // Affiche le nombre à trouver
    display_1.showNumberDec(randNumber, false);
    wrongNumber = 0;
    correctNumber = 0;
  }

// Update the binary display
  binaryToDisplay = 0;
  if (digitalRead(Pin0) == HIGH) {
    binaryToDisplay  += 1;
  }
  if (digitalRead(Pin1) == HIGH) {
    binaryToDisplay += 10;
  }
  if (digitalRead(Pin2) == HIGH) {
    binaryToDisplay += 100;
  }
  if (digitalRead(Pin3) == HIGH) {
    binaryToDisplay += 1000;
  }
  if (digitalRead(Pin4) == HIGH) {
    binaryToDisplay += 10000;
  }
  if (digitalRead(Pin5) == HIGH) {
    binaryToDisplay += 100000;
  }
  if (digitalRead(Pin6) == HIGH) {
    binaryToDisplay += 1000000;
  }
  if (digitalRead(Pin7) == HIGH) {
    binaryToDisplay += 10000000;
  }
  displayNumber(binaryToDisplay);
}

void checkNumber() {
  binaryValue = 0;

  if (digitalRead(Pin0) == HIGH) {
    binaryValue = 1;
  }
  if (digitalRead(Pin1) == HIGH) {
    binaryValue += 2;
  }
  if (digitalRead(Pin2) == HIGH) {
    binaryValue += 4;
  }
  if (digitalRead(Pin3) == HIGH) {
    binaryValue += 8;
  }
  if (digitalRead(Pin4) == HIGH) {
    binaryValue += 16;
  }
  if (digitalRead(Pin5) == HIGH) {
    binaryValue += 32;
  }
  if (digitalRead(Pin6) == HIGH) {
    binaryValue += 64;
  }
  if (digitalRead(Pin7) == HIGH) {
    binaryValue += 128;
  }

  if (binaryValue == randNumber) {
    correctNumber = 1;
  } else {
    wrongNumber = 1;
  }
}

void displayNumber(long number) {
  // Extraire chaque chiffre du nombre et l'afficher sur l'afficheur
  for (int i = 0; i < 8; i++) {
    int digit = number % 10;                 // Extraire le chiffre le moins significatif
    display_2.setDigit(0, i, digit, false);  // Afficher le chiffre sur l'afficheur
    number /= 10;                            // Supprimer le chiffre le moins significatif
  }
}

// Fonction pour afficher l'effet arc-en-ciel
void rainbowCycle(uint8_t wait) {
  uint16_t i, j;
  unsigned long start = millis(); // Enregistre le temps de départ

  while(millis() - start < DURATION) { // Continue l'animation pendant DURATION millisecondes
    for(j = 0; j < 256; j++) { // Un cycle d'arcs-en-ciel complet
      for(i = 0; i < strip.numPixels(); i++) {
        strip.setPixelColor(i, Wheel(((i * 256 / strip.numPixels()) + j) & 255));
      }
      strip.show();
      delay(wait);
      if(millis() - start >= DURATION) break; // Si la durée est atteinte, sortir de la boucle
    }
  }
}

// Fonction pour générer les couleurs de l'arc-en-ciel
uint32_t Wheel(byte WheelPos) {
  WheelPos = 255 - WheelPos;
  if(WheelPos < 85) {
    return strip.Color(255 - WheelPos * 3, 0, WheelPos * 3);
  } else if(WheelPos < 170) {
    WheelPos -= 85;
    return strip.Color(0, WheelPos * 3, 255 - WheelPos * 3);
  } else {
    WheelPos -= 170;
    return strip.Color(WheelPos * 3, 255 - WheelPos * 3, 0);
  }
}

// Fonction pour updater le score
void updateScore() {
  if (correctNumber == 1) {
    score++;  // Incrémente le score si la réponse est correcte
    if (score > maxScore) score = maxScore;  // Limite le score à la valeur maximale

    if (score == maxScore) {
      score = 0;
      playVictoryMelody();  // Jouer la mélodie de victoire si le score atteint la valeur maximale
      rainbowCycle(5);      // 20 ms de délai entre les mises à jour
    }
  }
  if (wrongNumber == 1) {
    score--;                   // Décrémente le score si la réponse est incorrecte
    if (score < 0) score = 0;  // Limite le score à 0

    if (score == 0) {
      playDefeatMelody();      // Jouer la mélodie de défaite si le score atteint 0
    }
  }
}

void updateScoreDisplay() {
  strip.clear();
  for (int i = 0; i < score; i++) {
    int colorIndex;
    do {
      colorIndex = random(0, 7); // Choose a color index from 0 to 6
    } while (colorIndex == lastColorIndex);
    strip.setPixelColor(i, rainbowColors[colorIndex]);
    lastColorIndex = colorIndex;
  }
  strip.show();
}

void playVictoryMelody() {
  // Joue la mélodie de victoire
  tone(buzzer, 523); // Note C5
  delay(200);
  tone(buzzer, 659); // Note E5
  delay(200);
  tone(buzzer, 784); // Note G5
  delay(200);
  tone(buzzer, 1047); // Note C6
  delay(400);
  noTone(buzzer); // Stop sound
}

void playDefeatMelody() {
    // Joue la mélodie de défaite
  int melody[] = { 262, 196, 196, 220, 196, 0, 247, 262 };  // Ton des notes
  int noteDurations[] = { 4, 8, 8, 4, 4, 4, 4, 4 };         // Durée des notes
  
  for (int thisNote = 0; thisNote < 8; thisNote++) {
    int noteDuration = 1000 / noteDurations[thisNote];
    tone(buzzer, melody[thisNote], noteDuration);
    int pauseBetweenNotes = noteDuration * 1.30;
    delay(pauseBetweenNotes);
    noTone(buzzer);
  }
}

/*
// Fonction pour la élection du mode (PAS UTILISÉE)
void mode(int ModePin_1, int ModePin_2, int ModePin_3, 
          int randEasy, int randNormal, int randMedium, int randHard, int randExtreme, int randInsane, int randNightmare, int randImpossible, 
          unsigned long timeLimitEasy, unsigned long timeLimitNormal, unsigned long timeLimitMedium, unsigned long timeLimitHard, unsigned long timeLimitExtreme, unsigned long timeLimitInsane, unsigned long Nightmare, unsigned long timeLimitImpossible) {
  // Set the played mode en cas de victoire
  if (digitalRead(ModePin_1) == HIGH && digitalRead(ModePin_2) == HIGH && digitalRead(ModePin_3) == HIGH) {
      randNumber = random(0, randEasy);
      timeLimit = timeLimitEasy;
      Serial.println("Easy Mode");
  } else if (digitalRead(ModePin_1) == HIGH && digitalRead(ModePin_2) == HIGH && digitalRead(ModePin_3) == LOW) {
      randNumber = random(0, randNormal);
      timeLimit = timeLimitNormal;
      Serial.println("Normal Mode");
  } else if (digitalRead(ModePin_1) == HIGH && digitalRead(ModePin_2) == LOW && digitalRead(ModePin_3) == HIGH) {
      randNumber = random(0, randMedium);
      timeLimit = timeLimitMedium;
      Serial.println("Medium Mode");
  } else if (digitalRead(ModePin_1) == HIGH && digitalRead(ModePin_2) == LOW && digitalRead(ModePin_3) == LOW) {
      randNumber = random(0, randHard);
      timeLimit = timeLimitHard;
      Serial.println("Hard Mode");
  } else if (digitalRead(ModePin_1) == LOW && digitalRead(ModePin_2) == HIGH && digitalRead(ModePin_3) == HIGH) {
      randNumber = random(0, randExtreme);
      timeLimit = timeLimitExtreme;
      Serial.println("Extreme Mode");
  } else if (digitalRead(ModePin_1) == LOW && digitalRead(ModePin_2) == HIGH && digitalRead(ModePin_3) == LOW) {
      randNumber = random(0, randInsane);
      timeLimit = timeLimitInsane;
      Serial.println("Insane Mode");
  } else if (digitalRead(ModePin_1) == LOW && digitalRead(ModePin_2) == LOW && digitalRead(ModePin_3) == HIGH) {
      randNumber = random(0, randNightmare);
      timeLimit = timeLimitNightmare;
      Serial.println("Nightmare Mode");
  } else if (digitalRead(ModePin_1) == LOW && digitalRead(ModePin_2) == LOW && digitalRead(ModePin_3) == LOW) {
      randNumber = random(0, randImpossible);
      timeLimit = timeLimitImpossible;
      Serial.println("Impossible Mode");
  }
}
*/
