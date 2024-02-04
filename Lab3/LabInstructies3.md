# Lab 3 - Data samenbrengen uit meerdere bronnen

*Vereisten*

Om het lab te kunnen starten is het van belang dat je toegang hebt tot een workspace gekoppeld aan een Fabric capacity.

*Doel*

In dit derde lab leer je Power Query in te zetten om data samen te voegen.

## Opdracht 1 - Samenvoegen van meerdere queries

1. Maak een nieuwe dataflow en lees bestand **L3O1 - Bikes.xlsx** in als **Bikes**. 

2. Lees **L3O1 - Accessories.xlsx** in als **Accessories**.

3. Creëer een nieuwe query **Products** als reference van **Accessories**. 

4. Check dat de nieuwe **Products** query geselecteerd is, open in de **Home** tab het dropdown menu **Append Queries** en kies de transformatie **Append Queries**. Selecteer in de dialoog die opent in het Dropdown menu **Bikes** en klik ok OK.

    > Laten we de M code bekijken die voor de Reference en Append stappen is gegenereerd. 
    > Als je de stap **Source** in het *Applied Steps* paneel bekijkt zie je daar de code `Accessories`.
    > Deze code staat voor de output van de query **Accessories**, het gevolg van de Reference transformatie.
    > Als je in de **home** tab de **Advanced Editor** selecteert zie je daar de volgende code:
    ```
    let
        Source = Accessories,
        #"Appended Query" = Table.Combine({Source, Bikes})
    in
        #"Appended Query"
    ```
    > Je ziet de twee stappen terug die we hebben toegevoegd, de Reference en de Append. 
    > Voor de Append transformatie heeft Power Query de M functie Table.Combine gebruikt, die de twee andere queries samenvoegt.
    > Deze functie accepteert een lijst van queries (te zien aan de accolades) als input.
    > Hetzelfde kun je voor elkaar krijgen door in het dropdown menu **Append Queries** te kiezen voor **Append Queries as New**.

5. Lees **L3O1 - Clothing.xlsx** en **L3O1 - Components.xlsx** als nieuwe queries en hernoem ze tot **Clothing** en **Components**.

6. Selecteer query **Products**, open de **Advanced Editor** en breidt de lijst van tabellen in de functie `Table.Combine` uit met de nieuwe queries. De nieuwe code ziet er dan als volgt uit: `Table.Combine({Source, Bikes, Clothing, Components})`.

7. Sluit de Advanced Editor en check het resultaat van de querystap door te kijken of de vier categorieën beschikbaar zijn in de filter control in de kolomkop van de kolom **ParentProductCategoryName**. Je kunt nu het doorladen voor de overige vier queries uitschakelen.
 
8. Geef de dataflow een toepasselijke Data destination en sluit het af. <br />

## Opdracht 2 - Samenvoegen van files uit een folder

Voor een beperkt aantal queries met dezelfde structuur zijn deze handmatige stappen goed uit te voeren. 
Dit wordt vervelend als je maandelijks een nieuwe file krijgt aangeleverd met bijvoorbeeld bijgewerkte budgetten.
Je wil hiervoor niet handmatig de query toevoegen en de lijst met gecombineerde queries bijwerken in Advanced Editor.
Gelukkig heeft Power Query daar een oplossing voor.

1. Ga naar **Workspace Jorik** en op en dataflow **L3O2**. Klik rechts bovenin het lint op **Export template** en sluit de dataflow af.
  
2. Start in jouw eigen workspace een nieuwe dataflow en selecteer midden onderin het canvas **Import from a Power Query template**. Navigeer naar het zojuist geëxporteerde template.

3. Als het geladen is en je ziet een melding over de credentials, kies dan voor **Configure connection** en in de dialoog op **Connect**. Als het goed is zie je dan data in de query preview.

    > Wat als template is klaargezet is een connectie naar een Lakehouse folder, waarbij op basis van de eerste file in de folder een Sample file is gedefinieerd. Alle files in de folder worden volgens die sample file definitie ingelezen. 

    > In het *Query Preview* paneel zie je in de eerste kolom **Source.Name** de filenaam staan. 
    > Klik op de filter control van de kolom om te verifiëren dat alle drie de files uit de folder zijn opgenomen.
    > De filenaam kan interessant zijn om eventueel elementen uit af te leiden, zoals in dit geval de **Release Year**.
    > Negeer voor nu de extra elementen die zijn toegevoegd in het *Queries* paneel, zoals de objecten in de folder **Helper Queries**.

4. Extraheer het jaartal uit **Source.Name** en hernoem het naar **Release Year**. Dit kan op verschillende manieren, bijvoorbeeld met **Replace Values** of **Split Column**. Als alternatief kun je ook **Column from Example** gebruiken op basis van **Source.Name**, de voorgestelde formule beoordelen en vervolgens **Source.Name** verwijderen.

5. Voeg een toepasselijke **Data destination** toe.

    > Wordt er een nieuwe file toegevoegd in de onderliggende folder, dan zal deze query die automatisch oppakken en hoeven er geen handmatige handelingen meer te worden uitgevoerd.
    > Dit kun je simuleren door een nieuwe file aan het lakehouse toe te voegen, **L3O2 - 2018.xlsx**.
    > Ververs nu de preview door in de **Home** tab **Refresh Preview** te selecteren en bekijk het resultaat. Mooi toch?<br />

## Opdracht 3 - Samenvoegen van worksheets uit een excel workbook

Een laatste voorbeeld van het massaal samenvoegen van bronnen is het samenvoegen van worksheets uit een excel workbook.
Kun je daar vergelijkbaar met de folder import een query definiëren die dynamisch rekening houdt met nieuwe data?

