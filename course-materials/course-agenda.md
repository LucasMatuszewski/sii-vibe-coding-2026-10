# Program szkolenia

Vibe coding - programowanie z agentami AI bez utraty kontroli

## Do kogo szkolenie jest skierowane?

Szkolenie jest skierowane do:

- Programistów, którzy chcą pracować z agentami AI (Claude Code, Cursor, Copilot agent mode) na całych zadaniach, nie pojedynczych podpowiedziach.

- Testerów automatyzujących budujących narzędzia i testy z pomocą agentów.

- Tech leadów, którzy chcą ustalić zespołowe zasady pracy AI-first zanim zrobi się chaos.

## Wymagania wstępne

- Praktyczne doświadczenie w programowaniu (dowolny język) i praca z Gitem (branch, commit, PR).

- Podstawowe doświadczenie z dowolnym asystentem AI w kodzie.

- Dostęp do narzędzia agentowego potwierdzamy przed szkoleniem z licencjami organizacji.

## Cel szkolenia

Nauczyć uczestników metodycznego "vibe codingu": pracy, w której agent AI pisze większość kodu, a człowiek zarządza specyfikacją, planem, weryfikacją i ryzykiem. Szkolenie uczy warsztatu, który odróżnia szybkie dowożenie od produkcji długu technicznego: spec przed kodem, małe kroki, testy jako siatka bezpieczeństwa i review każdego outputu.

## Korzyści z udziału w szkoleniu

Po ukończeniu szkolenia uczestnik:

- Zna aktualną taksonomię narzędzi: autocomplete, agent w IDE, agent terminalowy, agent chmurowy - i dobiera je do zadania.

- Prowadzi agenta metodą spec-first: od wymagań i planu do iteracyjnej implementacji.

- Utrzymuje kontrolę: małe kroki, testy przed scaleniem, review kodu AI jak kodu juniora.

- Konfiguruje pliki instrukcji projektu (CLAUDE.md, copilot-instructions.md, AGENTS.md), które trzymają agenta w standardach zespołu.

- Zna ryzyka: bezpieczeństwo, sekrety, prompt injection, licencje - i zespołowe zasady obrony.

## Czego nie ma na tym szkoleniu

- nauki programowania ani konfiguracji GitHub Copilot od podstaw (osobne szkolenie) - zakładamy doświadczenie developerskie.

## Program szkolenia

### DZIEŃ 1

**Warsztat pracy z agentem**

#### 1. Czym jest (a czym nie jest) vibe coding

- Od autocomplete do agentów: mapa narzędzi 2026 (Claude Code, Cursor, Copilot agent mode, Codex).

- Gdzie vibe coding błyszczy, a gdzie kończy się katastrofą - uczciwe granice.

#### 2. Spec przed kodem

- Specyfikacja i plan jako pierwszy artefakt: co agent musi wiedzieć, zanim zacznie.

- Tryby planowania w narzędziach agentowych; zatwierdzanie planu przed implementacją.

- Warsztat: od opisu funkcjonalności do zaakceptowanego planu.

#### 3. Iteracyjna implementacja z agentem

- Małe kroki zamiast "zrób całość": kontrola zakresu i cofanie zmian.

- Prowadzenie agenta przez feedback: jak poprawiać kurs zamiast zaczynać od nowa.

- Warsztat: implementacja funkcjonalności end-to-end na wspólnym repozytorium.

#### 4. Pliki instrukcji projektu

- CLAUDE.md / copilot-instructions.md / AGENTS.md: standardy zespołu, które agent czyta sam.

- Co włożyć w instrukcje: konwencje, architektura, zakazy, definicja ukończenia.

- Warsztat: instrukcje dla repozytorium warsztatowego i test ich działania.

### DZIEŃ 2

**Jakość, bezpieczeństwo i zespół**

#### 5. Testy jako siatka bezpieczeństwa

- Zasada: agent może pisać kod, bo testy pilnują zachowania.

- Agent piszący testy do własnego kodu - kiedy to działa, a kiedy oszukuje.

- Warsztat: rozwój funkcjonalności w pętli test-implementacja.

#### 6. Review kodu AI

- Typowe wady kodu agentowego: pewne siebie błędy, nadmiarowość, fałszywe API, za duże zmiany.

- Checklista review dla kodu AI; rola code review automatycznego.

- Warsztat: przegląd i poprawa PR wygenerowanego przez agenta.

#### 7. Bezpieczeństwo pracy agentowej

- Sekrety i dane wrażliwe: co agent widzi i czego nie może zobaczyć.

- Prompt injection przez pliki, zależności i treści zewnętrzne.

- Licencje i pochodzenie kodu; polityki organizacji.

#### 8. Vibe coding w zespole

- Zasady zespołowe: kiedy wolno delegować agentowi, co wymaga człowieka.

- Koszty pracy agentowej i ich kontrola.

- Jak mierzyć, czy zespół faktycznie przyspieszył (a nie tylko generuje więcej kodu).

#### 9. Projekt końcowy

- Zadanie end-to-end: spec, plan, implementacja z agentem, testy, review, merge.

- Retrospektywa: co z warsztatu wdrażam w swoim projekcie od poniedziałku.

## Metodyka prowadzenia szkolenia

- Minimum 70% czasu to warsztat na wspólnym repozytorium z realnym, niedoskonałym kodem.

- Pracujemy na narzędziach agentowych dostępnych w organizacji uczestników.

- Materiały: szablony plików instrukcji, checklista review kodu AI, zasady zespołowe do adaptacji.

## Kompetencje po szkoleniu

Po szkoleniu uczestnik potrafi:

- Dowozić funkcjonalności z agentem AI metodą spec-first, w małych kontrolowanych krokach.

- Utrzymać jakość kodu AI testami i systematycznym review.

- Skonfigurować projekt tak, by agent pracował w standardach zespołu.

- Rozpoznawać i ograniczać ryzyka bezpieczeństwa pracy agentowej.

## Forma realizacji

- 2 dni - 16 godzin

- Szkolenie online

- Praca warsztatowa + ćwiczenia indywidualne

- Temat przekrojowy: programiści + testerzy automatyzujący
