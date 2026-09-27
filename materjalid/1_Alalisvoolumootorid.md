# Alalisvoolumootorid

Alalisvoolumootorid *(ingl Direct Current motor, DC-motor)* on elektromehaanilised seadmed, mis muundavad alalisvoolu elektrienergia pöörlevaks liikumiseks. Need mootorid töötavad magnetvälja ja elektrivoolu vastastikmõju põhimõttel ([Lorentzi jõud](https://et.wikipedia.org/wiki/Lorentzi_j%C3%B5ud)), kus rootor hakkab pöörlema, kui vool läbib mootori mähiseid. Alalisvoolumootoreid leidub mitmesugustes seadmetes, alates mänguasjadest kuni tööstusautomaatika lahendusteni. Arduino UNO-ga juhitavad alalisvoolumootorid on tavaliselt harjadega alalisvoolumootorid *(ingl Brushed DC-motor)*. Harjadega alalisvoolumootori täpsema tööpõhimõtte kohta saad lugeda [siit.](https://en.wikipedia.org/wiki/Brushed_DC_electric_motor)

![Alalisvoolumootori skeem](meedia/DC-motor.png)

*Allikas: https://commons.wikimedia.org/wiki/File:Electric_motor_cycle_3.png*

Alalisvoolumootori juhtimiseks Arduino UNO-ga kasutatakse tavaliselt [transistoripõhist](https://github.com/nullyks/Arduino-baaselemendid/blob/main/materjalid/3_transistorid.md) lülitust või mootoridraiverit, näiteks L293D. Need draiverid võimaldavad mootori suunda ja kiirust reguleerida PWM-signaali abil. PWM-signaal lülitab mootoridraiverit kiiresti sisse ja välja. PWM-signaali töötsükli muutmine muudab mootorile rakenduvat keskmist pinget ja võimsust ning seeläbi tavaliselt mootori pöörlemiskiirust. Arduino UNO PWM-viikudest rääkisime [siin.](https://github.com/nullyks/Arduino-sissejuhatus/blob/main/materjalid/3_Arduino_UNO_viigud.md) Mootori pöörlemissuuna muutmiseks kasutatakse H-silda, mis vahetab voolu suunda mähistes. Mõned mootorid on varustatud enkoodritega, mis võimaldavad täpsemat kiiruse ja asendi juhtimist.

Arduino UNO suudab genereerida juhtsignaale, kuid ei suuda pakkuda enamikule mootoritele piisavalt voolu. Seetõttu tuleb mootori voolu lülitamiseks kasutada sobivat transistorlülitust või mootoridraiverit. Mootori toitepinge valitakse mootori nimipinge ja draiveri lubatud pingevahemiku järgi. Toiteallikas peab suutma anda vähemalt mootori seiskumisvoolule vastavat voolu. Välise toiteallika miinusklemm, Arduino GND ning transistorlülituse või mootoridraiveri GND tuleb omavahel ühendada.

## Alalisvoolumootori juhtimine MOSFET-transistoriga

Kõige lihtsamal juhul saame alalisvoolumootorit juhtida ühe MOSFET-[transistoriga](https://github.com/nullyks/Arduino-baaselemendid/blob/main/materjalid/3_transistorid.md). Ühest transistorist piisab mootori toiteahela lülitamiseks. PWM-signaali abil saab muuta mootori keskmist võimsust ja seeläbi selle pöörlemiskiirust, kuid mitte pöörlemissuunda. Näites saab kasutaja mootori pöörlemiskiiruse määrata potentsiomeetri asendiga. Näites kasutame N-kanaliga MOSFET-transistorit.

Kasuta loogikatasemega N-kanaliga MOSFET-transistorit, mille väike sisselülitustakistus on andmelehel määratud umbes 4,5–5 V paisupinge juures, näiteks IRLZ44N.

### Ühe MOSFET-transistoriga juhtimise näide
![Alalisvoolumootori ja MOSFET-transistori ühendamine Arduinoga](meedia/Mosfet_ja_mootor.png)

**Märkus.** Skeemil kujutatud 9 V plokkpatarei on näitlik. Tavaline 9 V plokkpatarei ei sobi üldjuhul mootori praktiliseks toitmiseks, sest selle vooluandmisvõime on väike ja pinge võib koormuse all tugevalt langeda. Füüsilises vooluahelas tuleb kasutada mootori nimipingele ja seiskumisvoolule vastavat toiteallikat.

**Märkus.** Skeemi lihtsustamiseks ei ole joonisel näidatud mootori kaitsedioodi. Füüsilises vooluahelas tuleb harjadega alalisvoolumootori klemmidega ühendada rööbiti kaitsediood: dioodi katood ehk triibuga ots mootori toiteplussi ja anood MOSFET-i-poolse klemmi külge. Diood vähendab MOSFET-i väljalülitamisel tekkiva induktiivse pingelöögi ohtu.

Samuti on skeemi lihtsustamiseks välja jäetud MOSFET-i paisu ja GND vahele ühendatav 10 kΩ tõmbetakisti. Füüsilises vooluahelas on soovitatav see lisada, et hoida MOSFET ja mootor Arduino käivitumise või lähtestamise ajal väljalülitatuna.

~~~cpp
#define gate 3 //selle viigu abil kontrollime MOSFET-i
#define pot A0 //selle viigu abil loeme potentsiomeetri asendi

void setup() {
	Serial.begin(9600);
    pinMode(gate, OUTPUT);
}


void loop() {
	int potValue=analogRead(pot); //loeme potentsiomeetri väärtuse
    int motorValue=map(potValue,0,1023,0,255); //vastendame loetud väärtuse MOSFETi juhtimiseks sobivasse vahemikku
    Serial.println(motorValue);
    analogWrite(gate, motorValue); //motorValue määrab MOSFET-i juhtiva PWM-signaali töötsükli
}
~~~
[Interaktiivne simulatsioon](https://www.tinkercad.com/things/aAgbmGxxBCX-alalisvoolumootor-transistoriga?sharecode=Arc8_eGUyrxYSLrNBUmZmxTYQaNH4XsKgko5yd7moFY)

## Alalisvoolumootori juhtimine L293D draiveriga
Ühe MOSFET-transistoriga lülitus võimaldab muuta mootori kiirust, kuid mitte pöörlemissuunda. Pöörlemissuuna muutmiseks kasutatakse H-silda, mis sisaldub näiteks L293D mootoridraiveris.
H-sild on vooluahel, mis võimaldab alalisvoolumootori pöörlemissuunda muuta, juhtides seda, millises suunas toitevool mootori läbib.

See koosneb tavaliselt neljast transistorist või lülitist, mis on paigutatud H-kujuliselt – sellest ka nimetus H-sild. Kui kaks diagonaalis olevat transistori sisse lülitada, voolab vool läbi mootori ühes suunas, ja kui sisse lülitatakse teised kaks, pöördub voolu suund, muutes mootori pöörlemissuunda.

![H-silla skeem](meedia/H_bridge.png)

*(Allikas: https://commons.wikimedia.org/wiki/File:H_bridge.svg)*

Tabelis tähendab `1`, et vastav lüliti on suletud ehk juhib voolu, ning `0`, et lüliti on avatud ega juhi voolu.

| S1 | S2| S3 | S4 | Tulemus |
|:---:|:---:|:---:|:---:|:---|
| 1 | 0| 0 | 1 | Mootor pöörleb paremale |
| 0 | 1| 1 | 0 | Mootor pöörleb vasakule |
| 0 | 0| 0 | 0 | Mootor veereb vabalt |
| 0 | 1| 0 | 1 | Dünaamiline pidurdus: mootori mõlemad klemmid on ühendatud GND-ga |
| 1 | 0| 1 | 0 | Dünaamiline pidurdus: mootori mõlemad klemmid on ühendatud toiteplussiga |
| 1 | 1| 0 | 0 | Lühis |
| 0 | 0| 1 | 1 | Lühis |
| 1 | 1| 1 | 1 | Lühis |

**NB!** Lühisena märgitud kombinatsioone ei tohi kasutada. Nende korral ühendatakse toitepluss ja GND sama sillaharu kaudu otse kokku, mis võib kahjustada lüliteid, draiverit või toiteallikat.

L293D mootoridraiver sisaldab kahte H-silda, mistõttu saab sellega juhtida korraga kahte mootorit teineteisest sõltumatult. See sobib näiteks roomik- ja kaherattaliste liikurplatvormide jaoks, mille vedavad rattad paiknevad vastaskülgedel.

### L293D ühendusskeem
![L293D viikude skeem](meedia/L293D.png)

*Allikas: https://www.ti.com/lit/ds/symlink/l293.pdf*

* **VCC1** - draiveri enda toide 5 V
* **VCC2** - mootorite toide 4,5–36 V
* **1,2EN** - vasakpoolse mootori sisselülitus (PWM-iga saab juhtida kiirust)
* **3,4EN** - parempoolse mootori sisselülitus (PWM-iga saab juhtida kiirust)
* **1Y** - vasakpoolse mootori üks klemm
* **2Y** - vasakpoolse mootori teine klemm
* **1A ja 2A** - juhivad vasakpoolse mootori suunda. Vastupidised loogikatasemed panevad mootori pöörlema; sama loogikatase mõlemal sisendil peatab mootori.
* **3Y** - parempoolse mootori üks klemm
* **4Y** - parempoolse mootori teine klemm
* **3A ja 4A** - juhivad parempoolse mootori suunda. Vastupidised loogikatasemed panevad mootori pöörlema; sama loogikatase mõlemal sisendil peatab mootori.
* **viigud 4,5,12,13** - maandus

**NB!** L293D ühe väljundkanali lubatud pidevvool on kuni 600 mA. Kuna ühe mootori juhtimiseks kasutatakse kahte kanalit, peab ka mootori pidev vool jääma alla 600 mA. Mootoridraiver tuleb valida mootori seiskumisvoolu järgi ning arvestada tuleb ka kiibi soojenemisega. Andmelehel nimetatud 1,2 A on üksnes väga lühiajaline tippvool, mitte lubatud pidev töövool.

L293D väljunditel tekib koormuse all pingelang, mistõttu võib mootori klemmipinge olla toitepingest mitu volti madalam. Arvesta sellega mootori toitepinge valimisel ja jälgi draiveri soojenemist.

### L293D draiveriga mootori juhtimise näide

Mootori pöörlemise suunda ja kiirust kontrollib [potentsiomeeter](https://github.com/nullyks/Arduino-baaselemendid/blob/main/materjalid/1_takistid.md). Keskmises asendis mootor seisab. Mida rohkem paremale on potentsiomeeter, seda kiiremini paremale mootor pöörleb ja mida rohkem vasakule on potentsiomeeter, seda kiiremini vasakule mootor pöörleb.

![L293D ühendamise näide](meedia/L293D_näide.png)

**Märkus.** Skeemil kujutatud 9 V plokkpatarei on näitlik. Füüsilises vooluahelas tuleb kasutada mootorite nimipingele ja mõlema mootori seiskumisvoolule vastavat toiteallikat ning jälgida, et L293D lubatud voolu ei ületataks.

**Märkus.** Skeemi lihtsustamiseks ei ole joonisel näidatud toite möödaviigukondensaatoreid. Füüsilises vooluahelas on soovitatav ühendada vähemalt 0,1 µF kondensaator nii VCC1- kui ka VCC2-viigu ja GND vahele võimalikult L293D lähedale.

~~~cpp
#define pot A0 //selle viigu kaudu loeme potentsiomeetri asendi
#define kiirus 3 //see viik määrab mootori kiiruse
// Need viigud määravad mootori pöörlemissuuna
#define suund1 4
#define suund2 5

void setup()
{
  pinMode(kiirus, OUTPUT);
  pinMode(suund1, OUTPUT);
  pinMode(suund2, OUTPUT);
}

void loop()
{
  int potData=analogRead(pot); //loeme poteka väärtuse
  if(potData<500){ // kui väärtus on väiksem kui 500, siis pöörleb mootor vasakule
    digitalWrite(suund1, HIGH);
    digitalWrite(suund2, LOW);
    int mapValue=map(potData,0,499,255,0); // mida väiksem poteka näit seda suurem kiirus
    analogWrite(kiirus,mapValue);
  }
  if(potData>=500 && potData<=520){ //kui potekas on vahemikus 500 - 520, siis mootor seisab
     analogWrite(kiirus,0);
  }
  if(potData>520){ //kui potekas väärtus on üle 520, siis mootor pöörleb paremale
    digitalWrite(suund1, LOW);
    digitalWrite(suund2, HIGH);
    int mapValue=map(potData,521,1023,0,255); //mida suurem poteka näit, seda suurem kiirus
    analogWrite(kiirus,mapValue);
  }
}
~~~

[Interaktiivne simulatsioon](https://www.tinkercad.com/things/fjxmPUpN59m-l293d?sharecode=uxteCFtOcrfE4VWT_eUc0wMcFpKebon9JHcF5isJFJo)
