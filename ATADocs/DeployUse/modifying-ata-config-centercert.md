---
title: "Ändern der ATA-Konfiguration – Zertifikat für ATA Center | Microsoft Advanced Threat Analytics"
description: Beschreibt den zweistufigen Vorgang zum Erneuern oder Ersetzen des Zertifikats im lokalen Computerspeicher auf dem ATA Center-Server.
keywords: 
author: rkarlin
manager: stevenpo
ms.date: 04/28/2016
ms.topic: article
ms.prod: identity-ata
ms.service: advanced-threat-analytics
ms.technology: security
ms.assetid: c8855287-de3b-4cdd-be8f-2128f48a6f27
ms.reviewer: bennyl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 8d1dedaf86031e8585cca23241aead58f7f3db4e
ms.openlocfilehash: 0cfeb67e663489d7264c88aafe04e77fcb63377f


---

# Ändern der ATA-Konfiguration – Zertifikat für ATA Center

>[!div class="step-by-step"]
[« IP-Adresse von ATA Center](modifying-ata-config-centerip.md)
[IP-Adresse der ATA-Konsole »](modifying-ata-config-consoleip.md)

## Ändern des Zertifikats für ATA Center
Wenn Zertifikate ablaufen und nach dem Installieren des neuen Zertifikats im lokalen Computerspeicher auf dem ATA Center-Server erneuert oder ersetzt werden müssen, können Sie das Zertifikat in einem zweistufigen Vorgang ersetzen:

-   Erste Stufe: Aktualisieren Sie das Zertifikat, das vom ATA Center-Dienst verwendet werden soll. Zu diesem Zeitpunkt ist der ATA Center-Dienst noch an das ursprüngliche Zertifikat gebunden. Beim Synchronisieren der Konfiguration der ATA-Gateways sind zwei mögliche Zertifikate vorhanden, die für die gegenseitige Authentifizierung gültig sind. Solange ein ATA-Gateway eine Verbindung über das ursprüngliche Zertifikat herstellen kann, wird das neue Zertifikat nicht verwendet.

-   Zweite Stufe: Nachdem alle ATA-Gateways mit der aktualisierten Konfiguration synchronisiert wurden, können Sie das neue Zertifikat aktivieren, an das der ATA Center-Dienst gebunden ist. Wenn Sie das neue Zertifikat aktivieren, wird der ATA Center-Dienst an das Zertifikat gebunden. Die ATA-Gateways können den ATA Center-Dienst nicht ordnungsgemäß gegenseitig authentifizieren und versuchen, das zweite Zertifikat zu authentifizieren. Nach dem Herstellen der Verbindung mit dem ATA Center-Dienst ruft das ATA-Gateway die neueste Konfiguration ab und verfügt über ein einzelnes Zertifikat für ATA Center. (Dies gilt, bis Sie den Vorgang erneut starten.)

> [!NOTE]
> -   Wenn ein ATA-Gateway während der ersten Stufe offline geschaltet war und die aktualisierte Konfiguration nicht abrufen konnte, müssen Sie die JSON-Konfigurationsdatei im ATA-Gateway manuell aktualisieren.
> -   Das verwendete Zertifikat muss von den ATA-Gateways als vertrauenswürdig eingestuft werden.
> -   Wenn Sie nach dem Aktivieren des neuen Zertifikats ein neues ATA-Gateway bereitstellen möchten, müssen Sie das ATA-Gateway-Setuppaket erneut herunterladen.

1.  Öffnen Sie die ATA-Konsole.

2.  Wählen Sie auf der Symbolleiste die Einstellungsoption und dann **Konfiguration** aus.

    ![Symbol der ATA-Konfigurationseinstellungen](media/ATA-config-icon.JPG)

3.  Wählen Sie **ATA Center** aus.

4.  Wählen Sie unter **Zertifikat** eines der Zertifikate in der Liste aus.

5.  Klicken Sie auf **Speichern**.

6.  Daraufhin wird eine Benachrichtigung über die Anzahl der ATA-Gateways angezeigt, die mit der neuesten Konfiguration synchronisiert wurden.

7.  Nachdem alle ATA-Gateways synchronisiert wurden, klicken Sie auf **Aktivieren**, um das neue Zertifikat zu aktivieren.
    >[!IMPORTANT]
    >Bevor Sie die neue Konfiguration aktivieren, vergewissern Sie sich, dass alle ATA-Gateways mit der neuesten Konfiguration synchronisiert sind. Wird die neue Konfiguration aktiviert, bevor alle ATA-Gateways synchronisiert sind, kann es passieren, dass die ATA-Gateway nicht mehr ordnungsgemäß funktionieren. Ist mindestens eines der ATA-Gateways nicht synchronisiert, erhalten Sie diesen Fehler, wenn Sie auf „Aktivieren“ klicken:
    >
    >    ![Synchronisierungsfehler bei ATA-Gateways](media/ataGW-not-synced.png)

8.  Stellen Sie sicher, dass die Konfiguration aller ATA-Gateways nach Aktivierung der Änderung synchronisiert werden kann.

>[!div class="step-by-step"]
[« IP-Adresse von ATA Center](modifying-ata-config-centerip.md)
[IP-Adresse der ATA-Konsole »](modifying-ata-config-consoleip.md)

## Siehe auch
- [Arbeiten mit der ATA-Konsole](working-with-ata-console.md)
- [Installieren von ATA](install-ata.md)
- [Weitere Informationen finden Sie im ATA-Forum.](https://social.technet.microsoft.com/Forums/security/home?forum=mata)



<!--HONumber=Jun16_HO4-->


