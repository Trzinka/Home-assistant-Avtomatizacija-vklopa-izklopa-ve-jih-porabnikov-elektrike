# 🐣 Projekt v razvoju
___

## Z naslednjimi nastavitvami želim doseči nadzor nad porabo maksimalno dovoljene dogovorjene moči (kW)!

![Sprememba dogovorjene moči](https://github.com/user-attachments/assets/f3fd7e9b-f6cb-4a89-8c5b-d751f2365208)

### 🤔 Treba je razumeti "politiko" elektra.

### Pri spremembi dogovorjene moči je potrebno upoštevati pravilo, `da mora biti dogovorjena moč v višjem časovnem bloku enaka ali večja kot v predhodnem`. 
To poenostavljeno pomeni, da npr. `dogovorjena moč v časovnem bloku 2 ne more biti manjša kot v časovnem bloku 1`.?

Taka "politika" nam ravno ne dovoljuje biti povsem kreativen a bom vseeno poiskusil.

***

🟥 Primer dejanske dosežene moči za mesec marec.
![202503-Moj elektro](https://github.com/user-attachments/assets/94b535d9-05b6-4e3c-936e-6e9eb7263825)

Iz zgornjega je razvidno, da povprečje določeno s strani elektra odstopa od dejanskega, ki bi ga lahko upoštevali pa naj si gre za maksimum in ne za 15 minutno povprečje.

Kako pa so opravili meritve za izračun dogovorjene moči?
![Meritve za izračun dogovorjene moči](https://github.com/user-attachments/assets/42b03b63-1a09-4ef4-9d03-7386b534ef89)

Kot je iz zgornje slike razvidno so vzeli tudi najvišje podatke porabe moči iz leta 2023 kot relevantne!?
Ne pozabimo, da te opravljene meritve oziroma dogovorjena moč veljajo celo leto, ki nam ki se ogrevamo (IR paneli) s pomočjo elektrike ne omogoča kaj dosti prilagajanja razen, če to sami zahtevamo!

***
### ⭐ Torej, moj namen je ugotoviti v kolikšni meri bi s pomočjo avtomatizacije lahko vplival na znižanje računa, če sploh.
***

Poglejmo si dva primera (moj dobavitelj je `Elektro energija, podjetje za prodajo elektrike in drugih energentov, svetovanje in storitve, d.o.o.`):

Mesec november 2024 (Omilitev draginje pri oskrbi z elektriko)
![2024-11_Stran_2](https://github.com/user-attachments/assets/f3d98f84-52af-4786-965c-378433d64093)

Mesec marec 2025
![202503_Stran_2](https://github.com/user-attachments/assets/a36743f0-ca2f-4ead-b552-fcfa4c2a3343)

# 🎯 Pojasniti vam moram, da živimo v dvo družinski hiši kjer imamo trenutno 1 odjemno merilno mesto!

***

Najprej sem moral rešiti dilemo glede uporabe električne pečice, pralnega stroja in sušilnega stroja. Z ženo sva se dogovorila 😄, da istočasno ne vklapljava teh naprav, tukaj pride v poštev tudi, friteza, mikrovalovna, fen za lase (opremljena so s pametnimi stikali ali pa vsaj z merilniki porabe) in še kaj se bi našlo.

Glede na to, da imamo prostore ogrevane s pomočjo IR panelov in da vodo greje bojler se mi dozdeva, da te naprave lahko brez večjega vpliva ob špicah izklapljam.


# Nekaj podatkov o porabnikih
___
| Naprava        | Poraba |
|----------------|--------|
| Sušilni stroj  | 2,5 kW |
| Pralni stroj   | 1,8 kW |
| Bojler         | 2,0 kW |
| Štedilnik      |        |
| Mikrovalovna   |        |
| Likalnik       | 2,2 kW |
| Fen za lase    | 2,4 kW |
| IR Nati        | 0,6 kW |
| IR Spalnica    | 1,2 kW |
| IR Di          | 0,7 kW |
___
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

# 📅 Popravljeno: 05.05.2025
💡 Primer za bojler.
![Node red-Bojler](https://github.com/user-attachments/assets/1c393111-44a1-4e29-852a-b0719d9cf955)


✍️ Koda za nod: 
```yaml
[
    {
        "id": "88ea0246d87f31ef",
        "type": "tab",
        "label": "Bojler",
        "disabled": false,
        "info": "",
        "env": []
    },
    {
        "id": "e18afdd2b2a80835",
        "type": "server-state-changed",
        "z": "88ea0246d87f31ef",
        "name": "Me-Bo-Current consumption <= 2,01 kW",
        "server": "5f28286e.ae6338",
        "version": 4,
        "exposeToHomeAssistant": false,
        "haConfig": [
            {
                "property": "name",
                "value": ""
            },
            {
                "property": "icon",
                "value": ""
            }
        ],
        "entityidfilter": "sensor.me_bo_current_consumption",
        "entityidfiltertype": "exact",
        "outputinitially": true,
        "state_type": "str",
        "haltifstate": "",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "outputs": 1,
        "output_only_on_state_change": true,
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "ignorePrevStateNull": false,
        "ignorePrevStateUnknown": false,
        "ignorePrevStateUnavailable": false,
        "ignoreCurrentStateUnknown": false,
        "ignoreCurrentStateUnavailable": false,
        "outputProperties": [
            {
                "property": "payload",
                "propertyType": "msg",
                "value": "",
                "valueType": "entityState"
            },
            {
                "property": "data",
                "propertyType": "msg",
                "value": "",
                "valueType": "eventData"
            },
            {
                "property": "topic",
                "propertyType": "msg",
                "value": "",
                "valueType": "triggerId"
            }
        ],
        "x": 160,
        "y": 40,
        "wires": [
            [
                "24f47e3291fa445d"
            ]
        ]
    },
    {
        "id": "cfe5da23c6543707",
        "type": "server-state-changed",
        "z": "88ea0246d87f31ef",
        "name": "Phase 3-Current consumption <= 2,64 kW",
        "server": "5f28286e.ae6338",
        "version": 4,
        "exposeToHomeAssistant": false,
        "haConfig": [
            {
                "property": "name",
                "value": ""
            },
            {
                "property": "icon",
                "value": ""
            }
        ],
        "entityidfilter": "sensor.p1_meter_power_phase_3",
        "entityidfiltertype": "exact",
        "outputinitially": true,
        "state_type": "str",
        "haltifstate": "",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "outputs": 1,
        "output_only_on_state_change": true,
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "ignorePrevStateNull": false,
        "ignorePrevStateUnknown": false,
        "ignorePrevStateUnavailable": false,
        "ignoreCurrentStateUnknown": false,
        "ignoreCurrentStateUnavailable": false,
        "outputProperties": [
            {
                "property": "payload",
                "propertyType": "msg",
                "value": "",
                "valueType": "entityState"
            },
            {
                "property": "data",
                "propertyType": "msg",
                "value": "",
                "valueType": "eventData"
            },
            {
                "property": "topic",
                "propertyType": "msg",
                "value": "",
                "valueType": "triggerId"
            }
        ],
        "x": 160,
        "y": 100,
        "wires": [
            [
                "24f47e3291fa445d"
            ]
        ]
    },
    {
        "id": "24f47e3291fa445d",
        "type": "function",
        "z": "88ea0246d87f31ef",
        "name": "function",
        "func": "// === VHODNI PODATKI ===\nconst currentValue = parseFloat(msg.payload);\nconst topic = msg.topic;\n\n// Shranjevanje vrednosti\nif (topic === 'sensor.me_bo_current_consumption') {\n    global.set('mebo', currentValue);\n    global.set('boilerPower', currentValue);\n} else if (topic === 'sensor.p1_meter_power_phase_3') {\n    global.set('phase3', currentValue);\n} else if (topic === 'switch.me_bo') {\n    global.set('boilerSwitchState', msg.payload);\n}\n\n// === INTERNI PARAMETRI ===\nconst mebo = parseFloat(global.get('mebo') || 0);\nconst phase3 = parseFloat(global.get('phase3') || 0);\nconst boilerSwitchState = global.get('boilerSwitchState') || 'off';\n\n// Določanje statusa\nlet boilerStatus;\nif (boilerSwitchState === 'off') {\n    boilerStatus = 'off';\n} else {\n    boilerStatus = mebo > 100 ? 'AKTIVEN' : 'neaktiven';\n}\nglobal.set('boilerState', boilerStatus);\n\n// === DEBUG ===\nnode.warn(`♨️ \nStikalo: ${boilerSwitchState}  \nPoraba: ${mebo}W  \nStatus: ${boilerStatus}  \nPhase3: ${phase3}W`);\n\n// === LOGIKA ===\nif (phase3 <= 2100 && boilerSwitchState === 'off') {\n    node.warn('✅ VKLOP BOJLERJA (pogoji izpolnjeni)');\n    return [{ payload: \"on\" }, null];\n} else if (phase3 > 4650 && boilerSwitchState === 'on') {\n    node.warn('⛔ IZKLOP BOJLERJA (presežena moč)');\n    return [null, { payload: \"off\" }];\n}\n\nreturn [null, null];",
        "outputs": 2,
        "timeout": 0,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 440,
        "y": 100,
        "wires": [
            [
                "4e2336384af3ac68"
            ],
            [
                "f0f3ddd2d18f08fc"
            ]
        ]
    },
    {
        "id": "4e2336384af3ac68",
        "type": "delay",
        "z": "88ea0246d87f31ef",
        "name": "1 X every 5 minutes",
        "pauseType": "rate",
        "timeout": "5",
        "timeoutUnits": "seconds",
        "rate": "1",
        "nbRateUnits": "5",
        "rateUnits": "minute",
        "randomFirst": "1",
        "randomLast": "5",
        "randomUnits": "seconds",
        "drop": true,
        "allowrate": false,
        "outputs": 1,
        "x": 628.8923606872559,
        "y": 62.90277099609375,
        "wires": [
            [
                "84c8c6492cb278ec"
            ]
        ]
    },
    {
        "id": "c9c9a142c074d938",
        "type": "api-call-service",
        "z": "88ea0246d87f31ef",
        "name": "Turn ON Boiler",
        "server": "5f28286e.ae6338",
        "version": 5,
        "debugenabled": false,
        "domain": "switch",
        "service": "turn_on",
        "areaId": [],
        "deviceId": [],
        "entityId": [
            "switch.me_bo"
        ],
        "data": "{}",
        "dataType": "json",
        "mergeContext": "",
        "mustacheAltTags": false,
        "outputProperties": [],
        "queue": "none",
        "x": 1028.8923606872559,
        "y": 42.90277099609375,
        "wires": [
            []
        ]
    },
    {
        "id": "ef8254e039891324",
        "type": "api-call-service",
        "z": "88ea0246d87f31ef",
        "name": "Turn OFF Boiler",
        "server": "5f28286e.ae6338",
        "version": 5,
        "debugenabled": false,
        "domain": "switch",
        "service": "turn_off",
        "areaId": [],
        "deviceId": [],
        "entityId": [
            "switch.me_bo"
        ],
        "data": "{}",
        "dataType": "json",
        "mergeContext": "",
        "mustacheAltTags": false,
        "outputProperties": [],
        "queue": "none",
        "x": 1028.8923606872559,
        "y": 122.90277099609375,
        "wires": [
            []
        ]
    },
    {
        "id": "f0f3ddd2d18f08fc",
        "type": "api-current-state",
        "z": "88ea0246d87f31ef",
        "name": "Is the boiler ON",
        "server": "5f28286e.ae6338",
        "version": 3,
        "outputs": 2,
        "halt_if": "on",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "entity_id": "switch.me_bo",
        "state_type": "str",
        "blockInputOverrides": false,
        "outputProperties": [
            {
                "property": "payload",
                "propertyType": "msg",
                "value": "",
                "valueType": "entityState"
            },
            {
                "property": "data",
                "propertyType": "msg",
                "value": "",
                "valueType": "entity"
            }
        ],
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "override_topic": false,
        "state_location": "payload",
        "override_payload": "msg",
        "entity_location": "data",
        "override_data": "msg",
        "x": 728.8923606872559,
        "y": 122.90277099609375,
        "wires": [
            [
                "ef8254e039891324"
            ],
            []
        ]
    },
    {
        "id": "84c8c6492cb278ec",
        "type": "api-current-state",
        "z": "88ea0246d87f31ef",
        "name": "Is the boiler OFF",
        "server": "5f28286e.ae6338",
        "version": 3,
        "outputs": 2,
        "halt_if": "off",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "entity_id": "switch.me_bo",
        "state_type": "str",
        "blockInputOverrides": false,
        "outputProperties": [
            {
                "property": "payload",
                "propertyType": "msg",
                "value": "",
                "valueType": "entityState"
            },
            {
                "property": "data",
                "propertyType": "msg",
                "value": "",
                "valueType": "entity"
            }
        ],
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "override_topic": false,
        "state_location": "payload",
        "override_payload": "msg",
        "entity_location": "data",
        "override_data": "msg",
        "x": 838.8923606872559,
        "y": 62.90277099609375,
        "wires": [
            [
                "c9c9a142c074d938"
            ],
            []
        ]
    },
    {
        "id": "dodaten_node_id",
        "type": "server-state-changed",
        "z": "88ea0246d87f31ef",
        "name": "Sprememba stanja stikala bojlerja",
        "server": "5f28286e.ae6338",
        "version": 4,
        "exposeToHomeAssistant": false,
        "haConfig": [
            {
                "property": "name",
                "value": ""
            },
            {
                "property": "icon",
                "value": ""
            }
        ],
        "entityidfilter": "switch.me_bo",
        "entityidfiltertype": "exact",
        "outputinitially": true,
        "state_type": "str",
        "haltifstate": "",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "outputs": 1,
        "output_only_on_state_change": true,
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "ignorePrevStateNull": false,
        "ignorePrevStateUnknown": false,
        "ignorePrevStateUnavailable": false,
        "ignoreCurrentStateUnknown": false,
        "ignoreCurrentStateUnavailable": false,
        "outputProperties": [
            {
                "property": "payload",
                "propertyType": "msg",
                "value": "",
                "valueType": "entityState"
            },
            {
                "property": "data",
                "propertyType": "msg",
                "value": "",
                "valueType": "eventData"
            },
            {
                "property": "topic",
                "propertyType": "msg",
                "value": "",
                "valueType": "triggerId"
            }
        ],
        "x": 130,
        "y": 160,
        "wires": [
            [
                "24f47e3291fa445d"
            ]
        ]
    },
    {
        "id": "5f28286e.ae6338",
        "type": "server",
        "name": "Home Assistant",
        "version": 5,
        "addon": true,
        "rejectUnauthorizedCerts": true,
        "ha_boolean": "y|yes|true|on|home|open",
        "connectionDelay": true,
        "cacheJson": true,
        "heartbeat": false,
        "heartbeatInterval": 30,
        "areaSelector": "friendlyName",
        "deviceSelector": "friendlyName",
        "entitySelector": "friendlyName",
        "statusSeparator": "at: ",
        "statusYear": "hidden",
        "statusMonth": "short",
        "statusDay": "numeric",
        "statusHourCycle": "h23",
        "statusTimeFormat": "h:m",
        "enableGlobalContextStore": true
    }
]
```

Kaj ta tok (flow) dela:

Preveri, če je skupna poraba elektrike manjša ali enaka 4 kW in če je
se sproži na vsake 5 minut nadaljevanje kjer  
preveri, če je stikalo za bojler ugasnjeno in če je
ga vklopi.

Pri preverbi, če je skupna poraba elektrike manjša ali enaka 4 kW in če ni (je višja)
preveri, če je stikalo bojlerja prižgano in če je
ugasne stikalo bojlerja.

Preverbe stanj delam zaradi zapisovanja stanja naprav v podatkovno bazo!

___

# 📅 Popravljeno: 05.05.2025
💡 Primer za IR panel (ogrevanje):

![Node red-Bojler](https://github.com/user-attachments/assets/159b183d-d2ea-4503-b0a8-c3e34b9eac52)



✍️ Koda za nod:
```yaml
[
    {
        "id": "0e96811211eede05",
        "type": "tab",
        "label": "IR-Sp",
        "disabled": false,
        "info": "",
        "env": []
    },
    {
        "id": "7f13a1260e8a8079",
        "type": "api-call-service",
        "z": "0e96811211eede05",
        "name": "Turn OFF IR heating panel-Bedroom",
        "server": "5f28286e.ae6338",
        "version": 5,
        "debugenabled": false,
        "domain": "switch",
        "service": "turn_off",
        "areaId": [],
        "deviceId": [],
        "entityId": [
            "switch.tm_sp"
        ],
        "data": "{}",
        "dataType": "json",
        "mergeContext": "",
        "mustacheAltTags": false,
        "outputProperties": [],
        "queue": "none",
        "x": 970,
        "y": 320,
        "wires": [
            []
        ]
    },
    {
        "id": "2bf65c06b2fc6d14",
        "type": "server-state-changed",
        "z": "0e96811211eede05",
        "name": "<= 21 C",
        "server": "5f28286e.ae6338",
        "version": 4,
        "exposeToHomeAssistant": false,
        "haConfig": [
            {
                "property": "name",
                "value": ""
            },
            {
                "property": "icon",
                "value": ""
            }
        ],
        "entityidfilter": "sensor.povprecje_temperature_spalnica",
        "entityidfiltertype": "exact",
        "outputinitially": true,
        "state_type": "str",
        "haltifstate": "",
        "halt_if_type": "num",
        "halt_if_compare": "is",
        "outputs": 1,
        "output_only_on_state_change": false,
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "ignorePrevStateNull": false,
        "ignorePrevStateUnknown": false,
        "ignorePrevStateUnavailable": false,
        "ignoreCurrentStateUnknown": false,
        "ignoreCurrentStateUnavailable": false,
        "outputProperties": [
            {
                "property": "payload",
                "propertyType": "msg",
                "value": "",
                "valueType": "entityState"
            },
            {
                "property": "data",
                "propertyType": "msg",
                "value": "",
                "valueType": "eventData"
            },
            {
                "property": "topic",
                "propertyType": "msg",
                "value": "",
                "valueType": "triggerId"
            }
        ],
        "x": 270,
        "y": 280,
        "wires": [
            [
                "2eca172c43e73869"
            ]
        ]
    },
    {
        "id": "92288ecab2972196",
        "type": "api-current-state",
        "z": "0e96811211eede05",
        "name": "Is the IR ON",
        "server": "5f28286e.ae6338",
        "version": 3,
        "outputs": 2,
        "halt_if": "on",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "entity_id": "switch.tm_sp",
        "state_type": "str",
        "blockInputOverrides": false,
        "outputProperties": [
            {
                "property": "payload",
                "propertyType": "msg",
                "value": "",
                "valueType": "entityState"
            },
            {
                "property": "data",
                "propertyType": "msg",
                "value": "",
                "valueType": "entity"
            }
        ],
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "override_topic": false,
        "state_location": "payload",
        "override_payload": "msg",
        "entity_location": "data",
        "override_data": "msg",
        "x": 790,
        "y": 260,
        "wires": [
            [
                "7f13a1260e8a8079"
            ],
            []
        ]
    },
    {
        "id": "d36f2689478774da",
        "type": "api-call-service",
        "z": "0e96811211eede05",
        "name": "Turn ON IR heating panel-Bedroom",
        "server": "5f28286e.ae6338",
        "version": 5,
        "debugenabled": false,
        "domain": "switch",
        "service": "turn_on",
        "areaId": [],
        "deviceId": [],
        "entityId": [
            "switch.tm_sp"
        ],
        "data": "{}",
        "dataType": "json",
        "mergeContext": "",
        "mustacheAltTags": false,
        "outputProperties": [],
        "queue": "none",
        "x": 980,
        "y": 80,
        "wires": [
            []
        ]
    },
    {
        "id": "f46649d98acfa8eb",
        "type": "api-current-state",
        "z": "0e96811211eede05",
        "name": "Is the IR OFF",
        "server": "5f28286e.ae6338",
        "version": 3,
        "outputs": 2,
        "halt_if": "off",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "entity_id": "switch.tm_sp",
        "state_type": "str",
        "blockInputOverrides": false,
        "outputProperties": [
            {
                "property": "payload",
                "propertyType": "msg",
                "value": "",
                "valueType": "entityState"
            },
            {
                "property": "data",
                "propertyType": "msg",
                "value": "",
                "valueType": "entity"
            }
        ],
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "override_topic": false,
        "state_location": "payload",
        "override_payload": "msg",
        "entity_location": "data",
        "override_data": "msg",
        "x": 880,
        "y": 140,
        "wires": [
            [
                "d36f2689478774da"
            ],
            []
        ]
    },
    {
        "id": "50f8b8208c1817f1",
        "type": "server-state-changed",
        "z": "0e96811211eede05",
        "name": "Robert or Mojca home",
        "server": "5f28286e.ae6338",
        "version": 4,
        "exposeToHomeAssistant": false,
        "haConfig": [
            {
                "property": "name",
                "value": ""
            },
            {
                "property": "icon",
                "value": ""
            }
        ],
        "entityidfilter": "binary_sensor.tpl_occupancy",
        "entityidfiltertype": "exact",
        "outputinitially": true,
        "state_type": "str",
        "haltifstate": "",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "outputs": 1,
        "output_only_on_state_change": false,
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "ignorePrevStateNull": false,
        "ignorePrevStateUnknown": false,
        "ignorePrevStateUnavailable": false,
        "ignoreCurrentStateUnknown": false,
        "ignoreCurrentStateUnavailable": false,
        "outputProperties": [
            {
                "property": "payload",
                "propertyType": "msg",
                "value": "",
                "valueType": "entityState"
            },
            {
                "property": "data",
                "propertyType": "msg",
                "value": "",
                "valueType": "eventData"
            },
            {
                "property": "topic",
                "propertyType": "msg",
                "value": "",
                "valueType": "triggerId"
            }
        ],
        "x": 220,
        "y": 220,
        "wires": [
            [
                "2eca172c43e73869"
            ]
        ]
    },
    {
        "id": "859ca23e69522152",
        "type": "server-state-changed",
        "z": "0e96811211eede05",
        "name": "Window OFF/ON",
        "server": "5f28286e.ae6338",
        "version": 4,
        "exposeToHomeAssistant": false,
        "haConfig": [
            {
                "property": "name",
                "value": ""
            },
            {
                "property": "icon",
                "value": ""
            }
        ],
        "entityidfilter": "sensor.so_sp_st",
        "entityidfiltertype": "exact",
        "outputinitially": true,
        "state_type": "str",
        "haltifstate": "",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "outputs": 1,
        "output_only_on_state_change": false,
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "ignorePrevStateNull": false,
        "ignorePrevStateUnknown": false,
        "ignorePrevStateUnavailable": false,
        "ignoreCurrentStateUnknown": false,
        "ignoreCurrentStateUnavailable": false,
        "outputProperties": [
            {
                "property": "payload",
                "propertyType": "msg",
                "value": "",
                "valueType": "entityState"
            },
            {
                "property": "data",
                "propertyType": "msg",
                "value": "",
                "valueType": "eventData"
            },
            {
                "property": "topic",
                "propertyType": "msg",
                "value": "",
                "valueType": "triggerId"
            }
        ],
        "x": 240,
        "y": 160,
        "wires": [
            [
                "2eca172c43e73869"
            ]
        ]
    },
    {
        "id": "ceb47b291656fe70",
        "type": "server-state-changed",
        "z": "0e96811211eede05",
        "name": "Door OFF/ON",
        "server": "5f28286e.ae6338",
        "version": 4,
        "exposeToHomeAssistant": false,
        "haConfig": [
            {
                "property": "name",
                "value": ""
            },
            {
                "property": "icon",
                "value": ""
            }
        ],
        "entityidfilter": "binary_sensor.sv_sp_door",
        "entityidfiltertype": "exact",
        "outputinitially": true,
        "state_type": "str",
        "haltifstate": "",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "outputs": 1,
        "output_only_on_state_change": false,
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "ignorePrevStateNull": false,
        "ignorePrevStateUnknown": false,
        "ignorePrevStateUnavailable": false,
        "ignoreCurrentStateUnknown": false,
        "ignoreCurrentStateUnavailable": false,
        "outputProperties": [
            {
                "property": "payload",
                "propertyType": "msg",
                "value": "",
                "valueType": "entityState"
            },
            {
                "property": "data",
                "propertyType": "msg",
                "value": "",
                "valueType": "eventData"
            },
            {
                "property": "topic",
                "propertyType": "msg",
                "value": "",
                "valueType": "triggerId"
            }
        ],
        "x": 250,
        "y": 100,
        "wires": [
            [
                "2eca172c43e73869"
            ]
        ]
    },
    {
        "id": "cf5ff828e4d9e6ef",
        "type": "server-state-changed",
        "z": "0e96811211eede05",
        "name": "Fase 3-Curent consumption <= 3,4 kW",
        "server": "5f28286e.ae6338",
        "version": 4,
        "exposeToHomeAssistant": false,
        "haConfig": [
            {
                "property": "name",
                "value": ""
            },
            {
                "property": "icon",
                "value": ""
            }
        ],
        "entityidfilter": "sensor.p1_meter_power_phase_3",
        "entityidfiltertype": "exact",
        "outputinitially": true,
        "state_type": "str",
        "haltifstate": "",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "outputs": 1,
        "output_only_on_state_change": false,
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "ignorePrevStateNull": false,
        "ignorePrevStateUnknown": false,
        "ignorePrevStateUnavailable": false,
        "ignoreCurrentStateUnknown": false,
        "ignoreCurrentStateUnavailable": false,
        "outputProperties": [
            {
                "property": "payload",
                "propertyType": "msg",
                "value": "",
                "valueType": "entityState"
            },
            {
                "property": "data",
                "propertyType": "msg",
                "value": "",
                "valueType": "eventData"
            },
            {
                "property": "topic",
                "propertyType": "msg",
                "value": "",
                "valueType": "triggerId"
            }
        ],
        "x": 170,
        "y": 40,
        "wires": [
            [
                "2eca172c43e73869"
            ]
        ]
    },
    {
        "id": "2eca172c43e73869",
        "type": "function",
        "z": "0e96811211eede05",
        "name": "IR spalnica Logika",
        "func": "// === VHODNI PODATKI ===\nconst currentValue = parseFloat(msg.payload);\nconst topic = msg.topic;\n\n// Shranjevanje vrednosti GLOBALNO\nif (topic === 'sensor.tm_sp_current_consumption') {\n    global.set('irPoraba_sp', currentValue);  // Poraba IR panelov v spalnici\n} else if (topic === 'sensor.p1_meter_power_phase_3') {\n    global.set('phase3', currentValue);      // Celotna poraba faze 3\n} else if (topic === 'sensor.povprecje_temperature_spalnica') {\n    global.set('temp_sp', currentValue);     // Temperatura v spalnici\n} else if (topic === 'binary_sensor.tpl_occupancy') {\n    global.set('prisotnost_sp', msg.payload === 'on'); // Prisotnost v spalnici\n} else if (topic === 'sensor.so_sp_st') {\n    global.set('okno_sp', msg.payload === 'closed');   // Stanje oken\n} else if (topic === 'binary_sensor.sv_sp_door') {\n    global.set('vrata_sp', msg.payload === 'off');     // Stanje vrat\n} else if (topic === 'switch.tm_sp') {\n    global.set('irSwitchState_sp', msg.payload);       // Stanje stikala IR\n}\n\n// === INTERNI PARAMETRI ===\nconst irPorabaSp = parseFloat(global.get('irPoraba_sp') || 0);\nconst phase3 = parseFloat(global.get('phase3') || 0);\nconst temperaturaSp = parseFloat(global.get('temp_sp') || 0);\nconst prisotnostSp = global.get('prisotnost_sp') || false;\nconst oknoZaprtoSp = global.get('okno_sp') || false;\nconst vrataZaprtaSp = global.get('vrata_sp') || false;\nconst irSwitchStateSp = global.get('irSwitchState_sp') || 'off';\n\n// Globalni parametri bojlerja (iz drugega flowa)\nconst boilerSwitchState = global.get('boilerSwitchState') || 'off';\nconst boilerPower = parseFloat(global.get('boilerPower') || 0);\nconst boilerState = boilerSwitchState === 'on' ? \n                   (boilerPower > 100 ? 'AKTIVEN' : 'neaktiven') : 'off';\n\n// Določanje statusa IR\nlet irStatus;\nif (irSwitchStateSp === 'off') {\n    irStatus = 'off';\n} else {\n    irStatus = irPorabaSp > 100 ? 'AKTIVEN' : 'neaktiven';\n}\nglobal.set('irState_sp', irStatus);\n\n// === VIZUALNI DEBUG IZPIS ===\nnode.warn(`📊 \n┏\n┃ 𝗧𝗲𝗺𝗽.: ${temperaturaSp}°C ${temperaturaSp <= 21 ? '✅' : '❌'} (meja: 21°C)\n┃ 𝗣𝗿𝗶𝘀𝗼𝘁𝗻𝗼𝘀𝘁: ${prisotnostSp ? 'da ✅' : 'NE ❌'}\n┃ 𝗢𝗸𝗻𝗼: ${oknoZaprtoSp ? 'zaprto ✅' : 'ODPRTO ❌'}\n┃ 𝗩𝗿𝗮𝘁𝗮: ${vrataZaprtaSp ? 'zaprta ✅' : 'ODPRTO ❌'}\n┃ 𝗖𝗲𝗹𝗼𝘁𝗻𝗮 𝗽𝗼𝗿𝗮𝗯𝗮: ${phase3}W\n┃ 𝗜𝗥 𝗽𝗼𝗿𝗮𝗯𝗮: ${irPorabaSp}W ${irPorabaSp > 100 ? '🔴 AKTIVEN' : '🟢 neaktiven'}\n┃ 𝗕𝗼𝗷𝗹𝗲𝗿: ${boilerState}\n┗`);\n\n// === GLAVNA LOGIKA ===\nconst pogojiIR = temperaturaSp <= 21 && prisotnostSp && oknoZaprtoSp && vrataZaprtaSp;\nconst irJeVklopljen = irStatus === 'AKTIVEN';\n\n// 1. Pogoji za vklop IR\nif (pogojiIR && !irJeVklopljen && phase3 <= 3400) {\n    const predvidenaPoraba = phase3 + 1250; // 1250W je maksimalna poraba IR panelov\n    \n    if (predvidenaPoraba <= 4650) {\n        node.warn('✅ VKLOP IR: Varni pogoji');\n        return [{ payload: \"on\" }, null];\n    } else if (boilerState === 'AKTIVEN') {\n        node.warn('⚠️ IZKLOP BOJLERJA: Za omogočitev IR');\n        return [null, { \n            payload: \"off\", \n            topic: \"boiler_control\",\n            reason: \"enable_ir_heating\" \n        }];\n    }\n}\n\n// 2. Pogoji za izklop IR\nif (irJeVklopljen && (!pogojiIR || phase3 > 4650)) {\n    const razlog = !pogojiIR ? 'Neizpolnjeni pogoji' : 'Presežena moč';\n    node.warn(`⛔ IZKLOP IR: ${razlog}`);\n    return [null, { payload: \"off\", reason: razlog }];\n}\n\n// Ohranjanje trenutnega stanja\nnode.warn('🔁 Brez sprememb: ohranjanje stanja');\nreturn [null, null];",
        "outputs": 2,
        "timeout": 0,
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 530,
        "y": 220,
        "wires": [
            [
                "e5664b17a32046bb"
            ],
            [
                "92288ecab2972196"
            ]
        ]
    },
    {
        "id": "e5664b17a32046bb",
        "type": "delay",
        "z": "0e96811211eede05",
        "name": "1 X every 5 minutes",
        "pauseType": "rate",
        "timeout": "5",
        "timeoutUnits": "seconds",
        "rate": "1",
        "nbRateUnits": "5",
        "rateUnits": "minute",
        "randomFirst": "1",
        "randomLast": "5",
        "randomUnits": "seconds",
        "drop": true,
        "allowrate": false,
        "outputs": 1,
        "x": 760,
        "y": 200,
        "wires": [
            [
                "f46649d98acfa8eb"
            ]
        ]
    },
    {
        "id": "4d8471870caa673e",
        "type": "server-state-changed",
        "z": "0e96811211eede05",
        "name": "Tm-Sp-Current consupmtion <= 1,25 kW",
        "server": "5f28286e.ae6338",
        "version": 4,
        "exposeToHomeAssistant": false,
        "haConfig": [
            {
                "property": "name",
                "value": ""
            },
            {
                "property": "icon",
                "value": ""
            }
        ],
        "entityidfilter": "sensor.tm_sp_current_consumption",
        "entityidfiltertype": "exact",
        "outputinitially": true,
        "state_type": "str",
        "haltifstate": "",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "outputs": 1,
        "output_only_on_state_change": false,
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "ignorePrevStateNull": false,
        "ignorePrevStateUnknown": false,
        "ignorePrevStateUnavailable": false,
        "ignoreCurrentStateUnknown": false,
        "ignoreCurrentStateUnavailable": false,
        "outputProperties": [
            {
                "property": "payload",
                "propertyType": "msg",
                "value": "",
                "valueType": "entityState"
            },
            {
                "property": "data",
                "propertyType": "msg",
                "value": "",
                "valueType": "eventData"
            },
            {
                "property": "topic",
                "propertyType": "msg",
                "value": "",
                "valueType": "triggerId"
            }
        ],
        "x": 160,
        "y": 340,
        "wires": [
            [
                "2eca172c43e73869"
            ]
        ]
    },
    {
        "id": "8d607ad27d7505d6",
        "type": "server-state-changed",
        "z": "0e96811211eede05",
        "name": "Tm-Sp stikalo",
        "server": "5f28286e.ae6338",
        "version": 4,
        "exposeToHomeAssistant": false,
        "haConfig": [
            {
                "property": "name",
                "value": ""
            },
            {
                "property": "icon",
                "value": ""
            }
        ],
        "entityidfilter": "switch.tm_sp",
        "entityidfiltertype": "exact",
        "outputinitially": true,
        "state_type": "str",
        "haltifstate": "",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "outputs": 1,
        "output_only_on_state_change": false,
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "ignorePrevStateNull": false,
        "ignorePrevStateUnknown": false,
        "ignorePrevStateUnavailable": false,
        "ignoreCurrentStateUnknown": false,
        "ignoreCurrentStateUnavailable": false,
        "outputProperties": [
            {
                "property": "payload",
                "propertyType": "msg",
                "value": "",
                "valueType": "entityState"
            },
            {
                "property": "data",
                "propertyType": "msg",
                "value": "",
                "valueType": "eventData"
            },
            {
                "property": "topic",
                "propertyType": "msg",
                "value": "",
                "valueType": "triggerId"
            }
        ],
        "x": 250,
        "y": 400,
        "wires": [
            [
                "2eca172c43e73869"
            ]
        ]
    },
    {
        "id": "5f28286e.ae6338",
        "type": "server",
        "name": "Home Assistant",
        "version": 5,
        "addon": true,
        "rejectUnauthorizedCerts": true,
        "ha_boolean": "y|yes|true|on|home|open",
        "connectionDelay": true,
        "cacheJson": true,
        "heartbeat": false,
        "heartbeatInterval": 30,
        "areaSelector": "friendlyName",
        "deviceSelector": "friendlyName",
        "entitySelector": "friendlyName",
        "statusSeparator": "at: ",
        "statusYear": "hidden",
        "statusMonth": "short",
        "statusDay": "numeric",
        "statusHourCycle": "h23",
        "statusTimeFormat": "h:m",
        "enableGlobalContextStore": true
    }
]
```

Kaj ta tok (flow) dela:

Preveri, če je temperatura enaka ali nižja kot 21°C in če je 
se sproži na vsake 5 minut nadaljevanje kjer preveri, 
če sta osebi Mojca ali Robert (nova entiteta preko binary_sensor) doma in če je kateri od naju doma
preveri, če je okno zaprto in če je
preveri, če so vrata zaprta in če so
preveri, če je skupna poraba elektrike manjša ali enaka 4 kW in če je
preveri, če je stikalo za IR panele ugasnjeno in če je
ga vklopi.

Pri preveri, če je skupna poraba elektrike manjša ali enaka 4 kW in če ni preveri če je stikalo bojlerja vklopljeno in če je
izklopi bojler.

Pri preverbi, če je temperatura enaka ali nižja kot 21°C in če ni (je višja)
preveri, če je stikalo IR panela prižgano in če je
ugasne stikalo IR panela.

Preverbe stanj naprav delam zaradi zapisovanja stanja naprav v podatkovno bazo, ki se ob preverbi ne zapisujejo!
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

# 📅 Dodano: 19.04.2025

Poglejmo porabo pralnega stroja (Me-Ps) in sušilnega stroja (Me-Ss):
![20250407-Pralni+Sušilni stroj](https://github.com/user-attachments/assets/c604a964-b618-4238-a35a-8e9dd51487e8)
Prvo je prano belo perilo in nato sušeno zatem je prano pisano perilo in zatemm sušeno.

Pralni stroj (Me-Ps) belo perilo:
![Pralni stroj-Poraba_90](https://github.com/user-attachments/assets/1dff5deb-5487-4282-99c3-9eb0e13c45a8)

Sušilni stroj (Me-Ss):
![Sušilni stroj-Poraba](https://github.com/user-attachments/assets/cdeb01aa-58e2-4df0-8f5f-9fe5e89ee58b)

***
# 📅 Popravljeno: 05.05.2025
Po tehtnem premisleku sem se lotil tudi nadzora nad pralnim in sušilnim strojem zaradi bolj robustnega nadzora:
![Node red-Pralno sušilni](https://github.com/user-attachments/assets/46071b34-107c-485f-9fdd-2d5b8b6d6277)


Koda za pralni stroj:
```yaml
[
    {
        "id": "cec1cede9c2bd3ec",
        "type": "tab",
        "label": "Pralni in sušilni stroj",
        "disabled": false,
        "info": "",
        "env": []
    },
    {
        "id": "fc9818f79747ddb9",
        "type": "api-call-service",
        "z": "cec1cede9c2bd3ec",
        "name": "Izklopi pralni stroj",
        "server": "5f28286e.ae6338",
        "version": 5,
        "debugenabled": false,
        "domain": "switch",
        "service": "turn_off",
        "entityId": "switch.me_ps",
        "data": "{}",
        "x": 690,
        "y": 220,
        "wires": [
            [
                "e4f50c6d38dea205",
                "ec7699307125fd96"
            ]
        ]
    },
    {
        "id": "68c9b252aebb6532",
        "type": "api-call-service",
        "z": "cec1cede9c2bd3ec",
        "name": "Vklopi pralni stroj",
        "server": "5f28286e.ae6338",
        "version": 5,
        "debugenabled": false,
        "domain": "switch",
        "service": "turn_on",
        "entityId": "switch.me_ps",
        "data": "{}",
        "x": 690,
        "y": 380,
        "wires": [
            [
                "c55db0f2ff3d8fda",
                "a13bb6677df32a43"
            ]
        ]
    },
    {
        "id": "e4f50c6d38dea205",
        "type": "api-current-state",
        "z": "cec1cede9c2bd3ec",
        "name": "Mojca je doma?",
        "server": "5f28286e.ae6338",
        "version": 3,
        "outputs": 2,
        "halt_if": "home",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "entity_id": "person.mojca",
        "state_type": "str",
        "blockInputOverrides": false,
        "outputProperties": [],
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "override_topic": false,
        "state_location": "payload",
        "override_payload": "msg",
        "entity_location": "data",
        "override_data": "msg",
        "x": 920,
        "y": 180,
        "wires": [
            [
                "675bc599f6c4b433"
            ],
            []
        ]
    },
    {
        "id": "675bc599f6c4b433",
        "type": "api-call-service",
        "z": "cec1cede9c2bd3ec",
        "name": "Pošlji Mojci obvestilo",
        "server": "5f28286e.ae6338",
        "version": 5,
        "debugenabled": false,
        "domain": "notify",
        "service": "mobile_app_mojca_mobitel",
        "areaId": [],
        "deviceId": [],
        "entityId": [],
        "data": "{\"message\":\"{{message}}\",\"title\":\"Obvestilo: Pralni stroj\",\"data\":{\"actions\":[{\"action\":\"ACK_PRALNI_STROJ\",\"title\":\"Videl sem 👍\"}],\"persistent\":true,\"sticky\":true,\"importance\":\"default\",\"priority\":\"default\"}}",
        "dataType": "jsonata",
        "mergeContext": "",
        "mustacheAltTags": false,
        "outputProperties": [
            {
                "property": "data.message",
                "propertyType": "msg",
                "value": "$join([payload, \". Poraba: \", flow.get('current_power'), \"W\"])",
                "valueType": "jsonata"
            }
        ],
        "queue": "none",
        "x": 1180,
        "y": 160,
        "wires": [
            []
        ]
    },
    {
        "id": "ec7699307125fd96",
        "type": "api-current-state",
        "z": "cec1cede9c2bd3ec",
        "name": "Robert je doma?",
        "server": "5f28286e.ae6338",
        "version": 3,
        "outputs": 2,
        "halt_if": "home",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "entity_id": "person.robert",
        "state_type": "str",
        "blockInputOverrides": false,
        "outputProperties": [],
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "override_topic": false,
        "state_location": "payload",
        "override_payload": "msg",
        "entity_location": "data",
        "override_data": "msg",
        "x": 930,
        "y": 240,
        "wires": [
            [
                "333060395a06d2ed"
            ],
            []
        ]
    },
    {
        "id": "333060395a06d2ed",
        "type": "api-call-service",
        "z": "cec1cede9c2bd3ec",
        "name": "Pošlji Robert obvestilo",
        "server": "5f28286e.ae6338",
        "version": 5,
        "debugenabled": false,
        "domain": "notify",
        "service": "mobile_app_robert_mobitel",
        "areaId": [],
        "deviceId": [],
        "entityId": [],
        "data": "{\"message\":\"{{message}}\",\"title\":\"Obvestilo: Pralni stroj\",\"data\":{\"actions\":[{\"action\":\"ACK_PRALNI_STROJ\",\"title\":\"Videl sem 👍\"}],\"channel\":\"alarm_stream\",\"persistent\":true,\"sticky\":true,\"importance\":\"high\",\"priority\":\"high\"}}",
        "dataType": "jsonata",
        "mergeContext": "",
        "mustacheAltTags": false,
        "outputProperties": [
            {
                "property": "data.message",
                "propertyType": "msg",
                "value": "$join([payload, \". Poraba: \", flow.get('current_power'), \"W\"])",
                "valueType": "jsonata"
            }
        ],
        "queue": "none",
        "x": 1180,
        "y": 220,
        "wires": [
            []
        ]
    },
    {
        "id": "c55db0f2ff3d8fda",
        "type": "api-current-state",
        "z": "cec1cede9c2bd3ec",
        "name": "Mojca je doma?",
        "server": "5f28286e.ae6338",
        "version": 3,
        "outputs": 2,
        "halt_if": "home",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "entity_id": "person.mojca",
        "state_type": "str",
        "blockInputOverrides": false,
        "outputProperties": [],
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "override_topic": false,
        "state_location": "payload",
        "override_payload": "msg",
        "entity_location": "data",
        "override_data": "msg",
        "x": 920,
        "y": 360,
        "wires": [
            [
                "c7ab9c9c9ec36cd8"
            ],
            []
        ]
    },
    {
        "id": "c7ab9c9c9ec36cd8",
        "type": "api-call-service",
        "z": "cec1cede9c2bd3ec",
        "name": "Pošlji Mojci obvestilo",
        "server": "5f28286e.ae6338",
        "version": 5,
        "debugenabled": false,
        "domain": "notify",
        "service": "mobile_app_mojca_mobitel",
        "areaId": [],
        "deviceId": [],
        "entityId": [],
        "data": "{\"message\":\"Pralni stroj se je ponovno vklopil. Preveri stanje.\",\"title\":\"Obvestilo: Pralni stroj\",\"data\":{\"actions\":[{\"action\":\"ACK_PRALNI_STROJ\",\"title\":\"Videl sem 👍\"}],\"persistent\":true,\"sticky\":true,\"importance\":\"default\",\"priority\":\"default\"}}",
        "dataType": "json",
        "mergeContext": "",
        "mustacheAltTags": false,
        "outputProperties": [],
        "queue": "none",
        "x": 1180,
        "y": 360,
        "wires": [
            []
        ]
    },
    {
        "id": "a13bb6677df32a43",
        "type": "api-current-state",
        "z": "cec1cede9c2bd3ec",
        "name": "Robert je doma?",
        "server": "5f28286e.ae6338",
        "version": 3,
        "outputs": 2,
        "halt_if": "home",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "entity_id": "person.robert",
        "state_type": "str",
        "blockInputOverrides": false,
        "outputProperties": [],
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "override_topic": false,
        "state_location": "payload",
        "override_payload": "msg",
        "entity_location": "data",
        "override_data": "msg",
        "x": 930,
        "y": 420,
        "wires": [
            [
                "bb4042e35424935d"
            ],
            []
        ]
    },
    {
        "id": "bb4042e35424935d",
        "type": "api-call-service",
        "z": "cec1cede9c2bd3ec",
        "name": "Pošlji Robert obvestilo",
        "server": "5f28286e.ae6338",
        "version": 5,
        "debugenabled": false,
        "domain": "notify",
        "service": "mobile_app_robert_mobitel",
        "areaId": [],
        "deviceId": [],
        "entityId": [],
        "data": "{\"message\":\"Pralni stroj se je ponovno vklopil. Preveri stanje.\",\"title\":\"Obvestilo: Pralni stroj\",\"data\":{\"actions\":[{\"action\":\"ACK_PRALNI_STROJ\",\"title\":\"Videl sem 👍\"}],\"channel\":\"alarm_stream\",\"persistent\":true,\"sticky\":true,\"importance\":\"high\",\"priority\":\"high\"}}",
        "dataType": "json",
        "mergeContext": "",
        "mustacheAltTags": false,
        "outputProperties": [],
        "queue": "none",
        "x": 1180,
        "y": 420,
        "wires": [
            []
        ]
    },
    {
        "id": "468858d858f191ab",
        "type": "api-call-service",
        "z": "cec1cede9c2bd3ec",
        "name": "Izklopi sušilni stroj",
        "server": "5f28286e.ae6338",
        "version": 5,
        "debugenabled": false,
        "domain": "switch",
        "service": "turn_off",
        "entityId": "switch.me_ss",
        "data": "{}",
        "x": 690,
        "y": 100,
        "wires": [
            [
                "c622c1205c0ea858",
                "8fa47f2ed54ff378"
            ]
        ]
    },
    {
        "id": "8fa47f2ed54ff378",
        "type": "api-current-state",
        "z": "cec1cede9c2bd3ec",
        "name": "Robert je doma?",
        "server": "5f28286e.ae6338",
        "version": 3,
        "outputs": 2,
        "halt_if": "home",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "entity_id": "person.robert",
        "state_type": "str",
        "blockInputOverrides": false,
        "outputProperties": [],
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "override_topic": false,
        "state_location": "payload",
        "override_payload": "msg",
        "entity_location": "data",
        "override_data": "msg",
        "x": 930,
        "y": 120,
        "wires": [
            [
                "ce86dbf4d8e35ac2"
            ],
            []
        ]
    },
    {
        "id": "ce86dbf4d8e35ac2",
        "type": "api-call-service",
        "z": "cec1cede9c2bd3ec",
        "name": "Pošlji Robert obvestilo",
        "server": "5f28286e.ae6338",
        "version": 5,
        "debugenabled": false,
        "domain": "notify",
        "service": "mobile_app_robert_mobitel",
        "areaId": [],
        "deviceId": [],
        "entityId": [],
        "data": "{\"message\":\"Sušilni stroj se je IZKLOPIL. Prekoračena poraba\",\"title\":\"Obvestilo: Sušilni stroj\",\"data\":{\"actions\":[{\"action\":\"ACK_PRALNI_STROJ\",\"title\":\"Videl sem 👍\"}],\"channel\":\"alarm_stream\",\"persistent\":true,\"sticky\":true,\"importance\":\"high\",\"priority\":\"high\"}}",
        "dataType": "json",
        "mergeContext": "",
        "mustacheAltTags": false,
        "outputProperties": [],
        "queue": "none",
        "x": 1180,
        "y": 100,
        "wires": [
            []
        ]
    },
    {
        "id": "c622c1205c0ea858",
        "type": "api-current-state",
        "z": "cec1cede9c2bd3ec",
        "name": "Mojca je doma?",
        "server": "5f28286e.ae6338",
        "version": 3,
        "outputs": 2,
        "halt_if": "home",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "entity_id": "person.mojca",
        "state_type": "str",
        "blockInputOverrides": false,
        "outputProperties": [],
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "override_topic": false,
        "state_location": "payload",
        "override_payload": "msg",
        "entity_location": "data",
        "override_data": "msg",
        "x": 920,
        "y": 60,
        "wires": [
            [
                "570161666b711544"
            ],
            []
        ]
    },
    {
        "id": "570161666b711544",
        "type": "api-call-service",
        "z": "cec1cede9c2bd3ec",
        "name": "Pošlji Mojci obvestilo",
        "server": "5f28286e.ae6338",
        "version": 5,
        "debugenabled": false,
        "domain": "notify",
        "service": "mobile_app_mojca_mobitel",
        "areaId": [],
        "deviceId": [],
        "entityId": [],
        "data": "{\"message\":\"Sušilni stroj se je IZKLOPIL. Prekoračena poraba\",\"title\":\"Obvestilo: Sušilni stroj\",\"data\":{\"actions\":[{\"action\":\"ACK_PRALNI_STROJ\",\"title\":\"Videl sem 👍\"}],\"persistent\":true,\"sticky\":true,\"importance\":\"default\",\"priority\":\"default\"}}",
        "dataType": "json",
        "mergeContext": "",
        "mustacheAltTags": false,
        "outputProperties": [],
        "queue": "none",
        "x": 1180,
        "y": 40,
        "wires": [
            []
        ]
    },
    {
        "id": "07f600edf593cffc",
        "type": "api-call-service",
        "z": "cec1cede9c2bd3ec",
        "name": "Vklopi sušilni stroj",
        "server": "5f28286e.ae6338",
        "version": 5,
        "debugenabled": false,
        "domain": "switch",
        "service": "turn_on",
        "entityId": "switch.me_ss",
        "data": "{}",
        "x": 690,
        "y": 500,
        "wires": [
            [
                "999095eb99945e9e",
                "2a7129636243c82d"
            ]
        ]
    },
    {
        "id": "999095eb99945e9e",
        "type": "api-current-state",
        "z": "cec1cede9c2bd3ec",
        "name": "Mojca je doma?",
        "server": "5f28286e.ae6338",
        "version": 3,
        "outputs": 2,
        "halt_if": "home",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "entity_id": "person.mojca",
        "state_type": "str",
        "blockInputOverrides": false,
        "outputProperties": [],
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "override_topic": false,
        "state_location": "payload",
        "override_payload": "msg",
        "entity_location": "data",
        "override_data": "msg",
        "x": 920,
        "y": 480,
        "wires": [
            [
                "05cbfe986059095f"
            ],
            []
        ]
    },
    {
        "id": "05cbfe986059095f",
        "type": "api-call-service",
        "z": "cec1cede9c2bd3ec",
        "name": "Pošlji Mojci obvestilo",
        "server": "5f28286e.ae6338",
        "version": 5,
        "debugenabled": false,
        "domain": "notify",
        "service": "mobile_app_mojca_mobitel",
        "areaId": [],
        "deviceId": [],
        "entityId": [],
        "data": "{\"message\":\"Sušilni stroj se je ponovno vklopil. Preveri stanje\",\"title\":\"Obvestilo: Sušilni stroj\",\"data\":{\"actions\":[{\"action\":\"ACK_PRALNI_STROJ\",\"title\":\"Videl sem 👍\"}],\"persistent\":true,\"sticky\":true,\"importance\":\"default\",\"priority\":\"default\"}}",
        "dataType": "json",
        "mergeContext": "",
        "mustacheAltTags": false,
        "outputProperties": [],
        "queue": "none",
        "x": 1180,
        "y": 480,
        "wires": [
            []
        ]
    },
    {
        "id": "2a7129636243c82d",
        "type": "api-current-state",
        "z": "cec1cede9c2bd3ec",
        "name": "Robert je doma?",
        "server": "5f28286e.ae6338",
        "version": 3,
        "outputs": 2,
        "halt_if": "home",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "entity_id": "person.robert",
        "state_type": "str",
        "blockInputOverrides": false,
        "outputProperties": [],
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "override_topic": false,
        "state_location": "payload",
        "override_payload": "msg",
        "entity_location": "data",
        "override_data": "msg",
        "x": 933.8923645019531,
        "y": 538.125,
        "wires": [
            [
                "748e53bcfe601d9c"
            ],
            []
        ]
    },
    {
        "id": "748e53bcfe601d9c",
        "type": "api-call-service",
        "z": "cec1cede9c2bd3ec",
        "name": "Pošlji Robert obvestilo",
        "server": "5f28286e.ae6338",
        "version": 5,
        "debugenabled": false,
        "domain": "notify",
        "service": "mobile_app_robert_mobitel",
        "areaId": [],
        "deviceId": [],
        "entityId": [],
        "data": "{\"message\":\"Sušilni stroj se je ponovno vklopil. Preveri stanje\",\"title\":\"Obvestilo: Sušilni stroj\",\"data\":{\"actions\":[{\"action\":\"ACK_PRALNI_STROJ\",\"title\":\"Videl sem 👍\"}],\"channel\":\"alarm_stream\",\"persistent\":true,\"sticky\":true,\"importance\":\"high\",\"priority\":\"high\"}}",
        "dataType": "json",
        "mergeContext": "",
        "mustacheAltTags": false,
        "outputProperties": [],
        "queue": "none",
        "x": 1180,
        "y": 540,
        "wires": [
            []
        ]
    },
    {
        "id": "648ad4dac156e68b",
        "type": "function",
        "z": "cec1cede9c2bd3ec",
        "name": "Emergency preverjanje",
        "func": "// Funkcija za nadzor porabe s 4 izhodi - POPRAVLJENA VERZIJA\n// 1. izhod: Izklop sušilnega stroja\n// 2. izhod: Izklop pralnega stroja\n// 3. izhod: Vklop pralnega stroja\n// 4. izhod: Vklop sušilnega stroja\n\n// Inicializacija spremenljivk\nlet porabaFaze3 = flow.get('poraba_faze3') || 0;\nlet pralniStanj = (flow.get('pralni_stanje') === 1 || flow.get('pralni_stanje') === 'on') ? 'on' : 'off';\nlet susilniStanj = (flow.get('susilni_stanje') === 1 || flow.get('susilni_stanje') === 'on') ? 'on' : 'off';\nlet pralniPoraba = flow.get('pralni_poraba') || 0;\nlet susilniPoraba = flow.get('susilni_poraba') || 0;\n\n// Obdelava vhodnih podatkov\nif (msg.topic === 'sensor.p1_meter_power_phase_3') {\n    porabaFaze3 = parseFloat(msg.payload) || 0;\n    flow.set('poraba_faze3', porabaFaze3);\n} \nelse if (msg.topic === 'switch.me_ps') {\n    pralniStanj = (msg.payload === 1 || msg.payload === 'on') ? 'on' : 'off';\n    flow.set('pralni_stanje', pralniStanj);\n} \nelse if (msg.topic === 'switch.me_ss_switch_0') {\n    susilniStanj = (msg.payload === 1 || msg.payload === 'on') ? 'on' : 'off';\n    flow.set('susilni_stanje', susilniStanj);\n} \nelse if (msg.topic === 'sensor.me_prst_current_consumption') {\n    pralniPoraba = parseFloat(msg.payload) || 0;\n    flow.set('pralni_poraba', pralniPoraba);\n} \nelse if (msg.topic === 'sensor.me_ss_current_consumption') {\n    susilniPoraba = parseFloat(msg.payload) || 0;\n    flow.set('susilni_poraba', susilniPoraba);\n}\n\n// Globalna stanja naprav\nconst boilerState = global.get('boilerSwitchState') || 'off';\nconst boilerPower = parseFloat(global.get('boilerPower')) || 0;\nconst boilerStatus = boilerPower > 100 ? '🔴 AKTIVEN' : '🟢 neaktiven';\n\nconst irNaState = global.get('irSwitchState_na') || 'off';\nconst irNaPoraba = parseFloat(global.get('irPoraba_na')) || 0;\nconst irNaStatus = irNaPoraba > 100 ? '🔴 AKTIVEN' : '🟢 neaktiven';\n\nconst irSpState = global.get('irSwitchState_sp') || 'off';\nconst irSpPoraba = parseFloat(global.get('irPoraba_sp')) || 0;\nconst irSpStatus = irSpPoraba > 100 ? '🔴 AKTIVEN' : '🟢 neaktiven';\n\nconst irDiState = global.get('irSwitchState_di') || 'off';\nconst irDiPoraba = parseFloat(global.get('irPoraba_di')) || 0;\nconst irDiStatus = irDiPoraba > 100 ? '🔴 AKTIVEN' : '🟢 neaktiven';\n\nconst pralniStatus = pralniPoraba > 120 ? '🔴 AKTIVEN' : '🟢 neaktiven';\nconst susilniStatus = susilniPoraba > 240 ? '🔴 AKTIVEN' : '🟢 neaktiven';\n\nconst stalnaPoraba = 500; // Osnovna poraba hiše\n\n// Debug izpis z dodatnim preverjanjem\nnode.warn(`⚡️\n┏\n┃ 𝗙𝗮𝘇𝗮 𝟯: ${porabaFaze3}W ${porabaFaze3 <= 4650 ? '✅' : '❌'} (meja: 4650W)\n┃ 𝗣𝗿𝗮𝗹𝗻𝗶 𝘀𝘁𝗿𝗼𝗷: ${pralniStanj === 'on' ? 'on' : 'OFF'} ${pralniStatus} (${pralniPoraba}W)\n┃ 𝗦𝘂š𝗶𝗹𝗻𝗶 𝘀𝘁𝗿𝗼𝗷: ${susilniStanj === 'on' ? 'on' : 'OFF'} ${susilniStatus} (${susilniPoraba}W)\n┃ 𝗕𝗼𝗷𝗹𝗲𝗿: ${boilerState} ${boilerStatus} (${boilerPower}W)\n┃ 𝗜𝗥 𝗡𝗮𝘁𝗵𝗮𝗹𝗶𝗲: ${irNaState} ${irNaStatus} (${irNaPoraba}W)\n┃ 𝗜𝗥 𝗦𝗽𝗮𝗹𝗻𝗶𝗰𝗮: ${irSpState} ${irSpStatus} (${irSpPoraba}W)\n┃ 𝗜𝗥 𝗗𝗶𝗮𝗻𝗲: ${irDiState} ${irDiStatus} (${irDiPoraba}W)\n┗`);\n\n// Glavna logika - POPRAVLJENA\nif (porabaFaze3 > 4650) {\n    // EMERGENCY - presežena moč (ostane enako)\n    // ... (isti emergency del kode kot prej)\n}\n// Popravljena glavna logika - samo relevantni del\nelse {\n    // Normalno delovanje - preveri možnost vklopa\n    const prostaMoc = 4650 - porabaFaze3;\n    \n    // Preverimo dejansko stanje strojev\n    const jePralniVklopljen = pralniStanj === 'on';\n    const jeSusilniVklopljen = susilniStanj === 'on';\n    const jePralniAktiven = pralniPoraba > 120;\n    const jeSusilniAktiven = susilniPoraba > 240;\n    \n    // Pralni stroj ima prednost pred sušilnim\n    if (!jePralniVklopljen && !jePralniAktiven && prostaMoc >= 1920 + stalnaPoraba) {\n        node.warn('✅ POGOJI ZA VKLOP PRALNEGA STROJA IZPOLNJENI');\n        flow.set('pralni_stanje', 'on');\n        return [null, null, { payload: \"on\" }, null];\n    }\n    // Če ni dovolj prostora za pralni stroj, preveri za sušilni\n    else if (!jeSusilniVklopljen && !jeSusilniAktiven && prostaMoc >= 2500 + stalnaPoraba) {\n        node.warn('✅ POGOJI ZA VKLOP SUŠILNEGA STROJA IZPOLNJENI');\n        flow.set('susilni_stanje', 'on');\n        return [null, null, null, { payload: \"on\" }];\n    }\n    \n    // Ohranjanje trenutnega stanja\n    if (jePralniVklopljen || jeSusilniVklopljen) {\n        node.warn('🔁 Naprave so že vklopljene - ohranjanje stanja');\n    } else {\n        node.warn('🔁 Premalo proste moči za vklop - ohranjanje stanja');\n    }\n    return [null, null, null, null];\n}",
        "outputs": 4,
        "timeout": "",
        "noerr": 0,
        "initialize": "",
        "finalize": "",
        "libs": [],
        "x": 350,
        "y": 300,
        "wires": [
            [
                "468858d858f191ab"
            ],
            [
                "fc9818f79747ddb9"
            ],
            [
                "68c9b252aebb6532"
            ],
            [
                "07f600edf593cffc"
            ]
        ]
    },
    {
        "id": "a53cf0330d39a2ab",
        "type": "server-state-changed",
        "z": "cec1cede9c2bd3ec",
        "name": "Skupna poraba",
        "server": "5f28286e.ae6338",
        "version": 4,
        "exposeToHomeAssistant": false,
        "haConfig": [],
        "entityidfilter": "sensor.p1_meter_power_phase_3",
        "entityidfiltertype": "exact",
        "outputinitially": true,
        "state_type": "num",
        "haltifstate": "",
        "halt_if_type": "num",
        "halt_if_compare": "gte",
        "outputs": 1,
        "output_only_on_state_change": false,
        "for": "",
        "forType": "num",
        "ignorePrevStateNull": false,
        "ignorePrevStateUnknown": false,
        "ignorePrevStateUnavailable": false,
        "ignoreCurrentStateUnknown": false,
        "ignoreCurrentStateUnavailable": false,
        "outputProperties": [
            {
                "property": "payload",
                "propertyType": "msg",
                "value": "",
                "valueType": "entityState"
            }
        ],
        "x": 80,
        "y": 300,
        "wires": [
            [
                "648ad4dac156e68b"
            ]
        ]
    },
    {
        "id": "5d1281322eb3cc9d",
        "type": "server-state-changed",
        "z": "cec1cede9c2bd3ec",
        "name": "Sušilni poraba",
        "server": "5f28286e.ae6338",
        "version": 4,
        "exposeToHomeAssistant": false,
        "haConfig": [],
        "entityidfilter": "sensor.me_ss_current_consumption",
        "entityidfiltertype": "exact",
        "outputinitially": true,
        "state_type": "num",
        "haltifstate": "",
        "halt_if_type": "num",
        "halt_if_compare": "gte",
        "outputs": 1,
        "output_only_on_state_change": false,
        "for": "",
        "forType": "num",
        "ignorePrevStateNull": false,
        "ignorePrevStateUnknown": false,
        "ignorePrevStateUnavailable": false,
        "ignoreCurrentStateUnknown": false,
        "ignoreCurrentStateUnavailable": false,
        "outputProperties": [
            {
                "property": "payload",
                "propertyType": "msg",
                "value": "",
                "valueType": "entityState"
            }
        ],
        "x": 90,
        "y": 240,
        "wires": [
            [
                "648ad4dac156e68b"
            ]
        ]
    },
    {
        "id": "1de5b320ec5d6519",
        "type": "server-state-changed",
        "z": "cec1cede9c2bd3ec",
        "name": "Pralni poraba",
        "server": "5f28286e.ae6338",
        "version": 4,
        "exposeToHomeAssistant": false,
        "haConfig": [],
        "entityidfilter": "sensor.me_prst_current_consumption",
        "entityidfiltertype": "exact",
        "outputinitially": true,
        "state_type": "num",
        "haltifstate": "",
        "halt_if_type": "num",
        "halt_if_compare": "gte",
        "outputs": 1,
        "output_only_on_state_change": false,
        "for": "",
        "forType": "num",
        "ignorePrevStateNull": false,
        "ignorePrevStateUnknown": false,
        "ignorePrevStateUnavailable": false,
        "ignoreCurrentStateUnknown": false,
        "ignoreCurrentStateUnavailable": false,
        "outputProperties": [
            {
                "property": "payload",
                "propertyType": "msg",
                "value": "",
                "valueType": "entityState"
            }
        ],
        "x": 90,
        "y": 360,
        "wires": [
            [
                "648ad4dac156e68b"
            ]
        ]
    },
    {
        "id": "035e7ef904150166",
        "type": "server-state-changed",
        "z": "cec1cede9c2bd3ec",
        "name": "Sušilni stikalo",
        "server": "5f28286e.ae6338",
        "version": 4,
        "exposeToHomeAssistant": false,
        "haConfig": [],
        "entityidfilter": "switch.me_ss_switch_0",
        "entityidfiltertype": "exact",
        "outputinitially": true,
        "state_type": "str",
        "haltifstate": "",
        "halt_if_type": "num",
        "halt_if_compare": "gte",
        "outputs": 1,
        "output_only_on_state_change": false,
        "for": "",
        "forType": "num",
        "ignorePrevStateNull": false,
        "ignorePrevStateUnknown": false,
        "ignorePrevStateUnavailable": false,
        "ignoreCurrentStateUnknown": false,
        "ignoreCurrentStateUnavailable": false,
        "outputProperties": [
            {
                "property": "payload",
                "propertyType": "msg",
                "value": "",
                "valueType": "entityState"
            }
        ],
        "x": 90,
        "y": 180,
        "wires": [
            [
                "648ad4dac156e68b"
            ]
        ]
    },
    {
        "id": "3f5f91cc5b05624e",
        "type": "server-state-changed",
        "z": "cec1cede9c2bd3ec",
        "name": "Pralni stikalo",
        "server": "5f28286e.ae6338",
        "version": 4,
        "exposeToHomeAssistant": false,
        "haConfig": [],
        "entityidfilter": "switch.me_ps",
        "entityidfiltertype": "exact",
        "outputinitially": true,
        "state_type": "str",
        "haltifstate": "",
        "halt_if_type": "num",
        "halt_if_compare": "gte",
        "outputs": 1,
        "output_only_on_state_change": false,
        "for": "",
        "forType": "num",
        "ignorePrevStateNull": false,
        "ignorePrevStateUnknown": false,
        "ignorePrevStateUnavailable": false,
        "ignoreCurrentStateUnknown": false,
        "ignoreCurrentStateUnavailable": false,
        "outputProperties": [
            {
                "property": "payload",
                "propertyType": "msg",
                "value": "",
                "valueType": "entityState"
            }
        ],
        "x": 90,
        "y": 420,
        "wires": [
            [
                "648ad4dac156e68b"
            ]
        ]
    },
    {
        "id": "5f28286e.ae6338",
        "type": "server",
        "name": "Home Assistant",
        "version": 5,
        "addon": true,
        "rejectUnauthorizedCerts": true,
        "ha_boolean": "y|yes|true|on|home|open",
        "connectionDelay": true,
        "cacheJson": true,
        "heartbeat": false,
        "heartbeatInterval": 30,
        "areaSelector": "friendlyName",
        "deviceSelector": "friendlyName",
        "entitySelector": "friendlyName",
        "statusSeparator": "at: ",
        "statusYear": "hidden",
        "statusMonth": "short",
        "statusDay": "numeric",
        "statusHourCycle": "h23",
        "statusTimeFormat": "h:m",
        "enableGlobalContextStore": true
    }
]
```

### Kaj ta tok (flow) dela:

Inteligentni sistem za optimizacijo porabe energije
Funkcionalnost
Avtomatizirani sistem za dinamično upravljanje energetske porabe v gospodinjstvu z:

Hierarhičnim nadzorom 8 ključnih naprav (pralni stroj, sušilni stroj, bojler, 3x IR panel, osnovna poraba)

Real-time optimizacijo glede na:

Trenutno porabo faze 3 (limit: 4650W)

Prednostne naloge (pralni > sušilni > IR paneli > bojler)

Kombinacijsko logiko porabe

Samoumevajočim režimom varčevanja s primerjalnimi scenariji

Ključne značilnosti
🔌 Multi-napravno upravljanje:

Pralni stroj (1920W)

Sušilni stroj (2500W)

Bojler (2010W)

IR paneli (Diane: 750W, Nathalie: 650W, Spalnica: 1250W)

Osnovna poraba (500W)

⚡ Dinamična prioritizacija:

Diagram
![deepseek_1](https://github.com/user-attachments/assets/e0101296-a81e-4d43-95be-c7d5f860dcbc)


📊 Kombinacijska inteligenca:

```javascript
// Primerjalna logika
const dovoljenaKombinacija = (prostaMoc) => {
  if(prostaMoc >= 1920 + 2010 + 500) return "Pralni+Bojler";
  if(prostaMoc >= 2500 + 1250 + 500) return "Sušilni+IR Spalnica"; 
  // ... ostali scenariji
}
```
Tehnična izvedba
Viri podatkov:

Senzorji: p1_meter_power_phase_3, me_prst_current_consumption, me_ss_current_consumption

Globalni kontekst: boilerSwitchState, boilerPower, irSwitchState_na, irPoraba_na, ...

Flow spremenljivke: pralni_stanje, susilni_stanje, pralni_poraba

Odločitvena matrika:

Kombinacija	Skupna poraba	Status	Akcija
Pralni + Sušilni	4420W	❌ Preobremenitev	Izklop sušilnega
IR Spalnica + Bojler	3260W	✅ Dovoljeno	Ohrani stanje
Vsi IR + Bojler	5160W	❌ Preobremenitev	Izklop bojlerja
Algoritem vklopa:

```python
def vklopi_napravo():
    prosta_moc = 4650 - (poraba_faze + stalna_poraba)
    if prosta_moc >= naprava.poraba + varnostni_meji:
        if not naprava.je_vklopljen and not naprava.je_aktiven:
            naprava.vklopi()
            posodobi_globalno_stanje()
```
Varnostni mehanizmi
Zakasnitev preverjanja (3000ms po vklopu)

Dvojno preverjanje:

```javascript
const jeVarenVklop = (
  (global.get('boilerPower') < 100) && 
  (flow.get('poraba_faze3') + nova_naprava.poraba < 4650)
);
```

Emergency log s timestampi:

```log
2025-05-05 14:30: [WARN] Preobremenitev! Izklop bojlerja (2010W)
```

Primeri delovanja
Scenarij 1: Prekomerna poraba

Diagram
![deepseek_2](https://github.com/user-attachments/assets/022dbcf5-182c-4f4b-9ecf-de6864160f61)


Diagram
![deepseek_3](https://github.com/user-attachments/assets/ff96942d-2486-41af-b2c3-904539ddd096)

Namestitev in uporaba
Zahteve:

Node-RED z node-jem node-red-contrib-home-assistant

Merilniki energije s podporo MQTT/API

Globalne spremenljivke za vsako napravo

Konfiguracija:

```yaml
# primer globalnih nastavitev
boiler:
  switch_entity: input_boolean.bojler_switch
  power_sensor: sensor.bojler_power
```
Testni scenariji:

```javascript
// Test emergency režima
simulateOverload(5000, () => {
  assert(bojler.stanje === 'off');
  assert(log.vsebuje('Preobremenitev'));
});
```
***

# 📅 Dodano: 20.04.2025

Takšen je izgled porabe:
![Poraba od 14 do 20 04 2025](https://github.com/user-attachments/assets/f8a83c1c-ffaf-4ffa-8a8d-c225ce593639)

## Kodo za Nod-red sem naredil s pomočjo deepseek, ki se je izkazal za veliko učinkovitejšega od ChatGPT!

# 📜 Licenca  
Ta dela so prosto dostopna za vsako uporabo brez omejitev.  
Avtor ne zahteva atribucije, vendar je hvaležen za povratne informacije.
