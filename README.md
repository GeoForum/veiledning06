# Veiledning 06

## Hente ut data fra OpenStreetMap og vise i kart

I denne veiledningen skal vi hente ut noe data fra OpenStreetMap og vise fram på et kart med enkel kartografi. Vi skal se på to ulike metoder for å hente ut data fra OpenStreetMap og vi skal bruke Cartodb tjenesten for å vise fram dataene. 

Det finnes også en veiledning 07 som bruker det vi har lært i denne veiledningen og gjør noen analyser på dataene i Cartodb. 

### Eksporter data fra OpenStreetMap

OpenStreetMap er et dugnadsbasert kart der alle kan bidra. Vi skal eksportere skiløyper og adressedata på to ulike metoder. 

#### Overpass API
Først skal vi eksportere de populære skiløypene på Sjusjøen ved Lillehammer. Vi skal bruke en tjeneste som heter Overpass Turbo for å eksportere disse dataene. Start med å åpne [http://overpass-turbo.eu/s/9Hj](http://overpass-turbo.eu/s/9Hj). Du ser nå at kartet allerede er plassert over Sjusjøen. 

Venstre del består at et tekstfelt med noe kode. Og høyre av kartet. Start med å trykke på "Run"-knappen oppe i venstre hjørne. Du ser nå at alle skiløyper vises i kartet til høyre. Vi skal ikke gå igjennom hvordan du koder kodden til venstre i denne veiledningen, men en way i OpenStreetMap betyr en linjeobjekt og skiløyper tagges med piste:type=nordic i OpenStreetMap. 

Den letteste måten å finne andre objekter på er å trykke på "Wizard"-knappen øverst og legge inn de taggene en ønsker. Det er gitt noen eksempler i dialogboksen som kommer opp. Prøv deg hjerne fram, men husk å gå tilbake til koden for å hente ut skiløyper før du går videre. 

Etter at du trykker på "Run"-knappen og får skiløypene i kartet så kan du enkelt eksportere disse dataene ved å trykke på "Export". Deretter trykker du på geoJSON linken og venter til dataene er lastet ned. Det er lurt å endre navnet på filen til skiloyper.geojson når filen er lastet ned. 

#### Osmosis og osmconvert

Så skal vi eksportere adressedata for Sjusjøen. Overpass er fin å bruke for å eksportere mindre data og selv om vi fint kunne ha brukt det for å hente ut adressedata også, så er det fint å kunne en metode når du vil ha data for et litt større område en Sjusjøen. Som f.eks. hele Norge eller hele verden. 

Vi skal først laste ned dagsoppdaterte OpenStreetMap data for hele Norge. Gå til [http://download.geofabrik.de/europe/norway.html](http://download.geofabrik.de/europe/norway.html) og last ned filen norway-latest.osm.pbf

Deretter skal vi bruke to verktøy. Osmosis for å hente ut akkurat de dataene vi trenger og osmconvert for å filtrere ut data kun over Sjusjøen. Start med å innstallere Osmosis og osmconvert. Du finner oppskrifter for hvordan du gjør dette for Windows og Mac her:
- [Osmosis på Windows](http://wiki.openstreetmap.org/wiki/Osmosis/Quick_Install_%28Windows%29)
- [Osmosis på Mac](http://wiki.openstreetmap.org/wiki/Osmosis/Installation)
- [Osmconvert på Windows](http://wiki.openstreetmap.org/wiki/Osmconvert#Download)
- Osmconvert på Mac. Kjør  wget -O - http://m.m.i24.cc/osmconvert.c | cc -x c - -lz -O3 -o osmconvert

OK, da er har vi forhåpentligvis alt på plass. Nå skal vi bruke Osmosis til å hente ut adressedata fra filen vi lastet ned tidligere. Filen er i [PBF-formatet](http://wiki.openstreetmap.org/wiki/PBF_Format) som er en komprimert alternativ til det vanlige XML-formatet som OSM bruker. Vi sier derfor til osmosis av vi leser inn en PBF file og skriver en vanlig xml fil. Vi sier også at vi kun skal ha ways(dvs linjer) med taggingen addr:housenumber. Det betyr alle punkter(eller noder i OSM) som har et husnummer. 

Kjør nå følgende kommando:
osmosis --read-pbf norway-latest.osm.pbf --tf accept-nodes addr:housenumber=*  --write-xml adresser.osm 

Vi har da en stor file med alle adresser i hele Norge som heter adresser.osm. Vi skal nå hente ut kun de adressene som gjelder for Sjusjøen. Det er mange måter å gjøre det på, men i dette tilfellet så har vi laget en poly fil som inneholder et polygon som dekker Sjusjøen. Du kan lese mer om [poly-formatet her.](http://wiki.openstreetmap.org/wiki/Osmosis/Polygon_Filter_File_Format)

Vi kan da hente ut adressene for Sjusjøen med osmconvert:
osmconvert adresser.osm -B=poly/sjusjoen.poly > adresser_sjusjoen.osm


### Importer data i Cartodb

### Vise data i kart




