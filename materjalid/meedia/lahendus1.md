### Lahenduse kood
~~~cpp
#include <Stepper.h>

#define STEPS_PER_REV 2048 // väljundvõlli ühe pöörde nominaalne täissammude arv
#define MOTOR_PIN_1 8
#define MOTOR_PIN_2 9
#define MOTOR_PIN_3 10
#define MOTOR_PIN_4 11
#define POT_SPEED A0
#define POT_STEPS A1
#define BUTTON 7

Stepper stepper(STEPS_PER_REV, MOTOR_PIN_1, MOTOR_PIN_3, MOTOR_PIN_2, MOTOR_PIN_4);
int eelmineNupuOlek = HIGH;

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

    // Üks vajutus käivitab mootori ühe korra vastupäeva
    int nupuOlek = digitalRead(BUTTON);
    if (eelmineNupuOlek == HIGH && nupuOlek == LOW) {
        stepper.step(-stepsToMove);
    }

    eelmineNupuOlek = nupuOlek;
    delay(20); // lihtne kontaktivärina tõrje
}
~~~

**Märkus.** `stepper.step()` on blokeeriv: mootori liikumise ajal ei töötle programm uusi nupuvajutusi ega potentsiomeetrite muutusi. Kui juhtseadmed peavad liikumise ajal reageerima, tuleb kasutada mitteblokeerivat lahendust, näiteks `AccelStepper`-teeki.
