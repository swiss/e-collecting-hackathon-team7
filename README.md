# 7) Von der St.Galler E-Collecting Plattform zur Lösung für eidgenössische Vorlagen

*Over the course of two days, you will develop your solution for collecting electronic signatures for popular initiatives and referendums from A to Z, addressing the 10 topics outlined in the [guidelines](https://www.bk.admin.ch/bk/de/home/politische-rechte/e-collecting/aktuelles.html). Your prototype can be conceptual, clickable, and/or technical. Either way, you should clearly present the interactions and data flows between actors, software, and infrastructure components over time, as well as the user experience of these actors.*

## Approach

Der Kanton St.Gallen hat einen parlamentarischen Auftrag für die Entwicklung von Rechtsgrundlagen und einer E-Collecting-Plattform für Pilotversuche mit kantonalen Initiativen und Referenden.

Die gesetzlichen Grundlagen wurden im September verabschiedet und sehen für die Pilotphase eine sogenannte Fixanteillösung vor. Das bedeutet, dass in einem ersten Schritt höchstens 50% der benötigten Unterschriften elektronisch gesammelt werden dürfen.

Die Entwicklung der E-Collecting-Plattform wurde öffentlich ausgeschrieben und ihre Umsetzung ist bereits sehr weit fortgeschritten. Der Go-Live ist fürs Frühjahr 2026 geplant.

Vorgängig wird der Quellcode der Plattform im Rahmen eines Bug-Bounty-Programms offengelegt. Das Private Bug Bounty läuft bereits seit Ende August; das Public Bug Bounty startet Anfang Dezember.

Die E-Collecting-Plattform wurde so umgesetzt, dass sie alle rechtlichen und politischen Anforderungen im Kanton St.Gallen erfüllt. Dazu gehört unter anderem, dass… 
- Mängel bei den Stimmrechtsbescheinigungen durch die Gemeinden von der Staatskanzlei mittels Stichprobenkontrolle behoben werden müssen (Artikel 26 des Gesetzes über Referenden und Initiativen);
- Mehrfachunterzeichnungen verhindert werden müssen (Fixanteillösung, i.e. 50 Prozent der Unterschriften müssen weiterhin auf herkömmlichem Weg gesammelt werden);
- die Bestimmungen des Datenschutzgesetzes des Kantons St.Gallens erfüllt werden;
- die Gemeinden entlastet werden müssen.

Damit erfüllt die St.Galler Plattform alle Voraussetzungen, um ab dem Frühjahr 2026 erste Erfahrungen mit E-Collecting zu sammeln und die Auswirkungen aufs politische System untersuchen zu können. Auf Basis dieser Erfahrungen kann dann beurteilt werden, ob eine Anpassung von Sammelfristen oder Quoren nötig ist.

Die E-Collecting-Plattform ist so gebaut, dass sie für zukünftige Sammlungen auf Bundesebene weiterentwickelt werden kann. Bereits heute ist es möglich, die Plattform für die Prüfung und Bescheinigung der physischen Unterschriften auf allen föderalen Ebenen zu verwenden.

## Documentation and Diagrams

*Together, you will contribute to comparing different ways of how to implement e-collecting in Switzerland from A to Z. As part of the [participatory process](https://www.bk.admin.ch/bk/de/home/politische-rechte/e-collecting/partizipativer_prozess.html), your solutions will be discussed in subsequent workshops and will possibly be taken into account for the official decision on the design of the federal e-collecting trials. Proper documentation is key to ensuring that your solution can be understood and evaluated:*

1. **[Mermaid](https://mermaid.js.org/) diagram(s) showing interactions and data flows between actors, software and infrastructure components of your solution over time.**
2. **Wireframes or mockups with user flow showing the user experience of different actors** (using e.g. Figma)
3. Explain how you addressed the topics presented in the [guidelines](https://www.bk.admin.ch/bk/de/home/politische-rechte/e-collecting/aktuelles.html), filling in the template below.
4. List the key strengths and weaknesses of your solution.
5. Explanation of features used (if applicable)
6. A requirements file with all packages and versions used (if applicable)
7. Environment code to be run (if applicable)

*For your reference, you will find below an example of two diagrams showing interactions and data flows between actors, software and infrastructure components of ordering a pizza via a third-party delivery website over time. Please replace them with diagrams for your solution.*

### Flowchart: High-level Process (Example)

*An overall process flow showing the main steps and system/actor interactions for ordering a pizza online via a delivery website, including software, infrastructure, and handoff to the restaurant and delivery driver.*

```mermaid

flowchart TD
    Customer([Customer])
    DeliverySite("Delivery Website (Web/App)")
    Backend("Website Backend Server")
    OrderDB[(Order Database)]
    Restaurant("Restaurant Order System")
    Driver("Delivery Driver")

   Customer-->|"1 Place Order (Pizza+Details)"|DeliverySite
   DeliverySite-->|"2 Send Order Data"|Backend
   Backend-->|"3 Store Order"|OrderDB
   Backend-->|"4 Send Order to Restaurant"|Restaurant
   Restaurant-->|"5 Ack/Confirmation"|Backend
   Backend-->|"6 Confirmation & ETA"|DeliverySite
   DeliverySite-->|"7 Show Confirmation"|Customer
   Restaurant-->|"8 Assign/Notify Delivery"|Driver
   Driver-->|"9 Pickup & Deliver"|Customer
   Driver-->|"10 Update Status"|DeliverySite
   DeliverySite-->|"11 Show Status"|Customer

```

### Sequence Diagram: Detailed Interactions & Data Flows (Example)

*A step-by-step illustration showing how data and requests are exchanged between actors (customer, delivery site, restaurant, infrastructure), and key software components in the order process.*

```mermaid

sequenceDiagram
    actor Customer
    participant WebApp as "Delivery Website (UI)"
    participant Backend as "Backend Server"
    participant DB as "Order Database"
    participant Restaurant as "Restaurant System"
    actor Driver as "Delivery Driver"

    Customer->>WebApp: Browse menu, select pizza (menu data)
    Customer->>WebApp: Submit order (pizza, address, payment info)
    WebApp->>Backend: Create order {pizza, address, payment}
    Backend->>DB: Save order {orderId, customer, items, payment}
    Backend->>Restaurant: API: send order {orderId, items, address}
    Restaurant-->>Backend: Ack/Confirmation {orderId, ETA}
    Backend-->>WebApp: Show confirmation {orderId, ETA}
    WebApp-->>Customer: Order confirmation {orderId, ETA}
    Restaurant->>Driver: Notify driver {pickup, delivery}
    Driver-->>Restaurant: Pickup ack
    Driver->>Customer: Deliver pizza
    Driver->>Backend: Update status {orderId, delivered}
    Backend->>WebApp: Status update {delivered}
    WebApp->>Customer: Show status {delivered}

```

## User Experience

*Add or reference wireframes or mockups with user flow showing the user experience of different actors.*

## Topics addressed

Für den Hackathon wollen wir uns mit der Frage auseinandersetzen, **wie das System weiterentwickelt werden kann, so dass es auch Kantonen einen Mehrwert bringt, die (noch) nicht über ein zentrales Stimmregister verfügen**.

Dabei stellen sich Fragen wie:
- Welche Daten müssen ausgetauscht werden und auf welchem Weg?
- Wie viel Zeitverlust zwischen Unterzeichnung und Bescheinigung einer Unterschrift ist akzeptabel?
- Wie muss der Prozess aussehen, damit die Gemeinden dennoch entlastet werden können?

Um Antworten auf diese Fragen zu finden, suchen wir insbesondere Vertreterinnen und Vertreter von Gemeinden und anderen Kantonen, die sich unserem Team anschliessen möchten.

## Key Strenghts and Weaknesses

*List the key strengths and weaknesses of your solution.*

### Strengths:
- ...
- ...

### Weaknesses:
- ...
- ...

## Getting Started

*These instructions will get you a copy of the technical prototype (if applicable) up and running on your local machine for development and testing purposes. **If you are not developing a technical prototype, please present or reference your conceptual and/or clickable prototype.***

### Prerequisites

*What things you need to install the software and how to install them.*

### Installation

*A step by step series of examples that tell you how to get a development env running.*

## Contributing

Please read [CONTRIBUTING.md](/CONTRIBUTING.md) for details on our code of conduct.

## Team Members

- Sebastian Fust (Staatskanzlei St.Gallen)
- Denis Morel (Mabuco)
- Darius Bohni (442 Security)
- Johannes Schuster (Solution Architect Solution Design; Abraxas)
- Mario Odenbach (ICT-Architekt Solution Design; Abraxas)
- Fabian Geiger (Software-Ingenieur .Net Development; Abraxas)

## License

This software is licensed under a AGPL 3.0 License - see the [LICENSE](LICENSE) file for details. Please feel free to [choose any other](https://choosealicense.com/) [Open Source Initiative approved license](https://opensource.org/licenses) (e.g. a permissive license such as [MIT](https://opensource.org/license/mit)). Other content (e.g. text, images, etc.) is licensed under a [Creative Commons CC BY-SA 4.0 license](https://creativecommons.org/licenses/by-sa/4.0/deed.de). Exceptions are possible in consultation with the organizers.
