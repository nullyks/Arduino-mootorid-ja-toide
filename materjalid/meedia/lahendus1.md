### Lahenduse kood
~~~cpp
#include <Stepper.h>

#define STEPS_PER_REV 2048 // 28BYJ-48 mootori täis pöörete arv
#define MOTOR_PIN_1 8
#define MOTOR_PIN_2 9
#define MOTOR_PIN_3 10
#define MOTOR_PIN_4 11
#define POT_SPEED A0
#define POT_STEPS A1
#define BUTTON 7

Stepper stepper(STEPS_PER_REV, MOTOR_PIN_1, MOTOR_PIN_3, MOTOR_PIN_2, MOTOR_PIN_4);

void setup() {
    pinMode(POT_SPEED, INPUT);
    pinMode(POT_STEPS, INPUT);
    pinMode(BUTTON, INPUT_PULLUP);
}

void loop() {
    int speedValue = analogRead(POT_SPEED);
    int stepValue = analogRead(POT_STEPS);
    
    // Kiiruse seadmine (1-15 RPM)
    int motorSpeed = map(speedValue, 0, 1023, 1, 15);
    stepper.setSpeed(motorSpeed);
    
    // Sammude seadmine (0-2048)
    int stepsToMove = map(stepValue, 0, 1023, 0, STEPS_PER_REV);
    
    // Kui nuppu vajutatakse, käivitub mootor vastupäeva
    if (digitalRead(BUTTON) == LOW) {
        stepper.step(-stepsToMove);
    }
}
~~~