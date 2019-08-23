# Echtzeitsysteme
## Echtzeitprogrammierung
### Anforderungen an Echtzeitsysteme

**Rechtzeitigkeit**: 
Es gibt weiche, feste und harte Echtzeitbedingungen. In Abhängigkeit davon sind die Folgen eines Nichteinhaltens der Deadlines ein verringter Nutzen, gar kein Nutzen oder ein negativer Nutzen.

**Gleichzeitigkeit**: 
Mehrere Prozesse müssen parallel abgearbeitet werden. Möglich über vollständige Parallelverarbeitung in Mehrprozessorsystemen oder quasi-parallele Verarbeitung in Ein-/Mehrprozessorsystemen.

**Verfügbarkeit**: 
Es darf keine längeren Reorganisationsphas
en geben, um die Verfügbarkeit des Systems nicht zu gefährden.

### Programmierverfahren
**Synchrone Programmierung**: 
Für zeitgesteuerte Systeme. Das zeitliche Verhalten period. Aktionen wird vorab geplant und in Zeitrastern $T$ synchronisiert. Praktisch, da festes und vorhersagbares Verhalten, somit besser überprüfbar. Bei richtiger Planung können Rechtzeitigkeit und Gleichzeitigkeit garantiert werden $\Rightarrow$ gut für zyklische, sicherheitsrelevante Aufgaben geeignet. Zudem einfachere Tests und Analyse des Systems. Allerdings geringe Flexibilität gegenüber Änderungen, keine Reaktion auf aperiodische Ereignisse, bzw. nur über periodische Überprüfung derselbigen.

**Asynchrone Programmierung**: 
Ablaufplanung zur Laufzeit durch Organisationsprogramm/Echtzeitbetriebssystem. Dieses überwacht Ereignisse und ermittelt Ablaufreihenfolgen in Reaktion hierauf $\Rightarrow$ Echtzeitscheduling, flexibler Programmablauf. Rechtzeitigkeit ist nicht immer garantiert, v.a. bei niedrigerer Priorität, Vorabanalysen möglich, generelle Systemanalyse und -tests jedoch schwierig. Große Schwankungen in der Ausführungszeit.

Beispiel: Fixed Priority Preemptive Scheduling. Ereignisse haben feste Priorität nach derer sich die Ausführungsreihenfolge richtet. Unterbrechen der Ausführung von Teilprogrammen niederer Priorität möglich. 

### Ablaufsteuerung
**Zyklisch**: 
Sequentielle Ausführung von Aktivitäten. Prozessor ständig aktiv.

**Zeitgesteuert**: 
Ausführung von Aktivitäten in Abhängigkeit der aktuellen Zeit. Prozessor ständig aktiv.

**Unterbrechungsgesteuert**:
Ereignisoder zeitgesteuerte Unterbrechungen triggern Aktivitäten. Bei gleichzeitigen Unterbrechungen/ Ereignissen wird Ausführung anhand der Prioritäten gewählt. Echtzeitscheduling, Prozessor nicht ständig aktiv.

## Echtzeitbetriebssysteme
Aufgaben Betriebssystem allgmein:

  * Taskverwaltung: Zuteilung des Prozessors an Tasks
  * Betriebsmittelverwaltung: Speicher und I/O-Verwaltung
  * Interprozesskommunikation
  * Synchronisation: Zeitliche Koordination der Tasks
  * Schutzmaßnahmen: Schutz von Betriebsmitteln vor unberechtigtem Zugriff durch Tasks

Zusätzliche Aufgaben Echzeitbetriebssysteme: Wahrung von Rechzeitigkeit, Gleichzeitigkeit und Verfügbarkeit.

**Schichtenmodell**: Schichten fügen Abstraktionsebene von Prozessor bis Anwendung hinzu. 
Das Modell eines Mikrokenrbetriebssystems fügt im Gegensatz zum Makrokernbetriebssystem eine zusätzliche Schicht direkt über dem realen Prozessor ein. Diese dient der Interprozesskommunikation, Synchronisation und Taskverwaltung $\Rightarrow$ Besser anpassbar an Aufgaben, gute Sklaierbarkiet, einfache Portierbarkeit, preemptiver (unterbrechbarer) Kern und lediglich kurze kritische Bereiche. Ein kritischer Bereich liegt vor, wenn Semaphoren geblockt sind

<div style="text-align:center">
	<img src="./assets/schichtenmodell_mikroprozessor.jpg" class="center" width="200"/>
</div>

**Mikrokernbetriebssystem**: 
Alle Schichten arbeiten im User-Mode. Lediglich die Mikrokern-Schicht arbeitet im Kernelmode, sie interagiert mit den anderen Schichten. Sehr schlanker Kern. Muss aber mindestens elementare Taskverwaltung, Synchronisation, Interprozesskommuikation und Adressraumverwaltung beinhalten.

Sehr gut anpassbar und skalierbar, gute Portierbarkeit. Bessere zeitliche Vorhersagarkeit da nur kurze kritische Bereiche. Kern ist fast immer unterbrechbar. Häufige Wechsel im User- und Kernelmode verlangsamen allerdings ein wenig.

**Makrokernbetriebssystem**: 
Besitzt einen monolithischen Kern welcher vollständig im Kernel-Mode (hat Zugriff auf privilegierte Befehle, Speicherbereiche und Ressourccen) arbeitet. Darüber hinaus sind Gerätetreiber, Task- und Ressourcenverwaltung beinhaltet.

Achtung: ein monolithisches Betriebssystem hingegen ist ein OS, welches alle Funktionalitäten in einem einheitlichen Block realisiert. Dies führt zu schlechter Wartbarkeit, hoher Fehleranfälligkeit.

### Taskverwaltung
**Task**: 
Schwergewichtig. Eigene Variablen und Betriebsmittel, Adressraum, Kommunikation via Interprozesskommunikation. Lange Umschaltdauer. Kontext umfasst Task-, Speicher- und Ein-/Ausgabeverwaltung!

**Thread**: 
Leichtgewichtig. Arbeitet innerhalb eines Tasks und teilt sich Adressraum mit anderen Threads in demselben Task. Kommunikation kann über globale Variablen erfolgen. Wenig Schutz/Abschirmung, dadurch aber schnelle Kommunikation/Umschaltung möglich. Kontext lediglich in der Taskverwaltung.

Zustände:

  * Ruhend: Zeitbedingungen oder ander Voraussetzugen noch nicht erfüllt
  * Ablaufwillig: Randbedingungen erfüllt, Betriebsmittel verfügbar, warten auf Prozessorzuteilung
  * Laufend: Ausführung durch Prozessor
  * Blockiert: Warten auf Ereignis oder Betriebsmittelzuteilung

#### Echtzeitscheduling
Prozessorzuteilung wrt. Zeitbedingungen. Ein Schedulingverfahren heißt *optimal*, wenn es eine Schedule findet, sofern diese existiert.

