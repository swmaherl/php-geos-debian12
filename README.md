# PHP-GEOS für Debian 12 / PHP 8.2

Temporärer GitHub-Actions-Build des offiziellen Debian-Quellpakets `php-geos 1.0.0-8` mit dem Debian-Patch gegen die PHP-8.2-`__toString`-Warnung.

## Build und Prüfungen

1. Unter **Actions** den Workflow **Build PHP-GEOS for Debian 12 / PHP 8.2** auswählen.
2. **Run workflow** auf Branch `main` klicken.
3. Nach erfolgreichem Lauf unter **Artifacts** `php-geos-debian12-php82` herunterladen.

Der Build erfolgt in einem kurzlebigen `debian:bookworm`-Container (amd64) mit Debian-PHP 8.2. Das Paket wird als `php-geos_1.0.0-8+bookworm1_amd64.deb` neu verpackt. Seine zuvor versionsunspezifischen `phpenmod`- und `phpdismod`-Aufrufe sind auf **8.2** eingegrenzt. Der Workflow prüft, dass keine 7.4-Dateien im Paket liegen, installiert es im Wegwerfcontainer, lädt die Erweiterung und liest eine Test-WKT-Geometrie.

## Stand der Originaltests

Der vorherige Lauf #45993954146 enthielt **einen fehlgeschlagenen PHPT-Test**: `Geometry_buffer` erzeugte leicht andere Polygon-Eckpunkte als erwartet. Debian ignoriert Testfehler im Quellpaket standardmäßig. Der Workflow ist deshalb **keine vollständige geometrische Freigabe**; der Unterschied wurde nicht durch eine Änderung an den GEOS-Berechnungen behoben.

## WICHTIG: Auf Hyperion nicht ungeprüft installieren

Das bisherige Paket `php-geos 1.0.0-7+b1` wird durch die neue Version ersetzt. Während des Paketupgrades könnten die **alten** Debian-Maintainer-Skripte laufen; die neue Versionierung allein schützt nicht vor deren Verhalten. Vor der Installation zuerst produktive 7.4-Nutzung, Paket-/Konfigurationsbesitz und APT-Simulation prüfen. PHP 7.4 muss bis zur finalen Migration aktiv bleiben.

Auch die Backdrop-/Geofield-Funktionstests auf dem echten Server stehen noch aus. Keine Produktivinstallation nur aufgrund eines grünen GitHub-Status.
