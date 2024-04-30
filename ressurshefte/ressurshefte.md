# Workshop SIKT - Datasettbeskrivelser

## Resource Description Framework (RDF) og lenkede data

- "Alt" består av navn (URI-er) eller verdier ("Literals")
- En URI ("Uniform Resource Identifier") er et navn som peker til en ressurs.
  - URI-en er en global, unik identifikator.
  - _Kan_ peke til en faktisk ressurs på nettet, men må ikke.
- Byggeklossen: subjekt, predikat, objekt
  - _Trippel_
  - **Fakta** eller **utsagn**
- (Forenklet) huskeregel:
  - Subjekt: URI
  - Predikat: URI
  - Objekt: URI eller verdi.

Med disse byggeklossene kan vi lage rettede grafer som består av ting og relasjonene mellom tingene.

TODO: lage tegning, erstatt bildet over med denne.

## Modellere Ada Lovelace

AdaLovelace type Person

AdaLovelace navn Ada Lovelace

AdaLovelace født 10.12.1815

AdaLovelace kjenner CharlesBabbage

AdaLovelace interesse Programmering

TODO: legg til bilde av graf

## Turtle - en RDF-syntaks

- En URI skrives med "<" og ">", f.eks. `<https://example.org>`
- Tekstverdier skrives med fnutter: `"en tekst"`
- Man kan angi typen til en verdi, f.eks. heltall: `"1"^^<http://www.w3.org/2001/XMLSchema#integer>`
- Du kan angi språket til teksten: `"An english text"@en` eller `"En tekst på bokmål"@nb`.

Vi bruker ressurser som allerede er definert av andre, f.eks. RDF, Schema.org eller Friend of a friend (FOAF).

```turtle
<https://example.org/people/adaLovelace> <http://www.w3.org/1999/02/22-rdf-syntax-ns#type> <http://xmlns.com/foaf/0.1/Person> .
<https://example.org/people/adaLovelace> <http://xmlns.com/foaf/0.1/name>                  "Ada Lovelace" .
<https://example.org/people/adaLovelace> <https://schema.org/birthDate>                    "1815-12-10"^^<http://www.w3.org/2001/XMLSchema#date> .
<https://example.org/people/adaLovelace> <https://schema.org/knows>                        <https://example.org/people/charlesBabbage> .
<https://example.org/people/adaLovelace> <https://schema.org/knowsAbout>                   "Programmering"@nb .
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

```turtle
@prefix rdf:    <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix dct:    <http://purl.org/dc/terms/> .
@prefix dcat:   <http://www.w3.org/ns/dcat#> .

@prefix utdanning: <https://data.utdanning.no/> .

utdanning:forvaltningsdatabasen
    rdf:type        dcat:Dataset ;

    dct:title       "SKRIV BOKMÅLSTITTEL HER"@nb, "SKRIV NYNORSKITTEL HER"@nn ;
    dct:description "SKRIV BOKMÅLSBESKRIVELSE HER"@nb, "SKRIV NYNORSKBESKRIVELSE HER"@nn ;
    dct:publisher   <https://organization-catalogue.fellesdatakatalog.digdir.no/organizations/NI_SIFRET_ORG_NUMMER_HER> ;

    dcat:theme      <http://publications.europa.eu/resource/authority/data-theme/GOVE>
    dct:identifier  "https://data.utdanning.no/forvaltningsdatabasen";
```

### 1.2 Valider turtle-syntaksen

Sjekk at Turtle-syntaksen er korrekt ved å kopiere den utfylte beskrivelsen din fra oppgave 1.1, lime den inn i tekstfeltet på https://felixlohmeier.github.io/turtle-web-editor/ og trykk på "Validate!".

### 1.3. Legg til API-beskrivelse

```turtle
@prefix rdf:    <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix dct:    <http://purl.org/dc/terms/> .
@prefix dcat:   <http://www.w3.org/ns/dcat#> .

@prefix utdanning: <https://data.utdanning.no/> .

utdanning:forvaltningsdatabasen
    rdf:type        dcat:Dataset ;

    dct:title       "Forvaltningsdatabasen"@nb, "Forvaltningsdatabasen"@nn ;
    dct:description "Kartlegging av organiseringen til staten og ytre etater etter 1947"@nb ;
    dct:publisher   <https://organization-catalogue.fellesdatakatalog.digdir.no/organizations/919477822> ;

    dcat:theme      <http://publications.europa.eu/resource/authority/data-theme/GOVE> ;
    dct:identifier  "https://data.utdanning.no/forvaltningsdatabasen";

    dcat:distribution LENK-TIL-DISTRIBUSJONS-RESSURSEN-HER .

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

Validatoren til data.norge.no (https://data.norge.no/validator)

Kopier og lim inn den utfylte beskrivelsen din fra oppgave 1.3 i feltet "Valider tekst", trykk "Valider" nederst på siden, og se hvilke meldinger du får.

### 1.5 Finn feilen

I denne beskrivelsen skjuler det seg tre syntaksfeil. Bruk turtle-validatoren for å finne og rette dem: https://felixlohmeier.github.io/turtle-web-editor/

```turtle
TODO
```

## OPPGAVER - For de viderekomne

### 2.1

Du sitter på et imaginært datasett med oversikt over all verdens programmeringsspråk, historiske og nåværende. Du vil nå formidle til verden at du sitter på denne dataen.

Fyll ut de obligatoriske feltene for datasettet:

- dct:title - Tekst
- dct:description - Tekst
- dct:identifier - Tekst
- dct:publisher - URI til organisasjon
- dcat:theme - URI til kode.

For `dcat:theme` kan du bruke koden `TECH` fra et kjent EU-vokabular: `<http://publications.europa.eu/resource/authority/data-theme/TECH>`

og distribusjonen:

- dcat:accessURL
- dct: description
- dct:format
- dct:license
- adms:status
- dcatap:availability

```turtle
@prefix rdf:    <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix dct:    <http://purl.org/dc/terms/> .
@prefix dcat:   <http://www.w3.org/ns/dcat#> .

@prefix utdanning: <https://data.utdanning.no/> .

utdanning:forvaltningsdatabasen
    rdf:type        dcat:Dataset ;
    dct:title       "Forvaltningsdatabasen"@nb, "Forvaltningsdatabasen"@nn ;
    dct:description "Kartlegging av organiseringen til staten og ytre etater etter 1947"@nb ;
    dct:publisher   <https://organization-catalogue.fellesdatakatalog.digdir.no/organizations/919477822> ;

    dcat:theme      <http://publications.europa.eu/resource/authority/data-theme/GOVE> ;
    dct:identifier  "https://data.utdanning.no/forvaltningsdatabasen" ;

    dcat:distribution LENK-TIL-DISTRIBUSJONEN-UNDER .

utdanning:forvaltningsdatabasen-distribusjon
    rdf:type dcat:Distribution ;
    # FYLL UT ...
    .

utdanning:forvaltningsdatabasen-api
    rdf:type            dcat:DataService ;
    # FYLL UT ...
    .
```

## Oppgaver - For de modige

### 3.1

TODO: scenario
Lag en datasettbeskrivelse.

## Validering

For å validere Turtle-syntaks: https://felixlohmeier.github.io/turtle-web-editor/

For å validere datasettbeskrivelsen mot DCAT-AP-NO: https://data.norge.no/validator

## Høsting

Informasjon om hvordan registrere kilder som kan høstes til data.norge.no: https://data.norge.no/publishing/about-harvesting
