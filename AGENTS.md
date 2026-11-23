# AGENTS.md

Ten plik opisuje obowiązkowy workflow dla wszystkich agentów AI (i ludzi) pracujących w tym repozytorium. Struktura `.ai/` jest częścią procesu pracy, nie opcjonalną dokumentacją — stosuj się do poniższych reguł przy każdym zadaniu.

## Struktura katalogu `.ai/`

```
.ai/
├── specs/                      # specyfikacje do Spec-Driven Development (SDD)
│   ├── YYYY-MM-DD-slug.md      # aktywne specyfikacje
│   ├── implemented/            # specyfikacje już zrealizowane
│   └── archive/                # specyfikacje odrzucone / nieaktualne
├── runs/                        # dzienniki zadań wykonywanych z AI
│   └── YYYY-MM-DD-slug.md      # jeden plik na zadanie, uzupełniany przyrostowo
├── lessons.md                   # indeks / krótkie wnioski i ustalenia
└── lessons/                      # dłuższe, tematyczne notatki (linkowane z lessons.md)
```

## 1. Specyfikacje (`.ai/specs/`)

- Przed rozpoczęciem nietrywialnego zadania (nowa funkcja, większa zmiana architektury) **sprawdź, czy istnieje odpowiadająca specyfikacja** w `.ai/specs/`. Jeśli nie istnieje a zadanie jest wystarczająco duże, zaproponuj jej utworzenie przed implementacją.
- Nazwa pliku specyfikacji: `YYYY-MM-DD-slug.md`, gdzie data to dzień utworzenia specyfikacji.
- Gdy specyfikacja zostanie w pełni zrealizowana, przenieś jej plik do `.ai/specs/implemented/` (zachowując oryginalną nazwę pliku).
- Gdy specyfikacja zostanie odrzucona lub stanie się nieaktualna bez implementacji, przenieś ją do `.ai/specs/archive/`.
- Nie edytuj treści specyfikacji po przeniesieniu do `implemented/` lub `archive/` — to zapis historyczny.

## 2. Dzienniki zadań (`.ai/runs/`)

Każde zadanie wykonywane z udziałem AI musi mieć własny, przyrostowo aktualizowany plik dziennika.

### Tworzenie pliku

- Utwórz `.ai/runs/YYYY-MM-DD-slug.md` **przy pierwszym commicie danego zadania**. Data w nazwie to dzień **rozpoczęcia** zadania, nawet jeśli praca potrwa wiele dni.
- **Nigdy nie zmieniaj nazwy pliku** i **nigdy nie twórz nowego pliku** dla kontynuacji tego samego zadania — kontynuuj w istniejącym.

### Nagłówek pliku (stały, aktualizowany w miarę postępu)

Na początku pliku umieść nagłówek w formacie:

```markdown
# <Tytuł zadania>

Specyfikacja: [.ai/specs/YYYY-MM-DD-slug.md](../specs/YYYY-MM-DD-slug.md) <!-- jeśli istnieje, inaczej usuń linię -->

Cele:
- <cel 1>
- <cel 2>

Status: in-progress
```

- Nagłówek możesz aktualizować w dowolnym momencie, gdy cele się zmieniają.
- `Status:` ma wartość `in-progress` lub `done`. Ustaw `done` **wyłącznie po faktycznym zakończeniu zadania**, nigdy wcześniej.

### Wpisy przy każdym commicie

- Przy **każdym commicie** dotyczącym zadania dopisz na końcu pliku (nigdy nie modyfikuj wcześniejszych wpisów) nowy wpis o długości **2–5 linii**, zawierający:
  1. datę,
  2. co obejmuje ten commit,
  3. decyzje podjęte lub pułapki napotkane podczas pracy,
  4. co zostało jeszcze do zrobienia.
- Wpis przygotuj **przed commitem** i dołącz go **do tego samego commita**, który opisuje. Nie odkładaj zapisu na koniec sesji — każdy commit niosący zmiany kodu w ramach zadania musi nieść też swój wpis w dzienniku.

Przykładowy wpis:

```markdown
## 2026-11-23

Dodano endpoint `/api/users/:id/export`. Zdecydowano o streamowaniu CSV zamiast buforowania w pamięci (duże zbiory danych). Pułapka: domyślny limit `express` na rozmiar odpowiedzi trzeba było podnieść. Do zrobienia: testy integracyjne i obsługa błędów przy przerwanym połączeniu.
```

### Wznawianie zadania

- Przy wznowieniu zadania (także po przejęciu go od innej osoby lub innego agenta) **najpierw przeczytaj cały istniejący plik dziennika** w `.ai/runs/`, aby odtworzyć kontekst, decyzje i stan prac.
- Kontynuuj pracę w tym samym pliku zgodnie z regułami powyżej.

## 3. Lekcje (`.ai/lessons.md` i `.ai/lessons/`)

- `.ai/lessons.md` to krótka, zwięzła lista ustaleń i wniosków — każdy wpis to kilka linii opisujących, co się wydarzyło i jaki jest wniosek na przyszłość. Traktuj go jako indeks / szybki przegląd.
- `.ai/lessons/` to katalog na dłuższe, tematyczne notatki (np. `.ai/lessons/retry-logic.md`, `.ai/lessons/database-migrations.md`), gdy wniosek wymaga szerszego opisu, przykładów kodu czy kontekstu. Z `.ai/lessons.md` dodaj link do odpowiadającego pliku w `.ai/lessons/`.
- **Zapisuj lekcję, gdy:**
  - naprawiono nietrywialny błąd i wniosek może się powtórzyć w przyszłości,
  - odkryto pułapkę, ograniczenie narzędzia/biblioteki lub nieintuicyjne zachowanie systemu,
  - podjęto decyzję architektoniczną, którą warto uzasadnić na przyszłość,
  - użytkownik poprawił podejście agenta i warto to zapamiętać na przyszłe sesje.
- Nie zapisuj w lekcjach rzeczy wynikających prosto z kodu (można je odtworzyć przez przeczytanie kodu) — tylko to, co nieoczywiste i warte zapamiętania.
- Przed rozpoczęciem pracy w obszarze, którego mogą dotyczyć istniejące lekcje, **sprawdź `.ai/lessons.md`**, aby nie powtarzać tych samych błędów.

## Reguły obowiązkowe — podsumowanie

1. Nietrywialne zadanie → sprawdź/utwórz specyfikację w `.ai/specs/`.
2. Każde zadanie z AI → własny, trwały plik w `.ai/runs/YYYY-MM-DD-slug.md`, tworzony przy pierwszym commicie, nigdy nie przenazywany.
3. Każdy commit zadania → wpis w jego pliku runs **w tym samym commicie**.
4. Zakończenie zadania → `Status: done` w nagłówku pliku runs; jeśli było powiązane ze specyfikacją, przenieś ją do `implemented/`.
5. Nietrywialny wniosek/pułapka → zapis w `.ai/lessons.md` (i ewentualnie w `.ai/lessons/`).
6. Wznowienie zadania → najpierw przeczytaj odpowiadający plik w `.ai/runs/`.
