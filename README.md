## 📜 Licenca  
Ta dela so prosto dostopna za vsako uporabo brez omejitev.  
Avtor ne zahteva atribucije, vendar je hvaležen za povratne informacije.

# 🐣 Projekt v razvoju
___

## Z naslednjimi nastavitvami želim doseči nadzor nad porabo maksimalno dovoljene dogovorjene moči (kW)!

![Sprememba dogovorjene moči](https://github.com/user-attachments/assets/f3fd7e9b-f6cb-4a89-8c5b-d751f2365208)

***
# 🎯 Pojasniti vam moram, da živimo v dvo družinski hiši kjer imamo trenutno 1 odjemno merilno mesto!

***

Najprej sem moral rešiti dilemo glede uporabe električne pečice, pralnega stroja in sušilnega stroja. Z ženo sva se dogovorila 😄, da istočasno ne vklapljava teh naprav, tukaj pride v poštev tudi, friteza, mikrovalovna, fen za lase (opremljena so s pametnimi stikali ali pa vsaj z merilniki porabe) in še kaj se bi našlo.

Glede na to, da imamo prostore ogrevane s pomočjo IR panelov in da vodo greje bojler se mi dozdeva, da te naprave lahko brez večjega vpliva ob špicah izklapljam.


Najprej sem moral rešiti dilemo glede uporabe električne pečice, pralnega stroja in sušilnega stroja. Z ženo sva se dogovorila 😄, da istočasno ne vklapljava teh naprav, tukaj pride v poštev tudi, friteza, mikrovalovna, fen za lase (opremljena so s pametnimi stikali ali pa vsaj z merilniki porabe) in še kaj se bi našlo.

Glede na to, da imamo prostore ogrevane s pomočjo IR panelov in da vodo greje bojler se mi dozdeva, da te naprave lahko brez večjega vpliva ob špicah izklapljam.


# Nekaj podatkov o porabnikih
___
| Naprava        | Poraba |
|----------------|--------|
| Sušilni stroj  | 2,5 kW |
| Pralni stroj   | 1,8 kW |
| Bojler         | 2,0 kW |
| Pečica         | 2,0 kW |
| Mikrovalovna   | 1,8 kW |
| Likalnik       | 2,2 kW |
| Fen za lase    | 2,4 kW |
| IR Nati        | 0,6 kW |
| IR Spalnica    | 1,2 kW |
| IR Di          | 0,7 kW |
___ 

Upoštevana je maksimalna poraba ob zagonu naprave,ki je nekoliko višja koz kasneje nadaljevanj

📊 Ugotovil sem, da je nekje stalna poraba okoli 500 W (hladilnik, skrinja, tv, glavni računalnik ...) tako, da čisto v skrajnost ne mislim iti!
![20250414-Floor plan](https://github.com/user-attachments/assets/b1559d97-85c4-457c-8a4b-35c9410fabea)

⚡ Oziroma je stalna poraba: Internet, mrežno stikalo, mrežni tiskalnik, računalnik (Home assistant in Windows `podatkovni strežnik, domači kino, kamere ...` "na Proxmox") dnevno:
![20250414-Poraba energije](https://github.com/user-attachments/assets/d8856e04-517b-45de-a776-7af279de2e97)


Primer poimenovanja entitet:

| Entiteta  | Pomen                            |
|-----------|----------------------------------|
| Me-Ss     | Merilnik elektrike-Sušilni stroj |
| Me-Ps     | Merilnik elektrike-Pralni stroj  |
| Me-Bo     | Merilnik elektrike-Bojler        |
| Tm-Sp     | Temperaturni merilnik-Spalnica   |

___
### ✨
___
### 🧠 Za avtomatizacijo/nadzor uporabljam dodatek Node-red in ne avtomatizacijo predvsem zaradi boljše preglednosi nad potekom avtomatizacije/nadzora.
***
# 📅 Dodano: 19.04.2025

Poglejmo porabo pralnega stroja (Me-Ps) in sušilnega stroja (Me-Ss):
![20250407-Pralni+Sušilni stroj](https://github.com/user-attachments/assets/c604a964-b618-4238-a35a-8e9dd51487e8)
Prvo je prano belo perilo in nato sušeno zatem je prano pisano perilo in zatemm sušeno.

Pralni stroj (Me-Ps) belo perilo:
![Pralni stroj-Poraba_90](https://github.com/user-attachments/assets/1dff5deb-5487-4282-99c3-9eb0e13c45a8)

Sušilni stroj (Me-Ss):
![Sušilni stroj-Poraba](https://github.com/user-attachments/assets/cdeb01aa-58e2-4df0-8f5f-9fe5e89ee58b)

***
# 📅 Popravljeno: 19.05.2025
Po tehtnem premisleku sem se lotil tudi nadzora nad pralnim in sušilnim strojem zaradi bolj robustnega nadzora:
![20250519-pralni in sušilni stroj + emergency flows](https://github.com/user-attachments/assets/22c5115a-28c5-4e82-bd3c-b8688c2b6c65)


✍️ Koda v nod-red za prenos: 
[20250519-pralni in sušilni stroj + emergency flows.zip](https://github.com/user-attachments/files/20280628/20250519-pralni.in.susilni.stroj.%2B.emergency.flows.zip)

___
Ta zavihek vsebuje dva toka (flow-a). Čisto zgoraj je tok, ki skrbi za nadzor delovanja nad vsemi napravami gled na njihovo porabo in skupno porabo (opisano kasnjeje podrobneje), spodnji tok pa skrbi za delovanje pralnega in sušilnega stroja s pomočjo zgornjega toka, ki skrbi za delovanje tudi drugih naprav, ki jih bom opisal kasneje
___
# 🔌 1.) Emergency Power Management Flow - Popoln opis

## 🌟 Opis flow-a

Ta flow v Node-RED nenehno spremlja porabo električne energije na **fazi 3** preko senzorja `p1_meter_power_phase_3` in aktivira emergency protokol, ko poraba preseže nastavljeno mejo.

---

## 📊 Komponente flow-a

### 1. `server-state-changed` node
- 👁️ Sledi spremembam vrednosti senzorja `p1_meter_power_phase_3`
- 🔄 Pošilja podatke samo ob spremembi vrednosti
- 📦 Nastavi:
  - `msg.payload` = trenutna poraba (W)
  - `msg.topic` = `"sensor.p1_meter_power_phase_3"`

### 2. `function` node: *Upravljanje napajanja v sili*
- 🧠 Glavna logična enota za emergency upravljanje
- ⚠️ Implementira:
  - Sledenje porabi v realnem času
  - Prioritiziran sistem izklopov
  - Zaščitne mehanizme

### 3. `server` node
- 🏠 Povezava s Home Assistant sistemom
- 🔗 Omogoča integracijo z ostalimi pametnimi napravami

---

## ⚙️ Podrobnosti delovanja

### 🔄 Prioritetno zaporedje izklopov

```javascript
const emergencySequence = [
    'bojler',     // ♨️ Najmanj kritična naprava
    'irNa',       // 🌞 IR panel Nathalie
    'irSp',       // 🛏️ IR panel spalnica
    'irDi',       // 👧 IR panel Diane
    'susilni',    // 🔥 Sušilni stroj
    'pralni'      // 🧺 Pralni stroj (najbolj kritičen)
];
```
---

## 🛡️ Varnostni mehanizmi

- ⏱️ **5-sekundni zaščitni zamik** med zaporednimi izklopi  
- 🧪 **Preverjanje svežosti podatkov (timeout 10s):**

```javascript
if (currentTime - global.get('last_phase3_update') > 10000) {
    node.warn("⚠️ OPOZORILO: Zastareli podatki!");
}
```
---

## 📑 Obsežno logiranje vseh dogodkov

Sistem zapisuje vsak dogodek, ki vključuje:
- sprožitev ali ponastavitev emergency režima
- izklop posamezne naprave
- opozorila o zastarelih podatkih

---

## 📤 Primeri izhodnih sporočil

### 🚨 Emergency režim

```json
{
  "event": "EMERGENCY_TRIGGERED",
  "device": "bojler",
  "power": 4820,
  "timestamp": "2023-11-15T14:23:45Z"
}
```
### ✅ Normalno stanje

```json
{
  "event": "POWER_NORMALIZED",
  "power": 4200,
  "timestamp": "2023-11-15T14:25:30Z"
}
```
## 🛠️ Konfiguracija

| Parameter              | Vrednost | Opis                           |
|------------------------|----------|--------------------------------|
| `max_dovoljena_poraba` | 4650 W   | Nastavljiva meja               |
| `zaščitni_zamik`       | 5 s      | Časovni zamik med izklopi      |
| `timeout_podatkov`     | 10 s     | Preverjanje svežosti podatkov  |

---

## 🌈 Delovni primer

1. Sistem zazna porabo **4800W** (> 4650W)
2. Izklopi ♨️ **bojler** (prvi v zaporedju)
3. Če po **5 sekundah** poraba še vedno presežena:
   - Izklopi 🌞 **IR panel Nathalie**
4. Ko poraba pade pod mejo:
   - ✅ Ponastavi vse *emergency flag-e*
   - 📝 Zabeleži dogodek v dnevnik
___

___
# 🌀 2.) Flow: Avtomatsko upravljanje pralnega in sušilnega stroja

## 🔍 Kratek opis

Ta Node-RED flow skrbi za pametno upravljanje **pralnega** in **sušilnega stroja**, glede na:

- trenutno **porabo elektrike**
- stanje naprav (vklop/izklop)
- in **emergency logiko**, če pride do presežene skupne porabe (glede na fazo 3)

Cilj sistema je:
- preprečiti preobremenitev električnega omrežja
- avtomatsko nadzorovati naprave
- **obveščati uporabnike** preko mobilne aplikacije Home Assistant companion če sta prisotnosti osebi Mojca ali Robert ob vklopu ali izklopu naprave

---

## 🧠 Glavne komponente

### 🔧 Glavna funkcija (`function` node: "Funkcija PSs")

Ta vozlišče sprejema podatke iz senzorjev porabe in stanja naprav ter izvaja naslednje logike:

- spremlja porabo pralnega in sušilnega stroja
- beleži stanje (vklop/izklop) pametnega stikala naprav
- izračuna trenutno porabo faze 3
- primerja s skupnim dovoljenim limitom (`max_dovoljena_poraba`)
- izvaja:
  - **emergency izklop** (če presežena moč)
  - **avtomatski vklop**, če je na voljo dovolj moči preko prej omenjene funkcije

🧮 Primer konfiguracije:

```javascript
const maxPoraba = parseFloat(global.get('max_dovoljena_poraba')) || 4650;
```

---

## 🖥️ Spremljanje stanj

### Entitete, ki sprožijo flow:

- `sensor.me_prst_current_consumption` (poraba pralnega stroja)
- `sensor.me_ss_current_consumption` (poraba sušilnega stroja)
- `switch.me_ps` (stanje pametnega stikala pralnega stroja)
- `switch.me_ss_switch_0` (stanje pametnega stikala sušilnega stroja)

Vse zgornje entitete so povezane z `server-state-changed` vozlišči in priklopljene na glavno funkcijsko vozlišče.

---

## 🔁 Emergency logika

- Če je `global.get('emergency_pralni_off') == true`, se pralni stroj **takoj izklopi**
- Enako za `emergency_susilni_off` in sušilni stroj- Za izklop skrbi zgornji flow
- Po izklopu se preveri, ali je uporabnik (Mojca ali Robert) doma
- Če je prisoten, se mu pošlje **obvestilo preko mobilne aplikacije Home assistant companion**

📦 Obvestilo:

```json
{
  "message": "Sušilni stroj se je IZKLOPIL. Prekoračena poraba",
  "title": "Obvestilo: Sušilni stroj"
}
```

---

## 🔔 Obvestila uporabnikom

Obvestila se pošiljajo preko:

- `notify.mobile_app_mojca_mobitel`
- `notify.mobile_app_robert_mobitel`

### Vsebina vključuje:

- **naslov obvestila** (npr. “Obvestilo: Pralni stroj”)
- **sporočilo** z dodatnimi podatki o porabi
- **akcijski gumb**: `Videl sem 👍`
- različno pomembnost glede na uporabnika (Robert dobi alarm)

---

## 🔄 Avtomatski vklopi

Če skupna poraba pade **pod dovoljeno mejo**, sistem:

1. ponastavi emergency zastavice
2. preveri stanje naprav
3. po potrebi **vklopi najprej pralni stroj**, če je na voljo ≥ 1920 W + rezerva
4. nato šele sušilni stroj, če je na voljo ≥ 2500 W + rezerva

📥 Primer logike za vklop pralnega stroja:

```javascript
if (pralniStanje === 'off' && !pralniStatus.includes('AKTIVEN') && prostaMoc >= (1920 + rezerva)) {
    return [null, null, { payload: "on" }, null];
}
```

---

## ♻️ Osveževanje podatkov

Flow vključuje tudi `inject` node, ki vsakih **5 sekund** pošlje `force_refresh` sporočilo, da se ponovno prebere trenutna vrednost iz `global.get('phase3')`.

---

## 💾 Pomožne spremenljivke

- `global.phase3` – trenutno stanje porabe na fazi 3
- `flow.pralni_poraba` in `flow.susilni_poraba` – poraba posameznih naprav
- `flow.pralni_stanje`, `flow.susilni_stanje` – stanja naprav
- `global.last_phase3_update` – čas zadnje osvežitve
- `global.emergency_pralni_off`, `global.emergency_susilni_off` – emergency zastavice

---

## 📋 Povzetek v obliki tabele

| Komponenta            | Opis                                                 |
|-----------------------|------------------------------------------------------|
| `switch.me_ps`        | Stikalo pralnega stroja                              |
| `switch.me_ss`        | Stikalo sušilnega stroja                             |
| `sensor.me_prst...`   | Poraba pralnega stroja                               |
| `sensor.me_ss...`     | Poraba sušilnega stroja                              |
| `function` node       | Glavna logika za izklop/vklop/emergency obdelavo    |
| `inject` node         | Periodično proži osvežitev                           |
| `notify.*`            | Pošiljanje obvestil uporabnikom                     |
| `current-state` node  | Preverjanje ali sta Mojca ali Robert doma           |

---

## 🧪 Primer poteka

1. Poraba naraste nad 4650 W
2. Flow izklopi sušilni stroj
3. Če je kdo doma (Mojca/Robert) → prejme obvestilo
4. Ko poraba pade:
   - sistem preveri, koliko moči je na voljo
   - najprej vklopi pralni, nato še sušilni stroj
   - pošlje obvestilo o ponovnem vklopu

---

## 🔚 Zaključek

Ta flow omogoča **dinamično in varno upravljanje gospodinjskih aparatov**, prilagojeno prisotnosti stanovalcev in trenutnim energetskim razmeram. Primeren je za okolja, kjer je **omejena priključna moč** in je potrebno pazljivo načrtovanje vklopov večjih porabnikov.

---

## 🖥️ Spremljanje stanj

### Entitete, ki sprožijo flow:

- `sensor.me_prst_current_consumption` – poraba pralnega stroja  
- `sensor.me_ss_current_consumption` – poraba sušilnega stroja  
- `switch.me_ps` – stanje pralnega stroja  
- `switch.me_ss_switch_0` – stanje sušilnega stroja  

Vse zgornje entitete so povezane z `server-state-changed` vozlišči in priklopljene na glavno funkcijsko vozlišče.

---

## 🔁 Emergency logika

- Če je `global.get('emergency_pralni_off') == true`, se pralni stroj **takoj izklopi**
- Enako velja za `emergency_susilni_off` in sušilni stroj
- Po izklopu se preveri, ali je uporabnik (Mojca ali Robert) doma
- Če je prisoten, se mu pošlje **obvestilo preko mobilne aplikacije**

📦 Obvestilo:

```json
{
  "message": "Sušilni stroj se je IZKLOPIL. Prekoračena poraba",
  "title": "Obvestilo: Sušilni stroj"
}
```

---

## 🔔 Obvestila uporabnikom

Obvestila se pošiljajo preko:

- `notify.mobile_app_mojca_mobitel`
- `notify.mobile_app_robert_mobitel`

### Vsebina vključuje:

- naslov obvestila (npr. “Obvestilo: Pralni stroj”)
- sporočilo z dodatnimi podatki o porabi
- akcijski gumb: **Videl sem 👍**
- različno pomembnost glede na uporabnika (Robert dobi alarmni kanal)

---

## 🔄 Avtomatski vklopi

Če skupna poraba pade **pod dovoljeno mejo**, sistem:

1. ponastavi emergency zastavice  
2. preveri stanje naprav  
3. po potrebi vklopi **najprej pralni stroj**, če je na voljo ≥ 1920 W + rezerva  
4. nato še **sušilni stroj**, če je na voljo ≥ 2500 W + rezerva  

📥 Primer logike za vklop pralnega stroja:

```javascript
if (pralniStanje === 'off' && !pralniStatus.includes('AKTIVEN') && prostaMoc >= (1920 + rezerva)) {
    return [null, null, { payload: "on" }, null];
}
```

---

## ♻️ Osveževanje podatkov

Flow vključuje tudi `inject` node, ki vsakih **5 sekund** pošlje `force_refresh` sporočilo, s katerim se ponovno prebere trenutna vrednost iz `global.get('phase3')`.

---

## 💾 Pomožne spremenljivke

- `global.phase3` – trenutno stanje porabe na fazi 3  
- `flow.pralni_poraba`, `flow.susilni_poraba` – poraba posameznih naprav  
- `flow.pralni_stanje`, `flow.susilni_stanje` – stanja naprav  
- `global.last_phase3_update` – čas zadnje osvežitve  
- `global.emergency_pralni_off`, `global.emergency_susilni_off` – emergency zastavice  

---

## 📋 Povzetek v obliki tabele

| Komponenta           | Opis                                              |
|----------------------|---------------------------------------------------|
| `switch.me_ps`       | Stikalo pralnega stroja                           |
| `switch.me_ss`       | Stikalo sušilnega stroja                          |
| `sensor.me_prst...`  | Poraba pralnega stroja                            |
| `sensor.me_ss...`    | Poraba sušilnega stroja                           |
| `function` node      | Glavna logika za izklop/vklop/emergency obdelavo |
| `inject` node        | Periodično proži osvežitev                        |
| `notify.*`           | Pošiljanje obvestil uporabnikom                  |
| `current-state` node | Preverjanje ali sta Mojca ali Robert doma        |

---

## 🧪 Primer poteka

1. Poraba naraste nad **4650 W**
2. Flow izklopi **sušilni stroj**
3. Če je kdo doma (Mojca/Robert) → prejme **obvestilo**
4. Ko poraba pade:
   - sistem preveri, koliko moči je na voljo
   - najprej vklopi **pralni**, nato še **sušilni stroj**
   - pošlje obvestilo o ponovnem vklopu

---

## 🔚 Zaključek

Ta flow omogoča **dinamično in varno upravljanje gospodinjskih aparatov**, prilagojeno prisotnosti stanovalcev in trenutnim energetskim razmeram. Primeren je za okolja, kjer je **omejena priključna moč** in je potrebno pazljivo načrtovanje vklopov večjih porabnikov.




***

# 📅 Popravljeno: 08.05.2025
💡 Primer za bojler ki ima najnižjo prioriteto delovanja (se prvi izklaplja).
![Bojler](https://github.com/user-attachments/assets/c5e84dd2-7bfe-4405-970b-0ad3444eb953)



✍️ Koda v nod-red za prenos: 
[20250508-Bojler flows.zip](https://github.com/user-attachments/files/20099574/20250508-Bojler.flows.zip)

// Funkcijska koda za upravljanje bojlerja v Node-RED okolju

## Namen
Koda upravlja stanje električnega bojlerja na podlagi trenutne porabe energije in stanja faze 3. Glavni cilj je optimizirana poraba energije in preprečevanje preobremenitve.

## Vhodni podatki
- `msg.payload`: Vrednost porabe energije (v W) ali stanje stikala
- `msg.topic`: Identifikator vira podatkov:
  - 'sensor.me_bo_current_consumption' - poraba bojlerja
  - 'sensor.p1_meter_power_phase_3' - poraba faze 3
  - 'switch.me_bo' - stanje stikala bojlerja

## Delovanje

### 1. Shranjevanje vrednosti
- Vrednosti se shranjujejo v globalne spremenljivke glede na topic:
  - 'mebo' in 'boilerPower' - trenutna poraba bojlerja
  - 'phase3' - poraba faze 3
  - 'boilerSwitchState' - stanje stikala ('on'/'off')

### 2. Določanje statusa bojlerja
- Če je stikalo izklopljeno ('off'), je status 'off'
- Če je stikalo vklopljeno, je status:
  - 'AKTIVEN' če poraba presega 100W
  - 'neaktiven' če poraba je ≤ 100W

### 3. Debug izpis
- Izpisuje podrobno stanje sistema v konzolo z vizualnimi indikatorji:
  - Stanje stikala
  - Porabo bojlerja
  - Status bojlerja
  - Skupno porabo faze 3

### 4. Logika upravljanja
- **Vklop bojlerja**: Če je poraba faze 3 ≤ 2100W in je stikalo izklopljeno
- **Izklop bojlerja**: Če je poraba faze 3 > 4650W in je stikalo vklopljeno

## Izhodi
- Prvi izhod: Pošlje ukaz 'on' za vklop bojlerja
- Drugi izhod: Pošlje ukaz 'off' za izklop bojlerja
- Če pogoji niso izpolnjeni, vrne [null, null]

## Varnostne meje
- Varna poraba faze 3: ≤ 4650W (izklop ob presežku)
- Optimalni pogoji za vklop: ≤ 2100W

___

# 📅 Popravljeno: 05.05.2025
💡 Primer za IR panel (ogrevanje):

![IR spalnica](https://github.com/user-attachments/assets/230c9dff-ea58-4fca-baff-012f2932f154)

✍️ Koda v nod-red za prenos: 
[20250508-IR Spalnica flows.zip](https://github.com/user-attachments/files/20099676/20250508-IR.Spalnica.flows.zip)

// Funkcijska koda za upravljanje IR ogrevanja v spalnici v Node-RED okolju

## Namen
Koda avtomatsko upravlja IR panele v spalnici na podlagi temperaturnega komforta, prisotnosti, stanja prostora in porabe energije, pri čemer upošteva tudi stanje bojlerja.

## Vhodni podatki
- `msg.payload`: Vrednost senzorjev ali stanje stikala
- `msg.topic`: Identifikator vira podatkov:
  - 'sensor.tm_sp_current_consumption' - poraba IR panelov
  - 'sensor.p1_meter_power_phase_3' - poraba faze 3
  - 'sensor.povprecje_temperature_spalnica' - temperatura v spalnici
  - 'binary_sensor.tpl_occupancy' - prisotnost v spalnici
  - 'sensor.so_sp_st' - stanje oken
  - 'binary_sensor.sv_sp_door' - stanje vrat
  - 'switch.tm_sp' - stanje stikala IR

## Delovanje

### 1. Shranjevanje vrednosti
- Vrednosti se shranjujejo v globalne spremenljivke:
  - 'irPoraba_sp' - poraba IR panelov
  - 'phase3' - poraba faze 3
  - 'temp_sp' - temperatura v spalnici
  - 'prisotnost_sp' - prisotnost v prostoru
  - 'okno_sp' - stanje oken
  - 'vrata_sp' - stanje vrat
  - 'irSwitchState_sp' - stanje stikala IR

### 2. Določanje statusa IR panelov
- Če je stikalo izklopljeno ('off'), je status 'off'
- Če je stikalo vklopljeno, je status:
  - 'AKTIVEN' če poraba presega 100W
  - 'neaktiven' če poraba je ≤ 100W

### 3. Debug izpis
- Podroben izpis stanja sistema z vizualnimi indikatorji:
  - Temperatura in meja 21°C
  - Prisotnost v prostoru
  - Stanje oken in vrat
  - Poraba energije (faza 3 in IR paneli)
  - Stanje bojlerja

### 4. Glavna logika upravljanja

#### Pogoji za vklop IR:
- Temperatura ≤ (21°C - histereza 0.5°C) ALI (zadnja temp ≤ 21°C in trenutna ≤ 20°C)
- Prisotnost v prostoru
- Zaprta okna in vrata
- Poraba faze 3 ≤ 2900W
- Predvidena skupna poraba (trenutna + 1250W IR) ≤ 4650W

#### Pogoji za izklop IR:
- Neizpolnjeni pogoji za vklop ALI
- Presežena moč faze 3 (>4650W)

#### Nadziranje moči:
- Če je IR aktiven in presežena moč, izklopi bojler

## Izhodi
- Prvi izhod: Vklop IR panelov (payload: "on")
- Drugi izhod: Izklop IR panelov (payload: "off")
- Tretji izhod: Ukaz za izklop bojlerja (Home Assistant service call)

## Varnostne meje
- Maksimalna varna poraba faze 3: 4650W
- Mejna temperatura za ogrevanje: 21°C s histerezo 0.5°C
- Maksimalna poraba IR panelov: 1250W

___
___
# 📅 Dodano: 17.04.2025

___

## Nekaj statistike in primerjave

Na spletni strani mojega elektra lahko po 1 dneh vidim:
![20250414-Povprečje 15 minut-ELEKTRO](https://github.com/user-attachments/assets/c7fec68a-ef59-4c2a-b494-2ce900f5d769)


V Home assistant:
![20250414-Povprečje 15 minut](https://github.com/user-attachments/assets/4e08c265-52c7-46f0-9800-cbc4963675a7)

Da v Home assistant sproti vidim nekaj približno podobnega sem moral narediti nekaj novih entitet:

Če ste do sedaj že spremljali moji objavi https://github.com/Trzinka/Home_Assistant-Obracun_porabe_elektricne_energije_po_novem-2025 in https://github.com/Trzinka/Home_Assistant-Obracun_porabe_elektricne_energije_po_novem-2025_Prikaz_porabe_za_prejsnji_mesec potem poznate strukturo mojega korenskega imenika.

✍️ Torej v mapi `share` in v njeni podmapi `sensors` naredite datoteko `15_minut.yaml` in v njo dodajte:
```yaml
- platform: statistics
  name: "Povprečna moč (15 min)-skupaj"
  unique_id: sensor_povprecna_moc_15min_skupaj
  entity_id: sensor.p1_meter_power
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300

- platform: statistics
  name: "Povprečna moč (15 min)-faza 1"
  unique_id: sensor_povprecna_moc_15min_faza1
  entity_id: sensor.p1_meter_power_phase_1
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300

- platform: statistics
  name: "Povprečna moč (15 min)-faza 2"
  unique_id: sensor_povprecna_moc_15min_faza2
  entity_id: sensor.p1_meter_power_phase_2
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300

- platform: statistics
  name: "Povprečna moč (15 min)-faza 3"
  unique_id: sensor_povprecna_moc_15min_faza3
  entity_id: sensor.p1_meter_power_phase_3
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300
#################################################
- platform: statistics
  name: "ME BO - Povprečna moč (15 min)"
  unique_id: sensor_me_bo_avg_power_15min
  entity_id: sensor.me_bo_current_consumption
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300

- platform: statistics
  name: "ME PRST - Povprečna moč (15 min)"
  unique_id: sensor_me_prst_avg_power_15min
  entity_id: sensor.me_prst_current_consumption
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300

- platform: statistics
  name: "ME SS - Povprečna moč (15 min)"
  unique_id: sensor_me_ss_avg_power_15min
  entity_id: sensor.me_ss_current_consumption
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300

- platform: statistics
  name: "ME KL - Povprečna moč (15 min)"
  unique_id: sensor_me_kl_avg_power_15min
  entity_id: sensor.me_kl_current_consumption
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300

- platform: statistics
  name: "TM DI - Povprečna moč (15 min)"
  entity_id: sensor.tm_di_current_consumption
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300
  unique_id: b6911e7b-49ec-40f5-ad48-dba6d604b9c6

- platform: statistics
  name: "TM NA - Povprečna moč (15 min)"
  entity_id: sensor.tm_na_current_consumption
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300
  unique_id: 523d8e47-2812-4e37-8325-431ca9d2f9d2

- platform: statistics
  name: "TM SP - Povprečna moč (15 min)"
  entity_id: sensor.tm_sp_current_consumption
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300
  unique_id: 45c0a4eb-749a-4600-939d-4c4c16756525

- platform: statistics
  name: "ME JE POD KLIMO - Povprečna moč (15 min)"
  entity_id: sensor.me_je_pod_klimo_current_consumption
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300
  unique_id: 3ef4acea-93ed-4745-9904-a2b125f4e520
```
s tem ste ustvarili entitete, ki v časovnem obdobju 15 minut izračuna povprečje porabe v W

✍️ Sedaj je potrebno v datoteki `template.yaml` v korenskem imeniku vpisati:
```yaml
- sensor:
  - name: "Povprečna moč 15 min (kW)-Skupaj"
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.povprecna_moc_15_min_skupaj') | float(0) / 1000) | round(2) }}
    unique_id: 6b269620-acc4-46b9-851e-823163bb96cd

  - name: "Povprečna moč 15 min (kW)-Faza 1"
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.povprecna_moc_15_min_faza_1') | float(0) / 1000) | round(2) }}
    unique_id: 84a65eba-4ef0-4777-b85c-9b578e987b97

  - name: "Povprečna moč 15 min (kW)-Faza 2"
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.povprecna_moc_15_min_faza_2') | float(0) / 1000) | round(2) }}
    unique_id: aac240b8-db54-4767-b807-5a00cd29ac5b

  - name: "Povprečna moč 15 min (kW)-Faza 3"
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.povprecna_moc_15_min_faza_3') | float(0) / 1000) | round(2) }}
    unique_id: 604cc278-4b73-48e6-9556-ec9a17f4a7bf
################################################################################################################
- sensor:
  - name: "ME BO - Povprečna moč 15 min (kW)"
    unique_id: sensor_me_bo_avg_power_15min_kw
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.me_bo_povprecna_moc_15_min') | float(0) / 1000) | round(2) }}

  - name: "ME PRST - Povprečna moč 15 min (kW)"
    unique_id: sensor_me_prst_avg_power_15min_kw
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.me_prst_povprecna_moc_15_min') | float(0) / 1000) | round(2) }}

  - name: "ME SS - Povprečna moč 15 min (kW)"
    unique_id: sensor_me_ss_avg_power_15min_kw
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.me_ss_povprecna_moc_15_min') | float(0) / 1000) | round(2) }}

  - name: "ME KL - Povprečna moč 15 min (kW)"
    unique_id: sensor_me_kl_avg_power_15min_kw
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.me_kl_povprecna_moc_15_min') | float(0) / 1000) | round(2) }}

  - name: "TM DI - Povprečna moč 15 min (kW)"
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.tm_di_povprecna_moc_15_min') | float(0) / 1000) | round(2) }} 
    unique_id: 972853ae-c019-41e8-8ba8-d1501cd54f15

  - name: "TM NA - Povprečna moč 15 min (kW)"
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.tm_na_povprecna_moc_15_min') | float(0) / 1000) | round(2) }} 
    unique_id: 4ec94641-d1bd-468a-be71-52488353e287

  - name: "TM SP - Povprečna moč 15 min (kW)"
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.tm_sp_povprecna_moc_15_min') | float(0) / 1000) | round(2) }} 
    unique_id: 831e2ca6-37cd-4289-a2af-4faf07a3dab9

  - name: "ME JE POD KLIMO - Povprečna moč 15 min (kW)"
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.me_je_pod_klimo_povprecna_moc_15_min') | float(0) / 1000) | round(2) }} 
    unique_id: 06520c72-9a7f-4743-bcb8-824f10e51595
```

✍️ in še v `automations.yaml`
```yaml
- id: posodobi_15_min_template_senzorje
  alias: Posodobi 15-minutne template senzorje
  triggers:
    - platform: time_pattern
      minutes: "/15"
  actions:
    - service: homeassistant.update_entity
      target:
        entity_id:
          - sensor.povprecna_moc_15_min_kw_skupaj
          - sensor.povprecna_moc_15_min_kw_faza_1
          - sensor.povprecna_moc_15_min_kw_faza_2
          - sensor.povprecna_moc_15_min_kw_faza_3
          - sensor.tm_na_povprecna_moc_15_min
          - sensor.tm_di_povprecna_moc_15_min
          - sensor.tm_sp_povprecna_moc_15_min
          - sensor.me_ss_avg_power_15min_kw
          - sensor.me_bo_avg_power_15min_kw
          - sensor.me_kl_avg_power_15min_kw
          - sensor.me_prst_avg_power_15min_kw
          - sensor.me_je_pod_klimo_povprecna_moc_15_min
  mode: single
```
s tem ustvarimo entitete v kW in avtomatizacijo zbiranja na 15 minut, ki jo uporabljam v grafu (zgodovinskem izpisu). 
Opazili boste, da prikazani podatki niso ravno na 15 minut, a so dovolj blizu za uporabo!

***


## 🧐 Čeprav bi 15 minutno povprečje moralo izgledati nekaj podobno temu, ker se primerjalni rezultat z Elektrom najbolj približa:
![20250414-Snapshot-Povprečje 15 minut](https://github.com/user-attachments/assets/5d8ee82c-8a1c-4002-b7f0-3ab7df0a71ac)

![20250414-Povprečje 15 minut-ELEKTRO](https://github.com/user-attachments/assets/e24c1654-b353-4697-b013-b6e72ac4b94b)


✍️ Da bi dobili podatke kot jih prikazuje zgornja slika v datoteki `template.yaml` v korenskem imeniku vpišite:
```yaml
##################################################################################################################
- sensor:
  - name: "Snapshot Povprečna moč 15 min (kW) - Skupaj"
    unique_id: sensor_snapshot_povprecna_moc_15_min_kw_skupaj
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_povprecna_moc_15_min_kw_skupaj') | float(0) }}

  - name: "Snapshot Povprečna moč 15 min (kW) - Faza 1"
    unique_id: sensor_snapshot_povprecna_moc_15_min_kw_faza_1
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_povprecna_moc_15_min_kw_faza_1') | float(0) }}

  - name: "Snapshot Povprečna moč 15 min (kW) - Faza 2"
    unique_id: sensor_snapshot_povprecna_moc_15_min_kw_faza_2
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_povprecna_moc_15_min_kw_faza_2') | float(0) }}

  - name: "Snapshot Povprečna moč 15 min (kW) - Faza 3"
    unique_id: sensor_snapshot_povprecna_moc_15_min_kw_faza_3
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_povprecna_moc_15_min_kw_faza_3') | float(0) }}

  - name: "Snapshot TM NA - Povprečna moč 15 min (kW)"
    unique_id: sensor_snapshot_tm_na_povprecna_moc_15_min
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_tm_na_povprecna_moc_15_min') | float(0) }}

  - name: "Snapshot TM DI - Povprečna moč 15 min (kW)"
    unique_id: sensor_snapshot_tm_di_povprecna_moc_15_min
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_tm_di_povprecna_moc_15_min') | float(0) }}

  - name: "Snapshot TM SP - Povprečna moč 15 min (kW)"
    unique_id: sensor_snapshot_tm_sp_povprecna_moc_15_min
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_tm_sp_povprecna_moc_15_min') | float(0) }}

  - name: "Snapshot ME SS - Povprečna moč 15 min (kW)"
    unique_id: sensor_snapshot_me_ss_avg_power_15min_kw
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_me_ss_avg_power_15min_kw') | float(0) }}

  - name: "Snapshot ME BO - Povprečna moč 15 min (kW)"
    unique_id: sensor_snapshot_me_bo_avg_power_15min_kw
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_me_bo_avg_power_15min_kw') | float(0) }}

  - name: "Snapshot ME KL - Povprečna moč 15 min (kW)"
    unique_id: sensor_snapshot_me_kl_avg_power_15min_kw
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_me_kl_avg_power_15min_kw') | float(0) }}

  - name: "Snapshot ME PRST - Povprečna moč 15 min (kW)"
    unique_id: sensor_snapshot_me_prst_avg_power_15min_kw
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_me_prst_avg_power_15min_kw') | float(0) }}

  - name: "Snapshot ME JE POD KLIMO - Povprečna moč 15 min (kW)"
    unique_id: sensor_snapshot_me_je_pod_klimo_povprecna_moc_15_min
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_me_je_pod_klimo_povprecna_moc_15_min') | float(0) }}
```

✍️ Poleg tega je potrebno v `configuration.yaml` dodati:
```yaml
input_number:
  snapshot_povprecna_moc_15min_skupaj:
    name: Snapshot - Povprečna moč 15 min (kW) - Skupaj
    unit_of_measurement: "kW"
    min: 0
    max: 20
    step: 0.01

  snapshot_povprecna_moc_15min_faza_1:
    name: Snapshot - Povprečna moč 15 min (kW) - Faza 1
    unit_of_measurement: "kW"
    min: 0
    max: 20
    step: 0.01

  snapshot_povprecna_moc_15min_faza_2:
    name: Snapshot - Povprečna moč 15 min (kW) - Faza 2
    unit_of_measurement: "kW"
    min: 0
    max: 20
    step: 0.01

  snapshot_povprecna_moc_15min_faza_3:
    name: Snapshot - Povprečna moč 15 min (kW) - Faza 3
    unit_of_measurement: "kW"
    min: 0
    max: 20
    step: 0.01

  snapshot_me_bo:
    name: Snapshot - ME BO
    unit_of_measurement: "kW"
    min: 0
    max: 10
    step: 0.01

  snapshot_me_prst:
    name: Snapshot - ME PRST
    unit_of_measurement: "kW"
    min: 0
    max: 10
    step: 0.01

  snapshot_me_ss:
    name: Snapshot - ME SS
    unit_of_measurement: "kW"
    min: 0
    max: 10
    step: 0.01

  snapshot_me_kl:
    name: Snapshot - ME KL
    unit_of_measurement: "kW"
    min: 0
    max: 10
    step: 0.01

  snapshot_tm_di:
    name: Snapshot - TM DI
    unit_of_measurement: "kW"
    min: 0
    max: 10
    step: 0.01

  snapshot_tm_na:
    name: Snapshot - TM NA
    unit_of_measurement: "kW"
    min: 0
    max: 10
    step: 0.01

  snapshot_tm_sp:
    name: Snapshot - TM SP
    unit_of_measurement: "kW"
    min: 0
    max: 10
    step: 0.01

  snapshot_me_je_pod_klimo:
    name: Snapshot - ME JE POD KLIMO
    unit_of_measurement: "kW"
    min: 0
    max: 10
    step: 0.01
```

✍️ in še v `automations.yaml`
```yaml
- id: shrani_15_min_snapshot
  alias: Shrani 15-minutni snapshot povprečne moči
  trigger:
    - platform: time_pattern
      minutes: "/15"
  action:
    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_povprecna_moc_15min_skupaj
      data:
        value: "{{ states('sensor.povprecna_moc_15_min_kw_skupaj') | float(0) }}"

    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_povprecna_moc_15min_faza_1
      data:
        value: "{{ states('sensor.povprecna_moc_15_min_kw_faza_1') | float(0) }}"

    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_povprecna_moc_15min_faza_2
      data:
        value: "{{ states('sensor.povprecna_moc_15_min_kw_faza_2') | float(0) }}"

    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_povprecna_moc_15min_faza_3
      data:
        value: "{{ states('sensor.povprecna_moc_15_min_kw_faza_3') | float(0) }}"

    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_me_bo
      data:
        value: "{{ states('sensor.me_bo_avg_power_15min_kw') | float(0) }}"

    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_me_prst
      data:
        value: "{{ states('sensor.me_prst_avg_power_15min_kw') | float(0) }}"

    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_me_ss
      data:
        value: "{{ states('sensor.me_ss_avg_power_15min_kw') | float(0) }}"

    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_me_kl
      data:
        value: "{{ states('sensor.me_kl_avg_power_15min_kw') | float(0) }}"

    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_tm_di
      data:
        value: "{{ states('sensor.tm_di_povprecna_moc_15_min') | float(0) }}"

    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_tm_na
      data:
        value: "{{ states('sensor.tm_na_povprecna_moc_15_min') | float(0) }}"

    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_tm_sp
      data:
        value: "{{ states('sensor.tm_sp_povprecna_moc_15_min') | float(0) }}"

    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_me_je_pod_klimo
      data:
        value: "{{ states('sensor.me_je_pod_klimo_povprecna_moc_15_min') | float(0) }}"
  mode: single
```

***



# 📅 Dodano: 20.04.2025

Takšen je izgled porabe:
![Poraba od 14 do 20 04 2025](https://github.com/user-attachments/assets/f8a83c1c-ffaf-4ffa-8a8d-c225ce593639)

## Kodo za Nod-red sem naredil s pomočjo deepseek, ki se je izkazal za veliko učinkovitejšega od ChatGPT!

🤝 Kako sva sodelovala s deepseek
Ta projekt je rezultat intenzivnega 7-dnevnega sodelovanja med človekom in AI, ki dokazuje, da so kompleksne rešitve dostopne tudi brez predhodnega programerskega znanja:

Ključni elementi uspeha
Komunikacija:
Natančni opisi problema v slovenščini
Iterativno izboljšanje skozi 400+ vzorcev dialoga

Delitev vlog:
Jaz: Domensko znanje (energetika, slovenski modeli) + vizualizacije
Ai: Prevajanje zahtev v kodo + debugiranje

Učni proces:
Diagram:
![deepseek_0](https://github.com/user-attachments/assets/edad3b07-da2d-4e34-8fcd-a385f501c186)


Zakaj je to pomembno?
✨ Dokaz, da lahko lokalne rešitve ustvarjajo neprogramerji
🌍 Model za sodelovanje človek-AI v manj zastopanih jezikih
⚡ Navdih za energetsko avtomatizacijo v drugih regijah


