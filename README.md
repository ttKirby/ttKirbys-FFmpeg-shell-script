# ttKirby's-FFmpeg-Shell-Script

- Es ist noch im Aufbau aber funktioniert soweit.
- Ein großes Update ist unterwegs, es wird aber noch ein Weilchen dauern.
  - Es enthält einige Bugfixes, Codeoptimierung, schmeißt Redundantes raus und hat große Neuerungen, wobei die Grundfunktion des Skriptes sich kaum verändert.

## Vorstellung

- Hier ein einfaches Shell-Skript zur Stapelverarbeitung von Videodateien mit FFmpeg.
- Das Skript durchsucht den aktuellen Ordner nach `.mkv`, `.mp4` und `.avi`-Dateien und bietet die Möglichkeit externe Untertitel-Dateien einzubinden.
- Es durchläuft alle Videos im Ordner, arbeitet mit Variablen udn Arrays und fragt einen was gemacht werden soll.
- Dieses Skript basiert entfernt auf dem eines Freundes. Viel erfahrung im schreiben von Skripten habe ich nicht, dies hier ist mein erstes mit größerem Umfang, aber ich hoffe, hier findet sich der ein oder andere, der einen Nutzen darin sieht.

## Funktionen

- Videos in einem Ordner automatisch verarbeiten
- Unterstützung für externe Untertiteldateien (`.srt` und `.ass`)
- Erkennt automatisch Audiostreams in Videodateien. Wendet passende Voreinstellungen basierend auf Anzahl und Eigenschaften der Audiotracks an
- Einfache Vorlagen für bestimmte FFmpeg-Konfigurationen
- Möglichkeit zum Entfernen von Untertiteln
- Viele Kommentare und Echos im Skript, die erklären wo was passiert, zum besseren nachzuvollziehen und gegebenfalls zur Ergänzung eigener Werte

## Verwendung

- Wenn umgebungsvariablen unter Windows gesetzt wurden, einfach doppelklick zum ausführen. Sonst über Konsole/Terminal öffnen
- Das Skript zeigt dir in einem Menu eine Auswahl an Optionen an.

## Funktionsweise

[1] Transkodieren
- Artbeitet mit festgelegten Werten für Video, Audio und Untertitel.
- Diese solltest du auch bei Bedarf für dich selbst anpassen!

[2] Transkodieren mit Auto-Audio
- Arbeitet mit festgelegten Werten für Video und Untertitel. Erkennt Audiokkanäle und -bitraten durch FFprobe und reagiert mit vorher festgelegten Werten.
- z.B. `2 Kanal = 224k` und `6 Kanal = 448k`
- Diese solltest du auch bei Bedarf für dich selbst anpassen!
- Anmerkung: Es kann mit `CBR` und `VBR` umgehen, aber nur `CBR` auslesen. Bei `VBR` nimmt es vordefinierte Werte.

[3] Vorlagen anwenden
- Verwendet Vordefinierte Vorlagen für Spezielle Anwendungsfälle

[4] Untertitel entfernen
- Entfern stumpf alle Untertitel, Video und Audio wird kopiert und Metadaten werden nicht angefasst.

[5] Beenden  (STRG+C)
- Beendet das Skript. Man kann auch jederzeit und überall `STRG+C` drücken um das Skript zu beendet.

[6] Herunterfahren und Pause
- Das Skript prüft, ob bestimmte Dateien vorhanden sind, um automatisch den PC herunterzufahren oder das Skript zu beenden.
- Pause: Wenn eine `pause.txt` im Ordner liegt, beendet das Skript nach Abschluss der aktuellen Verarbeitung sich von alleine. Kann jederzeit einfach im Ordner erstellt werden.
- Herunterfahren: Wenn ein `shutdown.txt` im Ordner liegt, fährt es den PC nach Abschluss der aktuellen Verarbeitung herunter. Kann jederzeit einfach im Ordner erstellt werden.
- Zum genaueren Ablauf siehe ganze unten im Skript unter der Überschrift `# SHUTDOWN` und `# PAUSE`.

## Anpassung 

- Das Skript muss im gleichen Ordner liegen wie die zu bearbeitenden Videos.  
- Untertiteldateien müssen den gleichen Namen wie das Video inne haben.

[1] Transcodieren
- CRF, Bitrate und Metadaten werden im Skript ganz oben angepasst.
- Die Reihenfolge der Untertitel ist in der Variable i_files geregelt:  
  `""i_files=(-i "${srt_files[0]}" -i "${srt_files[1]}" -i "${ass_files[0]}")""`  
- Empfohlene Struktur und Benennung ist:

```
/MyFolder
  ├── !FFMPEG beta1.sh
  ├── video1.mkv
  ├── video1.FORCED.srt
  ├── video1.FULL.srt
  ├── video1.FULL.ass
```

[2] Transcodieren mit Audo-Audio
- Das gleiche wie bei [1] nur, das man Audio hier anpassen muss:
-	`auto_bit_2="224"`
-	`auto_bit_6="448"`

[3] Vorlagen
- Ein paar Beispiele sind vorgegeben, orientiere dich daran.
- Das wichtigste um Bild, Ton und gegebenfalls Text in die Videodatei zu bekommen sind:

```
-map 0:v		# nutzt Videospur
-map 0:a		# nutzt Audio
-map 0:s 		# nutzt Untertitel
-c:v libx265		# Transkodiert zu h265
-c:a eac3		# Transkodiert zu E-AC3
-b:a 224k		# Nimmt eine konstante Audiobitrate (CBR) von 224k
-ac 2			# 2 Kanal steht für Stereo, 6 wäre für 5.1
-c:s srt		# Transcodiert den Untertitel in das srt-Format
```

[4] Untertitel entfernen
- Benutzt nur `-map 0:v -map 0:s -c:v copy -c:a copy` und kopiert somit alle Video- und Audiospuren, mehr nicht.

[5] Beenden
- Selbsterklärend. 5 drücken und mit Enter bestätigen. Oder 1x `STRG+C` drücken.

## Voraussetzungen, Empfehlungen und Anmerkungen

- FFmpeg muss installiert sein. Umgebungsvariablen müssen eventuell gesetzt werden.
- Git (https://git-scm.com/downloads) Wird zum ausführen benötigt.
- Zur Bearbeitung unter Windows empfehle ich: https://notepad-plus-plus.org/downloads/
- Getestet unter Windows. Funktioniert theoretisch auch unter Linux und MacOS.
- Untertitel bearbeitet man am besten mit SubtitleEdit: https://www.nikse.dk/subtitleedit
- Seine Videodateien kann man mit MediaInfo auslesen: https://mediaarea.net/en/MediaInfo

- Mächtiges Werkzeug zum bearbeiten von Videodateien: https://handbrake.fr/
- Auch sehr oraktisches Werkzeug um seine Videos zu handhaben: https://mkvtoolnix.download/

## Versionsverlauf

### beta3 
  - Bietet nun die Möglichkeit nur eine Audiospur zu bearbeiten wenn mehrere vorhanden sind

### beta2
  - Bugfixes

## Lizenz

MIT – frei verwendbar, veränderbar und teilbar.
