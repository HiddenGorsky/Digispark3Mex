# Digispark3Mex By: HellPineapple

#include "DigiKeyboard.h"

// Mapeo de teclas para dígitos 0-9
int num[] = {39, 30, 31, 32, 33, 34, 35, 36, 37, 38};

// Mapeo de teclas para los nodos del patrón
int patternNodes[] = {44, 45, 46, 47, 48, 49, 50, 51, 52}; // Ejemplo

// Configuraciones
const int PIN_LENGTH = 4; // Cambiar a 6 para PIN de 6 dígitos
bool usePattern = false; // Cambiar a true para usar el patrón de desbloqueo

// Variables para el PIN
int digits[6] = {0, 0, 0, 0, 0, 0};

// Variables para el patrón
const int PATTERN_LENGTH = 4; // Longitud del patrón de desbloqueo
int pattern[9] = {0, 0, 0, 0, 0, 0, 0, 0, 0}; // Almacena la secuencia del patrón

const int ENTER_KEY = 40;
const int DELAY_TIME = 31000;

int count = 0;

void setup() {
  DigiKeyboard.update();
  DigiKeyboard.sendKeyStroke(0);
  delay(3000);
}

void sendKeys(int* keys, int* values, int length) {
  for (int i = 0; i < length; i++) {
    DigiKeyboard.sendKeyStroke(keys[values[i]]);
  }
  DigiKeyboard.sendKeyStroke(ENTER_KEY); // Enter
  delay(1000);
}

void incrementKeys(int* keys, int length, int max) {
  for (int i = length - 1; i >= 0; i--) {
    keys[i]++;
    if (keys[i] < max) break;
    keys[i] = 0;
  }
}

void loop() {
  if (count == 5) {
    digitalWrite(1, HIGH); // LED on to indicate delay
    DigiKeyboard.sendKeyStroke(ENTER_KEY); // Enter to dismiss any popup
    delay(DELAY_TIME);
    count = 0;
    digitalWrite(1, LOW); // LED off
  }

  if (usePattern) {
    sendKeys(patternNodes, pattern, PATTERN_LENGTH);
    incrementKeys(pattern, PATTERN_LENGTH, 9);
  } else {
    sendKeys(num, digits, PIN_LENGTH);
    incrementKeys(digits, PIN_LENGTH, 10);
  }

  count++;
}
