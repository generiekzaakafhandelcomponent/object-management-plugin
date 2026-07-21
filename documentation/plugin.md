# Object Management-plugin

De Object Management-plugin biedt CRUD-acties op de ZGW Objecten-registratie (Objecten API / Objecttypen API).
Hiermee kan een BPMN-proces objecten aanmaken, bijwerken, verwijderen en ophalen zonder maatwerkcode, door de acties
aan service-taken te koppelen en procesgegevens aan objectvelden te mappen.

## Werking

De plugin communiceert niet rechtstreeks met de Objecten API. In plaats daarvan werkt hij bovenop een
**Object Management-configuratie** — een configuratie (beheerd in de Valtimo-beheeromgeving) die een objecttype, een
Objecten API-plugin en een Objecttypen API-plugin met elkaar verbindt. Elke plugin-actie verwijst naar zo'n
configuratie, via het `id` of via de `title`, en voert zijn bewerking uit tegen het daarin beschreven objecttype.

Waarden die naar een object worden weggeschreven (`objectData`) worden tijdens uitvoering opgelost via de
**value resolver** van Valtimo. Zo kun je objectvelden koppelen aan procesvariabelen, documentwaarden of letterlijke
waarden.

## Configuratie

Deze plugin heeft **geen plugin-configuratie-eigenschappen**. Eén instantie volstaat — al het gedrag wordt bepaald door
de actie-eigenschappen en door de gekoppelde Object Management-configuratie.

```json
{
  "pluginDefinitionKey": "object-management",
  "title": "Object Management",
  "properties": {}
}
```

Zorg dat er, voordat je de acties gebruikt, een Object Management-configuratie bestaat voor het objecttype waarmee je
wilt werken.

## Acties

De plugin biedt de volgende acties:

- **`create-object`** (Create Object) — maakt een nieuw object aan en slaat de URL op in een procesvariabele.
- **`update-object`** (Update Object) — werkt een bestaand object bij, aangeduid met zijn URL.
- **`delete-object`** (Delete Object) — verwijdert een bestaand object, aangeduid met zijn URL.
- **`get-objects-unpaged`** (Get Objects Unpaged) — haalt alle objecten op voor een Object Management-configuratie.
- **`get-object-data-by-url`** (Get object data by url) — haalt de gegevens van één object op via zijn URL.

Alle acties draaien op een **service-taak** bij event `Start` (`SERVICE_TASK_START`).

### `create-object`

Maakt een nieuw object aan op basis van de gekoppelde configuratie en schrijft de resulterende object-URL naar een
procesvariabele.

- **`objectManagementConfigurationId`** (UUID) — de Object Management-configuratie waarin het object wordt aangemaakt.
- **`objectData`** (lijst van sleutel/waarde-bindingen) — de in te stellen objectvelden (zie
  [Objectdata-bindingen](#objectdata-bindingen)).
- **`objectUrlProcessVariableName`** (String) — naam van de procesvariabele die de URL van het nieuwe object ontvangt.

### `update-object`

Werkt een bestaand object bij met nieuwe gegevens.

- **`objectUrl`** (URI) — URL van het bij te werken object.
- **`objectManagementConfigurationId`** (UUID) — de Object Management-configuratie waartoe het object behoort.
- **`objectData`** (lijst van sleutel/waarde-bindingen) — de in te stellen objectvelden (zie
  [Objectdata-bindingen](#objectdata-bindingen)).

### `delete-object`

Verwijdert een bestaand object.

- **`objectUrl`** (String) — URL van het te verwijderen object.
- **`objectManagementConfigurationId`** (UUID) — de Object Management-configuratie waartoe het object behoort.

### `get-objects-unpaged`

Haalt **alle** objecten op voor de opgegeven configuratie en slaat ze (een lijst met de `record.data` van de objecten)
op in een procesvariabele.

- **`objectManagementConfigurationTitle`** (String) — de **titel** van de Object Management-configuratie om uit te lezen.
- **`listOfObjectProcessVariableName`** (String) — naam van de procesvariabele die de lijst met objectdata ontvangt.

> Let op: deze actie is ongepagineerd en geeft elk overeenkomend object terug. Gebruik hem alleen wanneer je een
> beperkt aantal resultaten verwacht.

### `get-object-data-by-url`

Haalt één object op via zijn URL en slaat de `record.data` ervan op in een procesvariabele.

- **`objectManagementConfigurationId`** (UUID) — de Object Management-configuratie waartoe het object behoort.
- **`objectUrl`** (String) — URL van het op te halen object.
- **`objectDataProcessVariableName`** (String) — naam van de procesvariabele die de objectdata ontvangt.

## Objectdata-bindingen

De eigenschap `objectData` van `create-object` en `update-object` is een lijst met sleutel/waarde-paren:

- **sleutel** — een [JSON Pointer](https://datatracker.ietf.org/doc/html/rfc6901) in de data van het object, bijv.
  `/naam` of `/adres/straat`. Deze bepaalt waar de opgeloste waarde in het object wordt weggeschreven.
- **waarde** — een value-resolver-verwijzing die tijdens uitvoering tegen het lopende proces wordt opgelost, bijv. een
  procesvariabele (`pv:eenVariabele`), een documentwaarde (`doc:/een/pad`), of een letterlijke waarde.

Tijdens uitvoering wordt elke `waarde` opgelost via de value resolver:

- Als een verwijzing **niet kan worden opgelost**, faalt de actie met een `IllegalArgumentException` waarin de
  mislukte sleutel/waarde-paren worden vermeld.
- Opgeloste verwijzingen die een **`null`-waarde** opleveren, worden overgeslagen en niet naar het object weggeschreven
  (zie release notes 1.2.1).

## Release notes

Zie [release-notes.md](release-notes.md) voor de versiegeschiedenis.
