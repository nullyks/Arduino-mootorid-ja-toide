# Samm-mootorid
![alt text](meedia/sammmootor.jpg)

*Allikas: https://commons.wikimedia.org/wiki/File:28BYJ-48_unipolar_stepper_motor_with_ULN2003_driver.jpg*

Samm-mootorid on elektrimootorid, mis liiguvad kindla nurga kaupa ehk sammudena, võimaldades täpset positsioneerimist ja kiiruse juhtimist. Neid kasutatakse näiteks 3D-printerites ja teistes automaatikaseadmetes, kus on vaja kontrollitud liikumist. Erinevalt tavalistest alalisvoolumootoritest ei tööta samm-mootor pideva pöörlemisega, vaid liigub järk-järgult etteantud sammude võrra, mida määravad mootori juhtimissüsteem ja kontroller.

Arduino UNO-ga saab samm-mootoreid juhtida spetsiaalsete draiverite, näiteks ULN2003, abil. See draiver võimaldavad genereerida vajalikke juhtsignaale ja toita mootorit vajaliku pingega, kaitstes samal ajal Arduino plaati liigse voolukoormuse eest. Tavaliselt juhitakse samm-mootorit kahe või enama juhtsignaali abil, määrates sammude suuna ja kiiruse.

Arduino UNO abil saab samm-mootorit juhtida Stepper või AccelStepper teekide abil, mis lihtsustavad liikumise kodeerimist ja kiiruse reguleerimist. Programmikoodiga saab määrata sammude arvu, suuna ja kiirenduse, et saavutada soovitud liikumine. Kuigi samm-mootorid on väga täpsed, võivad nad kaotada samme, kui koormus on liiga suur või kiirendus liiga järsk.

## Samm-mootori 28BYJ-48 juhtiminde ULN2003 driveri ja Arduino UNO abil

28BYJ-48 on neljafaasiline (mootoril on neli mähist, millest korraga aktiveeritakse kaks), 5V nimipingega samm-mootor. Ühe sammu nurk on 11,25 kraadi ja mootori reduktsioonisuhe 1/64. See teeb ühe täispöörde sammude arvuks 2048 sammu. Mootor on võrdlemisi aeglane (kuni 15 pööret minutis 5V toitepinge juures) ja mitte eriti suure väändemomendiga (34,3 mN*m).

Selle mootori juhtimiseks sobib hästi ULN 2003 draiver, mis võimaldab ühendada lisatoiteallika ja kontrollida mootorit nelja viigu abil.
Draiveriga suhtlemiseks kasutame Stepper teeki.

![alt text](meedia/stepper_näide.png)

~~~cpp
#include <Stepper.h>
#define in1 8
#define in2 9
#define in3 10
#define in4 11

int sammu = 2038; //samme täispöörde kohta
Stepper mootor = Stepper(sammu, in1, in3, in2, in4);
//selline veider sisendite järjestus on selle mudeli puhul oluline

void setup() {
}

void loop() {
	//teeme 1 täispöörde päripäeva, kiirusega 10 sekundit pööre 
	mootor.setSpeed(6); //kiirus on pööret/minutis
	myStepper.step(sammu); //see on blokkeeriv funktsioon
	
	//teeme 2 täispööret vastupäeva kiirusega 5 sekundit pööre
	myStepper.setSpeed(12);
	myStepper.step(sammu*-2);
}
~~~~

See koodinäide põhineb [pikemal ja detailsemal õpetusel](https://lastminuteengineers.com/28byj48-stepper-motor-arduino-tutorial/), mida soovitan tungivalt lugeda.