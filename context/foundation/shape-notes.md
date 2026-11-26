---
project: "Sasiad-Zgloszenia"
context_type: greenfield
created: 2026-11-25
updated: 2026-11-26
checkpoint:
  current_phase: 8
  phases_completed: [1, 2, 3, 4, 5, 6, 7]
  gray_areas_resolved:
    - topic: "kategoria bólu"
      decision: "utrata danych (telefon nie zostawia śladu) + tarcie workflow + paraliż decyzyjny (brak obiektywnego priorytetu)"
    - topic: "insight vs status quo"
      decision: "nierozstrzygnięte — przeniesione do Open Questions"
    - topic: "zakres persony"
      decision: "konkretne role w jednej wspólnocie: zarządca (główna), mieszkańcy, zarząd"
    - topic: "model dostępu"
      decision: "konta zakłada uprawniona osoba (rola do tworzenia użytkowników, niekoniecznie administrator); role: mieszkaniec / pracownik (grupy scoped per wspólnota) / zarządca"
    - topic: "zakres MVP vs wielowspólnotowość"
      decision: "MVP = jedna wspólnota; design nieblokujący przyszłych wspólnot (## Forward: technical-roadmap)"
    - topic: "SLA jako konstrukt"
      decision: "termin docelowy (orientacyjny), nie kontrakt; automatyczny priorytet + korekta zarządcy (FR-014)"
    - topic: "prywatność zgłaszającego"
      decision: "wspólna lista bez tożsamości zgłaszającego; administracja/zarządca znają osobę; retencja wg potrzeby obsługi/rozliczeń + funkcja anonimizacji (FR-016)"
    - topic: "eskalacja priorytetu"
      decision: "dowolny mieszkaniec podbija zgłoszenie raz na osobę; licznik podbić tylko dla zarządcy (FR-017)"
    - topic: "konfiguracja reguł"
      decision: "reguły i czasy reakcji jako seed przy wdrożeniu; panel konfiguracji poza MVP (FR-018 nice-to-have)"
    - topic: "anonimizacja automatyczna vs ręczna"
      decision: "na żądanie (FR-016); brak harmonogramu retencji w MVP"
    - topic: "product_type"
      decision: "aplikacja webowa (MVP); przyszły desktop jako możliwy zwrot, decyzja odwracalna"
    - topic: "[przegląd 2026-11-26] alert o duplikacie w kryterium sukcesu"
      decision: "usunięty z Primary — duplikaty pozostają poza MVP"
    - topic: "[przegląd 2026-11-26] przepływ MVP 1–8"
      decision: "odtworzony z FR i potwierdzony przez użytkownika"
    - topic: "[przegląd 2026-11-26] zgłoszenie bez trafień w katalogu"
      decision: "status „Do oceny” z własnym, konfigurowalnym terminem oceny (seed jak czasy P1–P4)"
    - topic: "[przegląd 2026-11-26] zegar terminu reakcji"
      decision: "liczony całą dobę, 7 dni w tygodniu"
    - topic: "[przegląd 2026-11-26] wpływ podbić"
      decision: "tylko sygnał dla zarządcy — nie zmieniają priorytetu, terminu ani kolejności"
    - topic: "[przegląd 2026-11-26] edycja własnego zgłoszenia"
      decision: "reklasyfikacja po edycji, chyba że zarządca ręcznie skorygował priorytet (korekta ma pierwszeństwo)"
    - topic: "[przegląd 2026-11-26] role MVP"
      decision: "wybrane 4 role; po redukcji zakresu MVP ma 3: mieszkaniec, pracownik administracji, zarządca (serwis po MVP)"
    - topic: "[przegląd 2026-11-26] redukcja zakresu do 3 tygodni"
      decision: "poza MVP: FR-016 anonimizacja, FR-019/020 rola serwisu, FR-017 podbicia, FR-008 edycja z reklasyfikacją"
    - topic: "[przegląd 2026-11-26] zakładanie kont"
      decision: "zarządca oraz pracownik administracji"
    - topic: "[przegląd 2026-11-26] uprawnienia pracownika serwisu"
      decision: "widzi zgłoszenia swojej wspólnoty i dane kontaktowe zgłaszającego; po rundzie Socrates może ustawiać „W realizacji”/„Zamknięte” (FR-020); nie zmienia priorytetu"
    - topic: "[przegląd 2026-11-26] statusy zgłoszenia"
      decision: "Nowe → Przyjęte → W realizacji → Zamknięte, plus Odrzucone"
    - topic: "[przegląd 2026-11-26] pola formularza zgłoszenia"
      decision: "opis (jawny), miejsce usterki (tylko obsługa), kategoria z listy"
    - topic: "[przegląd 2026-11-26] kategoria vs słowa kluczowe"
      decision: "wyższy z dwóch priorytetów wygrywa"
    - topic: "[przegląd 2026-11-26] NFR"
      decision: "próg potwierdzenia akcji opisowo (bez liczby); na telefonie w pełni tylko ścieżka mieszkańca"
    - topic: "[przegląd 2026-11-26] insight vs gotowe systemy"
      decision: "niesprawdzone — pozostaje w Open Questions"
  frs_drafted: 20
  quality_check_status: accepted
