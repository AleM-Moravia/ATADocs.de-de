---
title: Planen der ATA-Bereitstellung | Microsoft Advanced Threat Analytics
description: "Hilft bei der Planung Ihrer Bereitstellung und der Entscheidung, wie viele ATA-Server für Ihr Netzwerk erforderlich sind."
keywords: 
author: rkarlin
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-ata
ms.service: advanced-threat-analytics
ms.technology: security
ms.assetid: 279d79f2-962c-4c6f-9702-29744a5d50e2
ms.reviewer: bennyl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: d6e7d7bef97bfc4ffde07959dd9256f0319d685f
ms.openlocfilehash: ff8eb5361d3dfeaa3715d325ed91c0ad422211ed


---

# ATA-Kapazitätsplanung
Dieses Thema hilft Ihnen bei der Ermittlung der für Ihr Netzwerk erforderlichen Anzahl der ATA-Server. Es wird erläutert, wie viele ATA-Gateways und ATA-Lightweight-Gateways benötigt werden und welche Serverkapazität für ATA Center und die ATA-Gateways erforderlich ist.

## Größenzuteilung für ATA Center
Für die Analyse des Benutzerverhaltens benötigt das ATA Center die Daten von mindestens 30 Tagen. Der erforderliche Speicherplatz für die ATA-Datenbank pro Domänencontroller wird unten ausgeführt. Wenn mehrere Domänencontroller vorhanden sind, ergibt sich der Speicherplatzbedarf insgesamt aus der Summe des erforderlichen Speicherplatzes für die einzelnen Domänencontroller.
> [!NOTE] 
> Bei Ausführung als virtueller Computer wird kein dynamischer Arbeitsspeicher und keine andere Speichererweiterungsfunktion unterstützt.

|Pakete pro Sekunde&#42;|CPU (Kerne&#42;&#42;)|Arbeitsspeicher (GB)|Datenbankspeicher pro Tag (GB)|Datenbankspeicher pro Monat (GB)|IOPS&#42;&#42;&#42;|
|---------------------------|-------------------------|-------------------|---------------------------------|-----------------------------------|-----------------------------------|
|1,000|2|32|0.3|9|30 (100)
|10.000|4|48|3|90|200 (300)
|40.000|8|64|12|360|500 (1.000)
|100.000|12|96|30|900|1.000 (1.500)
|400.000|40|128|120|1.800|2.000 (2.500)
&#42;Tägliche durchschnittliche Gesamtanzahl der Pakete pro Sekunde von allen Domänencontrollern, die von allen ATA-Gateways überwacht werden.

&#42;&#42;Hier eingeschlossen sind physische Kerne, keine Hyperthreading-Kerne.

&#42;&#42;&#42;Durchschnittliche Anzahlen (Spitzenwerte)
> [!NOTE]
> -   ATA Center kann in der Summe aller überwachten Domänencontroller maximal 400.000 Frames pro Sekunde behandeln.
> -   Die hier vorgegebenen Speichermengen sind Nennwerte und sollten stets im Hinblick auf künftiges Wachstum angepasst werden. Das Laufwerk, auf dem sich die Datenbank befindet, sollte mindestens 20 % freien Speicherplatz aufweisen.
> -   Wenn der freie Speicherplatz auf 20 % oder 100 GB fällt, werden die Daten der ältesten Sammlung gelöscht. Dieser Vorgang wird weitergeführt, bis entweder die Daten nur noch zwei Tagen entsprechen oder der verbleibende Speicherplatz unter 5 % oder unter 50 GB fällt. In diesem Fall wird die Datensammlung abgebrochen.
> -  Die Speicherlatenz für Lese- und Schreibvorgänge sollte unter 10 ms betragen.
> -  Das Verhältnis zwischen Schreib- und Lesevorgängen beträgt unterhalb von 100.000 Paketen pro Sekunde etwa 1:3 und oberhalb dieser Grenze etwa 1:6.

## Auswählen des richtigen Gatewaytyps für die Bereitstellung
In einer ATA-Umgebung wird jede Kombination der ATA-Gatewaytypen unterstützt:

- Ausschließlich ATA-Gateways
- Ausschließlich ATA-Lightweight-Gateways
- Eine Kombination aus beiden Typen

Berücksichtigen Sie Folgendes bei der Entscheidung für den Bereitstellungstyp für das Gateway:

