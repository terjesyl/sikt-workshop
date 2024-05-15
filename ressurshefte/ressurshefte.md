# Workshop SIKT - Datasettbeskrivelser

## Resource Description Framework (RDF) og lenkede data

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

## Modellere Ada Lovelace

AdaLovelace type Person

AdaLovelace navn Ada Lovelace

AdaLovelace født 10.12.1815

AdaLovelace kjenner CharlesBabbage

AdaLovelace interesse Programmering

![Ada Lovelace](./imgs/ada_lovelace_graph.drawio.svg)

## Turtle - en RDF-syntaks

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

### Prefikser/navnerom

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

### ...forenklet beskrivelse

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

### ... enda mer forenklet

Når vi gjentar subjektet kan det skrives om til

```turtle
people:adaLovelace    rdf:type               foaf:Person ;
                      foaf:name              "Ada Lovelace" ;
                      schema:birthDate       "1815-12-10"^^xsd:date ;
                      schema:knows           people:charlesBabbage ;
                      schema:knowsAbout      "Programmering"@nb .
```

Hvert utsagn som gjenbruker subjektet avsluttes med `;`. Vi avslutter med `.` som vanlig.

### Med flere verdier på samme predikat

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
          schema:knowsAbout "musikk"@nb , "matlaging"@nb , "programmering"@nb .
```

Hvert utsagn som gjenbruk subjekt+predikat avsluttes med `,`.

## DCAT-AP (Data Catalog Vocabulary – Application Profile)

Spesifikasjon for hvordan beskrive datasett og API-er.

Gjeldende versjon: https://data.norge.no/specification/dcat-ap-no

- Forteller hvilke ting som _må_, _bør_ og _kan_ beskrives.
- Viser hvilke etablerte vokabularer og kodelister som skal brukes.
- Angir multiplisitet for feltene (0..1, 1..1, 1..n)

## OPPGAVER - For de ferske

### 1.1 Fyll ut obligatoriske felter for en datasettbeskrivelse

```txt
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

# Vår egendefinerte prefix
@prefix utdanning: <https://data.utdanning.no/> .

utdanning:forvaltningsdatabasen
    rdf:type        dcat:Dataset ;

    dct:title       "SKRIV BOKMÅLSTITTEL HER"@nb, "SKRIV NYNORSKITTEL HER"@nn ;
    dct:description "SKRIV BOKMÅLSBESKRIVELSE HER"@nb, "SKRIV NYNORSKBESKRIVELSE HER"@nn ;
    dct:publisher   <https://organization-catalog.fellesdatakatalog.digdir.no/organizations/NI_SIFRET_ORG_NUMMER_HER> ;

    dcat:theme      <http://publications.europa.eu/resource/authority/data-theme/GOVE> ;
    dct:identifier  "https://data.utdanning.no/forvaltningsdatabasen" .


Test!
```

### 1.2 Valider turtle-syntaksen

Sjekk at Turtle-syntaksen er korrekt ved å kopiere den utfylte beskrivelsen din fra oppgave 1.1, lime den inn i tekstfeltet på https://felixlohmeier.github.io/turtle-web-editor/ og trykk på "Validate!".

### 1.3. Legg til API-beskrivelse

Når du skal peke fra en ressurs til en annen oppgir du bare URI-en til ressursen i objekt-posisjon. For eksempel angir du distribusjonen til et datasett med trippelet:

- `utdanning:forvaltningsdatabasen dcat:distribution utdanning:forvaltningsdatabasen-distribusjon .`.
- Husk at dette er akkurat det samme som å skrive
  - `<https://data.utdanning.no/forvaltningsdatabasen> <http://www.w3.org/ns/dcat#distribution> <https://data.utdanning.no/forvaltningsdatabasen-distribusjon> .`.

Valider Turtle-syntaksen underveis.

```turtle
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

    dcat:distribution PEK-TIL-DISTRIBUSJONS-RESSURSEN-UNDER .

utdanning:forvaltningsdatabasen-distribusjon
    rdf:type dcat:Distribution ;
    dcat:accessURL     <URL-FOR-Å-LASTE-NED-FIL> ;
    dcat:accessService PEK-TIL-DATATJENESTE-RESSURSEN-UNDER .

utdanning:forvaltningsdatabasen-api
    rdf:type            dcat:DataService ;
    dcat:endpointURL    <URL-TIL-APIETS-ENDEPUNKT> ;
    dct:identifier      "https://data.utdanning.no/forvaltningsdatabasen-api" ;
    dct:title           "FYLL-INN"@nb, "FYLL-INN"@nn .
```