---

# Shape Notes

## Seed idea

System triage'u i priorytetyzacji zgłoszeń wspólnoty mieszkaniowej (MVP): uporządkowanie obsługi usterek we wspólnocie mieszkaniowej poprzez klasyfikację priorytetów (P1–P4), wyznaczanie terminów SLA oraz wykrywanie duplikatów zgłoszeń w obrębie jednego budynku.

## Vision & Problem Statement

Zgłoszenia usterek w wspólnocie spływają dziś wyłącznie telefonicznie (rzadziej e-mailem); zarządca nie korzysta z komunikatorów. Telefon nie zostawia trwałego śladu — zgłoszenia gubią się, brak historii i potwierdzenia. Ta sama usterka jest zgłaszana wielokrotnie przez różnych mieszkańców (np. awaria dotycząca tego samego elementu wielu razy). Nie ma obiektywnego kryterium kolejności napraw — priorytet wyznacza natarczywość dzwoniącego („kto głośniej dzwoni"), nie waga usterek. Osoba zgłaszająca nie wie nic: czy ktoś przeczytał, zareagował, ani jakie jest ETA usunięcia awarii.

Insight vs status quo: nierozstrzygnięty — dlaczego gotowe oprogramowanie dla wspólnot nie rozwiązuje tego problemu (patrz Open Questions).

## User & Persona

**Primary: Zarządca (Manager)** — obsługuje zgłoszenia z jednego budynku; dziś odbiera telefony, gubi wątki i rozkłada zasoby serwisowe bez obiektywnego priorytetu. Sięga po produkt w momencie, gdy musi zdecydować, co naprawiać najpierw, i gdy ci sami mieszkańcy dopytują o status.

**Secondary persona: Mieszkaniec (Resident)** — zgłasza usterkę i chce mieć potwierdzenie, status i ETA; dziś dzwoni „w ciemno".

Trzecia osoba zainteresowana: **Zarząd wspólnoty** (eskalacje, presja przy nierozwiązanych awariach) — poza MVP.

## Access Control

- Konta zakładają **zarządca oraz pracownik administracji** (przegląd 2026-11-26). Brak samodzielnej rejestracji.
- MVP ma **3 role** (przegląd 2026-11-26 — rola serwisu wycięta przy redukcji zakresu do 3 tygodni):
  - **Mieszkaniec:** widzi **wszystkie zgłoszenia swojej wspólnoty i ich statusy** (jawność w obrębie wspólnoty, bez tożsamości zgłaszających i bez miejsca usterki); tworzy zgłoszenia.
  - **Pracownik administracji:** wprowadza zgłoszenia w imieniu mieszkańca (FR-013); widzi zgłaszającego i miejsce usterki; zakłada konta.
  - **Zarządca:** pełna obsługa zgłoszeń swojej wspólnoty (statusy, korekta priorytetu, ocena „Do oceny”); widzi zgłaszającego i miejsce usterki; zakłada konta.
- **Po MVP — pracownik serwisu** (FR-019, FR-020 = nice-to-have): należy do grupy przypisanej do wspólnoty, widzi zgłoszenia tylko tej wspólnoty wraz z kontaktem do zgłaszającego, może ustawiać „W realizacji” / „Zamknięte”, nie zmienia priorytetu. W MVP zarządca przekazuje serwisowi kontakt poza aplikacją.
- Model wielowspólnotowy: dane wspólnot są od siebie odizolowane (pracownik spoza grupy nie widzi wspólnoty).
- **Każde żądanie dostępu do danych wymaga uwierzytelnionej tożsamości i autoryzacji na poziomie roli** (wypowiedź użytkownika 2026-11-25). **Każdy punkt dostępu do danych jest chroniony** — brak dostępnych „furtką" danych publicznych poza tym, co jawne z założenia (treści zgłoszeń bez tożsamości widoczne dla zalogowanych mieszkańców wspólnoty). Mechanizm tokena → ## Forward: tech-stack.

Uwaga zakresowa: pomysł wyjściowy mówił „jeden budynek"; powyższy model zakłada wiele wspólnot i grupy pracowników — **rozstrzygnięte w Fazie 3: MVP obejmuje jedną wspólnotę**, wielowspólnotowość przeniesiona do ## Forward: technical-roadmap.

## Success Criteria

### Primary
- Mieszkaniec zgłasza usterkę i w ciągu jednej sesji widzi: potwierdzenie, priorytet P1–P4 (lub „Do oceny”) oraz termin docelowy; zarządca widzi zgłoszenie na liście sortowanej po pilności terminu i może zmienić jego status. Przepływ 1–8 (poniżej) działa end-to-end.

#### Przepływ MVP 1–8 (odtworzony z FR, potwierdzony przez użytkownika 2026-11-26)
1. Mieszkaniec loguje się (FR-002).
2. Tworzy zgłoszenie z opisem (FR-003).
3. System nadaje priorytet P1–P4 lub status „Do oceny” (FR-004).
4. System wyznacza termin docelowy (FR-005).
5. Mieszkaniec widzi potwierdzenie, priorytet i termin.
6. Zgłoszenie trafia na wspólną listę wspólnoty bez tożsamości zgłaszającego (FR-007, FR-015).
7. Zarządca widzi je na liście posortowanej według pilności terminu (FR-009).
8. Zarządca zmienia status; mieszkaniec widzi nowy status (FR-010).

### Secondary
- Mieszkańcy wspólnoty realnie adoptują narzędzie — liczba zgłoszeń telefonicznych spada na rzecz zgłoszeń w aplikacji.

### Guardrails
- Zgłoszenia nie znikają ani nie zmieniają stanu bez śladu — każda zmiana statusu jest widoczna (nieusuwalność danych).
- Treść zgłoszeń i konta nie są dostępne dla osób nieuprawnionych, nawet przy MVP dla jednej wspólnoty (prywatność danych).
- Klasyfikacja P1–P4 bywa błędna, ale termin SLA i potwierdzenie zgłoszenia muszą być zawsze widoczne i przewidywalne (przewidywalność SLA).

## Non-Goals

MVP **nie** robi (potwierdzone przez użytkownika 2026-11-25, multi-select):
- **Brak powiadomień wychodzących (e-mail/SMS/push)** — o stanie zgłoszenia wszyscy dowiadują się z list w aplikacji; P1 nie alarmuje zarządcy automatycznie.
- **Brak komentarzy/czatów pod zgłoszeniami** — komunikacja przy zgłoszeniu odbywa się poza aplikacją (telefon zarządcy / FR-013).
- **Brak modułu raportów i analityki dla zarządu wspólnoty.**

Dodatkowo, konsekwencje wcześniejszych decyzji (nice-to-have poza MVP):
- **Brak detekcji i obsługi duplikatów** (FR-006, FR-012 = nice-to-have) — przy skali jednej wspólnoty wartość duplikatów jest niska; wspólna lista zgłoszeń (FR-007) częściowo zastępuje sygnał „to już zgłaszane".
- **Brak panelu zarządzania regułami w aplikacji** (FR-018 = nice-to-have) — reguły, słowniki i czasy reakcji jako wartości startowe przygotowane przy wdrożeniu, zmiana ręcznie „w razie konieczności".
- **Brak obsługi wielu wspólnot** (MVP = jedna wspólnota) — produkt ma nie blokować tego w przyszłości (## Forward: technical-roadmap).
- **Brak automatycznego harmonogramu retencji/anonimizacji.**

Redukcja zakresu do 3 tygodni (decyzja użytkownika 2026-11-26, multi-select) — poza MVP:
- **Brak anonimizacji w aplikacji** (FR-016 = nice-to-have) — w MVP zastępuje ją ręczna procedura; zasada retencji pozostaje regułą operacyjną.
- **Brak roli pracownika serwisu** (FR-019, FR-020 = nice-to-have) — zarządca przekazuje kontakt serwisowi poza aplikacją i sam zmienia statusy.
- **Brak podbić zgłoszeń** (FR-017 = nice-to-have) — sygnałem „to już zgłoszone” jest wspólna lista (FR-007).
- **Brak edycji zgłoszenia przez mieszkańca** (FR-008 = nice-to-have) — poprawki przez zarządcę / telefon; odpada reguła reklasyfikacji po edycji.

## Quality cross-check

Przebieg: 2026-11-26 (przegląd po sesji z 2026-11-25). Wynik: **accepted** — bez braków.
- Access Control: present (3 role w MVP: mieszkaniec / pracownik administracji / zarządca; konta zakłada zarządca lub pracownik administracji; pełna autoryzacja role-based, każdy endpoint chroniony; mechanizm JWT → Forward: tech-stack).
- Business Logic: present (reguła jedno-zdaniowa: słownik atrybutów → P1–P4 + konfigurowalny czas reakcji; doprecyzowania: „Do oceny”, zegar 24/7, kategoria vs słowa kluczowe — wyższy wygrywa).
- Project artifacts: present.
- Timeline-cost ack: present (mvp_weeks = 3 ≤ 3 po redukcji zakresu — 11 FR must-have; hard_deadline 2027-01-10, praca po godzinach).
- Non-Goals: present (11 pozycji z uzasadnieniami).
- Preserved behavior: n/a (greenfield).

Naprawione w przeglądzie: sprzeczność w kryterium Primary (alert o duplikacie), brak zapisanego przepływu 1–8, brak US-01, 8 FR bez rundy Socrates.
Sprawy otwarte bez blokady: OQ 1 (insight vs gotowe systemy), OQ 6 („Do oceny” przy kategorii z domyślnym priorytetem), OQ 7 (wartości startowe).

## Product framing (dla /10x-prd)

- product_type: web-app (użytkownik: „aplikacja webowa"; przyszły desktop jako możliwy zwrot — ## Forward)
- target_scale: { users: medium (~100 osób), qps: low, data_volume: small }
- timeline_budget: { mvp_weeks: 3, hard_deadline: 2027-01-10, after_hours_only: true }
- context_type: greenfield

## Business Logic

**Reguła (jedno zdanie, wypowiedź użytkownika 2026-11-25):** Jeżeli w zgłoszeniu występuje informacja o awarii rury, zalewaniu, zapachu palonej gumy, dymie lub innych atrybutach zdefiniowanych w konfigurowalnym katalogu, system przypisuje zgłoszeniu kategorię P1 i termin reakcji 4 h — generalnie kategorie P1–P4, zasady kategoryzacji oraz czasy reakcji na poszczególne incydenty są konfigurowalne. *(Dopisek o sposobie przechowywania przeniesiony do ## Forward: tech-stack — 2026-11-26.)*

Rozwinięcie (z wypowiedzi użytkownika):
- **Wejście reguły:** treść zgłoszenia (opis) sprawdzana względem słownika słów kluczowych / atrybutów; słownik, mapowanie na kategorie P1–P4 i czasy reakcji — dane konfigurowalne, nie zaszyte na sztywno.
- **Wyjście reguły:** kategoria priorytetu P1–P4 + termin reakcji (docelowy) z konfiguracji; przykład z wypowiedzi: awaria rury / zalewanie / smród palonej gumy / dym → P1, termin reakcji 4 h.
- **Jak użytkownik spotyka regułę:** mieszkańcy **widzą priorytet** i termin zgłoszenia; zarządca może skorygować priorytet (FR-014). (Podbicia FR-017 — poza MVP.)

Doprecyzowania z przeglądu (2026-11-26, decyzje użytkownika):
- **Brak trafień w katalogu:** zgłoszenie dostaje status **„Do oceny”** z własnym **terminem oceny** — wartość konfigurowalna (wartość startowa przygotowana przy wdrożeniu, tak jak czasy reakcji P1–P4). Mieszkaniec zawsze widzi jakiś termin (guardrail przewidywalności zachowany).
- **Zegar terminu:** liczony **całą dobę, 7 dni w tygodniu** (bez godzin pracy).
- **Kategoria wybierana przez mieszkańca + słowa kluczowe:** **wyższy z dwóch priorytetów wygrywa** — kategoria ma domyślny priorytet, trafienie w katalogu słów kluczowych może go podnieść (np. kategoria „hydraulika” → niższy priorytet, ale „zalewa” w opisie → P1). Wpływ na „Do oceny” → Open Questions.
- **Korekta zarządcy (FR-014):** zapisuje kto, kiedy, z jakiego priorytetu na jaki.
- *Po MVP (decyzje zachowane na później):* podbicia (FR-017) są wyłącznie sygnałem dla zarządcy — nie zmieniają priorytetu, terminu ani kolejności; edycja zgłoszenia (FR-008) powoduje ponowną klasyfikację, chyba że zarządca ręcznie skorygował priorytet — wtedy korekta ma pierwszeństwo.

## Non-Functional Requirements

- Użytkownik widzi potwierdzenie każdej akcji natychmiast („strona reaguje od razu" — wypowiedź użytkownika); operacja dłuższa niż ~2 s pokazuje widoczny postęp. Próg potwierdzenia celowo bez liczby (decyzja użytkownika 2026-11-26).
- Produkt dostępny jako **aplikacja webowa** (decyzja użytkownika 2026-11-25); dopuszczalna przyszła zmiana w stronę desktopu — nie wymaga przepisania rdzenia (## Forward).
- Ścieżka mieszkańca (logowanie, zgłoszenie, wspólna lista, podgląd statusu) jest w pełni wykonalna na telefonie — mieszkańcy wchodzą głównie z komórki. Panel zarządcy i pracownika administracji może wymagać komputera (decyzja użytkownika 2026-11-26).
- Dostęp do treści zgłoszeń i danych mieszkańców mają wyłącznie uprawnione role: mieszkaniec (treści zgłoszeń bez tożsamości zgłaszających), administracja/zarządca (pełnia). Nikt nieuprawniony nie widzi danych.
- MVP nie zawiera anonimizacji w aplikacji (FR-016 poza MVP) ani harmonogramu retencji; zasada retencji pozostaje regułą operacyjną realizowaną ręcznie: „dane trzymamy tylko tak długo, jak wymaga obsługa zgłoszenia i rozliczenia wspólnoty".

## Forward: tech-stack

- Wymóg użytkownika (2026-11-25, przeniesiony z ## Business Logic 2026-11-26): kategorie P1–P4, zasady kategoryzacji i czasy reakcji „trzymane w bazie (słowa kluczowe wspomagające kategoryzację w osobnej tabeli)"; wartości startowe ładowane przy wdrożeniu (seed).
- Wymóg użytkownika (2026-11-25): logowanie i sprawdzanie uprawnień przez **token JWT**; każda operacja na danych autoryzowana przez **authentication + autoryzację rolami**; **każdy endpoint zautoryzowany**. Zasada brzegowa (co musi być prawdziwe) zapisana w ## Access Control; wybór technologii (biblioteka, przechowywanie/odświeżanie tokena, sekret, czas wygaśnięcia) — decyzja etapu doboru stacku / planowania implementacji.

## Forward: technical-roadmap

- MVP dla **jednej wspólnoty**, ale model danych i projekt mają nie blokować przyszłej obsługi wielu wspólnot (izolacja danych per wspólnota, grupy pracowników scoped do wspólnoty). Decyzja użytkownika z 2026-11-25: „zostaniemy przy mniejszym modelu, ale design tak, aby nas nie blokował w przyszłości".
- Kierunki z rozmowy (nie wymogi): wielowspólnotowość, grupy pracowników, zarząd wspólnoty jako dodatkowa persona z raportami.

## Functional Requirements

Format FR zażyczył sobie użytkownik: „[Akter] może [zdolność]" (zamiast „can").

### Konta i dostęp
- FR-001: Zarządca lub pracownik administracji może zakładać konta użytkowników. Priority: must-have
  > Socrates: Kontrargument: „ręczne konta to tarcie zabijające adopcję". Rozwiązanie: konta zakładane raz przy wdrożeniu; kanał telefoniczny obsługuje FR-013 — brak konta nie odcina mieszkańca od systemu.
- FR-002: Mieszkaniec może logować się do aplikacji. Priority: must-have
  > Socrates: Kontrargument: „logowanie zniechęci osoby starsze i zostaną przy telefonach". Rozwiązanie: logowanie zostaje (wymóg systemu i prywatności); kanał telefoniczny obsługuje pracownik administracji wprowadzający zgłoszenie w imieniu mieszkańca (FR-013).

### Zgłaszanie
- FR-003: Mieszkaniec może utworzyć zgłoszenie usterki dla swojej wspólnoty. Priority: must-have
  > Socrates: Kontrargument: „zbyt ubogie zgłoszenia, klasyfikator nie ma czego oceniać". Rozwiązanie: kontrargument rozważony, FR zostaje — doprecyzowanie należy do obsługi (FR-013/014); minimalne pola formularza do zdefiniowania przy projektowaniu.
  > Pola formularza (decyzja użytkownika 2026-11-26): **opis** (widoczny na wspólnej liście), **miejsce usterki** (widoczne tylko dla obsługi — rozstrzygnięcie FR-007), **kategoria wybierana z listy** (drugi sygnał dla klasyfikacji).
- FR-004: System może nadać zgłoszeniu priorytet P1–P4. Priority: must-have
  > Socrates: Kontrargument: „błędna klasyfikacja (zalana piwnica jako P4) podważy zaufanie szybciej niż brak klasyfikacji". Rozwiązanie: automat filtruje szum, zarządca zawsze może skorygować priorytet (FR-014) — błąd jest ludzko naprawialny i widoczny.
- FR-005: System może wyznaczyć zgłoszeniu termin SLA. Priority: must-have
  > Socrates: Kontrargument: „SLA bez mocy przerobowej serwisu staje się podstawą roszczeń ('obiecaliście 48h!')". Rozwiązanie: termin SLA prezentowany i nazywany w produkcie jako **termin docelowy** (orientacyjny), nie kontrakt.
- FR-006: Mieszkaniec może zobaczyć alert o potencjalnym duplikacie podczas zgłaszania. Priority: nice-to-have
  > Socrates (2026-11-26): Kontrargument: „kanał telefoniczny (FR-013) mnoży duplikaty — pracownik administracji nie wie, że usterka już jest zgłoszona". Rozwiązanie: ryzyko zaakceptowane; pracownik przegląda listę przed wpisaniem; FR pozostaje poza MVP.

### Przeglądanie
- FR-007: Mieszkaniec może przeglądać wszystkie zgłoszenia swojej wspólnoty z ich statusami. Priority: must-have
  > Socrates (2026-11-26): Kontrargument: „treść opisu może zdradzać tożsamość zgłaszającego (np. numer lokalu) — anonimowość z FR-015 byłaby pozorna". Rozwiązanie: FR zostaje; numer lokalu / miejsce usterki podawane w **osobnym polu widocznym tylko dla obsługi** (administracja, zarządca; po MVP także serwis), opis trafia na wspólną listę. Ryzyko resztkowe (dane wpisane w opis) zaakceptowane.
- FR-008: Mieszkaniec może edytować własne zgłoszenie. Priority: nice-to-have — poza MVP (redukcja zakresu 2026-11-26)
  > Socrates (2026-11-26): Kontrargument: „mieszkaniec dopisze 'dym!' / 'zalewa', żeby reklasyfikacja wymusiła P1". Rozwiązanie: FR zostaje bez dodatkowych zabezpieczeń — zarządca koryguje priorytet (FR-014), a jego korekta ma pierwszeństwo i nie jest nadpisywana przy kolejnych edycjach.

### Obsługa (zarządca)
- FR-009: Zarządca może przeglądać zgłoszenia sortowane wg pilności SLA. Priority: must-have
  > Socrates: Kontrargument rozważony („to tylko sortowanie"): zostaje — sortowanie jest nośnikiem reguły domenowej (termin docelowy) w codziennej pracy zarządcy.
- FR-010: Zarządca może zmieniać statusy zgłoszeń (Nowe → Przyjęte → W realizacji → Zamknięte, plus Odrzucone). Priority: must-have
  > Socrates: Kontrargument rozważony, FR zostaje (użytkownik: „reszta zostaje", 2026-11-25).
- FR-011: Zarządca może dodawać notatki serwisowe. Priority: nice-to-have
  > Socrates: Kontrargument rozważony, zostaje jako nice-to-have; poza MVP — do ## Non-Goals.
- FR-012: Zarządca może potwierdzać lub odpinać duplikaty. Priority: nice-to-have
  > Socrates: Kontrargument rozważony, zostaje jako nice-to-have wraz z FR-006; poza MVP — do ## Non-Goals.

### Kanał wspomagany
- FR-013: Pracownik administracji może wprowadzić zgłoszenie usterki w imieniu mieszkańca. Priority: must-have
  > Socrates: Kontrargument rozważony, FR zostaje (użytkownik: „reszta zostaje", 2026-11-25) — przejmuje kanał telefoniczny, domykając obawę o adopcję (FR-001/002).
- FR-014: Zarządca może skorygować nadany priorytet P1–P4 (w tym ocenić zgłoszenie „Do oceny”). Priority: must-have
  > Socrates (2026-11-26): Kontrargument: „korekta priorytetu bez śladu łamie guardrail 'nic nie zmienia stanu bez śladu'". Rozwiązanie: FR zostaje; każda korekta zapisuje **kto, kiedy, z jakiego priorytetu na jaki**.

### Serwis
- FR-019: Pracownik serwisu może przeglądać zgłoszenia swojej wspólnoty wraz z danymi kontaktowymi zgłaszającego. Priority: nice-to-have — poza MVP (redukcja zakresu 2026-11-26)
  > Socrates (2026-11-26): Kontrargument: „serwis nie może oznaczyć naprawy, więc status rozjedzie się z rzeczywistością". Rozwiązanie: FR zostaje; serwis dostaje możliwość zmiany statusu — FR-020 (zmienia wcześniejszą decyzję „serwis nie zmienia statusu").
- FR-020: Pracownik serwisu może ustawiać status zgłoszenia „W realizacji” i „Zamknięte”. Priority: nice-to-have — poza MVP (redukcja zakresu 2026-11-26)
  > Socrates: wynika z rozstrzygnięcia FR-019 (2026-11-26).

### Prywatność zgłaszania
- FR-017: Dowolny mieszkaniec budynku (nie tylko zgłaszający) może **podbic** zgłoszenie (eskalacja do zarządcy) — jedna osoba może podbić dane zgłoszenie tylko raz; licznik podbić jest widoczny **tylko dla zarządcy**. Priority: nice-to-have — poza MVP (redukcja zakresu 2026-11-26)
  > Socrates (2026-11-26): Kontrargument: „'przycisk placebo' — mieszkaniec nie widzi efektu podbicia i wraca do telefonu". Rozwiązanie: FR zostaje; mieszkaniec widzi potwierdzenie, że **sam** podbił zgłoszenie (bez licznika podbić innych).
- FR-018: Uprawniona osoba może edytować katalog reguł kategoryzacji (słowniki, mapowanie P1–P4, czasy reakcji) w aplikacji — panel konfiguracji. Priority: nice-to-have — poza MVP; reguły i czasy reakcji przygotowywane wstępnie przy wdrożeniu
  > Socrates (2026-11-26): Kontrargument: „bez panelu każda poprawka katalogu wymaga programisty — przy błędnej klasyfikacji zarządca jest bezradny". Rozwiązanie: ryzyko zaakceptowane; w MVP katalog (wartości startowe) poprawiany ręcznie, panel po MVP.
- FR-016: System może na żądanie zanonimizować dane zgłaszającego wybrane(ych) zgłoszeń(zenia) — „funkcja anonimizująca dane w razie konieczności". Priority: nice-to-have — poza MVP (redukcja zakresu 2026-11-26)
  > Socrates (2026-11-26): Kontrargument: „anonimizacja otwartego zgłoszenia odcina serwis i zarządcę od kontaktu ze zgłaszającym". Rozwiązanie: FR zostaje; anonimizacja dostępna **tylko dla zgłoszeń Zamkniętych lub Odrzuconych**.
  > Zapisane od użytkownika (2026-11-25): retencja — „dane trzymamy tylko tak długo, jak wymaga obsługa zgłoszenia i rozliczenia wspólnoty". Zakres/uszczegółowienie (kto wywołuje, co zostaje po anonimizacji) → Open Questions.
- FR-015: Zgłoszenie jest prezentowane pozostałym mieszkańcom **bez tożsamości zgłaszającego** (wspólna lista pokazuje treść i status, nie „kto zgłosił"); pracownik administracji i zarządca widzą zgłaszającego i mogą się z nim skontaktować (po MVP także pracownik serwisu — FR-019). Priority: must-have
  > Socrates: Kontrargument: pełna jawność listy (FR-007) może krępować mieszkańców przed zgłaszaniem („wszyscy zobaczą, że to ja zgłosiłem dziurę w chodniku"). Rozwiązanie użytkownika: tożsamość widoczna tylko dla administracji/zarządcy, motywacja RODO. Mieszkaniec zalogowany nadal widzi **własne** zgłoszenia (edycja FR-008 — poza MVP).

## User Stories

### US-01: Mieszkaniec zgłasza usterkę i widzi priorytet oraz termin docelowy
(zbudowana z przepływu MVP 1–8, potwierdzona przez użytkownika 2026-11-26)

- **Given** zalogowany mieszkaniec wspólnoty
- **When** wysyła zgłoszenie z opisem i miejscem usterki (osobne pole)
- **Then** widzi potwierdzenie, priorytet P1–P4 lub „Do oceny” oraz termin docelowy; zgłoszenie jest na wspólnej liście (bez tożsamości zgłaszającego i miejsca usterki), a u zarządcy na liście według pilności terminu

#### Acceptance Criteria
- Opis z atrybutem P1 (np. „zalewa”) → P1, termin 4 h liczony całą dobę, 7 dni w tygodniu.
- Brak trafień w katalogu → „Do oceny” z terminem oceny z konfiguracji.
- Zmiana statusu przez zarządcę jest widoczna dla mieszkańca.

Uwaga: FR-006 + FR-012 (detekcja i obsługa duplikatów) jako nice-to-have = duplikaty poza MVP; wymaga odnotowania w ## Non-Goals. Priorytetyzacja P1–P4 + SLA (FR-004/005) pozostają regułą domenową MVP.

## Open Questions

1. **Insight vs status quo** — dlaczego gotowe oprogramowanie dla wspólnot nie rozwiązuje tego problemu? — Owner: użytkownik. Block: nie (PRD nie jest od tego hollow, ale warto wiedzieć przed decyzją build-vs-buy).
2. ~~Kto eskaluje priorytet?~~ **ROZSTRZYNĘTE (2026-11-25):** dowolny mieszkaniec budynku podbija zgłoszenie (raz na osobę), licznik widoczny tylko dla zarządcy — FR-017.
3. ~~Kto konfiguruje katalog reguł w MVP?~~ **ROZSTRZYNĘTE (2026-11-25):** reguły i czasy reakcji przygotowywane wstępnie przy wdrożeniu; panel konfiguracji poza MVP jako nice-to-have — FR-018.
4. ~~Brak historyjki US-01~~ **ROZSTRZYGNIĘTE (2026-11-26):** US-01 dopisana z przepływu 1–8.
5. ~~Minimalne pola formularza (FR-003)~~ **ROZSTRZYGNIĘTE (2026-11-26):** opis, miejsce usterki (tylko dla obsługi), kategoria z listy.
6. **Kiedy zgłoszenie dostaje „Do oceny”, skoro kategoria ma domyślny priorytet?** — Jeżeli każda kategoria z listy ma domyślny priorytet, reguła „wyższy wygrywa” zawsze da wynik i „Do oceny” nigdy nie wystąpi. Do ustalenia: czy kategoria jest obowiązkowa, czy istnieje kategoria bez priorytetu (np. „Inne”) prowadząca do „Do oceny”. — Owner: użytkownik. Block: nie, ale wpływa na regułę domenową i US-01 (kryterium „brak trafień → Do oceny”).
7. **Wartości startowe** — lista kategorii z domyślnymi priorytetami, katalog słów kluczowych, czasy reakcji P1–P4 oraz termin oceny „Do oceny”. — Owner: użytkownik. Block: nie (potrzebne przed wdrożeniem, nie przed PRD).
