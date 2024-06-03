# Workshop SIKT - Datasettbeskrivelser

## Intro

### Resource Description Framework (RDF) og lenkede data

- "Alt" består av navn (URI-er) eller verdier ("Literals")
- En URI ("Uniform Resource Identifier") er et navn som peker til en ressurs.
  - URI-en er en global, unik identifikator.
  - _Kan_ peke til en faktisk ressurs på nettet, men må ikke.
- Byggeklossen i en RDF-graf: subjekt, predikat, objekt
  - _Trippel_
  - **Fakta** eller **utsagn**
- (Forenklet) huskeregel:
  - Subjekt: URI
  - Predikat: URI
  - Objekt: URI eller verdi.

Med disse byggeklossene kan vi lage rettede grafer som består av ting og relasjonene mellom tingene.

![Rettet graf](./imgs/graph_example1.drawio.svg)

### Hvorfor RDF?

- Maskinlesbart.
- Samhandlingsevne/interoperabilitet:
  - Har et globalt navnerom: betyr at vi kan snakke om de samme tingene på tvers av systemer og domener.
  - Ulike datasett kan enklere knyttes sammen (lenkede data).
  - Tilrettelegger for gjenbruk av vokabularer og egenskaper på tvers av systemer og løsninger.
- Semantikken følger med dataen når man bruker definerte egenskaper; dataen blir selvforklarende.
- Håndterer kompleks data som er vanskelig å definere i tabellform.
- Distribuerte spørringer.
- En stor verktøykasse er tilgjengelig (for modellering, validering, spørringer, distribuert data, med mer).

Viktig antakelse: åpent verdenssyn (Open world assumption).

