# Zigbee2MQTT Multi-Instanz

Home-Assistant-Add-on-Repository mit fuenf parallel installierbaren
Zigbee2MQTT-Instanzen, benannt nach den Standorten der Coordinatoren.

| Add-on | Slug | data_path | Coordinator | base_topic | Frontend |
|---|---|---|---|---|---|
| Zigbee2MQTT 1 Küche | `zigbee2mqtt_1_kuche` | `/config/zigbee2mqtt_1_kuche` | 192.168.2.191:6638 | `zigbee2mqtt_1` | 8091 |
| Zigbee2MQTT 2 Büro | `zigbee2mqtt_2_buro` | `/config/zigbee2mqtt_2_buro` | 192.168.2.192:6638 | `zigbee2mqtt_2` | 8092 |
| Zigbee2MQTT 3 Gästezimmer | `zigbee2mqtt_3_gastezimmer` | `/config/zigbee2mqtt_3_gastezimmer` | 192.168.2.193:6638 | `zigbee2mqtt_3` | 8093 |
| Zigbee2MQTT 4 Ankleide | `zigbee2mqtt_4_ankleide` | `/config/zigbee2mqtt_4_ankleide` | 192.168.2.194:6638 | `zigbee2mqtt_4` | 8094 |
| Zigbee2MQTT 6 Außen | `zigbee2mqtt_6_aussen` | `/config/zigbee2mqtt_6_aussen` | 192.168.2.196:6638 | `zigbee2mqtt_6` | 8096 |

Die -Werte bleiben numerisch wie in der Docker-Installation —
sie bestimmen die Entity-IDs in Home Assistant und dürfen sich nicht ändern.

## Was hier passiert

Dieses Repository enthaelt **keinen** eigenen Code und **kein** Dockerfile.
Jede Instanz besteht aus einer `config.json`, die aus der offiziellen
Add-on-Konfiguration erzeugt wird. Veraendert werden nur:

- `name` und `slug` — damit Home Assistant die Instanzen unterscheiden kann
- `options.data_path` — damit sich die Instanzen nicht dieselbe Datenbank teilen
- `ports["8485/tcp"]` auf `null` — sonst kollidieren alle Instanzen auf Host-Port 8485

Das Container-Image ist unveraendert `ghcr.io/zigbee2mqtt/zigbee2mqtt-{arch}`,
also exakt dasselbe wie beim offiziellen Add-on.

## Aktualisierung

`sync-zigbee2mqtt.yml` laeuft taeglich, vergleicht das neueste Release von
[zigbee2mqtt/hassio-zigbee2mqtt](https://github.com/zigbee2mqtt/hassio-zigbee2mqtt)
mit dem Inhalt von `.last-synced-tag` und schreibt bei Abweichung die fuenf
Instanz-Konfigurationen fort. Vorher prueft er, dass die Image-Referenz
unveraendert auf `ghcr.io/zigbee2mqtt` zeigt.

Der Workflow braucht nur `contents: write` — kein Release, kein Recht,
andere Workflows anzustossen.

Stand bei Erzeugung: **2.13.0-1**

## Credits

Die Idee und der urspruengliche Workflow stammen von
[studioIngrid/hassio-multiple-zigbee2mqtt](https://github.com/studioIngrid/hassio-multiple-zigbee2mqtt) (MIT).
Zigbee2MQTT selbst: [Koenkk/zigbee2mqtt](https://github.com/Koenkk/zigbee2mqtt).
