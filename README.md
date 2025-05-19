# Herhalingsoefening - Game Club
De leden van Hexion willen hun kostbare tijd enkel steken in het spelen van de beste spellen. Ze hebben met behulp van enquêtes de mening van alle PXL-studenten getest. Het resultaat van de enquête zijn twee csv bestanden. Om de data iets meer overzichtelijk en toegankelijk te maken hebben ze jou gecontacteerd om een applicatie te maken.

Aangezien de leden van Hexion zo’n sympathieke mensen zijn, hebben ze de applicatie ter beschikking gesteld aan de C# Advanced lectoren van de PXL. De lectoren kunnen zich inloggen met hun voornaam en het geheime wachtwoord om de resultaten van de enquête te bekijken.

Tot slot zal de applicatie de functionaliteit hebben om al de data te exporteren naar een XML-bestand, zodat de Hexion leden nog een flinke duit kunnen verdienen door de data van hun junior-collega’s te verkopen aan spelwinkels.

De applicatie bestaat uit twee delen: een class library, genaamd GameClubClassLibrary, en een WPF project, genaamd GameClubWPF, met twee vensters.

## 1. Class Library
In de Game Club toepassing maken we gebruik van een class library. De class library, genaamd GameClub.ClassLib, gebruiken we om de data van de videogames, boardgames en gebruikers te beheren en op te slaan.

In de class library zitten twee folders: één folder genaamd DataAccess en één folder genaamd Entities.

De DataAccess folder bevat statische klasses BoardGameData, UserData, VideoGameData. Deze klasses verwerken de csv bestanden en valideren gebruikers in het systeem. Deze klassen zitten in de GameClassLibrary.DataAccess namespace.

De tweede folder, genaamd Entities, bevat de entity klasses voor Game, BoardGame, VideoGame en een interface IRetailable. Deze klassen zitten in de GameClassLibrary.Entities namespace.

### 1.1 Statische klassen
#### 1.1.1 BoardGameData:
- Publieke eigenschap: BoardGameDataTable – DataTable
- Publieke methode: InitialiseerBoardGameData(string bgCsvPath) – void
  - Deze methode zorgt er voor dat de BoardGameDataTable geïnitialiseerd is. De methode gebruikt het pad naar het csv bestand dat meegegeven wordt om de DataTable te vullen met records.
- Publieke methode: GetBoardGameList() – List<BoardGame> of DataView
  - Deze methode zet de data uit BoardGameDataTable om naar een List van BoardGame objecten of naar een DataView. Je mag zelf kiezen of deze methode een List of een DataView teruggeeft.
 
#### 1.1.2 VideoGameData
- Publieke eigenschap: VideoGameDataTable – DataTable
- Publieke methode: InitializeVideoGameData(string vgCsvPath) – void
  - Deze methode zorgt er voor dat de VideoGameDataTable geïnitialiseerd is. De methode gebruikt het pad naar het csv bestand dat meegegeven wordt om de DataTable te vullen met records.
- Publieke methode: GetVideoGameList() – List<VideoGame>
  - Deze methode zet de data uit BoardGameDataTable om naar een List van BoardGame objecten. Gebruik hier een Linq query voor.
 
#### 1.1.3 UserData
- Private variable: adminNames - List<string>
  - Initialiseer de list met de volgende namen: Koen, Wim en Sander.
- Publieke methode: IsValidLogin(string userName, string password) – bool
  - Deze methode geeft true terug, als de gegeven userName voorkomt in de adminNames list en het password gelijk is aan “PXL123”.
 
### 1.2 Entities
Om videogames en boardgames te verwerken in de WPF applicatie gebruiken we hier entity klassen voor. Maak de volgende klassen: Game, BoardGame en VideoGame. Maak de volgende interface: IRetailable. De interface zorgt er voor dat elk spel beschikt over een prijs.

Game:
> De klasse Game implementeert de interface IRetailable. Het gedrag van de methodes wordt overschreven in de klasses BoardGame en VideoGame.
- Eigenschap: Id - int
- Eigenschap: Title - string
- Eigenschap: Rank - int
- Eigenschap: Source - string
- Eigenschap: ReleaseYear – int
- Eigenschap: GeekRating – double
- Eigenschap: AverageRating – double
- Eigenschap: NumberOfVoters – int
- Constructor met een parameter voor elke eigenschap