**Prozessorauslastung**: 
$H=\dfrac{\texttt{Benötigte Prozessorzeit}}{\texttt{Verfügbare Prozessorzeit}}=\sum_{i=1}^n \dfrac{e_i}{p_i}$ mit Ausführungszeit $e$ und Periodendauer $p$.

<table>
  <tr>
    <td>Fixed Priority</td>
    <td>Preemptiv möglich. Prioritätszuordnung entscheidend für Echtzeitverhalten. Kein optimales Schedulingverfahren.</td>
  </tr>
  <tr>
  	<td>Rate-Monotonic-Scheduling (RMS)</td>
  	<td>Prioritäten für periodische Tasks umgekehrt proportional zur Zeitschranke/Periodendauer. Optimales Scheduling für Zeitschranken äquivalent zur Periodendauer.</td>
  </tr>
  <tr>
  	<td>Earliest-Deadline-First-Scheduling (EDF)</td>
  	<td>Preemptiv möglich. Optimales Scheduling für preemptives Variante in Einprozessorsystemen.</td>
  </tr>
  <tr>
  	<td>Least-Laxity-First-Scheduling (LLF)</td>
  	<td>Laxity ist die theoretisch verbleibende Pufferzeit. Ebenfalls optimal in preemptiver Variante für Einprozessorsysteme. Mehr Taskwechsel als EDF, dafür besseres Verhalten bei Überlast.</td>
  </tr>
  <tr>
  	<td>Time-Slice-Scheduling</td>
  	<td>Jedem Task werden Zeitscheiben zugeordnet, Reihenfolge entspricht der Reihenfolge ablaufwilliger Tasks in der OS-Taskverwaltung. Zeitscheiben können an Tasks angepasst werden. Bei feinkörniger Zuteilung nahezu optimal.</td>
  </tr>
  <tr>
  	<td>Guaranteed Percentage Scheduling (GP)</td>
  	<td>Jedem Task wird fester Prozentsatz der verfügbaren Prozessorleistung einer Periode zugeteilt. Preemptiv, auf Einprozessorsystemen optimal.</td>
  </tr>
</table>



#### Synchronisation für Betriebsmittelzugriff
Nutzung von Betriebsmitteln wie Daten, Geräten, Programmen muss koordiniert werden. Implementierung mittels Semaphoren (Datenstruktur), welche passiert/verlassen werden können.

  * Sperrsynchronisation: Sperren des Zugriffs sobald ein Task den Mutex belegt. Dieser gibt Mutex auch wieder frei. 
  * Reihenfolgesynchronisation: Stellt sicher, dass Aktivitäten von Tasks in einer bestimmten Reihenfolge durchgeführt werden. Implementierung durch Semaphoren.

Verklemmungen:

  * Deadlock: Mehrere Tasks warten auf Betriebsmittel, die sie gegenseitig blockieren. $\Rightarrow$ Abhängigkeitsanalyse, Vergabe von Betriebsmitteln in gleicher Reihenfolge, Timeout oder Monitoring zur Vermeidung implementieren.
  * Livelock: Auch Starvation. Ein Task wird durch andere Tasks ständig an der Ausführung gehindert. Durch Prioritäteninversion auch bei höherprioren Tasks möglich $\Rightarrow$ Prioritätenvererbung.

Ein Protokoll um Verklemmungen zu vermeiden ist Priority Ceiling. Hierbei können nur Tasks ein Betriebsmittel bzw. deren Semaphoren blockieren, wenn sie eine höhere Priorität haben als *alle* anderen Tasks, die aktuell *eine* Semaphore blockieren.

### Taskkommunikation
Grundsätzlich ist es in Echtzeit-OS wichtig, dass die End-zu-End-Prioritäten gewahrt werden. Dies geschieht durch prioritätsbasierte Kommunikation, bei der hochpriore Nachrichten niederpriore überholen können und möglichst keine Blockierungen vorgenommen werden.

Unterscheidungsmerkmale:

  * Gemeinsamer Speicher (schneller, Synchronisierung durch Semaphore) oder nachrichtenbasiert
  * Asynchron (kein Warten, gut für ES! Dafür Nachrichtenpuffer nötig) oder synchron (Taskblockierung) 

### Speicherverwaltung
Teilt verfügbaren Speicher mittels Speicherabbildungsfunktion zu. Logische Adresse ist die Speicheradresse innerhalb einer Task, die resultierende physikalische Adresse die echte Arbeitsspeicheradresse. Die Speicherverwaltung ist selbst ein Task.

Speicherzuteilung:
 
  * Statisch (Tasks laufen immer im selben Segment) vs. dynamisch (First Fit/Best Fit/Worst Fit)
  * Verdrängend (FIFO/LRU/LFU. Ausführung on demand/vorausschauend) vs nichtverdrängend

Adressbildung und Adressierung:

  * Reell (tatsächlich verfügbarer Adressraum) vs. virtuell (größerer Adressraum, benötigt Verdrängungen und Adressauflösung, usw. Dafür aber leichtere Implementierung von Schutzmechanismen)
  * Linear (zusammenhängende Segmente) vs. streuend (Seiten, keine Zuteilungsstrategie nötig) vs. kombiniert (Segmente mit Seiten)


Für Mehrprogrammbetrieb benötigt: 
Verschiebbarkeit/Relozierbarkeit von Tasks im Adrbeitsspeicher, Auslagerungsfähigkeit, Schutzmaßnahmen.

Bewertungskriterien:

  * Vorhersagbarkeit Zeitverhalten
  * Speicherbereinigung nötig?
  * Verschiebbarkeit von Tasks
  * Speicherausnutzung
  * Reaktionszeit, Verwaltungsaufwand

Allgemeines Problem beim Einsatz von Caches, usw.: Um Echtzeitbedingungen einzuhalten, müssen wir in der Regel von den schlechtesten Laufzeiten ausgehen.

### I/O-Verwaltung
Zuteilung, Freigeben und Benutzen von Geräten. Teilfunktionen umfassen symbolische Namensgebung, Annahme von I/O-Aufforderungen, Vergabe/Zuteilung von Geräten, Synchronisation, Schutz der Geräte, Pufferung und einheitliches Datenformat

Varianten: 

<table>
  <tr>
  	<td>Polling</td>
  	<td>Prozessor wartet auf Freigabe im Statusregister. Kann auch durch mehrere Geräte innerhalb eines Tasks durchgeführt werden. Einfach und hohe Übertragungsleistung, aber dauerhaft aktiver Prozessor, bei mehreren Geräten zudem verlangsame Reaktionszeit</td>
  </tr>
  <tr>
  	<td>Busy Waiting</td>
  	<td>Wie Polling, allerdings sind kurze Nebentätigkeiten möglich. Diese verlangsamen aber Reaktionszeit</td>
  </tr>
  <tr>
  	<td>Interrupts</td>
  	<td>Gerät signalisiert Bereitschaft. Dadurch beliebige Nebenaktivitäten und schnelle Reaktionszeit. Aufruf der Interrupt Service Routine. Dafür aber verringerte Übertragungsraten (wegen Kommunikations-Overhead).</td>
  </tr>
  <tr>
  	<td>Polling/Interrupt mit Handshake</td>
  	<td>Sinnvoll, wenn das Gerät die Daten schneller liefert als der Task sie entgegennehmen kann.</td>
  </tr>
