---
sidebar_position: 1
description: "Architekturgrundlagen: Vom Monolithen zum modularen System"
---

# Modularer Systemaufbau

## Einleitung

Softwarearchitekturen entwickeln sich stetig weiter – angetrieben durch wachsende fachliche Anforderungen, steigende Komplexität und den Bedarf nach stabilen, wartbaren und erweiterbaren Systemen. Während klassische Monolithen lange Zeit der Standard waren, stehen heute modulare Architekturen im Fokus, die klare fachliche Strukturen schaffen und Teams effizienter arbeiten lassen.

Das Ziel eines modularen Systemaufbaus besteht darin, fachliche Domänen sauber abzugrenzen, Abhängigkeiten zu reduzieren und gleichzeitig eine Architektur zu etablieren, die langfristig evolvierbar bleibt. Modularität ist dabei kein Selbstzweck, sondern die Grundlage für robuste, verständliche und nachhaltig wartbare Software.

## Wie definiert man Modulgrenzen?

Klar definierte Modulgrenzen sind entscheidend für jede modulare Architektur – unabhängig davon, ob sie als modularer Monolith oder als Self-Contained Systems (SCS) umgesetzt wird. Gute Modulgrenzen minimieren Kopplung, schaffen fachliche Klarheit und ermöglichen eine stabile Weiterentwicklung über viele Jahre hinweg.

Modulgrenzen sollten **immer fachlich** – nicht technisch – motiviert sein. Domain-Driven Design (DDD) liefert hierfür ein geeignetes methodisches Fundament.

### Module sollten sich fachlich abgrenzen

Ein Modul bildet einen eigenständigen fachlichen Bereich mit eigener Sprache, eigenen Regeln und einem eigenen Modell.

Hinweise auf sinnvolle Modulgrenzen sind:

* unterschiedliche Bedeutungen oder Regeln für denselben Begriff  
* klar voneinander getrennte fachliche Prozesse oder Ziele  
* unterschiedliche Stakeholder oder Verantwortliche  
* verschiedene Datenmodelle  
* unterschiedliche Release- oder Änderungszyklen  

### Methoden, die bei der Identifikation von Modulgrenzen unterstützen

Zur systematischen Ermittlung fachlicher Strukturen haben sich verschiedene DDD-Methoden bewährt:

* **Storytelling**  
  Durchgehen realer Nutzungsszenarien, um Verantwortlichkeiten, Übergaben und Domänenobjekte sichtbar zu machen.
* **Event Storming**  
  Visualisierung fachlicher Ereignisse, Abläufe und Aggregates. Ereignisse zeigen sehr gut, wo Logik konzentriert ist und wo Grenzen verlaufen.
* **Context Mapping**  
  Darstellung der Beziehungen zwischen Modulen bzw. Subdomänen (z. B. Customer-Supplier, ACL, Shared Kernel), um Kopplung und Integrationspunkte zu erkennen.

Diese Methoden sind optional, aber äußerst hilfreich, um implizites Domänenwissen sichtbar zu machen und in robuste Modulgrenzen zu überführen.

### Vorgehensweise zur Ermittlung von Modulgrenzen

Ein erprobtes Vorgehen ist:

1. **Domäne explorieren**  
   Interviews, Storytelling, Prozessanalyse  
2. **Domänenverhalten sichtbar machen**  
   z. B. mittels Event Storming  
3. **Fachliche Kontexte identifizieren**  
   Bildung fachlicher Module / Subdomänen  
4. **Beziehungen und Verantwortlichkeiten klären**  
   z. B. über Context Mapping  

## Abbildung der Module in Softwaresystemen

Die Umsetzung fachlicher Module in der Softwarearchitektur beeinflusst Wartbarkeit, Erweiterbarkeit und langfristige Stabilität maßgeblich. Zwei grundlegende Architekturstile haben sich dabei etabliert:

* **Modularer Monolith**  
* **Self-Contained Systems (SCS)**

Beide basieren auf denselben Prinzipien der fachlichen Modularisierung, unterscheiden sich jedoch in Autonomie, Deployment und Skalierung.

### Self Contained System (SCS)

Ein Self-Contained System ist ein autonomes, vollständig lauffähiges Teilsystem, das **UI**, **Geschäftslogik** und **Datenhaltung** selbst kapselt. SCS eignen sich, wenn Module unabhängig voneinander entwickelt, betrieben und weiterentwickelt werden sollen.

![Self Contained System](./assets/scs.svg)

#### Vorteile

* **Klare fachliche Ownership**  
  Jedes SCS gehört einem dedizierten Team, das dafür vollständig verantwortlich ist.

* **Unabhängige Releasezyklen**  
  Änderungen können isoliert entwickelt, getestet und deployed werden.

* **Hohe fachliche und technische Entkopplung**  
  Kommunikation erfolgt über wohldefinierte APIs oder Events.

* **Unterschiedliche Technologie-Stacks möglich**  
  Teams können für ihr System die optimalen Tools und Frameworks wählen.

#### Nachteile

* **Höherer Betriebs- und Infrastrukturaufwand**  
  Mehr Deployments, Monitoring und Integrationsaufwände.

* **Verteilte Datenhaltung**  
  Globale Konsistenz muss über Events oder asynchrone Mechanismen hergestellt werden.

* **Komplexere Fehlersuche**  
  Probleme traversieren Systemgrenzen; observability muss bewusst aufgebaut werden.

* **Netzwerklatenzen und Ausfallrisiken**  
  Systemgrenzen liegen im Netzwerk – mit allen dazugehörigen Herausforderungen.