Se hva W3C selv [skriver om bruksområder for RDF](https://www.w3.org/TR/rdf11-primer/#section-use-cases).

#### RDF-serialiseringer/syntakser

Det fins flere ulike måter å uttrykke RDF-grafer. I tabellen under har vi listet noen av de vanligste syntaksene for å serialisere RDF. Du trenger ikke kunne alle, bare å kjenne til at de fins.

RDF-verktøy og biblioteker kan lese til og skrive fra en eller flere av disse syntaksene. De facto standard er RDF/XML, men Turtle støttes også av de aller fleste verktøy.

| Navn      | Mediatype               | File extension |
| --------- | ----------------------- | -------------- |
| Turtle    | `text/turtle`           | `.ttl`         |
| RDF/XML   | `application/rdf+xml`   | `.rdf`         |
| JSON-LD   | `application/ld+json`   | `.jsonld`      |
| N-Triples | `application/n-triples` | `.nt`          |
| N-Quads   | `application/n-quads`   | `.nq`          |
| TriG      | `application/trig`      | `.trig`        |
| Notation3 | `text/n3;charset=utf-8` | `.n3`          |

### Modellere Ada Lovelace

AdaLovelace type Person

AdaLovelace navn Ada Lovelace

AdaLovelace født 10.12.1815

AdaLovelace kjenner CharlesBabbage

AdaLovelace interesse Programmering

![Ada Lovelace](./imgs/ada_lovelace_graph.drawio.svg)

### Turtle - en RDF-syntaks

- En URI skrives med "<" og ">", slik: `<https://example.org>`
- Tekstverdier skrives med anførselstegn: `"en tekst"`
- Man kan angi typen til en verdi, f.eks. heltall: `"1"^^<http://www.w3.org/2001/XMLSchema#integer>`, eller dato: `"2024-06-05"^^<http://www.w3.org/2001/XMLSchema#date>`
- Du kan angi språket til teksten: `"An english text"@en` eller `"En tekst på bokmål"@nb`.

Vi bruker ressurser som allerede er definert av andre, f.eks. RDF, Schema.org eller Friend of a friend (FOAF). Vi kan da uttrykke grafen som beskriver Ada Lovelace i RDF:

```turtle
<https://example.org/people/adaLovelace>   <http://www.w3.org/1999/02/22-rdf-syntax-ns#type>   <http://xmlns.com/foaf/0.1/Person> .
<https://example.org/people/adaLovelace>   <http://xmlns.com/foaf/0.1/name>                    "Ada Lovelace" .
<https://example.org/people/adaLovelace>   <https://schema.org/birthDate>                      "1815-12-10"^^<http://www.w3.org/2001/XMLSchema#date> .
<https://example.org/people/adaLovelace>   <https://schema.org/knows>                          <https://example.org/people/charlesBabbage> .
<https://example.org/people/adaLovelace>   <https://schema.org/knowsAbout>                     "Programmering"@nb .
```

#### Prefikser/navnerom

Med prefikser (forkortelser) kan vi erstatte URL-ene med prefiksen vi har definert.
F.eks. kan vi skrive `foaf` i stedet for `<http://xmlns.com/foaf/0.1/>`.

Og om vi vil bruke egenskapen "navn", `<http://xmlns.com/foaf/0.1/name>` trenger vi kun skrive `foaf:name`.

Kjente og ofte brukte navnerom (+ noen vi trenger til datasettbeskrivelser):

```turtle
@prefix rdf:    <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix rdfs:   <http://www.w3.org/2000/01/rdf-schema#> .
@prefix foaf:   <http://xmlns.com/foaf/0.1/> .
@prefix xsd:    <http://www.w3.org/2001/XMLSchema#> .
@prefix dct:    <http://purl.org/dc/terms/> .
@prefix owl:    <http://www.w3.org/2002/07/owl#> .
@prefix schema: <https://schema.org/> .

@prefix cv:     <http://data.europa.eu/m8g/> .
@prefix dcat:   <http://www.w3.org/ns/dcat#> .
@prefix dcatap: <http://data.europa.eu/r5r> .
@prefix dcatno: <https://data.norge.no/vocabulary/dcatno#> .

# Egendefinert prefiks for eksemplene her
@prefix people: <https://example.org/people/> .
```

#### ...forenklet beskrivelse

Med dette kan vi forenkle beskrivelsen av Ada Lovelace

```turtle
@prefix rdf:    <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix foaf:   <http://xmlns.com/foaf/0.1/> .
@prefix xsd:    <http://www.w3.org/2001/XMLSchema#> .
@prefix schema: <https://schema.org/> .

@prefix people: <https://example.org/people/> .


people:adaLovelace rdf:type               foaf:Person .
people:adaLovelace foaf:name              "Ada Lovelace" .
people:adaLovelace schema:birthDate       "1815-12-10"^^xsd:date .
people:adaLovelace schema:knows           people:charlesBabbage .
people:adaLovelace schema:knowsAbout      "Programmering"@nb .
```

Fortsatt på formen subjekt, predikat, objekt

#### ... enda mer forenklet

Når vi gjentar subjektet kan det skrives om til

```turtle
people:adaLovelace    rdf:type               foaf:Person ;
                      foaf:name              "Ada Lovelace" ;
                      schema:birthDate       "1815-12-10"^^xsd:date ;
                      schema:knows           people:charlesBabbage ;
                      schema:knowsAbout      "Programmering"@nb .
```

Hvert utsagn som gjenbruker subjektet avsluttes med `;`. Vi avslutter med `.` som vanlig.

#### Med flere verdier på samme predikat

```turtle
people:adaLovelace  rdf:type  foaf:Person ;
          # ... kommentert vekk
          schema:knowsAbout   "litteratur"@nb ;
          schema:knowsAbout   "matematikk"@nb ;
          schema:knowsAbout   "programmering"@nb .
```

Når vi gjentar [ subjekt predikat ] kan vi skrive det om til

```turtle
people:adaLovelace   rdf:type        foaf:Person ;
          # ... kommentert vekk
          schema:knowsAbout "musikk"@nb , "matematikk"@nb , "programmering"@nb .
```

Hvert utsagn som gjenbruk subjekt+predikat avsluttes med `,`.

### DCAT-AP (Data Catalog Vocabulary – Application Profile)

Spesifikasjon for hvordan beskrive datasett og API-er.

Gjeldende versjon: https://data.norge.no/specification/dcat-ap-no

- Forteller hvilke ting som _må_, _bør_ og _kan_ beskrives.
- Viser hvilke etablerte vokabularer og kodelister som skal brukes.
- Angir multiplisitet for feltene (0..1, 1..1, 1..n)

### Datasettkatalogen: dcat:Catalog

En katalog er en samling av datasett. I de fleste av oppgavene under har vi for enkelhetsskyld utelatt `dcat:Catalog`, men for å lage en fullstendig beskrivelse som kan høstes til https://data.norge.no må du knytte alle datasettene til en katalog:

```turtle
@prefix dcat: <http://www.w3.org/ns/dcat#> .

<https://data.digdir.no/catalog/workshop-katalog> a dcat:Catalog ;
    # ...
    dcat:dataset <https://data.digdir.no/dataset/workshop-datasett1> ,
                 <https://data.digdir.no/dataset/workshop-datasett2> ;
    .

<https://data.digdir.no/dataset/workshop-datasett1> a dcat:Dataset ;
    # ...
    .

<https://data.digdir.no/dataset/workshop-datasett2> a dcat:Dataset ;
    # ...
    .
```

## OPPGAVER

### OPPGAVER - For de ferske

Det er ikke meningen du skal bruke veldig lang tid på hver av disse oppgavene. Om du står fast eller oppgaven er uklar, spør en av oss, eller ta en kikk i løsningsforslaget.

#### 1.0 Hva sier denne beskrivelsen?

```turtle
# // oppgaver/1_0.ttl


# - 1. Hva er bokmålstittelen til datasettet?
# - 2. Hva er den engelske tittelen til datasettet?
# - 3. Hva er organisasjonsnummeret til katalogens utgiver? (hint: se på dct:publisher)
# - 4. Hva er den fullstendige URI-en til datatjenesten ("DataService")? Skal angis som fullstendig URI og ikke på prefiks-form.
# - 5. Hva er URL-en til API-ets endepunkt?
# - 6. Hva er eposten man kan kontakte for spørsmål knyttet til datasettet?


@prefix dcat: <http://www.w3.org/ns/dcat#> .
@prefix dct: <http://purl.org/dc/terms/> .
@prefix foaf: <http://xmlns.com/foaf/0.1/> .
@prefix vcard: <http://www.w3.org/2006/vcard/ns#> .
@prefix foaf: <http://xmlns.com/foaf/0.1/> .
@prefix xsd: <http://www.w3.org/2001/XMLSchema#> .
@prefix vcard: <http://www.w3.org/2006/vcard/ns#> .

@prefix digdirdataservices: <https://data.digdir.no/dataservice/> .

<https://data.digdir.no/catalog/workshop-katalog> a dcat:Catalog ;
    dct:identifier "https://data.digdir.no/catalog/workshop-katalog" ;
    dct:publisher <https://organization-catalog.fellesdatakatalog.digdir.no/organizations/991825827> ;
    dct:title "Eksempelkatalog til workshop"@nb ;
    dct:description "Katalog for å vise eksempler til workshop"@nb ;
    dcat:dataset <https://data.digdir.no/dataset/workshop-datasett1> ;
    foaf:homepage <https://digdir.no> ;
    .

<https://data.digdir.no/dataset/workshop-datasett1> a dcat:Dataset ;
    dct:identifier "https://data.digdir.no/dataset/workshop-datasett1" ;
    dct:title "Kult datasett!"@nb, "Cool dataset!"@en ;
    dct:description "En eksempeldatasett for workshop."@nb ;
    dct:publisher <https://organization-catalog.fellesdatakatalog.digdir.no/organizations/991825827> ;
    dcat:contactPoint [ a vcard:Organization ;
            vcard:hasEmail <mailto:fellesdatakatalog@digdir.no> ;
            vcard:hasOrganizationName "DIGDIR Felles datakatalog"@nb ] ;
    dcat:landingPage <https://www.digdir.no/> ;
    .

digdirdataservices:workshop-api a dcat:DataService ;
    dct:identifier "https://data.digdir.no/apis/workshop-api" ;
    dct:title "Workshop API"@nb ;
    dct:description "Dette er en eksempel-API til workshop"@nb ;
    dct:publisher <https://organization-catalog.fellesdatakatalog.digdir.no/organizations/991825827> ;
    dcat:endpointURL <https://data.digdir.no/apis/workshop-api/> ;
    dcat:servesDataset <https://data.digdir.no/dataset/workshop-datasett1> ;
    .
```

#### 1.1 Fyll ut obligatoriske felter for en datasettbeskrivelse

```turtle
# // oppgaver/1_1.ttl

# Under er en beskrivelse av datasettet Forvaltningsdatabasen, med de obligatoriske feltene i henhold til DCAT-AP-NO.
# Men noen felter mangler riktig innhold:
# - Tittel under dct:title
# - Beskrivelse ved dct:description.
# - Organisasjonsnummer i URI-en ved dct:publisher
# Erstatt teksten i store bokstaver med din egen tekst.
# Du kan enten søke opp Forvaltningsdatabasen for å oppgi så korrekt informasjon som mulig,
# eller dikte opp informasjon selv.

@prefix rdf:    <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix dct:    <http://purl.org/dc/terms/> .
@prefix dcat:   <http://www.w3.org/ns/dcat#> .

# Vår egendefinerte prefix:
@prefix utdanning: <https://data.utdanning.no/> .

utdanning:forvaltningsdatabasen
    rdf:type        dcat:Dataset ;

    dct:title       "SKRIV BOKMÅLSTITTEL HER"@nb, "SKRIV NYNORSKITTEL HER"@nn ;
    dct:description "SKRIV BOKMÅLSBESKRIVELSE HER"@nb, "SKRIV NYNORSKBESKRIVELSE HER"@nn ;
    dct:publisher   <https://organization-catalog.fellesdatakatalog.digdir.no/organizations/NI_SIFRET_ORG_NUMMER_HER> ;

    dcat:theme      <http://publications.europa.eu/resource/authority/data-theme/GOVE> ;
    dct:identifier  "https://data.utdanning.no/forvaltningsdatabasen" .

```

#### 1.2 Valider turtle-syntaksen

Sjekk at Turtle-syntaksen er korrekt ved å kopiere den utfylte beskrivelsen din fra oppgave 1.1, lime den inn i tekstfeltet på https://felixlohmeier.github.io/turtle-web-editor/ og trykk på "Validate!". Du skal få tilbakemelding om at syntaksen er korrekt.

Hvis du får en feilmelding får du beskjed om linjenummer validatoren feiler på. Prøv å rette feilen og valider på nytt.

#### 1.3. Legg til API-beskrivelse

```turtle
# // oppgaver/1_3.ttl


# Under er en ufullstendig beskrivelse av et datasett som tilbys over et API. Erstatt URI-ene og tekstene i store bokstaver med passende innhold.

# - For dcat:endpointURL kan du finne et reelt endepunkt eller dikte opp et eget.
# - For dct:title kan du finne på et tittel for API-et.
# - For dcat:servesDataset må du peke til datasett-ressursen i beskrivelsen.

# Når du skal peke fra en ressurs til en annen oppgir du bare URI-en til ressursen i objekt-posisjon. For eksempel angir du datatjenesten/API-et til et datasett med trippelet:

# - `utdanning:forvaltningsdatabasen-api dcat:servesDataset utdanning:forvaltningsdatabasen .`.
# - Husk at dette er akkurat det samme som å skrive
#   - `<https://data.utdanning.no/forvaltningsdatabasen-api> <http://www.w3.org/ns/dcat#servesDataset> <https://data.utdanning.no/forvaltningsdatabasen> .`.

# Valider Turtle-syntaksen underveis.


@prefix rdf:    <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix dct:    <http://purl.org/dc/terms/> .
@prefix dcat:   <http://www.w3.org/ns/dcat#> .

@prefix utdanning: <https://data.utdanning.no/> .

utdanning:forvaltningsdatabasen
    rdf:type        dcat:Dataset ;

    dct:title       "Forvaltningsdatabasen"@nb, "Forvaltningsdatabasen"@nn ;
    dct:description "Kartlegging av organiseringen til staten og ytre etater etter 1947"@nb ;
    dct:publisher   <https://organization-catalog.fellesdatakatalog.digdir.no/organizations/919477822> ;

    dcat:theme      <http://publications.europa.eu/resource/authority/data-theme/GOVE> ;
    dct:identifier  "https://data.utdanning.no/forvaltningsdatabasen";
    .

utdanning:forvaltningsdatabasen-api
    rdf:type            dcat:DataService ;
    dct:identifier      "https://data.utdanning.no/forvaltningsdatabasen-api" ;
    dct:title           "FYLL-INN"@nb, "FYLL-INN"@nn ;
    dcat:endpointURL    <URL-TIL-APIETS-ENDEPUNKT> ;
    dcat:servesDataset  <URI-TIL-DATASETT-RESSURSEN-OVER> ;
    .

```

#### 1.4 Valider mot DCAT-AP-NO-validatoren

1. Gå til validatoren til data.norge.no (https://data.norge.no/validator)
2. Kopier og lim inn den utfylte beskrivelsen din fra oppgave 1.3 i feltet "Valider tekst"
3. Trykk "Valider" nederst på siden, og se hvilke meldinger du får.

Merk: Hvis du har fylt ut alle de obligatoriske feltene for hver ressurs i beskrivelsen skal det ikke komme noen feilmeldinger. Men det kan være at du får noen feilmeldinger fordi de eksterne ressursene beskrivelsen peker til (som f.eks. en kodeliste) har noen mangler. Om du er i tvil hva feilen kommer av, spør en av oss.

#### 1.5 Finn feilen

```turtle
# // oppgaver/1_5.ttl


# I denne beskrivelsen skjuler det seg fire syntaksfeil. Bruk turtle-validatoren for å finne og rette dem: https://felixlohmeier.github.io/turtle-web-editor/
# Validatoren skal til slutt gi beskjed om at syntaksen er korrekt.

@prefix rdf:    <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix dct:    <http://purl.org/dc/terms/> .
@prefix foaf:   <http://xmlns.com/foaf/0.1/> .
@prefix dcat:   <http://www.w3.org/ns/dcat#> .
@prefix prov:   <http://www.w3.org/ns/prov#> .
@prefix provno: <https://data.norge.no/vocabulary/provno#> .
@prefix vcard:  <http://www.w3.org/2006/vcard/ns#> .
@prefix xsd:    <http://www.w3.org/2001/XMLSchema#> .

@prefix public_data: <https://data.norge.no/public_datasets/> .
@prefix digdir: <https://digdir.no/>

public_data:ai_projects_norwegian_state_dataset a dcat:Dataset ;
    dct:identifier "https://data.norge.no/public_datasets/ai_projects_norwegian_state_dataset" ;
    dct:title "Kunstig intelligens - oversikt over prosjekter i offentlig sektor"@nb ;
    dct:description "En oversikt over kunstig intelligens-prosjekter i offentlig sektor. Oversikten er ikke komplett."@nb ;
    dcat:theme <http://eurovoc.europa.eu/3030 , <http://publications.europa.eu/resource/authority/data-theme/GOVE> ;
    dct:publisher <https://organization-catalog.fellesdatakatalog.digdir.no/organizations/991825827> ;
    prov:wasGeneratedBy provno:collectingFromThirdparty ;
    dcat:distribution public_data:ai_projects_norwegian_state_distribution ;
    dct:spatial <http://publications.europa.eu/resource/authority/country/NOR> ;
    dct:keyword "kunstig intelligens"@nb  "offentlige prosjekter"@nb ;
    dcat:contactPoint digdir:contactInfo ;
    dct:accessRights <http://publications.europa.eu/resource/authority/access-right/PUBLIC> ;
    foaf:page <https://data.norge.no/kunstig-intelligens> ;
    .

digdir:contactInfo a vcard:Organization ;
    vcard:email "kunstigintelligens@digdir.no" .

public_data:ai_projects_norwegian_state_distribution a dcat:Distribution ;
    dcat:accessURL <https://github.com/Informasjonsforvaltning/ai-project-service/blob/main/ai_projects_norwegian_state%20-%20Oversatt_v1.csv> ;
    dct:description "CSV-fil med oversikt over kunstig intelligens-prosjekter i offentlig sektor"@nb ;
    dct:format <https://www.iana.org/assignments/media-types/text/csv> ;
    dcat:mediaType <https://www.iana.org/assignments/media-types/text/csv> ;
    dct:license <http://publications.europa.eu/resource/authority/licence/APACHE_2_0>
    foaf:page <https://github.com/Informasjonsforvaltning/ai-project-service> ;
    dcat:downloadURL <https://raw.githubusercontent.com/Informasjonsforvaltning/ai-project-service/main/ai_projects_norwegian_state%20-%20Oversatt_v1.csv> ;
    dct:language <http://publications.europa.eu/resource/authority/language/NOB> ;
    dct:issued "2023-02-23"^^xsd:date ;
    .

```

### OPPGAVER - For de viderekomne

#### 2.1

```turtle
# // oppgaver/2_1.ttl


# Du sitter på et imaginært datasett med oversikt over all verdens programmeringsspråk, historiske og nåværende. Datasettet er tilgjengelig som en statisk JSON-fil. Du vil nå formidle til verden at du sitter på denne dataen.

# Fyll ut de obligatoriske feltene for datasettet:

# - dct:title - Tekst (bokmål og nynorsk)
# - dct:description - Tekst
# - dct:publisher - URI til organisasjon
# - dcat:theme - URI til kode.

# For `dct:publisher`, se i oppgavene over hvordan det er gjort der.

# For `dcat:theme` kan du bruke koden `TECH` fra et kjent EU-vokabular: `<http://publications.europa.eu/resource/authority/data-theme/TECH>`.

# Og legg til disse egenskapene for distribusjonen:

# - dcat:accessURL - URL
# - dct: description - Tekst
# - dct:format - URI til kode
# - dct:license - URI til kode
# - adms:status - URI til kode

# For `dct:format` kan du f.eks. bruke koden `JSON`: `<http://publications.europa.eu/resource/authority/file-type/JSON>`

# For `dct:license` kan du f.eks. bruke koden `CC_BY_4_0`: `<http://publications.europa.eu/resource/authority/licence/CC_BY_4_0>`.

# For`adms:status`kan du f.eks. bruke koden`UnderDevelopment`: `<http://purl.org/adms/status/UnderDevelopment>`.

@prefix rdf:    <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix dct:    <http://purl.org/dc/terms/> .
@prefix dcat:   <http://www.w3.org/ns/dcat#> .
@prefix adms:   <http://www.w3.org/ns/adms#> .

@prefix programming: <https://example.org/catalogs/programming/> .

programming:languages
    rdf:type           dcat:Dataset ;
    dct:identifier     "https://example.org/catalogs/programming/languages" ;
    dcat:distribution  programming:languages-distribution ;
    # FYLL UT ...
    .

programming:languages-distribution
    rdf:type dcat:Distribution ;
    # FYLL UT ...
    .

```

### Oppgaver - For de modige

#### 3.1

```turtle
# // oppgaver/3_1.ttl


# Dere sitter på et datasett som inneholder karakterer knyttet til studenter fra alle eksamener tatt ved høgskoler og universiteter i Norge. Denne dataen er ikke åpen, men dere vil allikevel synliggjøre at dataen fins. I tillegg er dataen tilgjengelig for autoriserte parter gjennom et API.

# Lag en beskrivelse for dette datasettet.

# - Husk å legge til de prefiksene du trenger, og definer dine egne om ønskelig.
# - Bruk passende koder fra kodelistene nevnt i oppgave 2.1, som vokabularene under http://publications.europa.eu/resource/authority.


# FYLL UT
```

## Validering

### Turtle-syntaks

For å validere Turtle-syntaks:
https://felixlohmeier.github.io/turtle-web-editor/

`easyrdf.org` lar deg også konvertere mellom ulike RDF-serialiseringer.
https://www.easyrdf.org/converter

### DCAT-AP-NO

For å validere datasettbeskrivelsen mot DCAT-AP-NO: https://data.norge.no/validator

## Høsting

Informasjon om hvordan registrere kilder som kan høstes til data.norge.no: https://data.norge.no/publishing/about-harvesting

## Flere ressurser

### Artikler

- [RDF Primer](https://www.w3.org/TR/rdf11-primer/): en innføring i RDF, fra W3C.
- [Best Practice Recipes for Publishing RDF Vocabularies](https://www.w3.org/TR/swbp-vocab-pub/), W3C.
- [Best Practices for Publishing Linked Data](https://www.w3.org/TR/ld-bp/), W3C.
- [10 regler for varige URI-er](https://joinup.ec.europa.eu/collection/semic-support-centre/document/10-rules-persistent-uris), Interoperable Europa.
- [data.norge.no sitt "showroom"](https://data.norge.no/showroom/overview) som gir et overblikk over de ulike katalogene på data.norge.no og hvordan de henger sammen, i tillegg til flere eksempler på beskrivelser.

### Verktøy

- [datacatalogtordf](https://github.com/Informasjonsforvaltning/datacatalogtordf): Et Python-bibliotek for å mappe fra Python-objekter til en datakatalog i RDF-format i henhold til DCAT-AP-NO.
- [SPARQL-spørringer på data.norge.no](https://data.norge.no/sparql): kan gjøre spørringer på alt innhold i den nasjonale datakatalogen.

### Spec'er (teknisk)

- [Data Catalog Vocabulary (DCAT)](https://www.w3.org/TR/vocab-dcat-3/): W3C-standard for beskrivelse av datakataloger.
- [DCAT-AP](https://semiceu.github.io/DCAT-AP/releases/3.0.0/#DataService): EU sin applikasjonsprofil for DCAT.
- [DCAT-AP-NO](https://data.norge.no/specification/dcat-ap-no): den norske applikasjonsprofilen for DCAT, basert på og kompatibel med DCAT-AP.
- [Shapes Constraint Language (SHACL)](https://www.w3.org/TR/shacl/): brukes for å definere regler for RDF-grafer og å validere i henhold til disse (brukes i DCAT-AP-NO-validatoren).
- [SPARQL Query Language](https://www.w3.org/TR/sparql11-query/): for å gjøre spørringer på RDF-grafer.

I tillegg inneholder data.norge.no oversikt over:

- begreper (https://data.norge.no/om-begrepskatalogen)
- datatjenester/API-er (https://data.norge.no/om-api-katalogen)
- informasjonsmodeller (https://data.norge.no/om-informasjonskatalogen)
- tjenester og hendelser (https://data.norge.no/specification/cpsv-ap-no)

I likhet med datasett-katalogen høstes disse ressursene fra virksomheter i offentlig sektor, og er ment å tilrettelegge for gjenbruk på tvers av virksomheter. De ulike katalogene kan brukes i sammenheng og knyttes opp mot hverandre. For eksempel kan man peke til relevante begreper i en datasettbeskrivelse, eller peke til en informasjonsmodell som et datasett eller en datatjeneste implementerer.
