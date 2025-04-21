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

💡 Primer za bojler.
![20250409-Bioler automation](https://github.com/user-attachments/assets/01e0d399-cc46-4d67-b7e6-06fff2f540c2)

✍️ Koda za nod: 
```yaml
[
    {
        "id": "4cbb8938f808bb5e",
        "type": "server-state-changed",
        "z": "b98142f5dd0202f2",
        "name": "Total consumption, less than or equal to 4 kW",
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
        "outputinitially": false,
        "state_type": "num",
        "haltifstate": "4000",
        "halt_if_type": "num",
        "halt_if_compare": "lte",
        "outputs": 2,
        "output_only_on_state_change": true,
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "ignorePrevStateNull": false,
        "ignorePrevStateUnknown": false,
        "ignorePrevStateUnavailable": false,
        "ignoreCurrentStateUnknown": false,
        "ignoreCurrentStateUnavailable": false,
        "outputProperties": [],
        "x": 170,
        "y": 60,
        "wires": [
            [
                "2714d185e9b1a5e8"
            ],
            [
                "884081337fdfd3af"
            ]
        ]
    },
    {
        "id": "2714d185e9b1a5e8",
        "type": "delay",
        "z": "b98142f5dd0202f2",
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
        "x": 500,
        "y": 40,
        "wires": [
            [
                "2a4203c12b093c8d"
            ]
        ]
    },
    {
        "id": "db18dd2777e3cc01",
        "type": "api-call-service",
        "z": "b98142f5dd0202f2",
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
        "x": 960,
        "y": 40,
        "wires": [
            []
        ]
    },
    {
        "id": "d86bec24d2daf958",
        "type": "api-call-service",
        "z": "b98142f5dd0202f2",
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
        "x": 960,
        "y": 120,
        "wires": [
            []
        ]
    },
    {
        "id": "884081337fdfd3af",
        "type": "api-current-state",
        "z": "b98142f5dd0202f2",
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
        "x": 620,
        "y": 100,
        "wires": [
            [
                "d86bec24d2daf958"
            ],
            []
        ]
    },
    {
        "id": "2a4203c12b093c8d",
        "type": "api-current-state",
        "z": "b98142f5dd0202f2",
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
        "x": 730,
        "y": 40,
        "wires": [
            [
                "db18dd2777e3cc01"
            ],
            []
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

💡 Primer za IR panel (ogrevanje):

![20250418-Nod-red-IR spalnica](https://github.com/user-attachments/assets/3f6276c8-76b8-43de-b52e-f3c29cea8a2f)


✍️ Koda za nod:
```yaml
[
    {
        "id": "3d32adea2c4c7661",
        "type": "api-call-service",
        "z": "794794b2e9403922",
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
        "x": 960,
        "y": 80,
        "wires": [
            []
        ]
    },
    {
        "id": "efd06012ca870d20",
        "type": "api-call-service",
        "z": "794794b2e9403922",
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
        "y": 440,
        "wires": [
            []
        ]
    },
    {
        "id": "96a9dfdfcc4877d4",
        "type": "server-state-changed",
        "z": "794794b2e9403922",
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
        "haltifstate": "21",
        "halt_if_type": "num",
        "halt_if_compare": "lte",
        "outputs": 2,
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
        "x": 50,
        "y": 420,
        "wires": [
            [
                "4d0804cc8c72c53b"
            ],
            [
                "46c7910839bd381b"
            ]
        ]
    },
    {
        "id": "bc8282dc1efe125d",
        "type": "api-current-state",
        "z": "794794b2e9403922",
        "name": "Window OFF/ON",
        "server": "5f28286e.ae6338",
        "version": 3,
        "outputs": 2,
        "halt_if": "closed",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "entity_id": "sensor.so_sp_st",
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
        "x": 130,
        "y": 240,
        "wires": [
            [
                "e3fa297a3f3184f0"
            ],
            [
                "efd06012ca870d20"
            ]
        ]
    },
    {
        "id": "1305d4da03553159",
        "type": "api-current-state",
        "z": "794794b2e9403922",
        "name": "Robert or Mojca home",
        "server": "5f28286e.ae6338",
        "version": 3,
        "outputs": 2,
        "halt_if": "on",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "entity_id": "binary_sensor.tpl_occupancy",
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
        "x": 140,
        "y": 300,
        "wires": [
            [
                "bc8282dc1efe125d"
            ],
            [
                "efd06012ca870d20"
            ]
        ]
    },
    {
        "id": "4d0804cc8c72c53b",
        "type": "delay",
        "z": "794794b2e9403922",
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
        "x": 140,
        "y": 360,
        "wires": [
            [
                "1305d4da03553159"
            ]
        ]
    },
    {
        "id": "e3fa297a3f3184f0",
        "type": "api-current-state",
        "z": "794794b2e9403922",
        "name": "Door OFF/ON",
        "server": "5f28286e.ae6338",
        "version": 3,
        "outputs": 2,
        "halt_if": "off",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "entity_id": "binary_sensor.sv_sp_door",
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
        "x": 120,
        "y": 180,
        "wires": [
            [
                "97a1142e22460697"
            ],
            [
                "efd06012ca870d20"
            ]
        ]
    },
    {
        "id": "97a1142e22460697",
        "type": "api-current-state",
        "z": "794794b2e9403922",
        "name": "Total consumption <= 4 kW",
        "server": "5f28286e.ae6338",
        "version": 3,
        "outputs": 2,
        "halt_if": "4000",
        "halt_if_type": "num",
        "halt_if_compare": "lte",
        "entity_id": "sensor.p1_meter_power_phase_3",
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
        "x": 160,
        "y": 120,
        "wires": [
            [
                "080dbc2779d63241"
            ],
            [
                "793b093121567302"
            ]
        ]
    },
    {
        "id": "46c7910839bd381b",
        "type": "api-current-state",
        "z": "794794b2e9403922",
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
        "x": 470,
        "y": 440,
        "wires": [
            [
                "efd06012ca870d20"
            ],
            []
        ]
    },
    {
        "id": "793b093121567302",
        "type": "api-current-state",
        "z": "794794b2e9403922",
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
        "x": 440,
        "y": 180,
        "wires": [
            [
                "c284372eba11273f"
            ],
            []
        ]
    },
    {
        "id": "c284372eba11273f",
        "type": "api-call-service",
        "z": "794794b2e9403922",
        "name": "Turn OFF Boiler",
        "server": "5f28286e.ae6338",
        "version": 5,
        "domain": "switch",
        "service": "turn_off",
        "entityId": [
            "switch.me_bo"
        ],
        "data": "{}",
        "dataType": "json",
        "x": 640,
        "y": 240,
        "wires": [
            [
                "efd06012ca870d20"
            ]
        ]
    },
    {
        "id": "080dbc2779d63241",
        "type": "api-current-state",
        "z": "794794b2e9403922",
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
        "x": 580,
        "y": 100,
        "wires": [
            [
                "3d32adea2c4c7661"
            ],
            []
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

Po tehtnem premisleku sem se lotil tudi nadzora nad pralnim in sušilnim strojem zaradi bolj robustnega nadzora:
![Nod red za pralni in sušilni stroj](https://github.com/user-attachments/assets/93a388e9-5a0c-4904-9ddb-2ebadc206530)

Koda za pralni stroj:
```yaml
[
    {
        "id": "30459f5f562c8a1f",
        "type": "server-state-changed",
        "z": "b98142f5dd0202f2",
        "name": "Total consumption, less than or equal to 4,5 kW",
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
        "outputinitially": false,
        "state_type": "num",
        "haltifstate": "4500",
        "halt_if_type": "num",
        "halt_if_compare": "lte",
        "outputs": 2,
        "output_only_on_state_change": true,
        "for": "0",
        "forType": "num",
        "forUnits": "minutes",
        "ignorePrevStateNull": false,
        "ignorePrevStateUnknown": false,
        "ignorePrevStateUnavailable": false,
        "ignoreCurrentStateUnknown": false,
        "ignoreCurrentStateUnavailable": false,
        "outputProperties": [],
        "x": 180,
        "y": 260,
        "wires": [
            [
                "989138568c271c9e"
            ],
            [
                "check_if_active"
            ]
        ]
    },
    {
        "id": "989138568c271c9e",
        "type": "delay",
        "z": "b98142f5dd0202f2",
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
        "x": 500,
        "y": 220,
        "wires": [
            [
                "74ff7b555bb27d6b"
            ]
        ]
    },
    {
        "id": "e2f3e046d780ab64",
        "type": "api-call-service",
        "z": "b98142f5dd0202f2",
        "name": "Turn ON washing machine",
        "server": "5f28286e.ae6338",
        "version": 5,
        "debugenabled": false,
        "domain": "switch",
        "service": "turn_on",
        "areaId": [],
        "deviceId": [],
        "entityId": [
            "switch.me_ps"
        ],
        "data": "{}",
        "dataType": "json",
        "mergeContext": "",
        "mustacheAltTags": false,
        "outputProperties": [],
        "queue": "none",
        "x": 1060,
        "y": 220,
        "wires": [
            [
                "6a3ff3d60fbbf0a0",
                "5adf39a56fa914e4"
            ]
        ]
    },
    {
        "id": "0e4fa37bc7717492",
        "type": "api-call-service",
        "z": "b98142f5dd0202f2",
        "name": "Turn OFF washing machine",
        "server": "5f28286e.ae6338",
        "version": 5,
        "debugenabled": false,
        "domain": "switch",
        "service": "turn_off",
        "areaId": [],
        "deviceId": [],
        "entityId": [
            "switch.me_ps"
        ],
        "data": "{}",
        "dataType": "json",
        "mergeContext": "",
        "mustacheAltTags": false,
        "outputProperties": [],
        "queue": "none",
        "x": 1060,
        "y": 340,
        "wires": [
            [
                "4225377457d7fddd",
                "00a66e801b6831d9"
            ]
        ]
    },
    {
        "id": "141e0719bc237fc3",
        "type": "api-current-state",
        "z": "b98142f5dd0202f2",
        "name": "Is the washing machine ON",
        "server": "5f28286e.ae6338",
        "version": 3,
        "outputs": 2,
        "halt_if": "on",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "entity_id": "switch.me_ps",
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
        "x": 800,
        "y": 340,
        "wires": [
            [
                "0e4fa37bc7717492"
            ],
            []
        ]
    },
    {
        "id": "74ff7b555bb27d6b",
        "type": "api-current-state",
        "z": "b98142f5dd0202f2",
        "name": "Is the washing machine OFF",
        "server": "5f28286e.ae6338",
        "version": 3,
        "outputs": 2,
        "halt_if": "off",
        "halt_if_type": "str",
        "halt_if_compare": "is",
        "entity_id": "switch.me_ps",
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
        "x": 760,
        "y": 220,
        "wires": [
            [
                "e2f3e046d780ab64"
            ],
            []
        ]
    },
    {
        "id": "check_if_active",
        "type": "api-current-state",
        "z": "b98142f5dd0202f2",
        "name": "Is power greater than 120 W?",
        "server": "5f28286e.ae6338",
        "version": 3,
        "outputs": 2,
        "halt_if": "120",
        "halt_if_type": "num",
        "halt_if_compare": "gt",
        "entity_id": "sensor.me_prst_current_consumption",
        "state_type": "num",
        "blockInputOverrides": false,
        "outputProperties": [],
        "for": "",
        "forType": "num",
        "x": 530,
        "y": 340,
        "wires": [
            [
                "141e0719bc237fc3"
            ],
            []
        ]
    },
    {
        "id": "6a3ff3d60fbbf0a0",
        "type": "api-current-state",
        "z": "b98142f5dd0202f2",
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
        "x": 1300,
        "y": 200,
        "wires": [
            [
                "f4bbd27d70ab2a1e"
            ],
            []
        ]
    },
    {
        "id": "f4bbd27d70ab2a1e",
        "type": "api-call-service",
        "z": "b98142f5dd0202f2",
        "name": "Pošlji Mojci obvestilo",
        "server": "5f28286e.ae6338",
        "version": 5,
        "debugenabled": false,
        "domain": "notify",
        "service": "mobile_app_mojca_mobitel",
        "areaId": [],
        "deviceId": [],
        "entityId": [],
        "data": "{\"message\":\"Pralni stroj se je ponovno vklopil. Preveri stanje.\",\"title\":\"Obvestilo: Pralni stroj\",\"data\":{\"actions\":[{\"action\":\"ACK_PRALNI_STROJ\",\"title\":\"Videl sem 👍\"}],\"channel\":\"alarm_stream\",\"persistent\":true,\"sticky\":true,\"importance\":\"high\",\"priority\":\"high\"}}",
        "dataType": "json",
        "mergeContext": "",
        "mustacheAltTags": false,
        "outputProperties": [],
        "queue": "none",
        "x": 1520,
        "y": 180,
        "wires": [
            []
        ]
    },
    {
        "id": "5adf39a56fa914e4",
        "type": "api-current-state",
        "z": "b98142f5dd0202f2",
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
        "x": 1310,
        "y": 260,
        "wires": [
            [
                "109f8e3feaaaa397"
            ],
            []
        ]
    },
    {
        "id": "109f8e3feaaaa397",
        "type": "api-call-service",
        "z": "b98142f5dd0202f2",
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
        "x": 1520,
        "y": 240,
        "wires": [
            []
        ]
    },
    {
        "id": "4225377457d7fddd",
        "type": "api-current-state",
        "z": "b98142f5dd0202f2",
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
        "x": 1300,
        "y": 320,
        "wires": [
            [
                "5ae4466a9e3b074c"
            ],
            []
        ]
    },
    {
        "id": "5ae4466a9e3b074c",
        "type": "api-call-service",
        "z": "b98142f5dd0202f2",
        "name": "Pošlji Mojci obvestilo",
        "server": "5f28286e.ae6338",
        "version": 5,
        "debugenabled": false,
        "domain": "notify",
        "service": "mobile_app_mojca_mobitel",
        "areaId": [],
        "deviceId": [],
        "entityId": [],
        "data": "{\"message\":\"Pralni stroj se je IZKLOPIL. Prekoračena poraba\",\"title\":\"Obvestilo: Pralni stroj\",\"data\":{\"actions\":[{\"action\":\"ACK_PRALNI_STROJ\",\"title\":\"Videl sem 👍\"}],\"channel\":\"alarm_stream\",\"persistent\":true,\"sticky\":true,\"importance\":\"high\",\"priority\":\"high\"}}",
        "dataType": "json",
        "mergeContext": "",
        "mustacheAltTags": false,
        "outputProperties": [],
        "queue": "none",
        "x": 1520,
        "y": 300,
        "wires": [
            []
        ]
    },
    {
        "id": "00a66e801b6831d9",
        "type": "api-current-state",
        "z": "b98142f5dd0202f2",
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
        "x": 1310,
        "y": 380,
        "wires": [
            [
                "4412586dfb1ffdbf"
            ],
            []
        ]
    },
    {
        "id": "4412586dfb1ffdbf",
        "type": "api-call-service",
        "z": "b98142f5dd0202f2",
        "name": "Pošlji Robert obvestilo",
        "server": "5f28286e.ae6338",
        "version": 5,
        "debugenabled": false,
        "domain": "notify",
        "service": "mobile_app_robert_mobitel",
        "areaId": [],
        "deviceId": [],
        "entityId": [],
        "data": "{\"message\":\"Pralni stroj se je IZKLOPIL. Prekoračena poraba\",\"title\":\"Obvestilo: Pralni stroj\",\"data\":{\"actions\":[{\"action\":\"ACK_PRALNI_STROJ\",\"title\":\"Videl sem 👍\"}],\"channel\":\"alarm_stream\",\"persistent\":true,\"sticky\":true,\"importance\":\"high\",\"priority\":\"high\"}}",
        "dataType": "json",
        "mergeContext": "",
        "mustacheAltTags": false,
        "outputProperties": [],
        "queue": "none",
        "x": 1520,
        "y": 360,
        "wires": [
            []
        ]
    },
    {
        "id": "1bb6c32c123dcb29",
        "type": "inject",
        "z": "b98142f5dd0202f2",
        "name": "",
        "props": [
            {
                "p": "payload"
            },
            {
                "p": "topic",
                "vt": "str"
            }
        ],
        "repeat": "",
        "crontab": "",
        "once": false,
        "onceDelay": 0.1,
        "topic": "",
        "payload": "",
        "payloadType": "date",
        "x": 1100,
        "y": 400,
        "wires": [
            [
                "00a66e801b6831d9"
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

Preveri, če je skupna poraba elektrike manjša ali enaka 4.5 kW in če je 
se sproži na vsake 5 minut nadaljevanje kjer 
preveri, če je stikalo za pralni stroj ugasnjeno in če je 
ga vklopi
ko ga vklopi (pogoj) preveri, če sva z ženo doma in če je pogoj izpolnjen
pošlje sporočilo "Pralni stroj se je ponovno vklopil. Preveri stanje." na mobitel (na mobilnem aparatu morate imeti nameščen Home assistant companion APP, da zadeva deluje), ker je potrebno na stroju ročno pritisniti gumb za nadaljevanje.

Pri preverbi, če je skupna poraba elektrike manjša ali enaka 4.5 kW in če ni (je višja) preveri, 
če je poraba pralnega stroja večja kot 120 W in če je
preveri, če je stikalo pralnega stroja prižgano in če je ugasne stikalo pralnega stroja
ko ga izklopi (pogoj) preveri, če sva z ženo doma in če je pogoj izpolnjen
pošlje sporočilo "Pralni stroj se je IZKLOPIL. Prekoračena poraba" na mobitel (na mobilnem aparatu morate imeti nameščen Home assistant companion APP, da zadeva deluje).

Preverbe stanj delam zaradi zapisovanja stanja naprav v podatkovno bazo!

***

# 📅 Dodano: 20.04.2025

Takšen je izgled porabe:
![Poraba od 14 do 20 04 2025](https://github.com/user-attachments/assets/f8a83c1c-ffaf-4ffa-8a8d-c225ce593639)