1. Maak een nieuwe dataflow en lees bestand **L3O3 - Year per Worksheet.xlsx** in. 

    > Je kunt hier individuele worksheets selecteren, maar dit leidt tot handmatige handelingen op de lange termijn. 
    > Nieuwe worksheets worden dan niet automatisch opgepakt door de query.

2. Hernoem de query tot "Products".

    > In het *Preview Query* paneel zie je een tabel met de worksheets als rijen.
    > De inhoud van de worksheets zit in de **Data** kolom verhuld.
    > Heb je verstopte worksheets of ongerelateerde worksheets in jouw workbook dan kun je ze nu eruit filteren.
    > Vervolgens kun je de relevante set samenvoegen.

5. Verwijder de kolommen **Item**, **Kind** en **Hidden**. 

6. Klik in de kolomkop van kolom **Data** op **Expand** (twee pijlen). Klik in de **Expand** dialoog die opent op OK. 

    > Als de data op elk worksheet uit tabelobjecten bestond in de workbook, zou je hier de kolomnamen kunnen selecteren in plaats van Column1, Column2 enz.
    > Aangezien dit in dit workbook niet het geval is, zul je de overbodige headerrijen moeten opschonen.

7. Selecteer in de **home** tab de transformatie **Use First Row as Headers**.

8. Open de filter control in de kolomkop van kolom **Name** en verwijder het vinkje voor waarde "Name". Dit verwijdert de headerrijen van de overige jaren.

9. Hernoem de eerste kolom tot **Release Year**.

    > Tijd om de schaalbaarheid van jouw rapport te testen. We gaan de data voor een nieuw jaar simuleren.

10. Open het onderliggende excelbestand, dupliceer worksheet **2017** en noem het "2018". Upload het bestand naar jouw Lakehouse en overschrijf het origineel.

11. Ga terug naar jouw Dataflow en ververs de preview. Controleer of de nieuwe data goed is doorgekomen. 

13. Open het onderliggende excelbestand opnieuw en voeg een nieuw eerste worksheet toe met data voor 2014. Upload het bestand naar jouw Lakehouse en overschrijf het origineel.

14. Probeer de query in Power Query Editor opnieuw te verversen. Dit levert nu een foutmelding op: "The column '2015' of the table was not found". Verander voor een korte termijnoplossing van deze fout in de formule van de stap **Changed Type** in het *Applied Steps* paneel de waarde "2015" naar "2014".

15. Doe vervolgens hetzelfde in de stap **Renamed Columns**. De query geeft nu weer een foutloos resultaat.

    > Als je kan aannemen dat de toegevoegde data in jouw worksheets altijd nieuwe data betreft, dan gaat de bovenstaande methode goed.
    > Maar wat als dit niet per se het geval hoeft te zijn. Hoe kun je de bovenstaande query aanpassen zodat er niet steeds handmatige handelingen nodig zijn?<br />

## Opdracht 4 - Een robuuste aanpak voor het samenvoegen van worksheets

Deze opdracht introduceert nieuwe concepten om de robuustheid van je queries te verbeteren en verversingsfouten te voorkomen.
De tekortkomingen van de vorige opdracht waren als volgt:
* In stap 7 werd de eerste rij als kolomkoppen gebruikt en daarbij werden de datatypen automatisch afgeleid. Dit resulteerde in een harde verwijzing naar "2015" in de formule. Door de formule niet aan te passen vond later de verversingsfout plaats.
* In stap 9 hernoemde je de kolom, wat ook resulteerde in een harde verwijzing naar "2015", wat ook kon leiden tot een verversingsfout. 
In deze opdracht ga we deze valkuilen verhelpen.

1. Selecteer de **Products** query en verwijder de stap **Changed Type** in het *Applied Steps* paneel. 

2. Selecteer de **Renamed Columns** stap en vervang in de formule de waarde "2014" door `Table.ColumnNames(#"Filtered Rows"){0}`

    > In plaats van een directe verwijzing naar de naam van de kolom, vertel je Power Query in M code de naam van de *eerste kolom* te wijzigen.
    > Hoe gebeurde dat precies? Door gebruik te maken van de functie **Table.ColumnNames**. Die geeft de kolomnamen terug van de tabel die als argument wordt meegegeven.
    > Die tabel is in dit geval **#"Filtered Rows"**, het resultaat van de vorige stap in de *Applied Steps*. 
    > Om Power Query naam naar het eerste element in de kolomnamen te doen verwijzen, heb je een index toegevoegd in accolades. 
    > De indicering begint bij 0, zodat {0} naar het eerste element in de lijst verwijst.

3. Om de creatie van de robuuste kolom te voltooien moet je nog de datatypen instellen:
    * Maak van de kolom **Release Year** een **Whole Number**.
    * Maak van de kolom **StandardCost** een **Decimal Number**.
    * Maak van de kolom **ListPrice** een **Decimal Number**.

4. Toets de query door in het onderliggende excel bestand een nieuw eerste worksheet toe te voegen, het bestand te uploaden en de preview te verversen.<br />

## Table of Contents

[Lab 1 - Een eerste blik op Dataflow Gen2](../Lab1/LabInstructies1.md)\
[Lab 2 - Datapreparatie uitdagingen](../Lab2/LabInstructies2.md)\
[Lab 3 - Data samenbrengen uit meerdere bronnen](../Lab3/LabInstructies3.md) (huidige module)\
[Lab 4 - Combineren van afwijkende tabellen](../Lab4/LabInstructies4.md)

