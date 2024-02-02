# Lab 1 - Een eerste blik op Dataflow Gen2

*Vereisten*

Om het lab te kunnen starten is het van belang dat je toegang hebt tot een workspace gekoppeld aan een Fabric capacity.

*Doel*

In dit eerste lab importeer je de eerste data als query en passen we eenvoudige transformaties toe.

## Opdracht 1 - De eerste query

1. Navigeer in jouw workspace naar Data Factory, klik op **New** en kies voor **Dataflow Gen2**. De Power Query interface wordt geopend.

2. Klik onder de tab **Home** op **Get Data**. De **Get Data** dialoog opent, waarna je alle beschikbare brontypen (of connectoren) kunt bekijken. Je kunt de zoekbalk gebruiken om gericht te zoeken naar **Lakehouse**. Selecteer deze als source. Klik dan op **Next** in de dialoog voor het gebruiken van jouw organizational account als connectie.

3. Navigeer in jouw Workspace naar **Files > Source** en selecteer **L1O1.xlsx**. Klik op **Create** om de dialoog te sluiten.

4. In het *Queries* paneel, hernoem de query **L1O1.xlsx** naar **Products**. Dit kan door op de query te dubbelklikken. Dit kan ook door in het *Query Settings* paneel (aan de rechterkant) in de properties de **Name** aan te passen.

5. Klik in het middenpaneel op **[Table]** in kolom **Data** en de inhoud van de excel wordt getoond.

## Opdracht 2 - De eerste transformatie

    > In het *Preview* paneel zie je dat de laatste twee kolommen **Cost** en **Price** zijn. Stel dat je een nieuwe kolom wilt toevoegen voor de Profit die de Cost van de Price aftrekt. Om een nieuwe kolom toe te voegen die is gebaseerd op een berekening op twee andere kolommen, zul je eerst de twee kolommen moeten selecteren. Om een kolom te selecteren kun je de kolomkop in het *Preview* paneel selecteren. Om meerdere kolommen te selecteren kun je de Shift toets gebruiken voor aangrenzende kolommen of de Ctrl toets voor niet aangrenzende kolommen.

1. Selecteer de kolom **Price**. Selecteer dan met behulp van Shift of Ctrl de kolom **Cost**.

2. Om een nieuwe kolom toe te voegen, zoek de relevante transformatie in de **Add column** tab. Ga naar **Add Column**, bekijk de verschillende opties en selecteer dan **Standard**. Je ziet nu de verschillende rekenkundige transformaties waarop je de nieuwe kolom kunt baseren. Selecteer **Subtract** in het drop-down menu.

3. Als de kolom is toegevoegd in het *Preview* paneel, hernoem het dan naar **Profit**. Om een kolom te hernoemen kun je op de kolomkop dubbelklikken. Je kunt **Rename** ook terugvinden als een van de verschillende transformatieopties als je rechtsklikt op de kolomkop.

