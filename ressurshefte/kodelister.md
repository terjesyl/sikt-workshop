# Oversikt over kodelister og eksempler på bruk

For flere av feltene i DCAT-AP-NO må du peke til en kode fra et kontrollert vokabular. Når du skal angi tema for et datasett `dcat:theme` må du for eksempel velge en kode fra EU sitt vokabular _Data Theme_. Kodene er organisert i lister eller hierarkiske strukturer, og er navngitt med en URI som andre RDF-ressurser.

Utfyllende oversikt over hvilke kontrollerte vokabularer som skal brukes i henhold til DCAT-AP-NO finner du her: [Kontrollerte-vokabularer-som-skal-brukes](https://data.norge.no/specification/dcat-ap-no#Kontrollerte-vokabularer-som-skal-brukes)

og for kontrollerte vokabularer som bør og kan brukes finner du her: [Kontrollerte-vokabularer-som-bør-og-kan-brukes](https://data.norge.no/specification/dcat-ap-no#Kontrollerte-vokabularer-som-b%C3%B8r-og-kan-brukes)

## Hvordan angi tema

Blant annet Datasett og Datatjeneste bruker egenskapen `dcat:theme`.

For datasett **skal** man angi en kode fra et av EU sine vokubular Data Theme eller EuroVoc.
Man bør også angi kode fra LOS.

### Data Theme

[Informasjon om Data Theme (op.europa.eu)](https://op.europa.eu/en/web/eu-vocabularies/dataset/-/resource?uri=http://publications.europa.eu/resource/dataset/data-theme)

[Gå gjennom innholdet i Data Theme (op.europa.eu)](https://op.europa.eu/en/web/eu-vocabularies/concept-scheme/-/resource?uri=http://publications.europa.eu/resource/authority/data-theme)

URI til bruk i beskrivelser: `http://publications.europa.eu/resource/authority/data-theme/`

Dette er en relativt liten kodeliste som inneholder noen svært generelle temaer, som:

- ECON: Economy and finance
- EDUC: Education, culture and sport
- TECH: Science and technology
- enda flere.

#### Eksempler på bruk

```turtle
@prefix rdf:    <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix dct:    <http://purl.org/dc/terms/> .
@prefix dcat:   <http://www.w3.org/ns/dcat#> .

# Datasett som handler om økonomi og finans
<https://example.org/datasett1> rdf:type dcat:Dataset ;
    dcat:theme <http://publications.europa.eu/resource/authority/data-theme/ECON> .

# Datasett som handler om utdanning
<https://example.org/datasett2> rdf:type dcat:Dataset ;
    dcat:theme <http://publications.europa.eu/resource/authority/data-theme/EDUC> .

# Datasett som handler om utdanning og teknologi
<https://example.org/datasett3> rdf:type dcat:Dataset ;
    dcat:theme  <http://publications.europa.eu/resource/authority/data-theme/EDUC> ,
                <http://publications.europa.eu/resource/authority/data-theme/TECH> .
```

### EuroVoc

EuroVoc er en stor Thesauri med svært mange koder organisert i en hierarkisk struktur.

[Informasjon om EuroVoc (op.europa.eu)](https://op.europa.eu/en/web/eu-vocabularies/dataset/-/resource?uri=http://publications.europa.eu/resource/dataset/eurovoc)

[Gå gjennom innholdet i EuroVoc (op.europa.eu)](https://op.europa.eu/en/web/eu-vocabularies/concept-scheme/-/resource?uri=http://eurovoc.europa.eu/100141)

#### Eksempler på bruk

For informasjon om koden "deep-sea fishing" se [denne siden (op.europa.eu)](https://op.europa.eu/en/web/eu-vocabularies/concept/-/resource?uri=http://eurovoc.europa.eu/2306&lang=en).

```turtle
@prefix rdf:    <http://www.w3.org/1999/02/22-rdf-syntax-ns#> .
@prefix dct:    <http://purl.org/dc/terms/> .
@prefix dcat:   <http://www.w3.org/ns/dcat#> .

# Datasett som handler om dypvannsfiske
<https://example.org/datasett1> rdf:type dcat:Dataset ;
    dcat:theme <http://eurovoc.europa.eu/2306> .
```

### LOS

[Dokumentasjon for LOS](https://data.norge.no/docs/los-dokumentasjon)

#### Eksempel på bruk

## Lisens

## Spatial