|Gatewaytyp|Vorteile|Kosten|Bereitstellungstopologie|Domänencontrollerverwendung|
|----|----|----|----|-----|
|ATA-Gateway|Eine Out-of-Band-Bereitstellung erschwert es Angreifern, herauszufinden, ob ATA vorhanden ist.|Höher|Neben dem Domänencontroller installiert (out-of-band).|Unterstützt bis zu 50.000 Pakete pro Sekunde.|
|ATA-Lightweight-Gateway|Erfordert keinen dedizierten Server und keine Konfiguration der Portspiegelung.|Kleinschreibung|Auf dem Domänencontroller installiert.|Unterstützt bis zu 10.000 Pakete pro Sekunde.|

Es folgen Beispiele für Szenarien, in denen Domänencontroller durch das ATA-Lightweight-Gateway abgedeckt werden sollten:


- Filialstandorte

- Virtuelle Domänencontroller, die in der Cloud bereitgestellt wurden (IaaS).


Es folgen Beispiele für Szenarien, in denen Domänencontroller durch das ATA-Gateway abgedeckt werden sollten:


- Rechenzentren an Hauptstandorten (mit Domänencontrollern mit mehr als 10.000 Paketen pro Sekunde).


## Dimensionierung von ATA-Lightweight-Gateways

Ein ATA-Lightweight-Gateway kann die Überwachung eines Domänencontrollers basierend auf der Menge des vom Domänencontroller erzeugten Netzwerkverkehrs unterstützen. 
> [!NOTE] 
> Bei Ausführung als virtueller Computer wird kein dynamischer Arbeitsspeicher und keine andere Speichererweiterungsfunktion unterstützt.

|Pakete pro Sekunde&#42;|CPU (Kerne&#42;&#42;)|Arbeitsspeicher (GB)&#42;&#42;&#42;|
|---------------------------|-------------------------|---------------|
|1,000|2|6|
|5,000|6|16|
    |10.000|10|24|

&#42;Gesamtanzahl der Pakete pro Sekunde auf dem von dem betreffenden ATA-Lightweight-Gateway überwachten Domänencontroller.

&#42;&#42;Gesamtmenge der Kerne ohne Hyperthreading, die auf diesem Domänencontroller installiert sind.<br>Hyperthreading ist zwar für das ATA-Lightweight-Gateway akzeptabel, bei der Kapazitätsplanung sollten Sie aber die tatsächlichen Kernen und nicht die Kerne mit Hyperthreading zählen.

&#42;&#42;&#42;Gesamtgröße des auf diesem Domänencontroller installierten Arbeitsspeichers.
> [!NOTE]   
> Wenn der Domänencontroller nicht über die für das ATA-Lightweight-Gateway erforderlichen Ressourcen verfügt, wird die Leistung des Domänencontrollers zwar nicht beeinträchtigt, aber das ATA-Lightweight-Gateway funktioniert möglicherweise nicht wie erwartet.


## Bemessung von ATA-Gateways

Berücksichtigen Sie Folgendes bei der Entscheidung, wie viele ATA-Gateways bereitgestellt werden sollen.

-   **Active Directory-Gesamtstrukturen und -Domänen**<br>
    ATA kann den Datenverkehr aus mehreren Domänen einer einzigen Active Directory-Gesamtstruktur überwachen. Die Überwachung mehrerer Active Directory-Gesamtstrukturen erfordert separate ATA-Bereitstellungen. Für die Überwachung des Netzwerkdatenverkehrs von Domänencontrollern in unterschiedlichen Gesamtstrukturen sollte keine einzelne ATA-Bereitstellung konfiguriert werden.

-   **Portspiegelung**<br>
Überlegungen zur Portspiegelung machen möglicherweise die Bereitstellung mehrerer ATA-Gateways pro Rechenzentrum oder Filialstandort erforderlich.

-   **Kapazität**<br>
    Ein ATA-Gateway kann die Überwachung von mehreren Domänencontrollern unterstützen, abhängig vom Umfang des Netzwerkverkehrs der überwachten Domänencontroller. 
<br>

> [!NOTE] 
> Dynamischer Arbeitsspeicher wird nicht unterstützt.

