# Proyecto-final-de-Bim2
Aplicar los principios de diseño de firmware a situaciones de la vida real.  • Diseñar sistemas embebidos que realicen acciones de acuerdo a problemáticas planteadas.
/*
   Fundacion Kinal
   Centro educativo tecnico laboral Kinal
   Quinto perito
   Quinto electronica
   Codigo Tecnico: EB5AM 
   Curso: Taller de electronica digital y reparacion de computadoras I
   Proyecto: Practica final
   Dev: Alvaro Angel Gabriel Estrada Morales
   Fecha: 26 de mayo de 2024
*/

#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <Servo.h>
#include <Keypad.h>

#define LCD_ADDRESS 0x27
#define LCD_ROWS 2
#define LCD_COLS 16

#define KEYPAD_ROWS 2
#define KEYPAD_COLS 3
#define KEYPAD_ROW_1 A0
#define KEYPAD_ROW_2 A1
#define KEYPAD_COL_1 2
#define KEYPAD_COL_2 3
#define KEYPAD_COL_3 4

#define LED_1 6          
#define LED_2 7  
#define LED_3 8          
#define LED_4 9 
#define LED_5 11          
#define LED_6 12 
#define LED_7 13
#define SERVO_MOTOR 10

// Pines para el display de 7 segmentos
#define SEG_A 5
#define SEG_B 11
#define SEG_C 12
#define SEG_D 13
#define SEG_E A3
#define SEG_F A2
#define SEG_G 7

#define TURN_ON(pin) digitalWrite(pin, HIGH)
#define TURN_OFF(pin) digitalWrite(pin, LOW)

int count = 0;

char keymap[KEYPAD_ROWS][KEYPAD_COLS] = {
  {'1', '2', '3'},
  {'4', '5', '6'}
};

byte rowPins[KEYPAD_ROWS] = {KEYPAD_ROW_1, KEYPAD_ROW_2};
byte colPins[KEYPAD_COLS] = {KEYPAD_COL_1, KEYPAD_COL_2, KEYPAD_COL_3};

LiquidCrystal_I2C lcd(LCD_ADDRESS, LCD_COLS, LCD_ROWS);
Keypad keypad(Keypad(makeKeymap(keymap), rowPins, colPins, KEYPAD_ROWS, KEYPAD_COLS));

Servo servoMotorParte5;

void setupPins(void);
void setupDisplay(void);
void displayLetterA(void);

void setup() {
  Serial.begin(9600);
  Serial.println("Proyecto Final Bim2\nDev: Alvaro Angel Gabriel Estrada Morales\nPancakes 7w7\n2020249");
  TURN_OFF(LED_1);
  TURN_OFF(LED_2);
  TURN_OFF(LED_3);
  TURN_OFF(LED_4);
  TURN_OFF(LED_5);
  TURN_OFF(LED_6);
  TURN_OFF(LED_7);

  setupPins();
  setupDisplay();
}

void loop() {
  char key = keypad.getKey();

  if (key) {
    if (key == '1') {
      action1();
    } else if (key == '2') {
      action2();
    } else if (key == '3') {
      action3();
    } else if (key == '4') {
      action4();
    } else if (key == '5') {
      action5();
    }
  }
}

void action1() {
  for (count = 0; count < 100; count++) {
    Serial.println(count);
    delay(150);
  }
}

void action2() {
  for (count = 99; count >= 0; count--) {
    Serial.println(count);
    delay(150);
  }
}

void action3() {
  int leds[] = {LED_1, LED_2, LED_3, LED_4, LED_5, LED_6, LED_7};
  int numLeds = sizeof(leds) / sizeof(leds[0]);
  int i = 0;

  while (i < numLeds) {
    digitalWrite(leds[i], HIGH);
    delay(200);
    digitalWrite(leds[i], LOW);
    i++;
  }

  i = numLeds - 1;
  while (i >= 0) {
    digitalWrite(leds[i], HIGH);
    delay(200);
    digitalWrite(leds[i], LOW);
    i--;
  }
}

void action4() {
  for (int i = 0; i < 3; i++) {
    displayLetterA();
    delay(1000); // Encendido por 1 segundo
    clearDisplay();
    delay(1000); // Apagado por 1 segundo
  }
}

void action5() {
  servoMotorParte5.write(180);
  delay(4000);
  servoMotorParte5.write(0);
}

void setupPins(void) {
  pinMode(SERVO_MOTOR, OUTPUT);
  pinMode(LED_1, OUTPUT);
  pinMode(LED_2, OUTPUT);
  pinMode(LED_3, OUTPUT);
  pinMode(LED_4, OUTPUT);
  pinMode(LED_5, OUTPUT);
  pinMode(LED_6, OUTPUT);
  pinMode(LED_7, OUTPUT);
  
  // Configuración de pines del display de 7 segmentos
  pinMode(SEG_A, OUTPUT);
  pinMode(SEG_B, OUTPUT);
  pinMode(SEG_C, OUTPUT);
  pinMode(SEG_D, OUTPUT);
  pinMode(SEG_E, OUTPUT);
  pinMode(SEG_F, OUTPUT);
  pinMode(SEG_G, OUTPUT);
}

void setupDisplay(void) {
  servoMotorParte5.attach(SERVO_MOTOR);
  lcd.init();
  lcd.backlight();
  lcd.setCursor(2, 0);
  lcd.print("Alvaro Estrada 7w7");
  lcd.setCursor(1, 1);
  lcd.print("Proyecto Final");
}

void displayLetterA(void) {
  TURN_ON(SEG_A);
  TURN_ON(SEG_B);
  TURN_ON(SEG_C);
  TURN_OFF(SEG_D);
  TURN_ON(SEG_E);
  TURN_ON(SEG_F);
  TURN_ON(SEG_G);
}

void clearDisplay(void) {
  TURN_OFF(SEG_A);
  TURN_OFF(SEG_B);
  TURN_OFF(SEG_C);
  TURN_OFF(SEG_D);
  TURN_OFF(SEG_E);
  TURN_OFF(SEG_F);
  TURN_OFF(SEG_G);
}