</table>

### Auswahlkriterien für Echtzeit-Betriebssysteme
  * Entwicklungs- und Zielumgebung, z.B. Programmiersprache
  * Modularität und Kerngröße, z.B. Konfigurierbarkeit
  * Anpassbarkeit an Zielumgebungen für Automatisierung, z.B. Feldbusse
  * Allgmeine Eigenschaften, z.B. GUI 
  * Leistungsdaten, z.B. maximale Taskanzahl

## Rechnerarchitekturen und HW-Schnittstellen 

**Mikroprozessor**:
Prozessorkern mit Steuer- und Rechenwerk sowie Schnittstellen.

**Mikrorechner**: 
Beinhaltet Mikroprozessor, I/O-Steuerung, Hauptspeicher $\Rightarrow$ Erweiterung zum Mikrorechnersystem durch Peripheriegeräte.

**Mikrocontroller**:
Einchip-Mikrorechner, welcher speziell für Steuerungsaufgaben optimierte Peripherie besitzt. Ziel: Möglichst wenige externe Bausteine. System-on-Chip.

### Prozessoren 

Skalare Befehlspipeline (zeitliche Parallelität):

  1. Befehl holen
  2. Befehl dekodieren
  3. Operanden holen
  4. Befehl ausführen
  5. Ergebnis speichern

$\Rightarrow$ Superskalare Pipeline parallelisiert 3 und 4 (räumliche Parallelität). Dazu brauchen wir Befehlsfenster, Zurdnungs- und Rückordnungseinheit zusätzlich als Puffer.

**Sprungvorhersage**: 
Passiert in Schritt 1. Möglich mittels Ein-/Zwei-Bit-Prädiktoren. Nur Sättigungszähler in der Vorlesung behandelt.
Aber: Echtzeitproblematik, da Worst Case deutlich längere Ausführungszeiten benötigt im Vergleich zur Anwendung ohne Sprungvorhersage.

**Unterbrechungsbehandlung**: 
Auslösung durch Exceptions, Software (durch laugendes Programm) oder Hardware Interrupt (durch externe Systemkomponente). Die Interrupt-Quelle liefert eine Vektornummer, welche mittels Interrupt-Vektor-Tabelle die Startadresse der Interrupt Service Routine (ISR) liefert. Diese behandelt dann den Interrupt.

**Priorisierungen**: 
Nur Interrupts mit höherer Priorität als das aktuelle Programm können Unterbrechung erzwingen. Prioritätsebenen können durch Unterbrechnungsleitungen oder intern im Prozessor implementiert werden.

Steuerungen:

  * Daisy Chain: Dezentrale Realisierung. Serielle Unterbrechungskette. Erste Quelle hat höchste Priorität, starre Priorisierung. Meldung an Mikroprozessor über Bus.
  * Interrupt Controller: Intermedär, welcher an alle Komponenten angeschlossen ist und die Unterbrechung an den Mikroprozessor meldet.

Starre Priorisierung (Fixed-priority-preemptive) führt zu schlechter zeitlicher Vorhersagbarkeit für Abarbeitung. Stattdessen dynamische Prioritäten (z.B. Earliest Deadline First) für Echtzeitbetrieb vorteilhafter.

### Aufbau Mikrocontroller
<div style="text-align:center">
	<img src="./assets/mikrocontroller.jpg" class="center" width="400"/>
</div>

**Zähler/Zeitgeber**:
Zählt in Abhängigkeit eines externen Takts. 

**Watchdog**: 
Zähler, welcher bei Ablauf einen Reset auslöst. Wenn Software Watchdog nicht anspricht, wird davon ausgegangen, dass Programm abgestürzt wurde. 
Bei Nulldurchgang werden Maßnahmen ergriffen, bspw. Prozessorkern zurücksetzen.

### Kommunikationskanäle

Serielle/ Parallele Schnittstellen mit Datenbussen.

**Jitter**: 
Zeitliche Fluktuation in digitalen Signalen. Ursache bspw. durch Speicherhierarchie, die durch verschiedene Zugriffszeiten führt. Daher Pufferregister/Echtzeit-Ausgabeeinheit verwenden um Daten entsprechend der gewünschten Taktung zu verwenden. 

**Direct Memory Access (DMA)**:
Daten direkt, ohne Prozessorkern, zwischen Peripherie und Speicher transferieren. Dadurch höhere Übertragungsrate möglich. Prozessor definiert lediglich Randbedingungen.

**Parallelverarbeitung**:
  * VLIW (parallele Abarbeitung von Befehlen) 
  * Horizontale Mikroprogrammierung: Hierbei können die einzelnen Bearbeitungsschritte im Mikroprozessor direkt durch den Nutzer angesteuert werden.


### Varianten von Echtezitsystemen
**Signalprozessor**: 
Spezielle Mikrorechnerarchitektur für die Verarbeitung analoger Signale. Trennung von Daten- und Befehlsspeicher/Programmspeicher. 

**Embedded Systems**: 
Mobile Geräte mit Schnittstellen. Integration eines Mikroprozessors in ein System, welcher mit dem Leitrechner verbunden ist. Regelkreis wird über den Mikrocontroller gesteuert. 

**PC-basierte Systeme**:
Kommunikation über Ethernet und Feldbusse.

### Systembusse

#### Charakterisierung

**Prozessorabhängigkeit**: 
Prozessorabhängige Busse verbinden Mikrorprozessor, Speicher und Peripherie direkt. Alle Komponeenten müssen auf den Prozessor ausgelegt sein. Prozessorunabhängige Busse haben eine Verbindung zum Prozessor mittels Bus Bridge. Rest des Systems dann unabhängig vom Prozessor, Speicher, Peripherie und Bus daher prozessunabhängig. 
Eine Erweiterung des modularen Konzepts ist der entkoppelte, gepufferte Bus, bei dem die Bus Bridge um einen FIFO-Speicher erweitert wird. So können langsame Komponenten den Bus nicht blockieren $\Rightarrow$ besser, um Echtzeitbedingungen einzuhalten.

**Synchronität**: 
Ein synchroner Systembus hat einen Takt. Dadurch sind allerdings Wartezyklen bei langsamen Komponenten nötig (und zusätzliches Signal, welches diese Wartezyklen signalisiert). Ein asynchroner Bus hat keinen Takt, dafür gibt es einen Handshake, mittels AS (Adresse liegt an)/DTACK (Datum liegt an).

**Datenübertragung**:
Seriell, wenn Daten hinterinander übertragen werden, parallel, wenn mehrere Daten zur selben Zeit übertragen werden können. 


