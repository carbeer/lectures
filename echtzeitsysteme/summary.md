# Echtzeitsysteme

## Echtezeitprogrammierung
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
Für zeitgesteuerte Systeme. Das zeitliche Verhalten period. Aktionen wird vorab geplant und in Zeitrastern $T$ synchronisiert. Praktisch, da festes und vorhersagbares Verhalten. Bei richtiger Planung können Rechtzeitigkeit und Gleichzeitigkeit garantiert werden $\Rightarrow$ gut für zyklische, sicherheitsrelevante Aufgaben geeignet. Allerdings geringe Flexibilität gegenüber Änderungen, keine Reaktion auf aperiodische Ereignisse, bzw. nur über periodische Überprüfung derselbigen.

**Asynchrone Programmierung**: 
Ablaufplanung zur Laufzeit durch Organisationsprogramm/Echtzeitbetriebssystem. Dieses überwacht Ereignisse und ermittelt Ablaufreihenfolgen in Reaktion hierauf $\Rightarrow$ Echtzeitscheduling. Rechtzeitigkeit ist nicht immer garantiert, v.a. bei niedrigerer Priorität, Vorabanalysen möglich.

Beispiel: Fixed Priority Preemptive Scheduling. Ereignisse haben feste Priorität nach derer sich die Ausführungsreihenfolge richtet. Unterbrechen der Ausführung von Teilprogrammen niederer Priorität möglich. 

### Ablaufsteuerung
**Zyklisch**: 
Sequentielle Ausführung von Aktivitäten. Prozessor ständig aktiv.

**Zeitgesteuert**: 
Ausführung von Aktivitäten in Abhängigkeit der aktuellen Zeit. Prozessor ständig aktiv.

**Unterbrechungsgesteuert**:
Ereignis- oder zeitgesteuerte Unterbrechungen triggern Aktivitäten. Bei gleichzeitigen Unterbrechungen/ Ereignissen wird Ausführung anhand der Prioritäten gewählt. Echtzeitscheduling, Prozessor nicht ständig aktiv.

## Echtzeitbetriebssysteme
## Rechnerarchitekturen und HW-Schnittstellen
## Echtzeitmiddleware
## Echtzeitkommunikation
## Hardwareschnittstelle zwischen Echtzeitsystem und -prozess
## Regelungstechnik für Informatiker
## Beispiele: Sichtprüfsysteme, Robotersteuerung und Automobil
## Gastvorlesung
## Sicherheitskritische Systeme
## Zusammenfassung und Abschluss

