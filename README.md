# Mootorite liidestamine Arduinoga ja väline toide
See õppematerjal selgitab alalisvoolu-, servo- ja samm-mootorite liidestamist ning juhtimist Arduino UNO-ga. Kuna enamik mootoreid vajab rohkem voolu, kui Arduino suudab pakkuda, käsitletakse ka eraldi toiteallika valimist ja ühendamist.

## Kasutatavad arendusplaadid

Õppematerjali ühendusskeemid ja Tinkercadi näited on koostatud Arduino UNO R3 jaoks. Näidetes kasutatavad digitaal- ja analoogviigud ning programmikood sobivad ka Arduino UNO R4 WiFi plaadile.

Mõlemad plaadid kasutavad 5 V loogikataset ning neil paiknevad kasutatud digitaal-, analoog- ja PWM-viigud samades kohtades. Arduino ametlike viigudiagrammide järgi on UNO R3 ühe I/O-viigu lubatud vool 20 mA ja UNO R4 WiFi puhul 8 mA. Need väärtused ei ole mootori toitmiseks piisavad. Seetõttu tuleb mootoreid mõlema plaadiga juhtida MOSFET-i või sobiva mootoridraiveri kaudu ning toita eraldi toiteallikast.

Plaatide täpsema võrdluse leiab [Arduino ametlikust UNO R3 ja UNO R4 võrdlusest](https://support.arduino.cc/hc/en-us/articles/9350551575964-What-s-the-difference-between-UNO-R3-and-UNO-R4-boards).

## Eelteadmised

Enne selle õppematerjali kasutamist peaks õppija oskama:

* ühendada Arduino UNO arendusplaadi arvutiga ja laadida sellele programmi;
* kasutada makettplaati ning lugeda lihtsat ühendusjoonist;
* kasutada Arduino digitaal- ja analoogviike;
* selgitada pinge, voolutugevuse, takistuse ja elektrivõimsuse põhimõisteid;
* kasutada potentsiomeetrit, dioodi ja transistorit lihtsas vooluahelas.

Vajaduse korral vaata enne jätkamist läbi [Arduino sissejuhatuse](https://github.com/nullyks/Arduino-sissejuhatus), [füüsika põhiteadmiste](https://github.com/nullyks/Arduino-fyysika-p6hiteadmised) ja [elektroonika baaselementide](https://github.com/nullyks/Arduino-baaselemendid) õppematerjalid.

## Õpieesmärgid
Selle materjali läbimisel õpid juhtima alalisvoolumootoreid MOSFET-transistori ja H-silla abil. Saad teada, mille poolest erinevad servo- ja samm-mootorid ning kus ja kuidas neid kasutada. Kuna mootorid vajavad tavaliselt rohkem voolu, kui Arduino suudab anda, õpid kasutama ka eraldi toiteallikat.

## Õpiväljundid
Materjali edukalt läbinud õppija:
* kirjeldab erinevate mootoritüüpide põhiomadusi;
* selgitab erinevate mootoritüüpide juhtimisviise Arduino UNO arendusplaadiga;
* valib konkreetse ülesande täitmiseks sobiva mootori ja liidestab selle Arduino UNO arendusplaadiga;
* koostab konkreetse ülesande täitmiseks sobiva juhtkoodi vastavalt valitud mootorile.

## Hindamisjuhend

Selle õppematerjali puhul ei rakendata eristavat hindamist.

Õppija on materjali omandanud kui ta on iseseisvalt võimeline lahendama vähemalt neli iseseisvat ülesannet viiest.

## Ohutus

* Koosta või muuda vooluahelat ainult siis, kui Arduino ja mootori väline toiteallikas on välja lülitatud ning lahti ühendatud.
* Enne toite ühendamist kontrolli toitepinget, polaarsust, juhtmete ühendusi ja ühise GND olemasolu.
* Seadista laboritoiteallika voolupiirang enne mootori käivitamist.
* Ära hoia töötava mootori võlli kinni. Blokeeritud mootor võib tarbida väga suurt voolu ning mootor või draiver võib üle kuumeneda.
* Hoia sõrmed, juuksed, riided ja ühendusjuhtmed liikuvatest osadest eemal.
* Lase kuumenenud mootoril või draiveril enne puudutamist jahtuda.

## Vajalikud vahendid
* 1 × personaalarvuti (sobib Windowsi, Linuxi või macOS-iga)
* 1 × Arduino UNO arendusplaat koos sobiva USB kaabliga
* 2 × ühesugust alalisvoolumootorit sobiva nimipinge ja L293D voolupiiriga
* 2 × potentsiomeeter (makettplaadiga ühilduv)
* 2 × nupp (makettplaadiga ühilduv)
* 1 × LED
* 1 × 470 Ω takisti
* 1 × N-kanaliga MOSFET (näiteks: IRLZ44N)
* 1 × mootori voolule ja pingele sobiv kaitsediood (näiteks 1N4001 või 1N5819)
* 1 × 10 kΩ takisti MOSFET-i paisu tõmbetakistiks
* 1 × L293D draiver
* 1 × DRV8833 kahekanaliline mootoridraiveri moodul
* 2 × 0,1 µF keraamiline kondensaator L293D toite möödaviiguks
* 1 × servomootor (näiteks: SG90 või KY66)
* 1 × samm-mootor (näiteks: 28BYJ-48)
* 1 × samm-mootoriga sobiv draiver (näiteks: ULN2003)
* 1 × neljajuhtmeline bipolaarne NEMA 17 samm-mootor (näiteks MT-1701HS140A)
* 1 × A4988 samm-mootori draivermoodul
* 1 × vähemalt 47 µF ja 25 V elektrolüütkondensaator
* 1 × reguleeritava voolupiiranguga laboritoiteallikas, mille väljundpinge on seadistatav vähemalt vahemikus 5–12 V ja vooluandmisvõime vähemalt 2 A
* 1 × nelja 1,5 V AA-patarei hoidik koos patareidega või reguleeritud 5–6 V toiteallikas servomootori jaoks
* 1 × kaugusandur
* 1 × digitaalne multimeeter koos mõõtejuhtmetega
* 1 × makettplaat
* makettplaadi juhtmed (isane-isane)
* makettplaadi juhtmed (emane-isane)

## Õppematerjali osad
* [Alalisvoolumootorid](materjalid/1_Alalisvoolumootorid.md)
* [Servomootorid](materjalid/2_Servomootorid.md)
* [Samm-mootorid](materjalid/3_Samm-mootorid.md)
* [Iseseisvad ülesanded](materjalid/4_iseseisvad_ülesanded.md)

## Õppematerjali koostajad

Tanel Toova (tanel.toova@tlu.ee)
