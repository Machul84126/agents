Jesteś doświadczonym Java Architect / Senior Backend Engineer. Pracujesz w repozytorium biblioteki Java o nazwie `investments-technical-commons`.

Biblioteka służy do wspólnego przechowywania powtarzalnych elementów technicznych używanych w wielu serwisach Java/Spring Boot, takich jak wspólne encje techniczne, klasy pomocnicze, funkcje, metody, utilsy, query buildery, paginacja, walidatory, wyjątki, typy wspólne, test utilities i inne rozwiązania wielokrotnego użytku.

Twoim zadaniem jest przygotować podstawową dokumentację dla agentów AI i developerów, tak aby:

1. agenci mogli sprawniej rozwijać bibliotekę `investments-technical-commons`,
2. agenci pracujący w innych serwisach wiedzieli, kiedy i jak używać tej biblioteki,
3. agenci nie tworzyli duplikatów funkcji, które już istnieją w `technical-commons`,
4. nowe funkcje dodawane do biblioteki były spójne, dobrze przetestowane i kompatybilne wstecznie.

Wykonaj następujące zadania.

## 1. Najpierw przeanalizuj repozytorium

Zanim utworzysz dokumentację, sprawdź rzeczywisty stan projektu. Nie zgaduj nazw pakietów, klas, modułów ani zależności.

Sprawdź co najmniej:

* strukturę katalogów,
* `pom.xml` lub inne pliki build toola,
* pakiety w `src/main/java`,
* testy w `src/test/java`,
* publiczne klasy, interfejsy, enumy i adnotacje,
* istniejące utilsy, encje, wyjątki, konfiguracje, test utilities,
* zależności zewnętrzne,
* standard testowania,
* używane konwencje nazewnicze,
* czy projekt jest jedno- czy wielomodułowy,
* czy biblioteka publikuje `sources.jar` i `javadoc.jar`.

Możesz użyć komend pomocniczych, np.:

```bash
find . -maxdepth 4 -type f | sort
find src/main/java -type f | sort
find src/test/java -type f | sort
rg -n "public class|public interface|public enum|public record|@Component|@Configuration|@Entity|Exception|Util|Utils|Builder|Mapper|Validator|Pagination|Page|Query|Specification|Criteria" src
rg -n "TODO|deprecated|@Deprecated|FIXME" .
```

Jeżeli projekt używa Maven, sprawdź również:

```bash
mvn test
mvn dependency:tree
```

Jeżeli jakaś komenda nie działa, opisz to krótko w podsumowaniu zmian.

## 2. Utwórz lub zaktualizuj plik `AGENTS.md`

Utwórz plik `AGENTS.md` w głównym katalogu repozytorium.

Plik ma być napisany po angielsku, ponieważ ma być używany przez agentów AI i developerów w różnych kontekstach.

`AGENTS.md` powinien zawierać następujące sekcje:

### `# AGENTS.md`

### `## Repository purpose`

Opisz, że `investments-technical-commons` jest współdzieloną biblioteką Java dla powtarzalnych funkcji technicznych używanych w wielu serwisach.

Opis może zawierać ogólną zasadę:

> This repository is not a business-domain service. It contains reusable technical building blocks that should remain generic, stable, well-tested, and safe to use across multiple services.

### `## What belongs in this library`

Wypisz kategorie kodu, które mogą znajdować się w bibliotece. Uwzględnij kategorie potwierdzone analizą repozytorium oraz ogólne kategorie, które pasują do takiej biblioteki, np.:

* reusable technical entities,
* pagination and sorting helpers,
* query builders,
* technical DTOs,
* reusable validators,
* exception abstractions,
* date/time helpers,
* mapping helpers,
* logging/tracing helpers,
* test utilities,
* integration helpers,
* common annotations,
* generic Spring configuration helpers, jeżeli projekt rzeczywiście takie posiada.

Nie wymyślaj szczegółowych klas. Jeżeli dana kategoria nie występuje w projekcie, oznacz ją jako possible future area albo pomiń.

### `## What does not belong in this library`

Dodaj twarde zasady, że do biblioteki nie powinny trafiać:

* business logic specific to one service,
* domain rules from a single bounded context,
* service-specific statuses, workflows or process logic,
* direct dependencies on one consuming application's internal model,
* temporary workaround code,
* duplicated code copied from a service without generalization,
* heavy dependencies that would be forced on all consuming services without clear justification.

### `## Mandatory workflow before adding new code`

Dodaj obowiązkowy workflow dla agenta pracującego nad biblioteką:

1. Search the existing code first.
2. Search tests and examples.
3. Search documentation.
4. Check whether similar functionality already exists.
5. Prefer extending existing API over creating a parallel abstraction.
6. Do not introduce duplicate utilities.
7. If adding new public API, add tests and documentation.
8. If changing existing public API, preserve backward compatibility or document a migration path.

Dodaj przykładowe komendy:

```bash
rg -n "keyword|synonym|relatedConcept" src/main/java src/test/java docs
rg -n "class .*Name|interface .*Name|enum .*Name|record .*Name" src/main/java
```

### `## Workflow for agents working in consuming services`

Dodaj sekcję dla agentów, którzy implementują inne serwisy i mają używać tej biblioteki.

Opisz zasadę:

> Before implementing reusable technical functionality in a consuming service, check whether `investments-technical-commons` already provides it.

Uwzględnij workflow:

1. Identify whether the needed code is generic technical code or service-specific business logic.
2. Search `investments-technical-commons` source code if available.
3. Search `docs/public-api-index.md`.
4. Search tests for usage examples.
5. Search dependency sources or Javadocs if the source repository is not checked out.
6. Use the existing commons API when available.
7. Do not copy commons code into the service.
8. If the commons API is almost sufficient, propose an extension to commons instead of creating local duplicate code.
9. Only implement locally if the functionality is truly specific to the service bounded context.

Dodaj przykładowe komendy dla zewnętrznego serwisu:

```bash
rg -n "Pagination|Page|Query|Criteria|Validator|Mapper|Exception|DateRange|TimeRange|Audit|Trace|Mdc" ../investments-technical-commons
rg -n "neededConcept|synonym|relatedTerm" ../investments-technical-commons/src/main/java ../investments-technical-commons/src/test/java ../investments-technical-commons/docs
```

Dodaj też wariant dla Maven dependency:

```bash
mvn dependency:tree | grep investments-technical-commons
mvn dependency:sources -DincludeArtifactIds=investments-technical-commons
```

### `## Public API rules`

Dodaj zasady:

* public classes are part of the contract,
* avoid breaking changes,
* prefer additive changes,
* document new public APIs,
* write tests for public behavior,
* do not expose internal implementation details,
* avoid leaking service-specific names into common APIs,
* use clear package names,
* keep APIs small and focused.

### `## Compatibility and versioning`

Dodaj zasady:

* prefer backward-compatible changes,
* use `@Deprecated` before removal,
* document replacement APIs,
* avoid changing method signatures without migration path,
* avoid changing serialized DTO contracts without clear versioning,
* update changelog or release notes if such file exists; if it does not exist, suggest creating one.

### `## Testing rules`

Dodaj zasady:

* every new public utility must have unit tests,
* every bug fix should include regression test,
* tests should document expected usage,
* avoid tests that depend on consuming services,
* keep test data generic,
* if Spring context is not required, prefer plain unit tests,
* run the project’s standard test command before finishing.

Jeżeli na podstawie repozytorium wykryjesz konkretny framework testowy, np. JUnit 5, AssertJ, Mockito, Testcontainers, wpisz to zgodnie z rzeczywistym stanem projektu.

### `## Documentation rules`

Dodaj zasady:

* update `docs/public-api-index.md` when adding new public API,
* add or update usage examples for non-trivial APIs,
* document intended use and non-goals,
* document migration notes for changed APIs,
* do not document classes that do not exist.

### `## Anti-patterns`

Dodaj listę antywzorców:

* creating `CommonUtils` or `TechnicalUtils` as a dumping ground,
* duplicating existing functionality,
* adding business-domain logic,
* adding service-specific dependencies,
* adding public APIs without tests,
* changing public contracts without migration notes,
* introducing global mutable state,
* hiding failures with broad `catch Exception`,
* overusing reflection,
* adding large dependencies for small helpers.

### `## Recommended local commands`

Na podstawie projektu wpisz rzeczywiste komendy build/test/check, np. Maven:

```bash
mvn clean test
mvn clean verify
```

Nie wpisuj komend, jeżeli nie działają lub projekt ich nie używa. Jeżeli nie jesteś pewien, dodaj krótką notatkę, że command must be verified in the project.

## 3. Utwórz katalog `docs`

Utwórz katalog:

```text
docs/
```

i dodaj poniższe pliki.

## 4. Utwórz `docs/public-api-index.md`

Ten plik ma być praktycznym indeksem API dla agentów i developerów.

Nie wymyślaj klas. Wypełnij go na podstawie rzeczywistych klas znalezionych w repozytorium.

Struktura:

```markdown
# Public API Index

## Purpose

This document helps agents and developers quickly discover reusable APIs available in `investments-technical-commons`.

## How to use this index

Before implementing reusable technical code in a service, search this file and the related source/test files.

## API areas

### Area name

Package:
- `actual.package.name`

Public types:
- `ClassName` - short description based on source code.
- `InterfaceName` - short description based on source code.

Typical use cases:
- ...

Related tests:
- `src/test/java/...`

Notes:
- ...
```

Grupuj API według rzeczywistych obszarów, np. pagination, query, exceptions, validation, date/time, logging, testing, persistence, Spring configuration. Nazwy obszarów mają wynikać z repozytorium.

Jeżeli nie jesteś pewien przeznaczenia klasy, napisz ostrożnie:

> Purpose requires confirmation from maintainers.

Nie zgaduj.

## 5. Utwórz `docs/usage-guide.md`

Ten plik ma opisywać, jak korzystać z biblioteki z poziomu innych serwisów.

Uwzględnij:

* kiedy używać `investments-technical-commons`,
* kiedy nie używać,
* jak szukać istniejących funkcji,
* jak czytać `public-api-index.md`,
* jak szukać przykładów w testach,
* jak dodać dependency Maven, jeżeli da się to odczytać z `pom.xml`,
* jak używać `sources.jar` i `javadoc.jar`, jeżeli projekt je publikuje albo powinien publikować,
* jak uniknąć duplikacji kodu,
* jak zgłosić brakującą funkcję do commons.

Dodaj sekcję:

```markdown
## Mandatory search workflow for consuming services
```

W tej sekcji opisz dokładnie, że agent implementujący usługę ma najpierw sprawdzić `technical-commons`.

## 6. Utwórz `docs/contribution-guide.md`

Ten plik ma opisywać zasady dodawania nowego kodu do biblioteki.

Uwzględnij:

* kryteria, kiedy kod powinien trafić do commons,
* kryteria, kiedy powinien zostać lokalnie w serwisie,
* wymagania dla nowego public API,
* wymagania dla testów,
* wymagania dla dokumentacji,
* kompatybilność wsteczną,
* deprecację,
* nazewnictwo,
* pakiety,
* zależności,
* pull request checklist.

Dodaj checklistę:

```markdown
## Pull request checklist

- [ ] I searched the existing source code for similar functionality.
- [ ] I searched tests and documentation for existing usage examples.
- [ ] I confirmed this code is generic technical code, not service-specific business logic.
- [ ] I avoided introducing unnecessary dependencies.
- [ ] I added or updated tests.
- [ ] I updated `docs/public-api-index.md`.
- [ ] I added usage documentation for non-trivial APIs.
- [ ] I preserved backward compatibility or documented migration notes.
- [ ] I ran the standard test command.
```

## 7. Utwórz `docs/consuming-services-agent-workflow.md`

Ten plik ma być krótką instrukcją do kopiowania do `AGENTS.md` innych serwisów.

Ma zawierać gotową sekcję:

```markdown
## Usage of investments-technical-commons

Before implementing reusable technical functionality, check whether it already exists in `investments-technical-commons`.

Do not duplicate commons functionality locally.
If an equivalent or near-equivalent API exists, use it.
If the existing API is insufficient, propose an extension to commons instead of creating a local copy.
Only implement locally when the code is specific to this service’s bounded context.
```

Dodaj też praktyczny workflow i komendy wyszukiwania.

## 8. Zasady jakości

W całej dokumentacji:

* pisz po angielsku,
* nie wymyślaj nieistniejących klas,
* odróżniaj zasady ogólne od informacji wynikających z repozytorium,
* tam gdzie potrzebna jest wiedza z projektu, najpierw ją sprawdź,
* jeżeli czegoś nie da się ustalić, oznacz to jako `Requires project verification`,
* nie twórz zbyt długiego `AGENTS.md`; szczegóły przenieś do `docs`,
* `AGENTS.md` ma być główną instrukcją operacyjną dla agentów,
* `docs/public-api-index.md` ma być praktyczną mapą API,
* `docs/usage-guide.md` ma pomagać serwisom używać biblioteki,
* `docs/contribution-guide.md` ma pomagać rozwijać bibliotekę,
* `docs/consuming-services-agent-workflow.md` ma być gotową instrukcją dla innych repozytoriów.

## 9. Na końcu podaj podsumowanie

Po wykonaniu zmian podaj:

* listę utworzonych/zmienionych plików,
* najważniejsze decyzje,
* rzeczy, które zostały uzupełnione na podstawie analizy repozytorium,
* rzeczy oznaczone jako wymagające potwierdzenia,
* komendy, które uruchomiłeś,
* wynik testów/builda.
