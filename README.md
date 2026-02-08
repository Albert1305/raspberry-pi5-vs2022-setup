# raspberry-pi5-vs2022-setup
Dokumentation und Setup-Anleitungen für Visual Studio 2022 + Raspberry Pi 5 (SSH, Toolchain, GUI) - 

============================================================
INSTALLATION & EINRICHTUNG: VISUAL STUDIO 2022 + RASPBERRY PI 5
============================================================

1) RASPBERRY PI 5 VORBEREITEN
-----------------------------
1. Raspberry Pi OS (64‑bit, Desktop) installieren.
2. System aktualisieren:
      sudo apt update && sudo apt full-upgrade -y
3. SSH aktivieren:
      sudo raspi-config
      → Interface Options → SSH → Enable
4. Benutzer prüfen (Standard: "pi") oder eigenen Benutzer anlegen.
5. IP-Adresse herausfinden:
      hostname -I
6. Notwendige Build-Tools installieren:
      sudo apt install -y build-essential gdb gdbserver rsync openssh-server
7. Optional: SDL2 installieren (für spätere Grafikprojekte):
      sudo apt install -y libsdl2-dev libsdl2-image-dev libsdl2-ttf-dev libsdl2-gfx-dev

Der Raspberry Pi ist jetzt bereit für Visual Studio.

------------------------------------------------------------

2) VISUAL STUDIO 2022 EINRICHTEN (WINDOWS)
------------------------------------------
1. Visual Studio Installer öffnen.
2. Workload installieren:
      "Linux development with C++"
3. Visual Studio starten.
4. Menü: Tools → Options → Cross Platform → Connection Manager
5. "Add" klicken → SSH-Verbindung einrichten:
      Hostname: (IP des Raspberry Pi)
      Benutzer: pi (oder eigener Benutzer)
      Authentifizierung: Passwort oder SSH-Key
6. Verbindung testen → sollte "Connected" anzeigen.

Der Raspberry Pi ist jetzt dauerhaft im Connection Manager gespeichert.

------------------------------------------------------------

3) NEUES LINUX-PROJEKT ERSTELLEN
--------------------------------
1. Visual Studio → Create new project
2. Vorlage wählen:
      "C++ Console App for Linux" oder "Empty Project (Linux)"
3. Projekt erstellen.
4. Oben in der Toolbar:
      "Remote Linux" auswählen
5. Zielsystem auswählen:
      Raspberry Pi 5 (aus dem Connection Manager)

------------------------------------------------------------

4) PROGRAMM AUF DEM RASPBERRY PI AUSFÜHREN
------------------------------------------
1. F5 drücken (Debug) oder STRG+F5 (Run)
2. Visual Studio macht automatisch:
      - Kompilieren
      - Deployment via SSH
      - Starten des Programms auf dem Raspberry Pi
3. Konsolenausgabe erscheint im VS-Fenster.
4. GUI-Programme öffnen sich über HDMI direkt auf dem Pi.

------------------------------------------------------------

5) GUI-FENSTER (BEISPIELPROGRAMM)
---------------------------------
Minimaler Test für ein Fenster (X11):

#include <X11/Xlib.h>
#include <unistd.h>

int main() {
    Display* d = XOpenDisplay(NULL);
    Window w = XCreateSimpleWindow(d, DefaultRootWindow(d),
                                   100, 100, 400, 300, 1,
                                   0, 0xFFFFFF);
    XMapWindow(d, w);
    XFlush(d);
    sleep(5);
    return 0;
}

Dieses Programm öffnet ein Fenster auf dem HDMI‑Monitor des Raspberry Pi.

-------------------------------------------------------------

6) WICHTIG: NACH NEUINSTALLATION VON WINDOWS
-------------------------------------------
Du musst NICHTS am Raspberry Pi neu einrichten.
Nur in Visual Studio:

1. Workload "Linux development with C++" installieren
2. Connection Manager öffnen
3. Raspberry Pi erneut hinzufügen (IP, Benutzer, Passwort)

Fertig. Alles andere bleibt unverändert.

=============================================================