#### Zuteilungsstrategien

**Single-Master/Slave**: Master hat aktiven Zugriff, kann Buszyklen auslösen und Komponenten addressieren. Im Gegenzug können Slaves auf diese Signale Reagieren und Daten bei Aufforderung transferieren. 

**Multiple-Master/Slave**:
Buszuteilung an die verschiendenen Master nötig, beipielsiwese bei Mikroporzessor und DMA.
$\Rightarrow$ Bus-Arbiter-Komponente erledigt Zuteilung. Bedarf kann angemeldet und wird dann gewährt. 

Arten von Bus-Arbitern:

  * Zentral: Direkte Anbindung jedes Masters an den Arbiter. Einfacher Aufbau und beleibige Priorisierungen möglich, dafür hoher Leitungsaufwand.
  * Dezentral: Implementierung bspw. via Daisy-Chain. Geringe Leitungszahl, dafür starre Priorisierung und Gefahr von Starvation für hintere Komponenten. Zuteilungszeit steight mit Anzahl der Komponenten $\Rightarrow$ überlappende Arbitrierung für letzten Punkt. 
  * Identifikationsbus: jede Komponente hat ID, welche Priorisierung beinhaltet. Interaktive Bestimmung des Busses mit der höchsten Priorität über Identifikationsbus.

Generell sollte das Zeitverhalten des Busses möglichst vorhersagbar sein um die Echtzeitfähigkeit zu wahren. 


#### Peripheral Component Interconnect (PCI)-Bus

Eigenschaften:

  * Kommandoorientiert (?)
  * Multiplex
  * Burst-Transfers erlaubt
  * Multi-Master-fähig, Zuteilung über zentralen Arbiter:
    * Master/Intiator darf Daten transferieren solange Slave/Target sie aufnehmen kann und kein anderer Master Bedarf anmeldet.
    * Wenn Latenzzähler Threshold erreicht, muss Intitiator ggf. Bus freigeben.
    * Target muss Datentransfer abbrechen, wenn er nicht innerh. von 8 Taktzyklen das nächste Datum bereitstellen kann. Dadurch Schachtelung von Transfers.
  * Echtzeitfähig
  * Bridge-Konzept $\Rightarrow$ erlaubt hierarchische Kaskadierung 
  * Fehlererkennung

Transferarten:
  <div style="text-align:center">
	<img src="./assets/pci_transfer.jpg" class="center" width="400"/>
  </div>

  * Standard Write: $n \cdot (T_A+T_D)$
  * Standard Read: $n \cdot (T_A+T_W+T_D)$
  * Burst Write: $T_A+n \cdot T_D$
  * Burst Read: $T_A+T_W+n \cdot T_D$

### PCIe: 
  <div style="text-align:center">
	<img src="./assets/pcie_transfer.jpg" class="center" width="400"/>
  </div>

Topologie:
  
  * Requester und Completer als zentrale Agenten
  * Root Complex verbindet CPU und RAM mit PCIe-Hierarchie. Ist Master auf Speicher-Bus und Slave auf CPU-/Host-Bus
  * Ports verbinden PCIe-Komponenten. 


Eigenschaften: 

  * Punkt-zu-Punkt-Verbindung
  * Doppelte Simplex-Verbindung 
  * Paket-basiert
  * Differentielle Übertragung: Kann Differenz zwischen zwei Leitungen übertragen, dadurch kann generelles Grundrauschen von Leitungen unterdrückt werden.  
  * Logischer Link besteht aus ggf. mehreren Lanes. Baut auf der Transaaction Layer auf. 

**Interrupt-Modell**: 
Message Signaled Interrupts (MSI). Weiterleitung der Interrupts zu Interrupt Controller in Root Complex wird durch Switches gewährleistet.

## Echtzeitmiddleware
**Middleware**:
Anwendungsneutrales Programm, welches zwischen Anwendungen vermittelt, sodass die Komplexität verborgen wird. Unterstützt die Kommunikation zwischen Prozessen.
Arbeiten zwischen verteilen Anwendungen und den Netzwerk-Services von einzelnen Maschinen. Alternative: Verteilungssteuerung in den verteilten Anwendungen auf jeder Maschine.

Aufgaben: 

  * Identifikation von verteilten Objekten
  * Lokalisierung von verteilten Objekten 
  * Interaktion 
  * Erzeugung und Vernichtung
  * Fehlerbehandlung

Vorteile bei Entwicklung (keine Organisation von Verteilung nötig) und Rekonfigurationen. Verteilung kann durch standardisierte Software übernommen werden. Zudem bessere Portierbarkeit, Testbarkeit und Wartbarkeit. Heterogene Umgebungen werden versteckt.
Aber Nachteile bzgl. Speicher- und Verarbeitungs-Overhead. Echtzeit-Middleware für echtzeitfähigen Betrieb nötig, sonst kann es bspw. zu einer Prioritäteninversion mangels End-zu-End-Priorisierung kommen. 

Charakteristika Echtzeit-Middleware:

  * Wahrung E2E-Prioritäten
  * Definition von Zeitbedingungen/-schranken
  * Echtzeitfähige Funktionsaufrufe 
  * Echtzeitfähige Ereignisbehandlung
  * Unterstützung von Echtzeitscheduling

### Data Distribution Service (DDS)
Pub/Sub Middleware, erlaubt Filtern von Daten. Geringe Latenz und vorhersagbare Datenverbreitung. Stabil auch bei Überlastung. Skalierbar. Erlaubt Event-Processing. Echtzeitfähig :)

**Data-Centric Publish-Subscribe (DCPS)**: 
Bietet API zum Daten austauschen. Es sind synchrone und asynchrone Zugriffsmöglichkeiten auf die Daten möglich. Besitzt Quality of Services (QoS), welche die nicht-funktionalen Eigenschaften sepzifiziert $\Rightarrow$ damit sehr wichtig für Echtzeitanwendungen!

**Data Local Reconstruction Layer (DLRL)**: 
Definiert lokale Objekte auf den Daten.  

Alternative: CORBA als Standardmiddleware. Problem: i.A. nicht echtzeitfähig. 

## Echtzeitkommunikation

**Industrie-PC(IPC)-basiertes Automatisierungssystem**:
Nutzung eines PCI-Busses zur Kommunikation.

### ISO/OSI-Referenzmodell