4. Bekijk de waarden in de kolom **Profit**. Die zouden positief moeten zijn. Zijn ze negatief, dan ligt dat aan de volgorde waarin je de kolommen hebt geselecteerd in stap 2. Om dat aan te passen moeten we de volgende stappen doorlopen:
  - Kijk naar het *Applied Steps* paneel aan de rechterkant van de Power Query Editor. Hier staan alle stappen die zijn gegenereerd. Je kunt elke stap selecteren en de output zien in het *Preview* paneel. Die data is niet opgeslagen, maar een cache preview van de werkelijke data.
  - Selecteer de stap **Inserted Subtraction** in **Applied Steps**.
  - Zoek in de formule in de **Formula bar** naar de volgende code: [Cost] - [Price] en vervang het door [Price] - [Cost]. De volledige formule ziet er nu als volgt uit: \
    = Table.AddColumn(#"Changed Type", "Subtraction", each [Price] - [Cost], Int64.Type)

    > Nu we net beginnen met Power Query is het nog niet nodig de syntax in de **Formula bar** te begrijpen. Deze formule is deel van de M taal. In de loop van de training gaan we leren wanneer en hoe deze formules aan te passen, zonder een expert te zijn in de M syntax.

5. Verwijder de **Product Number** kolom door de kolom te selecteren en op de **Delete** toets te drukken. Je kunt ook in de **Home** tab **Remove Columns** selecteren.

6. Probeer de data te filteren. Stel dat je alleen de rijen over wilt houden die de tekst "Mountain" bevatten. Om dit te doen, selecteer je de filter control in de kolomkop van de kolom **Product**. In het filterpaneel dat opent staan de verschillende producten. Selecteer **Text Filters** en dan **Contains...**. Vul in de **Filter Rows** dialoog die opent "Mountain" in en klik op OK.

    > Standaard behandelt Power Query tekst op een case-sensitive manier. Als je "mountain" invult bij stap 6 is de resultaatset leeg. Om een case-insensitive filter toe te passen kun je de M formule aanpassen. De originele formule ziet er als volgt uit:
    > = Table.SelectRows(#"Removed Columns", each Text.Contains([Product], "Mountain"))
    > Door Comparer.OrdinalIgnoreCase toe te voegen als derde argument aan de Text.Contains functie kunnen we een case-insensitive filter toepassen. De M formule ziet er dan zo uit:
    > = Table.SelectRows(#"Removed Columns", each Text.Contains([Product], "mountain", Comparer.OrdinalIgnoreCase))
    > Misschien ben je bezorgd dat deze aanpassing op dit moment nog te geavanceerd is, maar de meeste uitdagingen in datapreparatie kun je af zonder de formules te hoeven aanpassen. In een aantal gevallen kan zo'n aanpassing handig zijn, maar je hoeft niet de complete M syntax te kennen.

7. Voeg tenslotte een doelbestemming toe voor jouw dataflow. Klik rechts onderin op de **+** naast **Data destination** en kies voor **Lakehouse**. Klik op **Next** en navigeer naar jouw persoonlijke **Workspace** en **Lakehouse**. Geef de doeltabel en naam, bijvoorbeeld **Lab1_Products** en Klik op **Next**. Laat de **Update method** op **Replace** staan en klik op **Save settings**.

8. Klik op **Publish** om de dataflow te publiceren en af te sluiten.

## Opdracht 3 - De data bewerken

1. Open het bronbestand **L1O1.xlsx** en doe wat aanpassingen. Open, nadat je de file hebt bijgewerkt, jouw dataflow en kijk hoe jouw query omgaat met de gewijzigde data. Om de query te verversen, selecteer **Refresh** in de **Home** tab. Dit is de kern van automatisering en een tijdbesparend element in Power Query. Je kunt de data eenmalig prepareren en dan een refresh starten om de data automatisch te prepareren op elk moment.

    > Je kunt de verversing van de data ook periodiek inplannen, door bij de dataflow in jouw workspace naar **Schedule refresh** te gaan (wacht tot de publish gelukt is). 

2. Selecteer een van de bestaande stappen in **Applied Steps**, pas het aan of verwijder ze en voeg nieuwe stappen toe. Om een stap toe te voegen tussen bestaande stappen, selecteer de stap waarna de nieuwe stap moet komen en selecteer een transformatiestap uit het lint of na rechtsklikken op een kolomkop.

    Je hebt nu de originele brontabel geïmporteerd en getransformeerd.


## Table of Contents

[Lab 1 - Een eerste blik op Dataflow Gen2](../Lab1/LabInstructies1.md) (huidige module)\
[Lab 2 - Datapreparatie uitdagingen](../Lab2/LabInstructies2.md)\
[Lab 3 - Data samenbrengen uit meerdere bronnen](../Lab3/LabInstructies3.md)\
[Lab 4 - Combineren van afwijkende tabellen](../Lab4/LabInstructies4.md)
