# Release notes

Overzicht van wijzigingen per versie van de Object Management-plugin.

## 1.2.3
De `update-object`-actie haalt nu eerst de bestaande objectdata op en past de geconfigureerde velden daarop toe, in plaats van een object te versturen dat alleen de geconfigureerde velden bevat. Hierdoor wordt niet-geconfigureerde data niet langer overschreven en geweigerd door de schemavalidatie van de Objects API.

## 1.2.2

Valtimo bijgewerkt naar versie 13.41.0.

## 1.2.1
Lege (null) waarden worden niet langer weggeschreven naar het object bij de `create-object`- en `update-object`-acties. Alleen waarden die daadwerkelijk zijn opgelost, worden opgenomen in de objectdata.

## 1.0.0
Geschikt gemaakt voor Valtimo 13.24.0 en ondergebracht in een eigen repository, met voorbeeldapplicatie en aparte documentatie.

## 0.1.0
Eerste publieke release: objecten aanmaken, ophalen, bijwerken en verwijderen via de Objects API.