<table>
	<thead>
		<td>Schicht</td>
		<td>Zuständigkeit</td>
		<td>Beispiel</td>
	</thead>
	<tr>
		<td>Anwendungsschicht/Application Layer</td>
		<td>Domänenabhängige Bereitstellung von Diensten für den Endanwender</td>
		<td>HTTP</td>
	</tr>
	<tr>
		<td>Darstellungsschicht/Application Layer</td>
		<td>Wandelt Datenstrukturen um, zuständig für Verschlüsselung</td>
		<td>XDR</td>
	</tr>
	<tr>
		<td>Sitzungsschicht/Session Layer</td>
		<td>Sorgt für Prozesskommunikation zwischen zwei Systemen. Wandelt symbolische in reale Adressen um</td>
		<td>X.225</td>
	</tr>
	<tr>
		<td>Transportschicht/Transportation Layer</td>
		<td>Überträgt Daten eines gesamten Kommunikationsauftrags (Files) durch Zerlegung in Frames als Datensegmente/Datagramme</td>
		<td>TCP/UDP</td>
	</tr>
	<tr>
		<td>Vermittlungsschicht/Network Layer</td>
		<td>Zuständig für das Ende-zu-Ende-Routing von Paketen</td>
		<td>IP</td>
	</tr>
	<tr>
		<td>Sicherungsschicht/Data Link Layer</td>
		<td>Fehlerfreie Übetragung von Frames zwischen zwei Knoten, auch Zugriffscontrolle (MAC)</td>
		<td>Ethernet, CSMA/CD</td>
	</tr>
	<tr>
		<td>Bitübertragungsschicht/Physical Layer</td>
		<td>Übertragen der Bits über einen Kanal</td>
		<td>Lichtwellenleiter (LWL), Manchester</td>
	</tr>	
</table>

$\Rightarrow$ Feldbusse verwenden idR. die Schichten 1, 2 und 7. 

### Netztopologien
Beispiele: Ring, Bus, Baum, Hub, ...


### Zugriffsverfahren auf Echtzeitkommunikationssysteme
Bridges arbeiten auf der Sicherungsschicht, Repeater nur auf der Bitübertragungsschicht.

**Polling**:
Zyklisches Anfragen der Resource bzw. des Busses. Funktioniert gut für Auslastungen unter 40%.

**Token Passing**:
Tokens authorisieren zum Kommunizieren auf einem Bus. Es gibt keinen vorderfinierten Masterknoten wie bei Polling. MMAT gibt an, bis wann ein 

Signalkodierung:

  * No Return To Zero (NRZ): Signal liegt genau für einen Takt an.
  * Return to Zero (RZ): Wenn Signal auf High liegt, wird nach einem halben Takt zu Low zurückgekehrt.
  * Manchester-Kodierung: $\texttt{NRZ}\oplus \texttt{Takt}$. Integriert im Gegensatz zur NRZ und RZ die Taktinformation direkt im Signal. Dafür jedoch doppelte Bandbreite benötigt, da zwei Signalzustände/Symbole pro Taktperiode verwendet werden.

**Aufbau TCP-Verbindungen**:
Three-Way-Handshake zum Verbindungsaufbau. Beim Abbau gibt es ein Finish-Flag, ebenfalls Three-Way-Exchange (aber Finish-Flag und Ack wird parallel vom Empfänger zurückgesendet).

### Feldbusse 
Bussystem, welches Feldgeräte/Sensoren und Aktoren mit Automatisierungsgeräten verbindet. 

Anforderungen allgemein:

  * Echtzeitfähigkeit
  * Effiziente Übermittlung kleiner Datenmengen
  * Geringe Latenz
  * Robustheit
  * Netzausdehnung, Knotenanzahl
  * Interoperabilität

**Process Field Bus (Profibus)**:
Mono- und Multi-Master-System möglich. 
Bei Multi-Master wird ein hybrides Zugriffsverfahren mit Token-Passing unter aktiven Mastern verwendet. Dann Polling zwischen diesem Master und den dazugehörigen Slaves. Ist eine Linie mit Stichleitungen, an beiden Enden abgeschlossen. Verwendet NRZ oder Manchester. Hamming-Distanz von 4. 

**Controller Area Network (CAN)-Bus**:
Dominante 0, rezessive 1 $\Rightarrow$ Wenn beide Daten parallel gesendet werden (Kollision), setzt sich die 0 durch. Um den Bus zu nutzen, werden Identifier durch Teilnehmer gesendet, bei Kollision ziehen sich Sender einer 1 zurück $\Rightarrow$ CSMA/CD. Verwendet NRZ und hat Hamming-Distanz von 6.

Frametypen: Data Frame (Datenübertragung), Remote Frame (Sendeaufforderung), Error Frame, Overload Frame (Nicht-Bereitschaft signalisieren).

Fehler über der Frame-Ebene können mit klassischen Bussen nicht erkannt werden (z.B. falsche Reihenfolge), da auf Ebene 3-6 behandelt. Aber dennoch wichtig, daher CRC-Codes auf Frame-Ebene ebenfalls miteinbauen $\Rightarrow$ SafetyBus.

**Interbus**:
Mono-Master-System. Ringtopologie, kann durch Busklemmen hintereinander geschaltet werden. 
Daten werden über Summenrahmen-Frame versendet, welche wie ein Schieberegister funktionieren. Verwendet NRZ, Hamming-Distanz von 4.  

**ASI-Bus-Netzwerk**:
Mono-Master-System. Knoten sind an den Bus angeschlossen, teils in Linien. Polling und modifizierte Manchester-Kodierung.

#### Echtzeit-Ethernet-Systeme

**Ethernet Powerlink**:
Mono-Master-System. Switch zwischen Komponenten und Leitrechner. Wenn CSMA/CD genutzt wird, benötigen wir Determinismus. 
Mit Powerlink-Zeitscheibenverfahren wird zunächst in zyklische kommuniziert, danach asynchron sofern Kapazitäten frei. Kann Standard-Ethernet-Hardware verwenden. 
Polling der Slaves durch Master.
.
**EtherCAT**:
Mono-Master-System. Master sendet periodisch einen Frame, der durch alle Teilnehmer durchgereicht wird (Summenframe). Es können Abzweigungen gebildet werden, es existieren Hin- und Rückleiterungen.

**PROFInet**: 
Weiterentwicklung des Profisbus. Hat zwei Kanäle, einmal Soft Real Time (schwächere Anforderungen), einmal Isochronous Real Time. IRT wird priorisiert, danach offener Kanal. Switches sind zeitlich sehr genau synchronisiert und der Paketpfad wird vorab eingestellt.

**SERCOS III**:
Mono-Master-System. Benutzt Zeitschlitze für Slaves die durch Master-Frame synchronisiert werden.

**Ethernet TSN**:
Versucht mit Standardhardware zu arbeiten. 

## Hardwareschnittstelle zwischen Echtzeitsystem und -prozess

### Operationsverstärker
Wird aus mehreren Transistoren gebaut. 
  <div style="text-align:center">
	<img src="./assets/opv.jpg" class="center" width="400"/>
  </div>

Annahmen des idealen OPVs:

  * Eingangswiderstand unendlich
  * Ausgangswiderstand 0
  * Verstärkung ist unendlich

#### Nicht-invertierend
$U_A=\dfrac{R_1+R_N}{R_1}\cdot U_E=y\cdot E$. 
Eingang liegt auf $+$ an, $-$ liegt über Widerstand auf Masse.

  <div style="text-align:center">
	<img src="./assets/nicht_invertierender_opc.jpg" class="center" width="400"/>
  </div>

