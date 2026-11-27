# PHP-GEOS für Debian 12 / PHP 8.2

Dieses Repository versucht, aus dem offiziellen Debian-Quellpaket `php-geos 1.0.0-8` eine PHP-8.2-Erweiterung auf Debian 12 (Bookworm, amd64) zu bauen. Diese Version enthält den Fix für die Warnung zu `GEOSGeometry::__toString()`.

## Build starten

1. Im Repository **Actions** öffnen.
2. **Build PHP-GEOS for Debian 12 / PHP 8.2** auswählen.
3. **Run workflow** anklicken.
4. Erfolgreichen Lauf öffnen und unter **Artifacts** das ZIP `php-geos-debian12-php82` herunterladen.

Der Build verwendet einen kurzlebigen Debian-12-Container auf GitHub, keine Entwicklungspakete auf dem Produktivserver.

## Wichtiger Sicherheitshinweis

**Das erzeugte Paket nicht direkt auf Hyperion installieren.** Das bisherige `php-geos`-Paket stellt auch eine Konfiguration für PHP 7.4 bereit. Vor einem Ersatz müssen Paketinhalt, Abhängigkeiten, Verträglichkeit mit mod_php 8.2 und Rückweg geprüft werden. PHP 7.4 muss bis zur abgeschlossenen Migration weiter funktionieren.

Ein erfolgreicher Lade-Test der Erweiterung ist nicht gleichbedeutend mit einem Backdrop-/Geofield-Funktionstest.

## Build

Workflow: `.github/workflows/build-geos.yml` – manueller Start, Debian 12 Bookworm, PHP 8.2.
