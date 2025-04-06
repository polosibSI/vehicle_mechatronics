# vehicle_mechatronics
```
// Définir les broches pour les boutons du joystick
const int joyUpPin = 2;    // Broche pour le bouton UP
const int joyDownPin = 3;  // Broche pour le bouton DOWN
const int joyLeftPin = 4;  // Broche pour le bouton LEFT
const int joyRightPin = 5; // Broche pour le bouton RIGHT
const int joyMidPin = 6;   // Broche pour le bouton MID (centrale)
const int joySetPin = 7;   // Broche pour le bouton SET
const int joyRstPin = 8;   // Broche pour le bouton RESET

void setup() {
  // Initialiser les broches des boutons en mode entrée avec pull-up interne
  pinMode(joyUpPin, INPUT_PULLUP);
  pinMode(joyDownPin, INPUT_PULLUP);
  pinMode(joyLeftPin, INPUT_PULLUP);
  pinMode(joyRightPin, INPUT_PULLUP);
  pinMode(joyMidPin, INPUT_PULLUP);
  pinMode(joySetPin, INPUT_PULLUP);
  pinMode(joyRstPin, INPUT_PULLUP);

  Serial.begin(9600);
}

void loop() {
  // Lire l'état des boutons du joystick
  bool joyUp = digitalRead(joyUpPin) == LOW;
  bool joyDown = digitalRead(joyDownPin) == LOW;
  bool joyLeft = digitalRead(joyLeftPin) == LOW;
  bool joyRight = digitalRead(joyRightPin) == LOW;
  bool joyMid = digitalRead(joyMidPin) == LOW;
  bool joySet = digitalRead(joySetPin) == LOW;
  bool joyRst = digitalRead(joyRstPin) == LOW;

  // Afficher l'état des boutons dans le moniteur série pour débogage
  Serial.print("UP: ");
  Serial.print(joyUp);
  Serial.print(" DOWN: ");
  Serial.print(joyDown);
  Serial.print(" LEFT: ");
  Serial.print(joyLeft);
  Serial.print(" RIGHT: ");
  Serial.print(joyRight);
  Serial.print(" MID: ");
  Serial.print(joyMid);
  Serial.print(" SET: ");
  Serial.print(joySet);
  Serial.print(" RST: ");
  Serial.println(joyRst);

  delay(200);
}