### Invertierend
$U_A=- \dfrac{R_N}{R_1}\cdot U_E$
$+$ liegt auf Masse, Eingang auf $-$.

  <div style="text-align:center">
	<img src="./assets/invertierender_opv.jpg" class="center" width="400"/>
  </div>

#### Invertierender Addierer
$U_A=- (\dfrac{U_1}{R_1}+\dots +\dfrac{U_I}{R_I})\cdot R_N$
Summe der Eingänge $U_1$ bis $U_I$ liegen auf $-$

  <div style="text-align:center">
	<img src="./assets/invertierender_addierer.jpg" class="center" width="400"/>
  </div>

#### Subtrahierer
$U_A=\dfrac{R_0(R_1+R_3)}{R_1(R_0+R_2)}U_2-\cdot \dfrac{R_3}{R_1}U_1$. Mit normiertem $R_1=\dfrac{R3}{a}$ und $R2=\dfrac{R_0}{a}$ folgt: $U_A=a\cdot (U_2-U_1)$.

  <div style="text-align:center">
	<img src="./assets/subtrahierer.jpg" class="center" width="400"/>
  </div>

#### Integrierer
$U_A=-\dfrac{1}{R_1 C}\cdot \int U_E dt$

  <div style="text-align:center">
	<img src="./assets/integrierer.jpg" class="center" width="400"/>
  </div>

#### Differenzierer
$U_A=-R_N*C*\dfrac{dU_E}{dt}$

  <div style="text-align:center">
	<img src="./assets/differenzierer.jpg" class="center" width="400"/>
  </div>

## Analog und Digital-Wandlung

Parameter:

  * Relative Genauigkeit: ist durch Linearität der Umwandlung bestimmt
  * Skalenfehler: Abweichung der Umwandung zur tatsächlichen Übertragungsfunktion um konstanten Faktor
  * Offsetfehler: Konstanter Versatz im Vergleich zur originalen Übertragungsfunktion.

### D/A-Wandlung
Schaltung von Widerständen, welche Dezimalzahlen in dualer Kodierung repräsentieren. Danach OPV schalten.

$U_A=-\dfrac{R_N}{16R}(8z_3+4z_2+3z_1+z_0)\cdot U_Ref$

  <div style="text-align:center">
	<img src="./assets/d_a_wandler.jpg" class="center" width="400"/>
  </div>


### A/D-Wandlung 
Es existieren verschiedene Verfahren, welche man in Abhängigkeit der Frequenz und Auflösung (in Bits) wählen sollte. Auflösungverlust unvermeidbar. 

  <div style="text-align:center">
	<img src="./assets/a_d_wandler.jpg" class="center" width="400"/>
  </div>

**Wägevrefahren**:
Vergleiche den Wert eines D/A-Wandlers für gewisse Belegung mit dem Input über Komparator. Dazu lege beginnend beim höchstwertigen Bit eine 1 an. Setze das Bit auf 0, wenn erzeugte Spannung größer als Eingangssignal. Wiederhole mit dem nächsten Bit.

**Kompensations-/Zählverfahren**:
Inkrementiere die Zahl jeweils um eins, ansonsten wie beim Wägeverfahren. Damit werden im worst case allerdings $2^n -1$ Schritte bei $n$ Bits benötigt.

**Flash-A/D-Wandler/Parallelverfahren**:
Über $2^n -1$ Flip-Flops wird mithilfe eines Vergleichers für jeden möglichen Wert eine 1 gesetzt, wenn die erzeugte Spannung größer als die Referenz ist.

**Delta-Sigma-Wandler**:




## Regelungstechnik für Informatiker
**Regelung**:
Technischer Vorgang in abgegrenztem System, bei dem ein Regelwert fortlaufend erfasst und durch Vergleich mit einer Führungsgröße/Sollwert beeinflusst wird $\Rightarrow$ Rückkopplung des Ist-Werts. Im Gegensatz dazu wird bei der *Steuerung* kein Messglied, welches den Ist-Zustand liefert, benötigt.

Im Zeitbereich kann man ein System über DGLs, Sprungantworten oder Zustandsraumdarstellung abbilden. Im Bildbereich geschieht dies durch Übertragungsfunktion und Frequenzgang.

Charakterisierung dynamischer Systeme: 

  * Sprungfunktion
  * Sprungantwort
  	  * Anregelzeit $t_a$
	  * Ausregelzeit $t_s$
	  * Überschwingweite/Überschwinghöhe $t_p$ 
	  * Bleibende Regeldifferenz
  * Impulsfunktion
  * Frequenzgang: Amplitudenverlauf und Phasenverschiebung in Abhängigkeit der Kreisfrequenz 
  * Ortskurve: Amplitude und Phasenverschiebung in Abhängigkeit der Frequenz geplottet. Die Kreisfrequenz beginnt auf der x-Achse bei $\infty$ und endet bei 0.
  * Bode-Diagramme: Amplitude und Phase in zwei Diagrammen logarithmisch über der Kreisfrequenz aufgetragen.
  * Pol-Nullstellen-Diagramm: x für Pole und o für Nullstellen. Beide treten konjugiert komplex auf, das heißt sie treten sowohl im positiven und negativen Imaginärteil auf.


Mathematische Modelle zur Beschreibung von Signalen:

  * Kontinuierlich: Differenzialgleichungen
    * Periodisch: Fourier-Reihe
  	* Nichtperiodisch: Fourier-Transformation oder Laplace-Transformation
  * Diskret: Differenzengleichungen
  	* Diskrete Fourier-Transformation
  	* Fast Fourier-Transformation
  	* Z-Transformation

**Laplace-Transformation**: 
Transformiert vom Zeitbereich in den Bildbereich.

$F(s)=L\{f(t)\}=\int_{0}^{\infty}f(t)e^{-st}dt$

$f(t)=L^{-1}\{F(s)\}=\dfrac{1}{2\pi i}\int e^{st}F(s)ds$

$s=\sigma +j\omega$ 

Systeme werden in der Regel erst ab dem Zeitpunkt 0 betrachten, davor werden die Funktionen zu 0 angenommen. Um die Kausalität zu wahren, darf die Differentialgleichung zudem nicht von zukünftigen Werten abhängen

Regeln:

  * Linearität: $c\cdot f(t) + d\cdot g(t) \leftrightarrow c\cdot F(s)+d\cdot G(s)$
  * Differenzierung: $f'(t) \leftrightarrow s\cdot F(s)\cdot f(+0)$
  * Integration: $\int_0^t f(\theta)\cdot d\theta \leftrightarrow \dfrac{F(s)}{s}$

Durch die Transformation erhalten wir die Frequenzantwort auf der y-Achse und die Dämpfung auf der x-Achse --> Kombiniert die sofortige Antwort mit der steady-state-Antwort