#### Wann einsetzen?

Wenn **fachliche Autonomie**, **unabhängige Releases**, **getrennte Teams** oder **unterschiedliche Skalierungsanforderungen** wichtiger sind als der zusätzliche Betriebsaufwand.

### Modularer Monolith

Ein modularer Monolith (Modulith) ist eine **gemeinsame Deployment-Einheit**, die intern aus klar abgegrenzten Modulen besteht. Jedes Modul verfügt über eigene Regeln und definiert kontrollierte Schnittstellen.

![Modularer Monolith](./assets/modulith.svg)

#### Vorteile

* **Geringerer Betriebs- und Infrastrukturaufwand**  
  Weniger Deployments, einfacheres Monitoring, klarere Betriebsprozesse.

* **Konsistente Technologie- und Laufzeitumgebung**  
  Gemeinsamer Stack, gemeinsame Tools und einheitliche Entwicklungsprozesse.

* **Einfachere Transaktions- und Konsistenzmodelle**  
  Fachliche Konsistenz kann häufig ohne komplexe verteilte Kommunikation umgesetzt werden.

* **Gute interne Entkopplung bei geringerer Gesamtsystemkomplexität**  
  Module bleiben getrennt, aber koexistieren effizient innerhalb eines Deployments.

#### Nachteile

* **Gemeinsames Deployment**  
  Änderungen an einem Modul erfordern Regressionstests und Deployment des Gesamtsystems.

* **Begrenzte Skalierungsvielfalt**  
  Skalierung erfolgt in größeren technischen Einheiten, nicht pro Modul.

* **Risiko des Architekturverfalls**  
  Ohne Disziplin verschwimmen Modulgrenzen und das System degeneriert zum klassischen Monolithen.

#### Unterschied zum klassischen Monolithen

Ein modularer Monolith unterscheidet sich vom klassischen Monolithen durch:

* **Strikte Modularisierung nach Domänen**  
* **Klare Schnittstellen und Kapselung**  
* **Reduzierte Kopplung zwischen Modulen**  
* **Vorbereitete Extrahierbarkeit einzelner Module**

Während ein klassischer Monolith fachlich kaum strukturiert ist, dient der Modulith als stabile Grundlage für langfristig wartbare Systeme und mögliche Evolution in Richtung verteilter Architekturen.

## Module vertikal schneiden

Module sollten grundsätzlich **vertikal** geschnitten werden – entlang fachlicher Verantwortlichkeiten, nicht entlang technischer Layer. Ein vertikales Modul umfasst:

* UI / API  
* Anwendungslogik  
* Datenzugriff  
* Domänenmodelle  

![vertikal schneiden](./assets/vertikal-schneiden.svg)

Vertikale Schnitte erzeugen **klar abgegrenzte, fachlich vollständige Einheiten**, die autonom getestet, verstanden und weiterentwickelt werden können. Sie verhindern technische Layer-Monolithen und ermöglichen spätere Extraktionen in SCS oder eigenständige Dienste ohne tiefgreifende Umbauten.

## Mehrere Apps im Monolithen

Ein modularer Monolith kann aus mehreren ausführbaren Einheiten bestehen, die denselben modularen Kern nutzen, z. B.:

* Hintergrundprozesse  
* Batch-Jobs  
* UI-Applikationen  
* Admin-Tools  
* API-Gateways  

Wichtig ist nicht die Anzahl der Prozesse, sondern die **gemeinsame modulare Struktur**. Alle Apps greifen auf dieselben Module zu und teilen dieselben fachlichen Regeln.

Dies kombiniert:

* **Betriebseffizienz eines Monolithen**  
* **Flexibilität mehrerer Laufzeitkomponenten**  
* **Fachliche Konsistenz und gemeinsame Releases**  

Mehrere Apps machen den Monolithen **nicht automatisch** zu einem verteilten System – die fachliche Modularität bleibt zentral.

## Empfehlungen

1. **Vermeide klassische Monolithen**, wenn langfristige Evolution, Teamwachstum oder modulare Weiterentwicklung relevant sind. Für klar begrenzte Systeme können sie ausreichend sein.  
2. **Starte mit einem modularen Monolithen**, um hohe Entwicklungsgeschwindigkeit und saubere Domänenstrukturen zu vereinen.  
3. **Schneide Module vertikal entlang der Domäne**, damit sie klar abgegrenzt und später extrahierbar bleiben.  
4. **Nutze mehrere ausführbare Einheiten im Monolithen**, wenn unterschiedliche Betriebs- oder Skalierungsanforderungen dies sinnvoll machen.  
5. **Extrahiere Module erst als SCS oder eigenständige Dienste**, wenn organisatorische oder fachliche Gründe dies notwendig machen.  

Eine Architektur aus vielen SCS oder Services sollte nur umgesetzt werden, wenn Organisation und Betrieb dafür bereit sind – und der Nutzen den Aufwand klar übersteigt.


## Weiterführende Quellen

* [Eric Evans – Domain-Driven Design](https://www.oreilly.com/library/view/domain-driven-design-tackling/0321125215/)
* [Vladislav Khononov - Einführung in Domain-Driven Design](https://dpunkt.de/produkt/einfuehrung-in-domain-driven-design/)
* [Event Storming example for System Design](https://medium.com/@lambrych/can-eventstorming-guide-the-design-workflow-6f75d8aa20e0)
* [Event Storming – Ressourcen](https://www.eventstorming.com/resources/)
* [Domain Storytelling – Offizielle Seite](https://domainstorytelling.org/)