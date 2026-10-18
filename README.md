# Vibe coding - programowanie z agentami AI bez utraty kontroli

## Aktualności po szkoleniu — 18.10.2026

**Projekt demonstracyjny - dokumentacja i plan implementacji:** na gałęzi [`codex/poc-implementation-plan`](https://github.com/LucasMatuszewski/sii-vibe-coding-2026-10/tree/codex/poc-implementation-plan) możecie przejrzeć materiały wygenerowane przez agenta dla projektu prowadzącego: [PRD](https://github.com/LucasMatuszewski/sii-vibe-coding-2026-10/blob/codex/poc-implementation-plan/docs/PRD.md), [decyzje architektoniczne ADR](https://github.com/LucasMatuszewski/sii-vibe-coding-2026-10/tree/codex/poc-implementation-plan/docs/ADR) i [plan implementacji z podziałem prac](https://github.com/LucasMatuszewski/sii-vibe-coding-2026-10/tree/codex/poc-implementation-plan/docs/plans). To zapis przygotowania projektu podczas zajęć; katalog `app/` na tej gałęzi pozostaje szkieletem do dalszej implementacji.

Dostępne są już zapisy czatu Zoom z obu dni: [dzień 1 — 17.10](course-materials/zoom-summary-chat-history/day-1-chat-history.md) i [dzień 2 — 18.10](course-materials/zoom-summary-chat-history/day-2-chat-history.md). Zawierają linki, prompty i pytania z zajęć; zapis dnia 2 uzupełniłem o dodatkowe objaśnienia.

Materiały i uzupełnienia do dalszej pracy:

- **Wizualizacje i raporty HTML w Codexie:** [Web Artifacts Builder od Anthropic](https://www.skills.sh/anthropics/skills/web-artifacts-builder). Skill można wykorzystać także w Codexie do tworzenia interaktywnych raportów, dashboardów i wizualizacji danych. Pracuje w React, ale gotowy projekt pakuje do jednego pliku `bundle.html` z osadzonym JavaScriptem i CSS. W prompcie warto wprost poprosić o taki samodzielny plik HTML do otwarcia w przeglądarce.
- **Monorepo dla aplikacji w różnych technologiach:** [Nx](https://nx.dev/docs/features/multi-language-support) pozwala zarządzać wspólnym repozytorium frontendu i backendów, także w Javie i .NET. Ma oficjalne integracje z Gradle, Maven i .NET oraz wspólny graf zależności, uruchamianie zadań i cache wyników. Można go wykorzystać do uporządkowania budowania i testowania kilku aplikacji w jednym repozytorium.
- **CI/CD i automatyczne code review:** tego tematu nie zdążyliśmy omówić na zajęciach. W folderze [przykładów CI/CD](course-materials/cicd-headless/) znajdziecie konfiguracje pipeline’ów i agentów przeglądających pull requesty. Warto zajrzeć też do [PR-Agent](https://github.com/The-PR-Agent/pr-agent), narzędzia open source do wspomagania review, oraz [ćwiczenia z jego konfiguracją](course-materials/cicd-headless/PR-Agent-Qodo/README.md).
- **Statusline Claude Code z cache hit rate:** zaktualizowałem [skrypt `statusline.sh`](course-materials/agent-configs/claude/statusline.sh), aby pokazywał również `Cache: 88%` - procent tokenów wejściowych odczytanych z cache w głównej rozmowie. Skopiuj go do `~/.claude/statusline.sh`, jeśli korzystasz z tej konfiguracji. Wskaźnik wymaga `jq`, Claude Code 2.1.251 lub nowszego i przynajmniej jednej odpowiedzi API; przy braku danych pozostaje ukryty. [Opis konfiguracji i test](course-materials/agent-configs/README.md#claude-code-statusline-and-cache-hit-rate).
- **Zadania zaplanowane w chmurze:** Claude Code nadal obsługuje `/schedule` i alias `/routines`; chmurowe zadania działają przy zamkniętym laptopie. W Codexie harmonogramami zarządza się przez aplikację lub webowy panel `Scheduled`, a lokalny harmonogram wymagający plików z komputera potrzebuje działającej aplikacji. [Zweryfikowane różnice, wymagania kont i przykłady](course-materials/Research/codex-claude-cloud-scheduling-2026-10-18.md).
- **Ceny GPT-6 Sol i GPT-6.1 Sol:** w standardowym API OpenAI kosztują tyle samo za zwykły input/output: 2/10 USD za milion tokenów. Odczyt cache w Sol 6.1 jest o połowę tańszy: 0,10 zamiast 0,20 USD, czyli 5% zamiast 10% ceny inputu. Nie oznacza to dwukrotnie niższego kosztu całego zadania. [Zweryfikowane ceny i porównanie z Arena](course-materials/Research/model-pricing-sol-2026-10-18.md).

**Materiały szkoleniowe: [devpowers.com/szkolenia/futureskills/sii-vibe-coding-2026-10](https://devpowers.com/szkolenia/futureskills/sii-vibe-coding-2026-10/)**

- [Agenda i wszystkie materiały](https://devpowers.com/szkolenia/futureskills/sii-vibe-coding-2026-10/agenda.html)
- [Slajdy](https://devpowers.com/szkolenia/futureskills/sii-vibe-coding-2026-10/agenda.html#prezentacje)
- [Biblioteka promptów](https://devpowers.com/szkolenia/futureskills/sii-vibe-coding-2026-10/prompty.html) · [Ćwiczenia](https://devpowers.com/szkolenia/futureskills/sii-vibe-coding-2026-10/cwiczenia.html) · [Checklisty](https://devpowers.com/szkolenia/futureskills/sii-vibe-coding-2026-10/checklisty.html) · [Słownik AI](https://devpowers.com/szkolenia/futureskills/sii-vibe-coding-2026-10/slownik-ai.html)
- [Wyniki ankiety przed szkoleniem](https://devpowers.com/szkolenia/futureskills/sii-vibe-coding-2026-10/raport-przed.html) · [Odpowiedzi na pytania z kursu](https://devpowers.com/szkolenia/futureskills/sii-vibe-coding-2026-10/pytania.html) (uzupełniamy po każdym dniu)

Repozytorium uczestnika szkolenia Future Skills Institute dla SII (17-18.10.2026, online, 09:00-17:00). Przez dwa dni uczymy się metodycznego vibe codingu: agent AI pisze większość kodu, a człowiek odpowiada za specyfikację, plan, weryfikację i ryzyko. Pracujemy w małych krokach, z testami jako siatką bezpieczeństwa i review każdej zmiany. [Program szkolenia](AGENDA.md).

Główne narzędzie agentowe wybieramy wspólnie na początku zajęć (Claude Code, Codex, GitHub Copilot w trybie agentowym lub Cursor). Repozytorium ma gotowe konfiguracje dla każdego z nich: `AGENTS.md`, `CLAUDE.md`, `.claude/`, `.codex/`, `.github/` i `.mcp.json`. Możesz pracować w swoim stosie (np. Java, C# / .NET, Python); przykład prowadzącego powstaje w TypeScript z Next.js i Vercel AI SDK. Jeśli nie programujesz na co dzień, pracujesz na tym samym projekcie: czytasz plan i zmiany razem z agentem i prosisz go o wyjaśnienie każdego kroku.

Głównym projektem grupy jest czat AI do obsługi zwrotów i reklamacji elektroniki: formularz, zdjęcie produktu, rekomendacja modelu i rozmowa uzupełniająca. Rozwijamy go według PRD uzgodnionego podczas zajęć. Możesz też pracować nad własnym pomysłem, stosując ten sam proces. Po PRD omów architekturę i biblioteki, zapisz decyzje w ADR, ułóż plan i zależności zadań, a potem implementuj, testuj i przeglądaj zmiany przed utworzeniem PR. Przykładowy prompt PRD w `course-materials/` nie zastępuje dokumentu poprawionego z grupą.

### Jak zacząć

1. Zrób fork tego repozytorium na swoim koncie GitHub (przycisk **Fork**) i sklonuj swój fork: `git clone https://github.com/<twoj-login>/sii-vibe-coding-2026-10.git`.
2. Utwórz gałąź do pracy: `git switch -c feature/<krotka-nazwa>`.
3. Uruchom wybranego agenta w katalogu repozytorium i poproś go o krótkie podsumowanie `AGENTS.md`.

Pull requesty otwierasz w swoim forku. Dzięki temu nie potrzebujesz zaproszenia do repozytorium prowadzącego.

## Biblioteka dodatkowa

`course-materials/` zawiera szeroką bibliotekę pełnych promptów, danych syntetycznych, checklist, przykładów konfiguracji i materiałów o innych agentach. To zasób do późniejszego czytania, nie lista narzędzi wymaganych na warsztacie. [Pełne prompty historyczne](course-materials/Prompt%20examples/) zachowano bez skracania; starsze twierdzenia o produktach, licencjach i komendach wymagają ponownego sprawdzenia. Podczas pracy z wybraną aplikacją agent nie powinien sam przeszukiwać `course-materials/`: zawarte tam PRD, ADR i instrukcje są przykładami, a nie wymaganiami tego projektu. Otwórz konkretny materiał dopiero wtedy, gdy go o to poprosisz.

W bibliotece znajdują się również **archiwalne materiały z wcześniejszego kursu o Claude Code**: [agenda](course-materials/course-agenda.md), [slajdy](course-materials/slides/claude-code-2026-07/), [notatki](course-materials/Course%20Notes%20-%20AI%20in%20Programming.md), [quiz](course-materials/quizzes/day-1-anonymous-ai-basics-quiz.md), [przykłady konfiguracji](course-materials/agent-configs/claude/) i [słownik PDF](course-materials/AI%20dla%20Programist%C3%B3w%20-%20S%C5%82ownik%20przed%20szkoleniem.pdf). Zachowano je jako źródła do ponownego wykorzystania, **nie jako aktualną agendę ani konfigurację tego warsztatu**. Przykłady ustawień i skrypty należy dostosować i sprawdzić przed użyciem. `course-materials/exercise-data/hidden-patterns.md` jest archiwalnym kluczem trenerskim do danych syntetycznych.

Warto zacząć od: [promptu PRD](course-materials/Prompt%20examples/PRD-electronics-returns-complains-app.md) i [promptu ADR](course-materials/Prompt%20examples/ADR-generation-typescript-vercel-ai-sdk.md) z linkami do bibliotek (Chat SDK, OpenRouter Responses API, Vercel AI SDK), [jednego skryptu blokującego odczyt `.env`, kluczy SSH i `secrets/`](course-materials/hooks-example/) w Copilocie, Claude Code i Codex, oraz [agenta code review w CI/CD](course-materials/cicd-headless/agent-review/) - Azure Pipelines, GitLab CI, Bitbucket Pipelines, Jenkins i GitHub Actions, z wynikiem w komentarzu PR i zadaniu w Jira.

Skróty Codexa dla Bash i PowerShell znajdziesz w [przykładach profili](course-materials/agent-configs/README.md#codex-profiles). [Przykłady hooków](course-materials/hooks-example/) obejmują blokadę plików z sekretami oraz opcjonalne kontrole poleceń, treści zapisu, Javy i kontekstu sesji. Wybierz potrzebne wpisy i scal je z własną konfiguracją zgodnie z instrukcją.

## Granice

Używaj wyłącznie danych syntetycznych. Przed przekazaniem kontekstu agentowi sprawdź politykę organizacji i zakres dostępu. Przejrzyj każdy diff, wynik testów i uprawnienia proponowanych narzędzi. Testów nie zmieniaj po to, by ukryć nieukończone zachowanie.

Możliwość użycia agenta chmurowego, MCP, CLI i pul AI Credits zależy od planu oraz ustawień organizacji. Agent w IDE i agent chmurowy to osobne środowiska. Funkcje edytora mogą się różnić od CLI i od innych IDE. [Stan narzędzi i źródła](docs/tool-facts.md) wymagają ponownego sprawdzenia przed zajęciami.

Trener: Łukasz Matuszewski · [LinkedIn](https://www.linkedin.com/in/lukaszmatuszewski/) · [X](https://x.com/mrLumatic)
