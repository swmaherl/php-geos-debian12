# PHP-GEOS: regulärer Debian-12-Backport für PHP 8.2

## Ziel

PHP-GEOS unter Debian 12 und PHP 8.2 **aktiv** betreiben, ohne eine Erweiterung unter `/opt/` oder einen dauerhaft parallelen Paketnamen einzuführen.

Der GitHub-Workflow kompiliert das offizielle Debian-Quellpaket `php-geos 1.0.0-8` mit dem PHP-8.2-`__toString`-Fix auf Debian 12 und erstellt das reguläre Paket `php-geos_1.0.0-8~bpo12+1_amd64.deb`.

Die Versionsnummer ist wichtiger Teil der Strategie: neuer als das bestehende Bookworm-Paket `1.0.0-7+b1`, aber älter als Debians offizielle `1.0.0-8` unter Debian 13. Das ermöglicht den normalen Upgrade-Pfad.

## Workflow

1. **Actions** → **Build Debian 12 PHP 8.2 GEOS backport** → **Run workflow** (`main`).
2. Container installiert die Build-Werkzeuge nur auf GitHub.
3. Offizielle Debian-Quellen und Patches werden eingebunden.
4. Die Paketversion wird im Debian-Changelog auf `1.0.0-8~bpo12+1` gesetzt.
5. Eine frische Debian-12-Containerinstanz installiert das alte `php-geos`, führt das Upgrade aus und testet den neuen PHP-8.2-GEOS-WKT-Reader.
6. Das fertige .deb ist als Artifact `php-geos-bookworm-backport` abrufbar.

## Produktionsmigration mit Rückweg

**Nicht direkt auf Hyperion installieren!** Laut Serverkontrolle besitzt das vorhandene `php-geos 1.0.0-7+b1` die aktivierte Datei `/etc/php/7.4/mods-available/geos.ini`. Diese alte Konfiguration muss bis zum finalen Wechsel unverändert bleiben.

Geplante Reihenfolge:
- Alte PHP-7.4- und Apache-Konfiguration sowie aktuelle GEOS-Dateien sichern; produktive Dienste nicht verändern.
- Neues 8.2-Modul zunächst unabhängig vom Paket-Upgrade mit `php8.2 -n -d extension=/PFAD/geos.so ...` testen, danach parallel Backdrop-/Geofield-Tests auf geeigneter Testumgebung.
- Erst beim geplanten Wechsel auf 8.2 und nach festgelegtem Rückweg das Debian-Backport-Paket installieren, `phpenmod -v 8.2 geos` und Apache auf mod_php 8.2 umstellen.
- Stabilität, Cron, Geocoder, Geofield, Views und Umkreissuche prüfen.
- Erst danach PHP 7.4 und Sury-Altlasten kontrolliert entfernen. Bei Debian 13 das reguläre Debian-Paket übernehmen.

## Bekannter Testpunkt

Der Original-Build zeigt bei `Geometry_buffer` einen Koordinatenunterschied in einem Polygon-Test. Debian ignoriert Fehler der Quelltests bei diesem Paket; der GitHub-Workflow führt zusätzlich Lade-/WKT-Tests durch. Das bedeutet **nicht**, dass alle Geometrieoperationen freigegeben sind.

Dieser Build ist ein eigens gepflegter Backport. Er wird mit APT/dpkg verwaltet, aber bis zum späteren Wechsel auf ein offizielles Debian-Paket muss GEOS selbst auf Sicherheits- und Kompatibilitätskorrekturen kontrolliert werden. Reguläre Debian/PHP-Updates bleiben unberührt.