BoardGame:
- Eigenschap: Description - string
- Eigenschap: DistributorPrice - double
- Constructor met een parameter voor elke eigenschap. Let er op dat je geen duplicate code schrijft.
VideoGame:
- Eigenschap: IsSinglePlayerOnly - bool
- Constructor met een parameter voor elke eigenschap. Let er op dat je geen duplicate code schrijft.

### 1.3 Interface
IRetailable:
- Methode: GetAmazonPrice() – double
  - Voor een BoardGame bedraagt de Amazon prijs 120% van de DistributorPrice.
  - Voor een VideoGame bedraagt de Amazon prijs €9,99 indien het spel voor 1990 is uitgebracht, €59.99, indien het spel tussen 1990 en 2010 is uitgebracht en 69.99 indien het na 2010 is uitgebracht.
  - Let er op dat je alle prijzen afrondt op twee cijfers na de komma.
- Methode: GetGeekGameShopPrice() – double
  - Voor een BoardGame bedraagt de GeekGameShop prijs 115% van de DistributorPrice, als het spel een Rank heeft die groter is dan 50. Als het spel een Rank heeft die kleiner of gelijk is aan 50, dan bedraagt de prijs 125% van de DistributorPrice.
  - Voor een VideoGame bedraagt de GeekGameShop prijs €29,99 indien het spel voor 2000 is uitgebracht en €49.99, indien het spel na het jaar 2000 is uitgebracht. Vervolgens wordt de prijs verhoogd met 50%, als het spel een Rank heeft die kleiner of gelijk is aan 50.
  - Let er op dat je alle prijzen afrondt op twee cijfers na de komma.
 
### 1.4 Klassen Diagram
De klassen diagram van de class library ziet er als volgt uit:
#### 1.4.1 Entity klassen

![Interface](/media/Interface.png)
![Entity classes](/media/EntityClasses.png)

#### 1.4.2 Statische klassen

![Static classes](/media/StaticClasses.png)

## 2. WPF Vensters
De applicatie bevat een WPF project, genaamd GameClub.UI, dat bestaat uit twee vensters: een loginvenster (MainWindow), een overzichtsvenster (OverviewWindow).

### 2.1 Loginvenster (MainWindow)
XAML voorwaarden:
- De XAML van het overzichtsvenster kan je terugvinden in de startbestanden. Je moet deze niet zelf maken.

![Login venster](/media/Loginvenster.jpeg)

Programmeer voorwaarden:
- Als gebruiker kan ik op de “Log In”-knop klikken, om mezelf in te loggen. Een gebruiker kan enkel inloggen door de naam “Koen”, “Wim” of “Sander” in te geven met het wachtwoord “PXL123”.
  - Gebruik de IsValidLogin(string name, string password) methode uit UserData om de login poging te valideren.
  - Wanneer een gebruiker zich onsuccesvol inlogt, dan wordt een foutboodschap getoond, “Email of Password incorrect!”. Zie figuur 5.
  - Wanneer een gebruiker zich succesvol inlogt, dan wordt de gebruiker gevraagd om de source van de twee csv bestanden één per één door te geven in een dialoogvenster. Vervolgens wordt het overzichtsvenster (OverviewWindow) geopend als een dialoogvenster.
    - De gebruiker krijgt via een boodschap te zien dat hij of zij de BoardGames.csv file moet meegeven. Vervolgens wordt er dialoogvenster geopend om een bestand te selecteren. Zie figuur 6.
    - Vervolgens wordt er opnieuw een boodschap en een dialoogvenster getoond voor het VideoGames.csv bestand.

![InCorrectLogin](/media/InCorrectLogin.jpeg)

![CorrectLogin](/media/CorrectLogin.jpeg)

### 2.2 Overzichtsvenster (OverviewWindow)
XAML voorwaarden:
- De XAML van het overzichtsvenster kan je terugvinden in de startbestanden. Je moet deze niet zelf maken.