|Pakete pro Sekunde&#42;|CPU (Kerne&#42;&#42;)|Arbeitsspeicher (GB)|
|---------------------------|-------------------------|---------------|
|1,000|1|6|
|5,000|2|10|
|10.000|3|12|
|20.000|6|24|
|50.000|16|48|
&#42;Durchschnittliche Gesamtanzahl der Pakete pro Sekunde von allen Domänencontrollern, die von dem betreffenden ATA-Gateway überwacht werden, und zwar zum Zeitpunkt der höchsten Nutzung.

& #42;Die Gesamtmenge des per Portspiegelung verarbeiteten Domänencontroller-Datenverkehrs darf die Kapazität der für das Sammeln verwendeten NIC auf dem ATA-Gateway nicht überschreiten.

&#42;&#42;Hyperthreading muss deaktiviert sein.



## Abschätzung des Datenverkehrs für Domänencontroller
Es gibt verschiedene Tools, die Sie verwenden können, um die durchschnittliche Anzahl der Pakete pro Sekunde eines Domänencontrollers zu ermitteln. Auch ohne den Einsatz solcher Werkzeuge können Sie diesen Leistungsindikator mit dem Systemmonitor ermitteln.

Um die Pakete pro Sekunde zu ermitteln, gehen Sie auf jedem Domänencontroller wie folgt vor:

1.  Öffnen Sie den Systemmonitor.

    ![Abbildung des Systemmonitors](media/ATA-traffic-estimation-1.png)

2.  Erweitern Sie **Datensammlersätze**.

    ![Abbildung der Datensammlersätze](media/ATA-traffic-estimation-2.png)

3.  Klicken Sie mit der rechten Maustaste auf **Benutzerdefiniert**, und wählen Sie **Neu** &gt; **Datensammlersatz** aus.

    ![Abbildung eines neuen Datensammlersatzes](media/ATA-traffic-estimation-3.png)

4.  Geben Sie einen Namen für den Sammlersatz ein, und wählen Sie **Manuell erstellen (Erweitert)** aus.

5.  Wählen Sie unter **Welcher Datentyp soll eingeschlossen werden?** die Option **Datenprotokolle und Leistungsindikator erstellen** aus.

    ![Abbildung für Datentyp des neuen Sammlersatzes](media/ATA-traffic-estimation-5.png)

6.  Klicken Sie unter **Welche Leistungsindikatoren möchten Sie protokollieren?** auf **Hinzufügen**.

7.  Erweitern Sie **Netzwerkadapter**, wählen Sie **Pakete/Sek.** aus, und wählen Sie die richtige Instanz aus. Wenn Sie sich dabei nicht sicher sind, können Sie **&lt;Alle Instanzen&gt;** auswählen und auf **Hinzufügen** und **OK** klicken.

    > [!NOTE]
    > Führen Sie die Befehlszeile `ipconfig /all` aus, um den Namen des Adapters und die Konfiguration zu ermitteln.

    ![Abbildung – Hinzufügen von Leistungsindikatoren](media/ATA-traffic-estimation-7.png)

8.  Ändern Sie das **Abtastintervall** auf **1 Sekunde**.

9. Legen Sie den Speicherort fest, an dem die Daten gespeichert werden sollen.

10. Wählen Sie unter **Neuen Datensammlersatz erstellen** die Option **Diesen Datensammlersatz jetzt starten** aus, und klicken Sie auf **Fertig stellen**.

    Jetzt sollte der gerade erstellte Datensammlersatz mit einem grünen Dreieck als Zeichen der Funktion dargestellt werden.

11. Beenden Sie nach 24 Stunden den Datensammlersatz, indem Sie mit der rechten Maustaste auf das zugehörige Symbol klicken und die Option **Beenden** auswählen.

    ![Abbildung für das Beenden des Datensammlersatzes](media/ATA-traffic-estimation-12.png)

12. Wechseln Sie im Datei-Explorer zu dem Ordner, in dem die BLG-Datei gespeichert wurde, und doppelklicken Sie darauf, um sie im Systemmonitor zu öffnen.

13. Wählen Sie den Leistungsindikator für Pakete/Sekunde aus, und notieren Sie die durchschnittlichen und maximalen Werte.

    ![Abbildung Leistungsindikator für Pakete pro Sekunde](media/ATA-traffic-estimation-14.png)

## Siehe auch
- [Voraussetzungen für ATA](ata-prerequisites.md)
- [ATA-Architektur](ata-architecture.md)
- [Weitere Informationen finden Sie im ATA-Forum.](https://social.technet.microsoft.com/Forums/security/home?forum=mata)



<!--HONumber=Jun16_HO4-->