### Schaltfunktionen und deren Umformungen
Übertragungsfunktion: $G(s)=\dfrac{\texttt{Ausgangsfunktion}}{\texttt{Eingangsfunktion}}=\dfrac{X(s)}{U(s)}$

Umformung:

  * Reihenschaltung: Multiplikation der Übertragungsfunktionen
  * Parallelschaltung/Addierschaltung: Addition der Übertragungsfunktionen
  * Rückkopplung/Gegenkopplung
  <div style="text-align:center">
	<img src="./assets/Gegenkopplung.jpg" class="center" width="400"/>
  </div>
  * Rückkopplung/Mitkopplung
  <div style="text-align:center">
	<img src="./assets/Mitkopplung.jpg" class="center" width="400"/>
  </div>
  * Verschiebung von Summationsstellen oder Verzweigungsstellen

Ein geschlossener Regelkreis lässt sich als offener Regelkreis darstellen:
  <div style="text-align:center">
	<img src="./assets/geschlossener_offener_regelkreis.jpg" class="center" width="400"/>
  </div>
$\Rightarrow G(s) = \dfrac{G_0(s)}{1+G_0(s)}$ für geschlossene Systeme, wobei $G_0(s)$ die Funktion für offene Systeme darstellt.

### Übertragungsglieder
  
  * P-Glied: $y=Ku \Rightarrow F(s)=K$. Verstärker-Glied, proportional zum Eingangssignal. 
  * I-Glied: $y=K\int_0^t u(r)dr \Rightarrow F(s)=\dfrac{K}{s}$. Integriert über die Regeldifferenz.
  * D-Glied: $y=K\dot{u} \Rightarrow F(s)=Ks$. Differenziert die Regeldifferenz. Verbessert Regelgeschwindigkeit und dynamische Regelabweichung. Verstärken allerdings verrauschte Anteile des Eingangssignal, dadurch wird Neigung zu Schwingungen verstärkt.
  * Totzeit(TZ)-Glied: $y=Ku(t-T) \Rightarrow F(s)=Ke^{-T_ts}$
  * VZ(Verzögerung)/PT-Glied: $T\dot{y}+y=K\cdot u \Rightarrow F(s)=\dfrac{K}{1+Ts}$
  * VD(Verzögerung)/DT-Glied: $t\dot{y}+y=K\cdot T\dot{u} \Rightarrow F(s)=\dfrac{K\cdot Ts}{1+Ts}$

$Ts$ ist die Streckenzeitkonstante, $K$ die Verstärkung.
Ein PID-Regler besteht aus einer Parallelschaltung von P-, I- und D-Glied.

### Gütekriterien 

**Stabilität**: 

  * Stabile Systeme antworten bei Anregung mit beschränkten Stellgrößen mit beschränkten Ausgangssignalen (BIBO: Bounded Input, Bounded Output). Instabile Systeme erzeugen hingegen unbeschränkte Ausgangssignale.
  * Hurwitz-Kriterium: Wenn alle Pole der Übertragungsfunktion einen negativen Realteil haben, ist ein lineares System asymptotisch stabil. Berechnung über alle Nullstellen des Nenners möglich $\Rightarrow$ Koeffizienten in Matrix schreiben und alle Unterdeterminanten überprüfen. Wenn alle nichtnegativ, dann ist Stabilität erfüllt. Es reicht bereits ein negativer Koeffizient aus, damit das Kriterium nicht erfüllt ist.
  * Nyquist-Kriterium: Verlauf der Ortskurve überprüfen. Wenn sie den Punkt $(-1, 0j)$ umschließt, ist das System instabil. 
  * Bode-Diagramm: Ist Phasengang bei der der Amplitudengang die $10^0$-Amplitude schneidet größer als 180°? Wenn ja, dann ist das System instabil.

**Schnelligkeit**: Wie schnell reagiert die Regelung?
**Genauigkeit**: Wie genau kann die Regelung auf Störungen reagieren? 
**Robustheit**: Wie robust ist die Regelung gegenüber Störungen?

  <div style="text-align:center">
	<img src="./assets/pid_regler.jpg" class="center" width="400"/>
  </div>

$R(s)=K_p(1+\dfrac{1}{T_N s}+T_V s)$ mit Vorhaltzeit $T_V$ und Nachstellzeit $T_N$


### Varianten von Regelungssystemen

 * Vorsteuerung: Steuerung, welche durch Regler nachgebessert wird um die Wirkungen von Störungen zu mitigieren. 
 * Kaskadierte Regelung: In Reihe geschaltete Regler
 * Mehrschleifige Regelung
 * Parameteradaptiv: Adaptiert Reglerparameter über Regelstrecke (beispielsweise bei Greifen von harten/weichen Gegenständen)
 * Indirekte adaptive Regelstruktur: Adaptiert Reglerparameter indrekt
 * Strukturadaptive Regelung: Struktur des Reglers (Auswahl aus mehreren Reglern) wird angepasst in Abhängigkeit der Regelstrecke 

### Diskrete Signalverarbeitung
Regler ist in der Regel Mikrorechner. Output wird D/A tranformiert um analoges Signal für Streke zu erhalten. Zeit- und wertdiskrete Darstellung. Es ist möglich zeitkontinuierliche Regelstrecken in zeitdiskrete Varianten zu überführen. 

**A/D-Wandlung**:
Abtastung (führt zu Quantisierungsfehler) 

**D/A-Wandlung**:
Ausgabe eines analogen Signals über Halteglied 

**Abtasttheorem nach Shannon**: 
Sind bei einem Signal Frquenzen im Band von $0-f_max$ vorhanden, so reicht es, das Signal in Abständen von $T_0=\dfrac{1}{2f_max}$ abzutasten um die ursprüngliche Größe ohne Informationsverlust zurückzugewinnen. 
Problem: Stimmt nicht immer. In der Praxis werden daher meist Abtastfrequenzen von 5 bis 10 $f_max$ verwendet.

Es können eine oder mehrere Abtastperioden im Regelungssystem verbaut werden. die Abtastfrequenzen sollten im Verlauf des Signalflusses steigend sein. 

**z-Transformation**:
Substituiere in der Laplace-Transformation der Faltung eines Diracstoßes mit einem Signal (Abtastung) $z=e^{Ts}$. Damit wird alles in den Einheitskreis projiziert. Hat offensichtlich dieselben Eigenschaten, wie die Laplace-Transformation. Damit kann man genauso wie im zeitkontinuierlichen System arbeiten.

**Stabilität in zeitdiskreten Systemen**:
Z-Übertragungsfunktion kann als Quotient der Produkte ihrer Nullstellen und Pole beschrieben werden. 
$G(z)=\dfrac{Y(z)}{U(z)}=b_0\dfrac{\prod_{j=1}^m (z-z_i)}{\prod_{j=1}^n (z-p_i)}$
Ein zeitdiskretes System ist stabil für $|z|<1$. Das bedeutet die Beträge aller Pole müssen kleiner als 1 sein: $\forall p: |p_i|<1$.