Programmeer voorwaarden:
- De constructor van OverviewWindow ontvangt twee argumenten: een pad naar de BoardGame.csv file en een pad naar de VideoGame.csv file. OverviewWindow(string boardGameCsvPath, string videoGameCsvPath)
  - De constructor maakt gebruik van de InitialiseerBoardGameData(boardGameCsvPath) methode van BoardGameData en de InitializeVideoGameData(videoGameCsvPath) methode van VideoGameData.
  - Nadat de bestanden succesvol zijn ingelezen, gebruik je de methode GetBoardGameList() van BoardGameData om de ItemsSource van DataGridBoardGames in te stellen.
  - Nadat de bestanden succesvol zijn ingelezen, gebruik je de methode GetVideoGameList() van VideoGameData om de ItemsSource van ListBoxVideoGames in te stellen.
  - Nadat de bestanden succesvol zijn ingelezen, zorg je er voor dat het eerste record in DataGridBoardGames geselecteerd is.
  - Indien er een fout optreed tijdens het inlezen van de bestanden, dan toon je een foutboodschap aan de gebruiker. Het DataGrid en de ListBox blijven in dit geval leeg.

![CorrectLogin](/media/CorrectLogin.jpeg)

![InCorrectFile](/media/InCorrectFile.jpeg)

![BoardGames](/media/BoardGames.jpeg)

Programmeer voorwaarden:
- Als gebruiker kan ik een BoardGame selecteren in DataGridBoardGames om de afbeelding en prijzen van het geselecteerde spel te zien. Gebruik hier de voorziene besturingselementen, genaamd: ImageBoardGame, TextBlockBoardGameAmazonPrice en TextBlockBoardGameGeekGameShopPrice.
- Als gebruiker kan ik op de “Export Video and Board Games to XML”-knop klikken om alle data in de BoardGameDataTable van BoardGameData en de VideoGameDataTable van VideoGameData te exporteren naar één xml bestand genaamd “Games.xml”. Je mag dit bestand opslaan via een dialoogvenster of automatisch in de root folder van je project.
- Gebruik Linq voor de volgende voorwaardes:
  - Als gebruiker kan ik op de “Top 10”-knop klikken om enkel de tien BoardGame’s te zien met een Rank die kleiner is of gelijk aan tien in DataGridBoardGames. De BoardGame’s zijn gesorteerd van één tot tien op basis van Rank.
  - Als gebruiker kan ik op de “Under €50”-knop klikken om enkel de BoardGame’s te zien die minstens één prijs hebben die lager is dan
€50,00. Sorteer de BoardGame’s van laag naar hoog op basis van hun laagste prijs. o Als gebruiker kan ik op de “Reset”-knop klikken om alle BoardGame’s te zien. De BoardGame’s zijn gesorteerd op basis van Rank.
  - Als gebruiker kan ik op de “Post 2015”-knop klikken om alle BoardGame’s te zien die zijn uitgebracht na 2015. De BoardGame’s zijn gesorteerd van nieuw naar oud.
 
![VideoGames](/media/VideoGames.jpeg)

Programmeer voorwaarden:
- Als gebruiker kan ik op een VideoGame klikken in ListBoxVideoGames om meer informatie te zien van het spel.
  - Wanneer een item geselecteerd is, toon je het ReleaseYear in TextBlockYear.
  - Wanneer een item geselecteerd is, toon je de GeekRating in TextBlockGeekRating.
  - Wanneer een item geselecteerd is, toon je de AverageRating in TextBlockAvgRating.
  - Wanneer een item geselecteerd is, toon je het aantal stemmen (NumberOfVoters) in TextBlockNumberOfVoters.
  - Wanneer een item geselecteerd is, toon je “Singleplayer Only” of “Has Multiplayer” in TextBlockGameMode afhankelijk van de waarde.
  - Wanneer een item geselecteerd is, toon je de AmazonPrice in TextBlockVideoGameAmazonPrice.
  - Wanneer een item geselecteerd is, toon je de GeekGameShopPrice in TextBlockVideoGameGeekGameShopPrice.
