---
project: "Sasiad-Zgloszenia"
version: 1
status: draft
created: 2026-11-25
context_type: greenfield
product_type: web-app
target_scale:
  users: medium
  qps: low
  data_volume: small
timeline_budget:
  mvp_weeks: 3
  hard_deadline: 2027-01-10
  after_hours_only: true
---

## Vision & Problem Statement

Zgłoszenia usterek w wspólnocie spływają dziś wyłącznie telefonicznie (rzadziej e-mailem); zarządca nie korzysta z komunikatorów. Telefon nie zostawia trwałego śladu — zgłoszenia gubią się, brak historii i potwierdzenia. Ta sama usterka jest zgłaszana wielokrotnie przez różnych mieszkańców (np. awaria dotycząca tego samego elementu wielu razy). Nie ma obiektywnego kryterium kolejności napraw — priorytet wyznacza natarczywość dzwoniącego („kto głośniej dzwoni"), nie waga usterek. Osoba zgłaszająca nie wie nic: czy ktoś przeczytał, zareagował, ani jakie jest ETA usunięcia awarii.

Insight vs status quo: nierozstrzygnięty — dlaczego gotowe oprogramowanie dla wspólnot nie rozwiązuje tego problemu (patrz Open Questions).

## User & Persona

**Primary: Zarządca (Manager)** — obsługuje zgłoszenia z jednego budynku; dziś odbiera telefony, gubi wątki i rozkłada zasoby serwisowe bez obiektywnego priorytetu. Sięga po produkt w momencie, gdy musi zdecydować, co naprawiać najpierw, i gdy ci sami mieszkańcy dopytują o status.

**Secondary persona: Mieszkaniec (Resident)** — zgłasza usterkę i chce mieć potwierdzenie, status i ETA; dziś dzwoni „w ciemno".

Trzecia osoba zainteresowana: **Zarząd wspólnoty** (eskalacje, presja przy nierozwiązanych awariach) — poza MVP.

## Success Criteria

### Primary
- Mieszkaniec zgłasza usterkę i w ciągu jednej sesji widzi: potwierdzenie, priorytet P1–P4, termin SLA/ETA oraz alert o potencjalnym duplikacie; zarządca widzi zgłoszenie na liście sortowanej po pilności SLA i może zmienić jego status. Przepływ 1–8 działa end-to-end.

### Secondary
- Mieszkańcy wspólnoty realnie adoptują narzędzie — liczba zgłoszeń telefonicznych spada na rzecz zgłoszeń w aplikacji.

### Guardrails
- Zgłoszenia nie znikają ani nie zmieniają stanu bez śladu — każda zmiana statusu jest widoczna (nieusuwalność danych).
- Treść zgłoszeń i konta nie są dostępne dla osób nieuprawnionych, nawet przy MVP dla jednej wspólnoty (prywatność danych).
- Klasyfikacja P1–P4 bywa błędna, ale termin SLA i potwierdzenie zgłoszenia muszą być zawsze widoczne i przewidywalne (przewidywalność SLA).

## User Stories

# TODO: User Stories — see Open Questions (brak historyjek Given/When/Then w notatkach wejściowych; wymagana co najmniej US-01 pokrywająca główny przepływ „zgłoś → klasyfikacja → termin docelowy → status")

## Functional Requirements

Format FR (ustalony przez użytkownika): „[Aktor] może [zdolność]".

### Konta i dostęp
- FR-001: Osoba z uprawnieniem może zakładać konta mieszkańców i zarządcy. Priority: must-have
  > Socrates: Kontrargument: „ręczne konta to tarcie zabijające adopcję". Rozwiązanie: konta zakładane raz przy wdrożeniu; kanał telefoniczny obsługuje FR-013 — brak konta nie odcina mieszkańca od systemu.
- FR-002: Mieszkaniec może logować się do aplikacji. Priority: must-have
  > Socrates: Kontrargument: „logowanie zniechęci osoby starsze i zostaną przy telefonach". Rozwiązanie: logowanie zostaje (wymóg systemu i prywatności); kanał telefoniczny obsługuje pracownik administracji wprowadzający zgłoszenie w imieniu mieszkańca (FR-013).

### Zgłaszanie
- FR-003: Mieszkaniec może utworzyć zgłoszenie usterki dla swojej wspólnoty. Priority: must-have
  > Socrates: Kontrargument: „zbyt ubogie zgłoszenia, klasyfikator nie ma czego oceniać". Rozwiązanie: kontrargument rozważony, FR zostaje — doprecyzowanie należy do obsługi (FR-013/014); minimalne pola formularza do zdefiniowania przy projektowaniu.
- FR-004: System może nadać zgłoszeniu priorytet P1–P4. Priority: must-have
  > Socrates: Kontrargument: „błędna klasyfikacja (zalana piwnica jako P4) podważy zaufanie szybciej niż brak klasyfikacji". Rozwiązanie: automat filtruje szum, zarządca zawsze może skorygować priorytet (FR-014) — błąd jest ludzko naprawialny i widoczny.
- FR-005: System może wyznaczyć zgłoszeniu termin SLA. Priority: must-have
  > Socrates: Kontrargument: „SLA bez mocy przerobowej serwisu staje się podstawą roszczeń ('obiecaliście 48h!')". Rozwiązanie: termin SLA prezentowany i nazywany w produkcie jako **termin docelowy** (orientacyjny), nie kontrakt.
- FR-006: Mieszkaniec może zobaczyć alert o potencjalnym duplikacie podczas zgłaszania. Priority: nice-to-have

### Przeglądanie
- FR-007: Mieszkaniec może przeglądać wszystkie zgłoszenia swojej wspólnoty z ich statusami. Priority: must-have
- FR-008: Mieszkaniec może edytować własne zgłoszenie. Priority: must-have

### Obsługa (zarządca)
- FR-009: Zarządca może przeglądać zgłoszenia sortowane wg pilności SLA. Priority: must-have
  > Socrates: Kontrargument rozważony („to tylko sortowanie"): zostaje — sortowanie jest nośnikiem reguły domenowej (termin docelowy) w codziennej pracy zarządcy.
- FR-010: Zarządca może zmieniać statusy zgłoszeń. Priority: must-have
  > Socrates: Kontrargument rozważony, FR zostaje (użytkownik: „reszta zostaje", 2026-11-25).
- FR-011: Zarządca może dodawać notatki serwisowe. Priority: nice-to-have
  > Socrates: Kontrargument rozważony, zostaje jako nice-to-have; poza MVP — do ## Non-Goals.
- FR-012: Zarządca może potwierdzać lub odpinać duplikaty. Priority: nice-to-have
  > Socrates: Kontrargument rozważony, zostaje jako nice-to-have wraz z FR-006; poza MVP — do ## Non-Goals.

### Kanał wspomagany
- FR-013: Pracownik administracji może wprowadzić zgłoszenie usterki w imieniu mieszkańca. Priority: must-have
  > Socrates: Kontrargument rozważony, FR zostaje (użytkownik: „reszta zostaje", 2026-11-25) — przejmuje kanał telefoniczny, domykając obawę o adopcję (FR-001/002).
- FR-014: Zarządca może skorygować nadany priorytet P1–P4. Priority: must-have

### Prywatność zgłaszania
- FR-015: Zgłoszenie jest prezentowane pozostałym mieszkańcom **bez tożsamości zgłaszającego** (wspólna lista pokazuje treść i status, nie „kto zgłosił"); pracownik administracji i zarządca widzą zgłaszającego i mogą się z nim skontaktować. Priority: must-have
  > Socrates: Kontrargument: pełna jawność listy (FR-007) może krępować mieszkańców przed zgłaszaniem („wszyscy zobaczą, że to ja zgłosiłem dziurę w chodniku"). Rozwiązanie użytkownika: tożsamość widoczna tylko dla administracji/zarządcy, motywacja RODO. Mieszkaniec zalogowany nadal widzi i edytuje **własne** zgłoszenia (FR-008).
- FR-016: System może na żądanie zanonimizować dane zgłaszającego wybrane(ych) zgłoszeń(zenia) — „funkcja anonimizująca dane w razie konieczności". Priority: must-have
- FR-017: Dowolny mieszkaniec budynku (nie tylko zgłaszający) może **podbic** zgłoszenie (eskalacja do zarządcy) — jedna osoba może podbić dane zgłoszenie tylko raz; licznik podbić jest widoczny **tylko dla zarządcy**. Priority: must-have
- FR-018: Uprawniona osoba może edytować katalog reguł kategoryzacji (słowniki, mapowanie P1–P4, czasy reakcji) w aplikacji — panel konfiguracji. Priority: nice-to-have — poza MVP; reguły i czasy reakcji przygotowywane wstępnie (seed) przy wdrożeniu

## Non-Functional Requirements

- Użytkownik widzi potwierdzenie każdej akcji natychmiast („strona reaguje od razu" — wypowiedź użytkownika); operacja dłuższa niż ~2 s pokazuje widoczny postęp.
- Produkt dostępny jako aplikacja webowa (decyzja użytkownika 2026-11-25); dopuszczalna przyszła zmiana w stronę desktopu — nie wymaga przepisania rdzenia.
- Interfejs pozostaje użyteczny na telefonie — mieszkańcy wchodzą głównie z komórki.
- Dostęp do treści zgłoszeń i danych mieszkańców mają wyłącznie uprawnione role: mieszkaniec (treści zgłoszeń bez tożsamości zgłaszających), administracja/zarządca (pełnia). Nikt nieuprawniony nie widzi danych.
- Anonimizacja **na żądanie** (FR-016), nie automatyczna — MVP nie wdraża harmonogramu retencji; zasada retencji pozostaje regułą operacyjną: „dane trzymamy tylko tak długo, jak wymaga obsługa zgłoszenia i rozliczenia wspólnoty".

## Business Logic

**Reguła (jedno zdanie):** Jeżeli treść zgłoszenia zawiera atrybuty zdefiniowane w konfigurowalnym katalogu (np. awaria rury, zalewanie, zapach palonej gumy, dym), system przypisuje zgłoszeniu kategorię priorytetu (np. P1) oraz termin reakcji z konfigurowalnej tabeli czasów (np. 4 h); kategorie P1–P4, zasady kategoryzacji i czasy reakcji na poszczególne incydenty są konfigurowalne, nie zaszyte na sztywno.

- **Wejście reguły:** treść zgłoszenia (opis) sprawdzana względem słownika słów kluczowych / atrybutów; słownik, mapowanie na kategorie P1–P4 i czasy reakcji — dane konfigurowalne.
- **Wyjście reguły:** kategoria priorytetu P1–P4 + termin reakcji (docelowy) z konfiguracji; przykład z wypowiedzi użytkownika: awaria rury / zalewanie / smród palonej gumy / dym → P1, termin reakcji 4 h.
- **Jak użytkownik spotyka regułę:** mieszkańcy **widzą priorytet** zgłoszenia; dowolny mieszkaniec może podbić zgłoszenie (FR-017), zarządca skorygować priorytet (FR-014); licznik podbić widoczny tylko dla zarządcy.

*Uwaga generatora: oryginalne sformułowanie reguły (z wskazaniem miejsca przechowywania słownika) zachowane w `context/foundation/shape-notes.md` ## Business Logic; tutaj przepisane jako reguła domenowa niezależna od implementacji, zgodnie ze schematem PRD.*

## Access Control

- Konta nadaje **osoba z uprawnieniem do tworzenia użytkowników** — przypisana rola, niekoniecznie administrator. Brak samodzielnej rejestracji.
- **Mieszkaniec:** widzi **wszystkie zgłoszenia swojej wspólnoty i ich statusy** (jawność w obrębie wspólnoty, bez tożsamości zgłaszających — FR-015); może **edytować własne zgłoszenie**.
- **Pracownik administracji:** wprowadza zgłoszenia w imieniu mieszkańca (FR-013); widzi zgłaszającego i może się z nim skontaktować.
- **Zarządca:** pełna obsługa zgłoszeń wspólnoty — statusy, korekta priorytetu, korekta klasyfikacji.
- **Każde żądanie dostępu do danych wymaga uwierzytelnionej tożsamości i autoryzacji na poziomie roli.** Każdy punkt udostępniający dane jest chroniony — brak dostępnych „furtką" danych publicznych poza tym, co jawne z założenia (treści zgłoszeń bez tożsamości widoczne dla zalogowanych mieszkańców wspólnoty).

Uwaga zakresowa: MVP obejmuje **jedną wspólnotę**; model danych ma nie blokować przyszłej obsługi wielu wspólnot i grup pracowników (patrz hand-off / shape-notes ## Forward).

## Non-Goals

MVP **nie** robi (potwierdzone przez użytkownika 2026-11-25):
- **Brak powiadomień wychodzących (e-mail/SMS/push)** — o stanie zgłoszenia wszyscy dowiadują się z list w aplikacji; P1 nie alarmuje zarządcy automatycznie.
- **Brak komentarzy/czatów pod zgłoszeniami** — komunikacja przy zgłoszeniu odbywa się poza aplikacją (telefon zarządcy / FR-013).
- **Brak modułu raportów i analityki dla zarządu wspólnoty.**
- **Brak detekcji i obsługi duplikatów** (FR-006, FR-012 = nice-to-have) — przy skali jednej wspólnoty wartość duplikatów jest niska; licznik podbić (FR-017) częściowo zastępuje sygnał „to już zgłaszane".
- **Brak panelu zarządzania regułami w aplikacji** (FR-018 = nice-to-have) — reguły, słowniki i czasy reakcji jako seed przy wdrożeniu, zmiana ręcznie „w razie konieczności".
- **Brak obsługi wielu wspólnot** (MVP = jedna wspólnota) — model danych ma nie blokować tego w przyszłości.
- **Brak automatycznego harmonogramu retencji/anonimizacji** — anonimizacja tylko na żądanie (FR-016).

## Open Questions

1. **Insight vs status quo** — dlaczego gotowe oprogramowanie dla wspólnot nie rozwiązuje tego problemu? — Owner: użytkownik. Block: nie (warto wiedzieć przed decyzją build-vs-buy).
2. **Brak historyjek użytkownika Given/When/Then** — wymagana co najmniej US-01 pokrywająca główny przepływ (mieszkaniec zgłasza → system nadaje P i termin docelowy → zgłoszenie widoczne na liście zarządcy → zmiana statusu). — Owner: użytkownik (do wypracowania przed implementacją; sekcja ## User Stories pozostaje TODO). Block: tak (PRD nie powinien trafić do sign-off bez US-01).
3. **Minimalne pola formularza zgłoszenia (FR-003)** — co dokładnie wypełnia mieszkaniec (opis, lokalizacja, kategoria?), skoro od treści zależy klasyfikacja? — Owner: użytkownik. Block: nie, ale wpływa na kształt reguły domenowej w praktyce.
4. **Zakres anonimizacji (FR-016)** — kto wywołuje anonimizację i co dokładnie zostaje po niej (samo zgłoszenie bez danych osobowych? historia statusów?) — Owner: użytkownik. Block: nie.
