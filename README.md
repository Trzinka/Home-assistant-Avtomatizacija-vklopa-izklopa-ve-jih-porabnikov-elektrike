# 🐣 Projekt v razvoju
___

## Z naslednjimi nastavitvami želim doseči nadzor nad porabo maksimalno dovoljene dogovorjene moči (kW)!

![Sprememba dogovorjene moči](https://github.com/user-attachments/assets/f3fd7e9b-f6cb-4a89-8c5b-d751f2365208)

Treba je razumeti "politiko" elektra.

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

🎯 Pojasniti vam moram, da živimo v dvo družinski hiši kjer imamo trenutno 1 odjemno merilno mesto!

***

Najprej sem moral rešiti dilemo glede uporabe električne pečice, pralnega stroja in sušilnega stroja. Z ženo sva se dogovorila 😄, da istočasno ne vklapljava teh naprav, tukaj pride v poštev tudi, friteza, mikrovalovna, fen za lase (opremljena so s pametnimi stikali ali pa vsaj z merilniki porabe) in še kaj se bi našlo.

Glede na to, da imamo prostore ogrevane s pomočjo IR panelov in da vodo greje bojler se mi dozdeva, da te naprave lahko brez večjega vpliva ob špicah izklapljam.


# Nekaj podatkov o porabnikih
___
| Naprava        | Poraba |
|----------------|--------|
| Sušilni stroj  | 2,4 kW |
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


### Za avtomatizacijo/nadzor uporabljam dodatek Node-red in ne avtomatizacijo predvsem zaradi boljše preglednosi nad potekom avtomatizacije/nadzora.

Primer za bojler.
![20250409-Bioler automation](https://github.com/user-attachments/assets/01e0d399-cc46-4d67-b7e6-06fff2f540c2)

Koda za nod:
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
___

Primer za IR panel (ogrevanje):
![IR panel avtomatizacija](https://github.com/user-attachments/assets/ab840934-22f4-42c6-88eb-60aedb920cac)

Koda za nod:
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
        "x": 1400,
        "y": 60,
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
        "x": 1410,
        "y": 240,
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
        "y": 220,
        "wires": [
            [
                "4d0804cc8c72c53b"
            ],
            [
                "efd06012ca870d20"
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
        "x": 670,
        "y": 120,
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
        "x": 460,
        "y": 160,
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
        "x": 220,
        "y": 200,
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
        "x": 860,
        "y": 100,
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
        "x": 1080,
        "y": 80,
        "wires": [
            [
                "3d32adea2c4c7661"
            ],
            [
                "efd06012ca870d20"
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

📅 Zadnja posodobitev: 10.04.2025


