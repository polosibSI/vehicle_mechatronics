# vehicle_mechatronics_report
This code enables to see if the buttons work
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
```
In my case, the monitor constantly return the value "high" for the center button. I think the center button is broken. Otherwise, all the other button are working.

This code make the stepper motor rotate : 
```
#include <Stepper.h>

// Définir les broches de contrôle pour le moteur
const int motorPin1 = 8;  // IN1
const int motorPin2 = 9;  // IN2
const int motorPin3 = 10; // IN3
const int motorPin4 = 11; // IN4

// Définir le nombre de pas par révolution (2048 pas pour le moteur 28BYJ-48)
const int stepsPerRevolution = 2048;

// Créer un objet "stepper" de la classe Stepper
// Le moteur utilise 4 broches (IN1, IN2, IN3, IN4)
Stepper myStepper(stepsPerRevolution, motorPin1, motorPin2, motorPin3, motorPin4);

void setup() {
  // Initialiser la vitesse du moteur
  myStepper.setSpeed(5);  // Définir la vitesse du moteur (15 RPM dans cet exemple)

  // Afficher un message dans le moniteur série pour indiquer que le programme commence
  Serial.begin(9600);
  Serial.println("Moteur en fonctionnement...");
}

void loop() {
  // Faire tourner le moteur d'un tour complet dans un sens (horaire)
  Serial.println("Tour horaire");
  myStepper.step(stepsPerRevolution);
  delay(1000);  // Pause d'une seconde

  // Faire tourner le moteur d'un tour complet dans l'autre sens (antihoraire)
  Serial.println("Tour antihoraire");
  myStepper.step(-stepsPerRevolution);
  delay(1000);  // Pause d'une seconde
}
```
For me, the motor only turn in one way.

Code pour piloter le moteur avec un bouton directionnel :
```
#include <Stepper.h>

// Nombre de pas pour une révolution complète
const int stepsPerRevolution = 2048;

// Définir les pins du moteur (IN1-IN2-IN3-IN4)
Stepper myStepper(stepsPerRevolution, 8, 10, 9, 11);

// Définir les pins des boutons
const int buttonUp = 2;
const int buttonDown = 3;
const int buttonLeft = 4;
const int buttonRight = 5;

int motorSpeed = 10; // Vitesse de départ (RPM)

void setup() {
  // Initialisation du moteur
  myStepper.setSpeed(motorSpeed);

  // Initialisation des boutons
  pinMode(buttonUp, INPUT_PULLUP);
  pinMode(buttonDown, INPUT_PULLUP);
  pinMode(buttonLeft, INPUT_PULLUP);
  pinMode(buttonRight, INPUT_PULLUP);

  // Communication série (pour debug)
  Serial.begin(9600);
}

void loop() {
  if (digitalRead(buttonUp) == LOW) { // Bouton appuyé (LOW car INPUT_PULLUP)
    motorSpeed += 1;
    if (motorSpeed > 30) motorSpeed = 30; // Limite max
    myStepper.setSpeed(motorSpeed);
    Serial.print("Vitesse augmentée: ");
    Serial.println(motorSpeed);
    delay(200); // Anti-rebond
  }

  if (digitalRead(buttonDown) == LOW) {
    motorSpeed -= 1;
    if (motorSpeed < 1) motorSpeed = 1; // Limite min
    myStepper.setSpeed(motorSpeed);
    Serial.print("Vitesse diminuée: ");
    Serial.println(motorSpeed);
    delay(200);
  }

  if (digitalRead(buttonLeft) == LOW) {
    Serial.println("Rotation gauche...");
    myStepper.step(-stepsPerRevolution / 8); // Petit mouvement à gauche
    delay(200);
  }

  if (digitalRead(buttonRight) == LOW) {
    Serial.println("Rotation droite...");
    myStepper.step(stepsPerRevolution / 8); // Petit mouvement à droite
    delay(200);
  }
}
````
