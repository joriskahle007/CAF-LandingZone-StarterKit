# CAF-LandingZone-StarterKit

## Wofür brauchst Du das Landing Zone StarterKit?

In vielen Azure-Abonnements fehlt es an grundlegender Governance. Dieses Repository hilft Dir, genau das zu verbessern – es stellt Dir grundlegende Governance-Einstellungen für Dein Azure-Abonnement bereit. Wichtig: Dieses StarterKit ersetzt keine gut durchdachte Azure Landing Zone.

Wenn Du gerade erst in die Welt der Azure Landing Zones eintauchst, schau Dir unbedingt diesen <a href:"https://learn.microsoft.com/azure/cloud-adoption-framework/ready/landing-zone/">Artikel zum Cloud Adoption Framework (CAF)</a>an.
Falls Du (noch) nicht mit einer CAF-basierten Landing Zone starten willst, bietet Dir dieses Repository einen guten Einstieg in die wichtigsten Governance-Grundlagen.


### Die 5 Disziplinen der Cloud Governance

Diese fünf Disziplinen bilden das Fundament für Cloud-Governance und helfen Dir, Dein Unternehmen vor typischen Stolperfallen zu schützen:

- Kostenmanagement
- Sicherheitsgrundlagen
- Ressourcenkonsistenz
- Identitätsmanagement
- Bereitstellungsbeschleunigung

Dieses StarterKit unterstützt Dich bei den ersten Schritten in den Bereichen **Kostenmanagement, Sicherheit und Identität**.

#### Kostenmanagement

Das Minimum, das Du für Dein Azure-Abonnement umsetzen solltest: Zwei grundlegende Warnmeldungen in Azure Cost Management einrichten – eine für Dein Budget und eine für Anomalien im Verbrauch.

##### Budget-Warnung einrichten

In diesem Repository findest Du ein Modul zur Bereitstellung einer Budget-Warnung. Alle benötigten Parameter dafür legst Du in der Datei azskmain.parameters.json im Stammverzeichnis fest. Hier ein Überblick:


|Parameter|Beschreibung|Standardwert|
|---|---|---|
|azskBudgetname|Der Name der Budget-Warnung|'Azure StarterKit Budget'|
|azskBudgetAmount|Der Budgetwert, gegen den getestet werden soll. Sie legen den Geldbetrag fest, auf den der Alarm reagieren soll. Wenn Sie den Wert auf 150 setzen, wird der Alarm ausgelöst, sobald die Ausgaben den prozentualen Anteil des primären und sekundären Schwellenwerts erreichen|no default|
|azskEmailsForAlert|E-Mail-Adresse, an die der alert gesendet werden soll|no default|
|azskBudgetStartDate|Das Startdatum für die Prüfung der aktuellen Kosten anhand der Budgetgrenze|'2022-12-01'|
|azskBudgetEndDate|Das Enddatum dieses Kosten-/Budgettests.|'2025-12-01'|
|azskBudgetFirstThreshold|Wenn dieser Prozentsatz erreicht ist, wird der erste Alarm gesendet |80|
|azskBudgetSecondThreshold|der zweite prozentuale Schwellenwert für die Auslösung des Alarms|100|

##### Anomalie-Warnung
Azure Cost Management erlaubt Dir auch das Einrichten von Anomalie-Warnungen. Du bekommst automatisch eine E-Mail, wenn ungewöhnliche Kostenprognosen festgestellt werden. Du kannst mehrere solcher Alarme im Portal definieren.

#### Grundlegende Sicherheit
Das StarterKit weist Deinem Abonnement eine Reihe von Azure-Richtlinien zu. Damit legst Du die Grundlage für Sicherheit und Zugriffssteuerung. Alle Richtlinien sind in Gruppen unterteilt:

##### Identitäts- und Zugriffsmanagement
|Policy|Descitpion|Referenz|
|---|---|---|
|Lege einen Sicherheitskontakt fest, der bei sicherheitsrelevanten Vorfällen benachrichtigt wird.|[Definition](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Government/Security%20Center/ASC_Security_contact_email.json)|
|Maximal 3 Eigentümer pro Abonnement zur Risikominimierung|[Definition](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Government/Security%20Center/ASC_DesignateLessThanXOwners_Audit.json)|

##### Netzwerk
|Policy|Descitpion|Referenz|
|---|---|---|
|Alle Netzwerkports müssen durch NSGs abgesichert sein|[Definition](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Government/Security%20Center/ASC_UnprotectedEndpoints_Audit.json)|
|Öffentlich erreichbare VMs sollten durch NSGs geschützt sein|Schützen Sie Ihre virtuellen Maschinen vor potenziellen Bedrohungen, indem Sie den Zugriff auf sie mit Netzwerksicherheitsgruppen (NSGs) beschränken. Erfahren Sie mehr über die Kontrolle des Datenverkehrs mit NSGs unter [https://aka.ms/nsg-doc](https://aka.ms/nsg-doc)|[Definition](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Government/Security%20Center/ASC_NetworkSecurityGroupsOnInternetFacingVirtualMachines_Audit.json)|
|Subnetze sollten mit NSGs verknüpft sein|[Definition](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Government/Security%20Center/ASC_NetworkSecurityGroupsOnSubnets_Audit.json)|
|Network Watcher sollte aktiviert sein, um den Netzwerkverkehr zu überwachen|[Defintion](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Network/NetworkWatcher_Enabled_Audit.json)|

##### Sicherheit
|Policy|Descitpion|Referenz|
|---|---|---|
|Stelle sicher, dass Deine Systeme regelmäßig Updates erhalten|[Definition](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Azure%20Government/Security%20Center/ASC_MissingSystemUpdates_Audit.json)|

Alle Richtlinien werden in einer **Initiative** gebündelt und automatisch Deinem Abonnement zugewiesen. So kannst Du auf einen Blick sehen, wie gut Deine Azure-Ressourcen mit den Sicherheitsvorgaben übereinstimmen:
![Ansicht zur Einhaltung von Azure-Richtlinien](/media/AuditReport.png)


## Wie setzt Du das StarterKit ein?
Ganz einfach – folge diesen Schritten:

1. Melde Dich bei Azure an

    ```azurecli
    az login
    ```

2. Wähle Dein Abonnement aus

    ```azurecli
    az account show --output table
    ```

    Suche in der Ausgabe das gewünschte Abonnement und setze es:

    ```azurecli
    $subscriptionID = "your subscription ID"
    ``` 

    Einrichten des Kontos für die Verwendung der Subscription

    ```azurecli
    az account set --subscription $subscriptionID
    ```

3. Bereitstellung starten
Du hast zwei Optionen:

- Option 1: Klassische Bereitstellung mit .json-Datei

    ```$location = "your preferred location"
    az deployment sub create --location $location --template-file "azskmain.bicep" --parameters "azskmain.parameters.json" --confirm-with-what-if
    ```

- Option 2: Neue Bereitstellung mit .bicepparam-Datei

  Erstelle eine Datei azskmain.bicepparam, um die Parameter zu definieren. Dafür brauchst Du:
- Azure CLI **2.48.1** oder neuer
- Bicep **0.16.2** oder neuer
- Eine konfigurierte bicepconfig.json (Beispiel im Repo)

    ```
    $location = "your preferred location"
    az deployment sub create --location $location --template-file "azskmain.bicep" --parameters "azskmain.bicepparam" --confirm-with-what-if
    ```
  
