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

Koda funkcije:
``` javascript
// === Upravljanje napajanja v sili ===
const phase3 = parseFloat(msg.payload) || 0; // Pridobi podatke iz vhodnega noda in jih shrani v phase3.
const lastEmergencyTime = flow.get('lastEmergencyTime') || 0;
const currentTime = Date.now();

// NASTAVITEV IN POSODOBITEV GLOBALNE SPREMENLJIVKE (ker je to edini vir podatkov)
global.set('phase3', phase3);  // Nastavitev globalnega objekta pod imenom phase3
global.set('last_phase3_update', currentTime);  // Za sledenje svežini podatkov
global.set('max_dovoljena_poraba', 4650); // Privzeta vrednost za maksimalno dovoljeno energijo

// Določi prioritetno zaporedje izklopov
const emergencySequence = [
    'bojler',
    'irNa',
    'irSp', 
    'irDi',
    'susilni',
    'pralni'
];

// Preveri svežost podatkov
if (currentTime - (global.get('last_phase3_update') || 0) > 10000) {
    node.warn("⚠️ OPOZORILO: Podatki phase3 niso sveži!");
}
// Nastavitev konstante za maksimalno dovoljeno energijo
const maxPoraba = parseFloat(global.get('max_dovoljena_poraba')) || 4650; 
// Pridobi VSE potrebne gobalne vrednosti in status iz drugih zavihkov in flowov v njih
const boilerState = global.get('boilerState') || 'off';  // Stanje bojlerja
const boilerPower = parseFloat(global.get('boilerPower') || 0);  // Poraba energije bojlerja
const pralniStanje = global.get('pralni_stanje') || 'off';
const pralniPoraba = parseFloat(global.get('pralni_poraba') || 0);
const susilniStanje = global.get('susilni_stanje') || 'off';
const susilniPoraba = parseFloat(global.get('susilni_poraba') || 0);
// IR panelei
const irNaState = global.get('irSwitchState_na') || 'off';
const irNaPoraba = parseFloat(global.get('irPoraba_na') || 0);
const irSpState = global.get('irSwitchState_sp') || 'off';
const irSpPoraba = parseFloat(global.get('irPoraba_sp') || 0);
const irDiState = global.get('irSwitchState_di') || 'off';
const irDiPoraba = parseFloat(global.get('irPoraba_di') || 0);
// V emergency funkciji ohranite obstoječe
const activeDevices = {irSp: global.get('irSpStatus') === 'AKTIVEN'};

// Pripravi debug sporočilo
const debugMsg = `
⚡️ STANJE URGENCE ⚡️
┏
┃ 🔌 Faza 3: ${phase3}W ${phase3 <= maxPoraba ? '✅' : '❌'} (meja: ${maxPoraba})
┃ ♨️ Bojler: ${boilerState.toUpperCase()} ${boilerPower}W ${boilerState === 'AKTIVEN' ? '🔴' : '🟢'}
┃ 🌞 IR Nathalie: ${irNaState.toUpperCase()} ${irNaPoraba}W ${irNaPoraba > 100 ? '🔴' : '🟢'}
┃ 🛏️ IR Spalnica: ${irSpState.toUpperCase()} ${irSpPoraba}W ${irSpPoraba > 100 ? '🔴' : '🟢'}
┃ 👧 IR Diane: ${irDiState.toUpperCase()} ${irDiPoraba}W ${irDiPoraba > 100 ? '🔴' : '🟢'}
┃ 🧺 Pralni: ${pralniStanje.toUpperCase()} ${pralniPoraba}W ${pralniPoraba > 120 ? '🔴' : '🟢'}
┃ 🔥 Sušilni: ${susilniStanje.toUpperCase()} ${susilniPoraba}W ${susilniPoraba > 240 ? '🔴' : '🟢'}
┗
${phase3 > maxPoraba ? '🚨 PREKORAČITEV MOČI!' : '✅ Normalna poraba'}`;

node.warn(debugMsg);

// Preveri prekoračitev moči
if (phase3 > maxPoraba) {
    // Če je prva prekoračitev ali je minilo 5 sekund od zadnje
    if (currentTime - lastEmergencyTime > 5000) {
        // Pridobi trenutno aktivne naprave
        const activeDevices = {
            bojler: global.get('boilerState') === 'AKTIVEN',
            irNa: global.get('irNaStatus') === 'AKTIVEN',
            irSp: global.get('irSpStatus') === 'AKTIVEN',
            irDi: global.get('irDiStatus') === 'AKTIVEN',
            susilni: global.get('susilniStatus') === 'AKTIVEN',
            pralni: global.get('pralniStatus') === 'AKTIVEN'
        };
        
        // Najdi prvo aktivno napravo v zaporedju za izklop
        const deviceToTurnOff = emergencySequence.find(device => activeDevices[device]);
        
        if (deviceToTurnOff) {
            // Nastavi globalni flag za izklop
            global.set(`emergency_${deviceToTurnOff}_off`, true);
            flow.set('lastEmergencyTime', currentTime);
            
            node.warn(`⚠️ EMERGENCY: Zahtevan izklop ${deviceToTurnOff}`);
            
            // Dodatno sporočilo za debug
            msg.payload = {
                event: "EMERGENCY_TRIGGERED",
                dogodek: "URGENCA_AKTIVIRANA",
                device: deviceToTurnOff,
                phase3: phase3,
                timestamp: new Date(currentTime).toISOString()
            };
            
            return msg;
        } else {
            node.warn('ℹ️ EMERGENCY: Vse naprave so že izklopljene');
        }
    } else {
        // Čakamo še na potek 5s zamika za ponovno preverbo
        const remaining = (5000 - (currentTime - lastEmergencyTime)) / 1000;
        node.warn(`⏳ EMERGENCY: Čakamo na potek 5s zamika (preostalo: ${remaining.toFixed(1)}s)`);
    }
} else {
    // Ponastavi vse emergency flag-e, če je poraba normalna
    emergencySequence.forEach(device => {
        global.set(`emergency_${device}_off`, false);
    });
    
    // Ponastavi števec časa
    flow.set('lastEmergencyTime', 0);
    
    // Debug sporočilo
    msg.payload = {
        event: "POWER_NORMALIZED",
        dogodek: "MOČ_NORMALIZIRANA",
        phase3: phase3,
        timestamp: new Date().toISOString()
    };
    
    return msg;
}

return null;
```
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
| `function` node       | Glavna logika za izklop/vklop/emergency obdelavo     |
| `inject` node         | Periodično proži osvežitev                           |
| `notify.*`            | Pošiljanje obvestil uporabnikom                      |
| `current-state` node  | Preverjanje ali sta Mojca ali Robert doma            |

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
___

