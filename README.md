# Isoliertes PHP-GEOS für Debian 12 / PHP 8.2

Der Workflow kompiliert den offiziellen Debian-Quellcode `php-geos 1.0.0-8` (einschließlich des PHP-8.2-`__toString`-Patches) in einem temporären Debian-12-Container.

## Warum ein separates Paket?

Auf Hyperion ist `php-geos 1.0.0-7+b1` installiert. Dieses alte Paket **besitzt die unter PHP 7.4 aktive Konfiguration** `/etc/php/7.4/mods-available/geos.ini`. Ein gewöhnliches Upgrade von `php-geos` kann daher die laufende 7.4-Konfiguration verändern.

Das zusätzliche Paket heißt deshalb `mjh-php82-geos` und enthält **nur** `/opt/mjh/php-geos-8.2/geos.so`. Es installiert **keine** PHP-INI, hat **keine** Maintainer-Skripte und ersetzt weder `php-geos` noch PHP-7.4-Dateien. Die Aktivierung unter PHP 8.2 erfolgt später bewusst separat.

## Build starten

**Actions** → **Build PHP-GEOS for Debian 12 / PHP 8.2** → **Run workflow** (`main`).

Das Artifact heißt `mjh-php82-geos-isolated`; die enthaltene Datei `mjh-php82-geos_1.0.0-8+bookworm1_amd64.deb`.

Der GitHub-Test installiert zuerst Debians altes `php-geos`, anschließend das separate Paket. Er prüft, dass die alte Binärdatei unverändert bleibt, und lädt die neue Erweiterung explizit mit PHP 8.2.

## Grenzen der Tests

Ein ursprünglicher Debian-PHPT-Geometrietest (`Geometry_buffer`) weicht bei Polygonkoordinaten ab. Debians Quellpaket ignoriert diesen Testfehler beim Paketbau. Der neue Lade- und WKT-Test ist ein Smoke-Test, keine vollständige Backdrop-/Geometrie-Freigabe.

**Noch nicht auf Hyperion installieren oder aktivieren**, bis das neue Artifact geprüft und ein gesicherter Rückweg besprochen wurde. Das ursprüngliche Paket `php-geos_1.0.0-8+bookworm1_amd64.deb` aus dem älteren Workflow soll nicht verwendet werden.