**Zeitdiskrete Ersatzregelstrecke für zeitkontinuierliche Regelstrecke**: 
Nur Sprung- *oder* Impulsinvariant möglich. Dazu Laplace-Rücktransformation und dann z-Transformation anwenden (oder direkt aus Tabelle transformieren).

Die Reglerauslegung in z ist eher komplziert, in s einfach!

**Korrespondenzen zwischen s- und z-Ebene**:
<table>
	<tr>
		<td>s-Ebene</td>
		<td>z-Ebene</td>x
	</tr>
	<tr>
		<td>Imaginäre Achse</td>
		<td>Einheitskreis</td>
	</tr>
	<tr>
		<td>linke komplexe Ebene</td>
		<td>Innere des Einheitskreises</td>
	</tr>
	<tr>
		<td>rechte komplexe Ebene</td>
		<td>Äußere des Einheitkreises</td>
	</tr>
</table>


**Filter**:
Hochpassfilter filtert hohe Frequenzen, Tiefpassfilter anaolg niedrige Frequenzen. Weiterhin können Bandpass/ Bandsperre gewisse Frequenzen durchlassen oder sperren. Können inzwischen auch digital implementiert werden. 
Ein digitaler Tiefpassfilter kann aber für Anti-Aliasing (Shannon-Theorem) nicht eingesetzt werden. Das Shannon-Theorem muss bereits bei der Abtastung eingehalten werden, hierfür sind analoge Filter hilfreich!


## Beispiele: Robotersteuerung und Sortierung
Robotik regelt Aktuatoren mittels Sensorfeedback (Perzeption). 

Informationsflussebenen: I/O- und Speicherebene, Verarbeitungsebene und Prozess-I/O-Ebene 

Steuerungsarten:
  
  * Point-to-Point: Vorgeben von Punkten, bspw. für Punktschweißen
  * Bahnsteuerung: Vorgeben von Bahnen mittels verschiedener Interpolationsverfahren, bspw. für Lichtbogenschweißen
  * Vielpunktsteuerung: Genaues Vorgeben des Weges, bspw. für Spritzlackieren

Bewegungssteuerung: 

  1. Interpretation der Bewegungsbefehlen (Interpreter)
  2. Interpolationsvorbereitung, bspw. Lookahead
  3. Interpolation
  4. Transformation 
  5. Regelung einzelner Bewegungen auf Achsebene (Servosteuerung)

Aufgabenstellungen von Robotern:

  * Bearbeitungsaufgaben: Positionierung/Führen von Werkzeugen
  * Handhabungsaufgaben
  * Montageaufgaben
  * Inspektionsaufgaben: Sensoraufnahmen, bspw. Inspektion von Schweißnähten

Problemstellung bei Sichtprüfung: variabler Bildinhalt führt zu variabler Rechenlast, worst case als Annahme allerdings nicht zielführend da nur selten Realität. Multikriterielles Optimierungsproblem, schwierig für Systemauslegung. Durchsatz limitiert durch Datenverarbeitung

**Approximate Computing**: 
Genauigkeit auf nötiges Niveau sodass Qualität eingehalten wird, reduzieren. Adaptive Regelung möglich in Abhängigkeit der verfügbaren Zeit.

## Sicherheitskritische Systeme

**IEC 61508**: 
Internationale Sicherheitsnorm. Entstanden da 44% der Industrie-Unfallursachen in falscher Spezifikation begründet sind. Es gibt technische und sonstige Requirements.

**Safety**: 
Schutz des Menschen vor der Maschine.
**Security**: 
Schutz der Maschine vor dem Menschen.
**Reliability**:
Wahrscheinlichkeit, dass ein Item die benötigte Funktionalität für eine spezifizierte Dauer unter spezifizierten/angenommenen Bedingungen richtig ausführt. 

Generell: Redundanz, Selbst-Tests, Scheduling von Modulaustausch, ... um Fehlerraten zu reduzieren. 

Gefahren mitigieren:

  1. Hazard Analysis
  2. Safety Function: Wie kann der Hazard verhindert werden?
  3. Safe State: Zustand in dem der Hazard nicht mehr besteht. 

**Safety Integrity Level (SIL)**:
Eine Risk Analysis bestimmt die Anforderungen an die Safety Function mittels SIL-Level. Parameter sind Auswirkungen und Wahrscheinlichkeit eines Hazards. Großes SIL-Level $\Rightarrow$ hohe Anforderungen.

  <div style="text-align:center">
	<img src="./assets/sil_levels.jpg" class="center" width="400"/>
  </div>


**Risk Assessment Protocol**:
Vorab: Analyse-Parameter festelegen, bspw. Limitationen von Maschinen. Risikoanalyse muss immer für das gesamte System erfolgen, die Analyse einer Maschine ansich ist nicht ausreichend.
  1. Identifiziere Hazards Hazards
  2. Risiken einschätzen (Severity/Likeliness fließen ein)
  3. Risiko-Rating ableiten
  4. Risiken reduzieren
  5. Effektivität evaluieren (und ggf. zurück zu Schritt 1)
  6. Resultate dokumentieren

**V-Modell**:
  * Anforderungsanalyse $\Rightarrow$ System testing
  * High-level design $\Rightarrow$ Integration testing
  * Detailliertes design $\Rightarrow$ Unit testing
  * Implementierung


**Hardware-Fehler**:
  * Systematische und zufällige Fehler
  * Permanente (Kondensator platzt) und transiente Fehler (temporär, z.B. Bitkipper)


**Software-Fehler**:
Keine zufälligen Fehler, nur systematisch. $\Rightarrow$ Testing, Entwiclkungsprozess überprüfen. Es gibt under anderem Verifizierungtools um Code zu überprüfen.

# Appendix: Things I should've known already
## Betriebssysteme
### CPU-Ausführungs-Modi
Privilegierungs-Ebenen (aufsteigend): 

  * Anwendungen (Ring 3)
  * Gerätetreiber (Ring 1+2)
  * Kernel (Ring 0)

**User Mode**: 
Nur nicht-privilegierte Befehle (Ring 3) dürfen ausgeführt werden. Kann keine Hardware managen $\Rightarrow$ geschützter Modus.

**Kernel Mode**: 
Alle Befehle erlaubt.

### Datenübertragung

Betriebsmodi:

  * Voll-Duplex: Übertragung in beide Richtungen zur gleichen Zeit
  * Halb-Duplex: Übertragung in beide Richtungen, aber nur eine zu jedem Zeitpunkt
  * Simplex: Übertragung lediglich in eine Richtung

Schnittstellen:

  * Seriell: Bits werden nacheinander übertragen
  * Parallel: Bits werden parallel übertragen, bspw. über mehrere Leitungen, an denen das Signal synchron anliegt.
  * Asynchron: Aufeinanderfolgende Zeichen kommen in variablen Abständen an. 
  * Synchron: Ankunft der Zeichen in fixen Abständen. Blöcke haben weiterhin variable Lücken.