Koda funkcije
```javascript
// ===== FUNKCIJA PRALNO-SUŠILNI STROJ =====
// Izhodi:
// [0] Izklop sušilnega stroja
// [1] Izklop pralnega stroja 
// [2] Vklop pralnega stroja
// [3] Vklop sušilnega stroja

// Prisilno osveževanje podatkov preko inject noda
if (msg.topic === "force_refresh") { // Preveri, ali je tema (topic) dobljenega sporočila enaka nizu "force_refresh".
    msg.payload = global.get('phase3') || 0; // Prebere globalno vrednost 'phase3' ali vrne 0, če ne obstaja
    msg.topic = "sensor.p1_meter_power_phase_3"; // Spremeni temo sporočila
}

// === INICIALIZACIJA ===
const currentValue = parseFloat(msg.payload) || 0;
const entityId = msg.topic || (msg.data && msg.data.entity_id) || (msg._event && msg._event.entity_id);  // Prepoznati, katera naprava/entiteta je poslala podatek.
const maxPoraba = parseFloat(global.get('max_dovoljena_poraba')) || 4650;

// === SHRAJEVANJE PODATKOV GLEDE NA ENTITETO ===
if (entityId === 'sensor.me_prst_current_consumption') {
    flow.set('pralni_poraba', currentValue);  // Shrani v flow
    global.set('pralni_poraba', currentValue);  // Shrani v globalno spremenljivko
} 
else if (entityId === 'sensor.me_ss_current_consumption') {
    flow.set('susilni_poraba', currentValue);
    global.set('susilni_poraba', currentValue);
} 
else if (entityId === 'switch.me_ps') {
    flow.set('pralni_stanje', msg.payload);
    global.set('pralni_stanje', msg.payload);
} 
else if (entityId === 'switch.me_ss' || entityId === 'switch.me_ss_switch_0') {
    flow.set('susilni_stanje', msg.payload);
    global.set('susilni_stanje', msg.payload);
}

// === BRANJE TRENUTNIH STANJ ===
const porabaFaze3 = parseFloat(global.get('phase3')) || 0; // Globalna spremenljivka
global.set('last_phase3_update', Date.now());  // Shrani se čas osvežitve
const pralniPoraba = parseFloat(flow.get('pralni_poraba')) || 0;
const susilniPoraba = parseFloat(flow.get('susilni_poraba')) || 0;
const pralniStanje = flow.get('pralni_stanje') || 'off';
const susilniStanje = flow.get('susilni_stanje') || 'off';

// === DOLOČANJE STATUSOV  IN LEPŠI PRIKAZ V DEBUG ===
const pralniStatus = (pralniStanje === 'on' && pralniPoraba > 120) ? '🔴 AKTIVEN' : '🟢 neaktiven';
const susilniStatus = (susilniStanje === 'on' && susilniPoraba > 240) ? '🔴 AKTIVEN' : '🟢 neaktiven';

// Preverjanje svežosti podatkov
const lastUpdate = global.get('last_phase3_update') || 0;
if (Date.now() - lastUpdate > 10000) {
    node.warn("⚠️ OPOZORILO: Globalni podatki niso sveži!");
}

// === EMERGENCY LOGIKA ZA IZKLOP===
const emergencyPralniOff = global.get('emergency_pralni_off') || false;
const emergencySusilniOff = global.get('emergency_susilni_off') || false;
// EMERGENCY FUNKCIJA IZKLOPI PRALNI STROJ
if (emergencyPralniOff && pralniStanje === 'on') {
    node.warn('⛔ EMERGENCY: Izklop pralnega stroja');
    return [null, { payload: "off" }, null, null];
}
// EMERGENCY FUNKCIJA IZKLOPI SUŠILNI STROJ
if (emergencySusilniOff && susilniStanje === 'on') {
    node.warn('⛔ EMERGENCY: Izklop sušilnega stroja');
    return [{ payload: "off" }, null, null, null];
}

// === NORMALNO DELOVANJE PONOVNI VKLOP PO POTREBI ===
if (porabaFaze3 <= maxPoraba) {
    // Reset emergency stanja
    flow.set('lastEmergencyAlert', 0);
    
    const prostaMoc = maxPoraba - porabaFaze3;
    const rezerva = 50;
    
    // Pralni stroj ima prednost (1920W + rezerva)
    if (pralniStanje === 'off' && !pralniStatus.includes('AKTIVEN') && prostaMoc >= (1920 + rezerva)) {
        node.warn(`✅ AVTOMATSKI VKLOP: Pralni stroj (prosta moč: ${prostaMoc}W)`);
        return [null, null, { payload: "on" }, null];
    }
    // Sušilni stroj (2500W + rezerva)
    else if (susilniStanje === 'off' && !susilniStatus.includes('AKTIVEN') && prostaMoc >= (2500 + rezerva)) {
        node.warn(`✅ AVTOMATSKI VKLOP: Sušilni stroj (prosta moč: ${prostaMoc}W)`);
        return [null, null, null, { payload: "on" }];
    }
    else {
        node.warn(`ℹ️ Ohranjanje stanja (prosta moč: ${prostaMoc}W)`);
        return [null, null, null, null];
    }
}

// Če pridemo do sem, vrni prazne izhode
return [null, null, null, null];
```
***