### 1.4 Valider mot DCAT-AP-NO-validatoren

1. Gå til validatoren til data.norge.no (https://data.norge.no/validator)
2. Kopier og lim inn den utfylte beskrivelsen din fra oppgave 1.3 i feltet "Valider tekst"
3. Under "Regelsett" velger du "Regelsett lenke" og limer inn lenken https://raw.githubusercontent.com/Informasjonsforvaltning/dcat-ap-no/develop/shacl/DCAT-AP-NO-shacl_shapes_2.00.ttl . Da bruker du siste versjon av valideringsreglene.
4. Trykk "Valider" nederst på siden, og se hvilke meldinger du får.

### 1.5 Finn feilen

I denne beskrivelsen skjuler det seg fire syntaksfeil. Bruk turtle-validatoren for å finne og rette dem: https://felixlohmeier.github.io/turtle-web-editor/

```turtle
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

## OPPGAVER - For de viderekomne

### 2.1

Du sitter på et imaginært datasett med oversikt over all verdens programmeringsspråk, historiske og nåværende. Du vil nå formidle til verden at du sitter på denne dataen.

Fyll ut de obligatoriske feltene for datasettet:

- dct:title - Tekst (bokmål og nynorsk)
- dct:description - Tekst
- dct:publisher - URI til organisasjon
- dcat:theme - URI til kode.

For `dct:publisher`, se i oppgavene over hvordan det er gjort der.

For `dcat:theme` kan du bruke koden `TECH` fra et kjent EU-vokabular: `<http://publications.europa.eu/resource/authority/data-theme/TECH>`.

Og fyll ut disse feltene for distribusjonen:

- dcat:accessURL - URL
- dct: description - Tekst
- dct:format - URI til kode
- dct:license - URI til kode
- adms:status - URI til kode

For `dct:format` kan du f.eks. bruke koden `JSON`: `<http://publications.europa.eu/resource/authority/file-type/JSON>`

For `dct:license` kan du f.eks. bruke koden `CC_BY_4_0`: `<http://publications.europa.eu/resource/authority/licence/CC_BY_4_0>`.

For`adms:status`kan du f.eks. bruke koden`UnderDevelopment`: `<http://purl.org/adms/status/UnderDevelopment>`.

```turtle
@prefix rdf:    <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix dct:    <http://purl.org/dc/terms/> .
@prefix dcat:   <http://www.w3.org/ns/dcat#> .

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
    dcat:accessService <programming:languages-dataservice> .
    .

programming:languages-dataservice
    rdf:type            dcat:DataService ;
    dcat:accessURL      <https://example.org/api/programming/languages/> .
```

## Oppgaver - For de modige

### 3.1

Dere sitter på et datasett som inneholder karakterer knyttet til studenter fra alle eksamener tatt ved høgskoler og universiteter i Norge. Denne dataen er ikke åpen, men dere vil allikevel synliggjøre at dataen fins. I tillegg er dataen tilgjengelig for autoriserte parter gjennom et API.

Lag en beskrivelse for dette datasettet.

- Husk å legge til de prefiksene du trenger, og definer dine egne om ønskelig.
- Bruk passende koder fra kodelistene nevnt i oppgave 2.1, som vokabularene under http://publications.europa.eu/resource/authority.

```turtle
# FYLL UT

```

## Validering

### Turtle-syntaks

For å validere Turtle-syntaks: https://felixlohmeier.github.io/turtle-web-editor/

### DCAT-AP-NO

For å validere datasettbeskrivelsen mot DCAT-AP-NO: https://data.norge.no/validator

Husk å legge til lenken https://raw.githubusercontent.com/Informasjonsforvaltning/dcat-ap-no/develop/shacl/DCAT-AP-NO-shacl_shapes_2.00.ttl under "Regelsett" -> "Regelsett lenke" for å validere mot siste versjon av reglene.

## Høsting

Informasjon om hvordan registrere kilder som kan høstes til data.norge.no: https://data.norge.no/publishing/about-harvesting
