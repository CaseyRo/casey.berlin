Welkom bij onze unieke muzikale ontdekkingstocht naar de 'lost records' van de NPO Top 2000. Samen met Emma heb ik, gebruikmakend van mijn IT-achtergrond en liefde voor muziek, een geautomatiseerd proces ontwikkeld om deze vergeten nummers op te sporen en samen te brengen in een Spotify playlist. 

In deze blogpost deel ik ons avontuur en de technieken die we gebruikten, van Google Sheets tot Postman, om deze speciale collectie samen te stellen. Duik met ons mee in de wereld van muzikale nostalgie en technische creativiteit!

Ondanks dat er in 2023 nog 500 extra nummers te horen waren bij NPO Radio 2, zijn er nog 2790 nummers die _ooit_ in de top 2000 stonden.

**enter: de top 2000 lost records**
Ja, we worden ook ouder. De top 2000 is constant in ontwikkeling, dus besloten Emma en ik zelf de 'lost records' van de top 2000 op te zoeken. Gelukkig is daar de wikipedia lijst die ons al een goede start gaf.

[TL:DR; hier is onze Lost Records Top 2000 lijst op spotify.](https://open.spotify.com/playlist/5Ll6uCdReV5IrNh8Rd4miH?si=975eda4970874a8d)

Het proces waar wij voor hebben gekozen:

1. De gehele lijst is in een spreadsheet gestopt. Deze hebben we vervolgens op twee criteria gefilterd:
	1. De nummers die slechts één of twee keer in de lijst hebben gestaan negeren we, die zijn niet echt spannend (volgens ons)
	2. We hebben overwogen platen van ná 2015 te negeren in de lijst, maar we hebben ze toch laten staan. Pareltjes als Mercy van muse of Sorry van Justin Bieber (grapje) mogen er ook gewoon bij zijn.
2. de lijst is nu nog simpel gesorteerd op 'van A naar Z', op artiest (en daarna de titel). We hebben ook een ranking gemaakt van 'mist lost', naar 'meest lost' record, daarvoor publiceren we later meer lijsten.

### Voor de techneuten
1. De lijst staat dus in een google sheet, elke track zoeken we vervolgens op in spotify. (gelukkig niet met de hand)
2. Het zoeken van deze platen is dmv Postman geautomatiseerd. Het eerste resultaat wat spotify ons geeft gebruiken we als trackID.
3. De trackID is vervolgens aan de lijst toegevoegd dmv een `vlookup`.
4. Na het filteren hebben we dan een lijst met trackIDs die we in een playlist moeten gooien.
5. Postman helpt ons daar weer met een `runner` op het POST `/playlists/{{playlist_id}}/tracks?uris={{trackID}}` endpoint.