# 📅 Popravljeno: 19.05.2025
💡 Primer za bojler ki ima najnižjo prioriteto delovanja (se prvi izklaplja).
![20250519-Bojler flows](https://github.com/user-attachments/assets/aa6b5113-5e29-4a37-ae4d-48c07741607c)


✍️ Koda v nod-red za prenos: 
[20250519-Bojler flows.zip](https://github.com/user-attachments/files/20281763/20250519-Bojler.flows.zip)

___
# ♨️ Bojler Flow - Avtomatsko upravljanje

## 🌟 Opis flow-a
Ta Node-RED flow avtomatsko upravlja delovanje bojlerja glede na:
- Trenutno porabo energije (`sensor.me_bo_current_consumption`)
- Stanje stikala (`switch.me_bo`)
- Emergency stanje sistema

### 📊 Komponente flow-a

1. **`server-state-changed` node**  
   - 👁️ Sledi spremembam porabe bojlerja (`sensor.me_bo_current_consumption`)
   - 🔄 Pošilja podatke ob vsaki spremembi
   - 📦 Nastavi:
     - `msg.payload`: trenutna poraba (W)
     - `msg.topic`: ime senzorja

2. **`inject` node**  
   - ⏲️ Vsakih 5 minut pošlje signal za osvežitev podatkov
   - 🔄 Zagotavlja redno posodabljanje stanja

3. **`function` node "Funkcija bojler"**  
   - 🧠 Glavna logična enota (opisana spodaj)
   - ⚠️ Ima dva izhoda:
     - Vklop bojlerja
     - Izklop bojlerja

4. **`api-call-service` node**  
   - 🏗️ Izvaja ukaze v Home Assistant:
     - Vklop (`turn_on`)
     - Izklop (`turn_off`)

## ⚙️ Podrobnosti delovanja funkcije

### 🔄 Inicializacija
```javascript
// Inicializacija globalnih spremenljivk
if (global.get('boilerPower') === undefined) global.set('boilerPower', 0);
if (global.get('boilerSwitchState') === undefined) global.set('boilerSwitchState', 'off');
if (global.get('phase3') === undefined) global.set('phase3', 0);
```

# ♨️ Podrobna razlaga delovanja bojler flow-a

## 🔍 Obdelava vhodnih podatkov

### ⚡ Poraba energije
```javascript
if (msg.topic === 'sensor.me_bo_current_consumption') {
  const poraba = parseFloat(msg.payload) || 0;
  global.set('boilerPower', poraba);
  posodobiBoilerStatus();
}
```

Preverja sporočila s tematiko porabe energije

Pretvori vrednost v število (0 če napaka)

Shrani v globalno spremenljivko `boilerPower`

Avtomatsko posodobi status bojlerja

# ♨️ Podrobna razlaga delovanja bojler flow-a

## 🔘 Stanje stikala
```javascript
else if (msg.topic === 'switch.me_bo' && ['on','off'].includes(msg.payload)) {
  global.set('boilerSwitchState', msg.payload);
  posodobiBoilerStatus();
}
```

# ♨️ Upravljanje Bojlerja v Node-RED

Ta skripta obdeluje spremembe stanja stikala za bojler ter avtomatsko upravlja njegovo delovanje na podlagi porabe električne energije in zaščitnih mehanizmov.

---

## ✅ Veljavna stanja

Sprejeta stanja stikala:

- `on`
- `off`

---

## 🔁 Sinhronizacija stanja

Ob vsaki spremembi:

- Se **posodobi globalna spremenljivka** `boilerSwitchState`
- Sproži se **ponovno izračunavanje statusa** bojlerja

---

## 📊 Debug izpis

```javascript
const debugMsg = `♨️ STANJE BOJLERJA
┌
│  🏷️ STIKALO    ${global.get('boilerSwitchState').toUpperCase()} 
│  🔧 STATUS     ${global.get('boilerState').toUpperCase()}
│  ⚡ PORABA      ${global.get('boilerPower')}W
└`;
node.warn(debugMsg);
```

# 🔥 Pametno upravljanje bojlerja z Node-RED

Ta logika omogoča varno in učinkovito upravljanje električnega bojlerja v realnem času s pomočjo **Node-RED**, senzorjev porabe in Home Assistant integracije.

---

## 🧩 Prikazuje

- 🔘 **Trenutno stanje stikala** (ON/OFF)
- 🛠️ **Status bojlerja** (AKTIVEN / neaktiven)
- ⚡ **Trenutno porabo** v W
- 🎨 **Uporabo emojijev** za boljšo preglednost v Node-RED konzoli

---

## 🚨 Emergency izklop

```javascript
if (global.get('emergency_bojler_off') && trenutniPodatki.switchState === 'on') {
    flow.set('lastBoilerTurnOffTime', Date.now());
    return [null, { payload: "off" }];
}
```
### Pogoji:
- `emergency_bojler_off` je `true`
- Bojler je trenutno **vklopljen**

### Akcije:
- 🕒 Zabeleži čas izklopa
- 📴 Pošlje ukaz za izklop
- 🛡️ Prepreči preobremenitev omrežja

## ⏱️ Avtomatski vklop

```javascript
if (trenutniPodatki.phase3 <= 2100 && trenutniPodatki.switchState === 'off') {
    const lastTurnOffTime = flow.get('lastBoilerTurnOffTime') || 0;
    if (Date.now() - lastTurnOffTime >= 300000) {
        return [{ payload: "on" }, null];
    }
}
```

### Pogoji:
- 📉 Poraba faze 3 ≤ **2100W**
- 🔌 Bojler je izklopljen
- ⏳ Minilo je **vsaj 5 minut** od zadnjega izklopa

### Akcija:
- 🔁 Pošlje ukaz za **vklop bojlerja**

## 🛠️ Konfiguracijske nastavitve

| Parameter               | Vrednost   | Opis                                                       |
|-------------------------|------------|------------------------------------------------------------|
| `max_dovoljena_poraba`  | 4650 W     | Mejna vrednost za sprožitev emergency režima               |
| `zaščitni_zamik`        | 5 minut    | Minimalni čas med avtomatskim vklopom po izklopu           |
| `osveževanje`           | 5 minut    | Interval samodejnega preverjanja podatkov v Node-RED flowu |

## 🌈 Delovni primeri

### 1️⃣ Normalno delovanje

| Pogoj           | Rezultat                |
|----------------|--------------------------|
| Poraba ≤ 2100W | ✅ Bojler se lahko vklopi |
| Poraba > 2100W | ⛔ Bojler ostane izklopljen |

### 2️⃣ Emergency scenarij

- ⚠️ Zaznana **preobremenitev omrežja**
- `emergency_bojler_off = true`
- 🔌 Bojler se **takoj izklopi**
- 🛡️ Omrežna zaščita se **aktivira**

### 3️⃣ Ročno upravljanje

- 👆 Stikalo `switch.me_bo` omogoča **ročno upravljanje**
- 🔄 Sistem še vedno spoštuje varnostne mehanizme
- 🔐 Ročno vklopljen bojler bo izklopljen ob emergency pogoju

## 💡 Razširljivost

- ➕ Enostavno dodajanje novih naprav z minimalnimi spremembami v kodi
- 🧠 Vsa logika temelji na **globalnih spremenljivkah** in osrednjemu nadzoru
- ⚙️ Sistem se lahko razširi na **večfazne** porabnike ali dodatne scenarije

## 🧪 Diagnostika in testiranje

| Kaj preveriti                          | Orodje / metoda              |
|----------------------------------------|------------------------------|
| Stanje `switchState`                   | `debug` node / `node.warn()` |
| Porabo `phase3`                        | Preveri senzor v HA          |
| Čas `lastBoilerTurnOffTime`            | Uporabi `flow.get()`         |
| Vrednost `emergency_bojler_off`        | Preveri z `global.get()`     |
| Odziv na ročno vklop / izklop stikala  | Spremljaj `switch.me_bo`     |

## 📡 Tehnične zahteve

- ✅ **Node-RED** okolje (verzija 3.x ali višje priporočena)
- ✅ **Home Assistant** ali drug MQTT strežnik
- ✅ Aktivni senzorji:
  - `switch.me_bo` – stikalo za bojler
  - `sensor.phase3_power` – trenutna poraba faze 3

## 📌 Shema poteka

```
[Senzorji (MQTT / HA)]
        │
        ▼
[Function Node: Upravljanje bojlerja]
        │
   ┌────┴────┐
   ▼         ▼
[Stikalo] [Debug / Notification]
```

## 🔐 Varnostne opombe

- Sistem nikoli ne vklopi bojlerja, če bi to **preseglo porabniški limit**
- Ročni vklop **ne zaobide varnostnih pogojev**
- Emergency izklop je **neodvisen od uporabnika** in temelji izključno na senzorjih


___

___
Koda funkcije:
```javascript
// === BOJLER - AVTOMATSKO UPRAVLJANJE ===
// Izhodi:
// [0] Vklop bojlerja
// [1] Izklop bojlerja

// ===== INICIALIZACIJA GLOBALNIH SPREMENLJIVK NUJO =====
if (global.get('boilerPower') === undefined) global.set('boilerPower', 0);
if (global.get('boilerSwitchState') === undefined) global.set('boilerSwitchState', 'off');
if (global.get('phase3') === undefined) global.set('phase3', 0);
const maxPoraba = parseFloat(global.get('max_dovoljena_poraba')) || 4650;

// ===== OBDELAVA INJECT OSVEŽEVANJA PODATKOV =====
if (msg.topic === "force_refresh_boiler") {
    msg = {
        payload: {
            phase3: parseFloat(global.get('phase3')),  // Brez || 0, ker je že inicializiran
            boilerPower: parseFloat(global.get('boilerPower')),  // Brez || 0
            switchState: global.get('boilerSwitchState')  // Brez || 'off'
        },
        topic: "rocno_osvezeno"
    };
    node.warn("🔁 Osvežujem podatke za bojler");
}

// ===== OBDELAVA VHODNIH PODATKOV =====
let obdelano = false;
// Stanje bojlerja
function posodobiBoilerStatus() {
    const power = parseFloat(global.get('boilerPower')) || 0;
    const switchState = global.get('boilerSwitchState');
    const newStatus = (switchState === 'on' && power > 100) ? 'AKTIVEN' : 'neaktiven';
    global.set('boilerState', newStatus);
    return newStatus;
}

// Obdelava porabe
if (msg.topic === 'sensor.me_bo_current_consumption') {
    const poraba = parseFloat(msg.payload) || 0;
    global.set('boilerPower', poraba);
    posodobiBoilerStatus(); // Samo enkrat nastavi status
    obdelano = true;
} 
// Obdelava stikala
else if (msg.topic === 'switch.me_bo' && ['on','off'].includes(msg.payload)) {
    global.set('boilerSwitchState', msg.payload);
    posodobiBoilerStatus(); // Samo enkrat nastavi status
    obdelano = true;
}

// ===== PRIKAZ TRENUTNEGA STANJA (DEBUG) =====
const trenutniPodatki = msg.topic === "rocno_osvezeno" ? msg.payload : {
    phase3: parseFloat(global.get('phase3')) || 0,
    boilerPower: parseFloat(global.get('boilerPower')) || 0,
    switchState: global.get('boilerSwitchState') || 'off'
};

if (msg.topic === "rocno_osvezeno" || obdelano) {
    const debugMsg = `♨️ STANJE BOJLERJA
    ┌
    │  🏷️ STIKALO    ${global.get('boilerSwitchState').toUpperCase()} ${global.get('boilerSwitchState') === 'on' ? '🟢' : '🔴'}
    │  🔧 STATUS     ${global.get('boilerState').toUpperCase()} ${global.get('boilerState') === 'AKTIVEN' ? '🔴' : '🟢'}
    │  ⚡ PORABA      ${global.get('boilerPower')}W ${global.get('boilerState') === 'AKTIVEN' ? '🔴' : '🟢'} 
    └`;
    node.warn(debugMsg);
}

// ===== GLAVNA LOGIKA UPRAVLJANJA =====

// 1. EMERGENCY IZKLOP (samo če je globalni flag aktiven)
if (global.get('emergency_bojler_off') && trenutniPodatki.switchState === 'on') {
    flow.set('lastBoilerTurnOffTime', Date.now()); // Shrani čas izklopa
    node.warn(`⛔ EMERGENCY IZKLOP: Poraba=${trenutniPodatki.phase3}W, Stanje=${global.get('boilerState')}`);
    return [null, { payload: "off" }];
}

// 2. AVTOMATSKI VKLOP (z 5-minutno zakasnitvijo)
if (trenutniPodatki.phase3 <= 2100 && trenutniPodatki.switchState === 'off') {
    const lastTurnOffTime = flow.get('lastBoilerTurnOffTime') || 0;
    const currentTime = Date.now();
    const minDelay = 5 * 60 * 1000; // 5 minut v milisekundah

    if (currentTime - lastTurnOffTime >= minDelay) {
        node.warn(`✅ AVTOMATSKI VKLOP: Poraba ${trenutniPodatki.phase3}W ≤ 2100W`);
        return [{ payload: "on" }, null];
    } else {
        const remainingSec = Math.round((minDelay - (currentTime - lastTurnOffTime)) / 1000);
        node.warn(`⏳ Zamik vklopa: še ${remainingSec} sekund`);
        return [null, null];
    }
}

// 3. NORMALNO STANJE - brez sprememb
node.warn("ℹ️ Ohranjam trenutno stanje");
return [null, null];
```
***

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


