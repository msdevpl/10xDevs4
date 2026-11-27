---
project: "Sasiad-Zgloszenia"
version: 1
status: draft
created: 2026-11-26
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

# TODO: insight vs status quo — see Open Questions (dlaczego gotowe oprogramowanie dla wspólnot nie rozwiązuje tego problemu)

## User & Persona

**Primary: Zarządca (Manager)** — obsługuje zgłoszenia z jednego budynku; dziś odbiera telefony, gubi wątki i rozkłada zasoby serwisowe bez obiektywnego priorytetu. Sięga po produkt w momencie, gdy musi zdecydować, co naprawiać najpierw, i gdy ci sami mieszkańcy dopytują o status.

### Secondary persona

**Mieszkaniec (Resident)** — zgłasza usterkę i chce mieć potwierdzenie, status i ETA; dziś dzwoni „w ciemno".

Trzecia osoba zainteresowana: **Zarząd wspólnoty** (eskalacje, presja przy nierozwiązanych awariach) — poza MVP.

## Success Criteria

### Primary
- Mieszkaniec zgłasza usterkę i w ciągu jednej sesji widzi: potwierdzenie, priorytet P1–P4 (lub „Do oceny”) oraz termin docelowy; zarządca widzi zgłoszenie na liście sortowanej po pilności terminu i może zmienić jego status. Przepływ 1–8 (poniżej) działa end-to-end.

#### Przepływ MVP 1–8
1. Mieszkaniec loguje się (FR-002).
2. Tworzy zgłoszenie z opisem (FR-003).
3. System nadaje priorytet P1–P4 lub status „Do oceny” (FR-004).
4. System wyznacza termin docelowy (FR-005).
5. Mieszkaniec widzi potwierdzenie, priorytet i termin; przy zagrożeniu (np. dym, gaz, zalewanie) także kartę bezpieczeństwa z instrukcją, co zrobić od razu (FR-021).
6. Zgłoszenie trafia na wspólną listę wspólnoty bez tożsamości zgłaszającego (FR-007, FR-015).
7. Zarządca widzi je na liście posortowanej według pilności terminu (FR-009); zgłoszenie P1 dodatkowo od razu trafia do niego e-mailem (FR-023).
8. Zarządca zmienia status; mieszkaniec widzi nowy status (FR-010).

### Secondary
- Mieszkańcy wspólnoty realnie adoptują narzędzie — liczba zgłoszeń telefonicznych spada na rzecz zgłoszeń w aplikacji.

### Guardrails
- Zgłoszenia nie znikają ani nie zmieniają stanu bez śladu — każda zmiana statusu jest widoczna (nieusuwalność danych).
- Treść zgłoszeń i konta nie są dostępne dla osób nieuprawnionych, nawet przy MVP dla jednej wspólnoty (prywatność danych).
- Klasyfikacja P1–P4 bywa błędna, ale termin SLA i potwierdzenie zgłoszenia muszą być zawsze widoczne i przewidywalne (przewidywalność SLA).

## User Stories

### US-01: Mieszkaniec zgłasza usterkę i widzi priorytet oraz termin docelowy

- **Given** zalogowany mieszkaniec wspólnoty
- **When** wysyła zgłoszenie z opisem i miejscem usterki (osobne pole)
- **Then** widzi potwierdzenie, priorytet P1–P4 lub „Do oceny” oraz termin docelowy; zgłoszenie jest na wspólnej liście (bez tożsamości zgłaszającego i miejsca usterki), a u zarządcy na liście według pilności terminu

#### Acceptance Criteria
- Opis z atrybutem P1 (np. „zalewa”) → P1, termin 4 h liczony całą dobę, 7 dni w tygodniu.
- Brak trafień w katalogu → „Do oceny” z terminem oceny z konfiguracji.
- Zmiana statusu przez zarządcę jest widoczna dla mieszkańca.
- Opis z atrybutem, do którego w katalogu przypisano kartę bezpieczeństwa (np. „dym”), → mieszkaniec widzi tę kartę razem z potwierdzeniem zgłoszenia (FR-021).

## Functional Requirements

Format FR (ustalony przez użytkownika): „[Aktor] może [zdolność]".

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
- FR-015: Zgłoszenie jest prezentowane pozostałym mieszkańcom **bez tożsamości zgłaszającego** (wspólna lista pokazuje treść i status, nie „kto zgłosił"); pracownik administracji i zarządca widzą zgłaszającego i mogą się z nim skontaktować (po MVP także pracownik serwisu — FR-019). Priority: must-have
  > Socrates: Kontrargument: pełna jawność listy (FR-007) może krępować mieszkańców przed zgłaszaniem („wszyscy zobaczą, że to ja zgłosiłem dziurę w chodniku"). Rozwiązanie użytkownika: tożsamość widoczna tylko dla administracji/zarządcy, motywacja RODO. Mieszkaniec zalogowany nadal widzi **własne** zgłoszenia (edycja FR-008 — poza MVP).
- FR-016: System może na żądanie zanonimizować dane zgłaszającego wybrane(ych) zgłoszeń(zenia) — „funkcja anonimizująca dane w razie konieczności". Priority: nice-to-have — poza MVP (redukcja zakresu 2026-11-26)
  > Socrates (2026-11-26): Kontrargument: „anonimizacja otwartego zgłoszenia odcina serwis i zarządcę od kontaktu ze zgłaszającym". Rozwiązanie: FR zostaje; anonimizacja dostępna **tylko dla zgłoszeń Zamkniętych lub Odrzuconych**.
  > Zapisane od użytkownika (2026-11-25): retencja — „dane trzymamy tylko tak długo, jak wymaga obsługa zgłoszenia i rozliczenia wspólnoty". Zakres/uszczegółowienie (kto wywołuje, co zostaje po anonimizacji) → Open Questions.
- FR-017: Dowolny mieszkaniec budynku (nie tylko zgłaszający) może **podbić** zgłoszenie (eskalacja do zarządcy) — jedna osoba może podbić dane zgłoszenie tylko raz; licznik podbić jest widoczny **tylko dla zarządcy**. Priority: nice-to-have — poza MVP (redukcja zakresu 2026-11-26)
  > Socrates (2026-11-26): Kontrargument: „'przycisk placebo' — mieszkaniec nie widzi efektu podbicia i wraca do telefonu". Rozwiązanie: FR zostaje; mieszkaniec widzi potwierdzenie, że **sam** podbił zgłoszenie (bez licznika podbić innych).

### Konfiguracja reguł
- FR-018: Uprawniona osoba może edytować katalog reguł kategoryzacji (słowniki, mapowanie P1–P4, czasy reakcji) w aplikacji — panel konfiguracji. Priority: nice-to-have — poza MVP; reguły i czasy reakcji przygotowywane wstępnie przy wdrożeniu
  > Socrates (2026-11-26): Kontrargument: „bez panelu każda poprawka katalogu wymaga programisty — przy błędnej klasyfikacji zarządca jest bezradny". Rozwiązanie: ryzyko zaakceptowane; w MVP katalog (wartości startowe) poprawiany ręcznie, panel po MVP.

### Wsparcie AI dla zarządcy
- FR-022: Zarządca może poprosić o **podpowiedź AI** dla zgłoszenia „Do oceny” — system zwraca proponowaną kategorię, priorytet P1–P4 i krótkie uzasadnienie; zarządca stosuje ją jednym kliknięciem (przez korektę FR-014) albo ignoruje. Priority: nice-to-have — w MVP, realizowane po ścieżce głównej (logowanie → zgłoszenie → lista → statusy)
  > Decyzja użytkownika (2026-11-27): mała funkcja AI zamiast czatu. AI wyłącznie **doradza** zarządcy — nigdy sam nie zmienia priorytetu, nic nie widzi mieszkaniec, porady bezpieczeństwa pozostają w kartach (FR-021). Do modelu trafia **tylko opis usterki** (bez tożsamości zgłaszającego i miejsca usterki). System zapisuje, czy podpowiedź została zastosowana (miara trafności AI; odrzucone podpowiedzi wskazują słowa warte dopisania do katalogu).

### Bezpieczeństwo mieszkańca
- FR-023: System może natychmiast powiadomić zarządcę **e-mailem** o nowym zgłoszeniu z priorytetem P1 (treść, priorytet, termin reakcji, link do zgłoszenia w aplikacji). Priority: must-have
  > Decyzja użytkownika (2026-11-27): bez alertu zgłoszenie P1 złożone np. w nocy czeka niezauważone, a mieszkaniec widzi termin 4 h, którego nikt nie pilnuje — w najważniejszym scenariuszu aplikacja byłaby gorsza niż telefon. Wyjątek od zasady „brak powiadomień wychodzących” (## Non-Goals) ograniczony do P1 i tylko do zarządcy.
- FR-021: Mieszkaniec może zobaczyć **kartę bezpieczeństwa** — stałą, przygotowaną wcześniej instrukcję, co zrobić od razu (np. „wyłącz bezpiecznik, jeśli to bezpieczne; przy zagrożeniu dzwoń 112”) — gdy jego zgłoszenie trafi w atrybut katalogu, do którego przypisano kartę. Priority: must-have
  > Decyzja użytkownika (2026-11-27): zamiast czatu z AI w MVP. Treść kart jest **deterministyczna** (część katalogu reguł, wartości startowe przy wdrożeniu) — porady bezpieczeństwa nigdy nie pochodzą z modelu językowego. Karta jest informacją, nie blokuje wysłania zgłoszenia i nie zastępuje telefonu alarmowego.

## Non-Functional Requirements

- Użytkownik widzi potwierdzenie każdej akcji natychmiast („strona reaguje od razu" — wypowiedź użytkownika); operacja dłuższa niż ~2 s pokazuje widoczny postęp. Próg potwierdzenia celowo bez liczby (decyzja użytkownika 2026-11-26).
- Produkt dostępny jako **aplikacja webowa** (decyzja użytkownika 2026-11-25); dopuszczalna przyszła zmiana w stronę desktopu — nie wymaga przepisania rdzenia.
- Ścieżka mieszkańca (logowanie, zgłoszenie, wspólna lista, podgląd statusu) jest w pełni wykonalna na telefonie — mieszkańcy wchodzą głównie z komórki. Panel zarządcy i pracownika administracji może wymagać komputera (decyzja użytkownika 2026-11-26).
- Dostęp do treści zgłoszeń i danych mieszkańców mają wyłącznie uprawnione role: mieszkaniec (treści zgłoszeń bez tożsamości zgłaszających), administracja/zarządca (pełnia). Nikt nieuprawniony nie widzi danych.
- Alert P1 (FR-023) wychodzi w ciągu ok. 1 minuty od utworzenia zgłoszenia; niedostępność usługi e-mail nie blokuje tworzenia zgłoszeń.
- Niedostępność usługi AI (FR-022) nie blokuje żadnej innej funkcji; podpowiedź ma ograniczony czas oczekiwania i pokazuje postęp zgodnie z progiem ~2 s.
- MVP nie zawiera anonimizacji w aplikacji (FR-016 poza MVP) ani harmonogramu retencji; zasada retencji pozostaje regułą operacyjną realizowaną ręcznie: „dane trzymamy tylko tak długo, jak wymaga obsługa zgłoszenia i rozliczenia wspólnoty".

## Business Logic

Jeżeli w zgłoszeniu występuje informacja o awarii rury, zalewaniu, zapachu palonej gumy, dymie lub innych atrybutach zdefiniowanych w konfigurowalnym katalogu, system przypisuje zgłoszeniu kategorię P1 i termin reakcji 4 h — generalnie kategorie P1–P4, zasady kategoryzacji oraz czasy reakcji na poszczególne incydenty są konfigurowalne.

- **Wejście reguły:** treść zgłoszenia (opis) sprawdzana względem słownika słów kluczowych / atrybutów; słownik, mapowanie na kategorie P1–P4 i czasy reakcji — dane konfigurowalne, nie zaszyte na sztywno.
- **Wyjście reguły:** kategoria priorytetu P1–P4 + termin reakcji (docelowy) z konfiguracji; przykład z wypowiedzi: awaria rury / zalewanie / smród palonej gumy / dym → P1, termin reakcji 4 h.
- **Jak użytkownik spotyka regułę:** mieszkańcy **widzą priorytet** i termin zgłoszenia; zarządca może skorygować priorytet (FR-014).

Doprecyzowania (decyzje użytkownika 2026-11-26):
- **Brak trafień w katalogu:** zgłoszenie dostaje status **„Do oceny”** z własnym **terminem oceny** — wartość konfigurowalna (wartość startowa przygotowana przy wdrożeniu, tak jak czasy reakcji P1–P4). Mieszkaniec zawsze widzi jakiś termin (guardrail przewidywalności zachowany).
- **Zegar terminu:** liczony **całą dobę, 7 dni w tygodniu** (bez godzin pracy).
- **Kategoria wybierana przez mieszkańca + słowa kluczowe:** **wyższy z dwóch priorytetów wygrywa** — kategoria ma domyślny priorytet, trafienie w katalogu słów kluczowych może go podnieść (np. kategoria „hydraulika” → niższy priorytet, ale „zalewa” w opisie → P1). Wpływ na „Do oceny” → Open Questions.
- **Korekta zarządcy (FR-014):** zapisuje kto, kiedy, z jakiego priorytetu na jaki.
- **Podpowiedź AI (FR-022):** dostępna tylko dla zgłoszeń „Do oceny”, wywoływana na żądanie zarządcy (przycisk), bez przetwarzania w tle. Wynik ma stały format (kategoria z listy kategorii, P1–P4, uzasadnienie); odpowiedź spoza listy kategorii lub priorytetów jest odrzucana. Zastosowanie podpowiedzi = zwykła korekta FR-014 (ślad: kto, kiedy, z jakiego na jaki) z oznaczeniem, że pochodziła z AI. Błąd lub przekroczenie czasu modelu → komunikat „podpowiedź niedostępna”, ręczna ocena działa bez zmian.
- **Alert P1 (FR-023):** wysyłany, gdy zgłoszenie **przy utworzeniu** dostaje P1 (przez mieszkańca lub pracownika administracji — FR-013). Korekta priorytetu na P1 przez zarządcę (FR-014) alertu nie wysyła — zarządca już wie. Adres(y) odbiorcy z konfiguracji. Wysyłka po zapisaniu zgłoszenia; błąd wysyłki nie cofa ani nie blokuje zgłoszenia — jest logowany, a fakt wysłania lub błędu zapisywany przy zgłoszeniu (widoczny dla zarządcy). E-mail nie zawiera danych zgłaszającego poza tym, co i tak widzi zarządca.
- **Karta bezpieczeństwa (FR-021):** atrybut katalogu może mieć przypisaną kartę bezpieczeństwa (stały tekst). Trafienie w taki atrybut pokazuje kartę mieszkańcowi razem z potwierdzeniem zgłoszenia i w podglądzie jego zgłoszenia; przy kilku trafieniach pokazywane są wszystkie przypisane karty. Korekta priorytetu przez zarządcę nie zmienia kart.
- *Po MVP (decyzje zachowane na później):* podbicia (FR-017) są wyłącznie sygnałem dla zarządcy — nie zmieniają priorytetu, terminu ani kolejności; edycja zgłoszenia (FR-008) powoduje ponowną klasyfikację, chyba że zarządca ręcznie skorygował priorytet — wtedy korekta ma pierwszeństwo.

## Access Control

- Konta zakładają **zarządca oraz pracownik administracji**. Brak samodzielnej rejestracji.
- MVP ma **3 role**:
  - **Mieszkaniec:** widzi **wszystkie zgłoszenia swojej wspólnoty i ich statusy** (jawność w obrębie wspólnoty, bez tożsamości zgłaszających i bez miejsca usterki); tworzy zgłoszenia.
  - **Pracownik administracji:** wprowadza zgłoszenia w imieniu mieszkańca (FR-013); widzi zgłaszającego i miejsce usterki; zakłada konta.
  - **Zarządca:** pełna obsługa zgłoszeń swojej wspólnoty (statusy, korekta priorytetu, ocena „Do oceny”, podpowiedź AI — FR-022); widzi zgłaszającego i miejsce usterki; zakłada konta.
- **Po MVP — pracownik serwisu** (FR-019, FR-020 = nice-to-have): należy do grupy przypisanej do wspólnoty, widzi zgłoszenia tylko tej wspólnoty wraz z kontaktem do zgłaszającego, może ustawiać „W realizacji” / „Zamknięte”, nie zmienia priorytetu. W MVP zarządca przekazuje serwisowi kontakt poza aplikacją.
- Model wielowspólnotowy: dane wspólnot są od siebie odizolowane (pracownik spoza grupy nie widzi wspólnoty).
- **Każde żądanie dostępu do danych wymaga uwierzytelnionej tożsamości i autoryzacji na poziomie roli.** **Każdy punkt dostępu do danych jest chroniony** — brak dostępnych „furtką" danych publicznych poza tym, co jawne z założenia (treści zgłoszeń bez tożsamości widoczne dla zalogowanych mieszkańców wspólnoty).

Uwaga zakresowa: pomysł wyjściowy mówił „jeden budynek"; powyższy model zakłada wiele wspólnot i grupy pracowników — **MVP obejmuje jedną wspólnotę**, wielowspólnotowość poza MVP (## Non-Goals).

## Non-Goals

MVP **nie** robi (potwierdzone przez użytkownika 2026-11-25):
- **Brak powiadomień wychodzących (e-mail/SMS/push)** — o stanie zgłoszenia wszyscy dowiadują się z list w aplikacji; **Wyjątek: e-mail do zarządcy o zgłoszeniu P1 (FR-023, w MVP).** Po MVP do rozważenia kolejne powiadomienia, np. zmiana statusu dla mieszkańca, SMS przy P1.
- **Brak komentarzy/czatów pod zgłoszeniami** — komunikacja przy zgłoszeniu odbywa się poza aplikacją (telefon zarządcy / FR-013).
- **Brak modułu raportów i analityki dla zarządu wspólnoty.**

Konsekwencje wcześniejszych decyzji (nice-to-have poza MVP):
- **Brak detekcji i obsługi duplikatów** (FR-006, FR-012 = nice-to-have) — przy skali jednej wspólnoty wartość duplikatów jest niska; wspólna lista zgłoszeń (FR-007) częściowo zastępuje sygnał „to już zgłaszane".
- **Brak panelu zarządzania regułami w aplikacji** (FR-018 = nice-to-have) — reguły, słowniki i czasy reakcji jako wartości startowe przygotowane przy wdrożeniu, zmiana ręcznie „w razie konieczności".
- **Brak obsługi wielu wspólnot** (MVP = jedna wspólnota) — produkt ma nie blokować tego w przyszłości.
- **Brak automatycznego harmonogramu retencji/anonimizacji.**

Redukcja zakresu do 3 tygodni (decyzja użytkownika 2026-11-26):
- **Brak anonimizacji w aplikacji** (FR-016 = nice-to-have) — w MVP zastępuje ją ręczna procedura; zasada retencji pozostaje regułą operacyjną.
- **Brak roli pracownika serwisu** (FR-019, FR-020 = nice-to-have) — zarządca przekazuje kontakt serwisowi poza aplikacją i sam zmienia statusy.
- **Brak podbić zgłoszeń** (FR-017 = nice-to-have) — sygnałem „to już zgłoszone” jest wspólna lista (FR-007).
- **Brak edycji zgłoszenia przez mieszkańca** (FR-008 = nice-to-have) — poprawki przez zarządcę / telefon; odpada reguła reklasyfikacji po edycji.

Pomysły na po MVP (nice-to-have, decyzja użytkownika 2026-11-27):
- **Brak asystenta AI (czatu) przy zgłaszaniu problemu** — mieszkaniec opisuje problem (np. „śmierdzi instalacja”), agent AI analizuje go i podpowiada działania (wyłącz bezpieczniki, zadzwoń po straż pożarną lub elektryka) i pomaga wypełnić zgłoszenie. W MVP zastępuje go karta bezpieczeństwa (FR-021); integracja z modelem z FR-022 (Azure OpenAI, `IChatClient`) jest punktem wyjścia dla asystenta. Ustalenia na później:
  - porady bezpieczeństwa (112, bezpieczniki, gaz) zawsze z deterministycznych reguł (FR-021), nie z modelu; AI tylko pomaga opisać problem i proponuje kategorię;
  - gateway wejściowy (ograniczenie do tematu zgłoszeń, ochrona przed prompt injection) i wyjściowy (ton, konwencja, brak treści obraźliwych) — rozważane: LiteLLM jako gateway, Langfuse do monitorowania zapytań; prostsza alternatywa w obecnym stacku: Azure OpenAI (content filters + Prompt Shields), Microsoft.Extensions.AI (`IChatClient`), Langfuse Cloud przez OpenTelemetry lub Application Insights;
  - RODO: opisy zgłoszeń mogą zawierać dane osobowe — model w regionie UE, bez trenowania na danych.
- **Brak asystenta głosowego (telefonicznego)** — kolejne fazy, po asystencie AI. Mieszkaniec dzwoni na zwykły numer telefonu; **Twilio** odbiera połączenie z sieci telefonicznej (PSTN) i przekazuje strumień głosu do **ElevenLabs**, który zamienia mowę na tekst i komunikuje się z naszym backendem. W backendzie agent otrzymuje komunikat i wykonuje akcje, np. zakłada zgłoszenie, sprawdza status zgłoszenia, informuje dzwoniącego o stanie sprawy. Odpowiedź wraca do ElevenLabs, który zamienia tekst na mowę i odpowiada dzwoniącemu. Przejmuje kanał telefoniczny, który dziś obsługuje administracja (FR-013). Ustalenia na później:
  - identyfikacja dzwoniącego: numer telefonu przypisany do konta plus dodatkowe potwierdzenie (np. PIN), bo numer można podszyć; bez potwierdzenia agent nie podaje danych zgłoszeń;
  - prywatność (FR-015): dzwoniący słyszy tylko status własnych zgłoszeń i informacje jawne we wspólnocie;
  - bezpieczeństwo: przy zagrożeniu (dym, gaz, zalewanie) agent odczytuje kartę bezpieczeństwa z reguł (FR-021) i kieruje na 112, nigdy nie improwizuje porady; zawsze jest możliwość przełączenia do człowieka;
  - akcje agenta to te same endpointy API co w aplikacji (te same uprawnienia, ślad zmian), a nie bezpośredni dostęp do bazy;
  - RODO: informacja o nagrywaniu i przetwarzaniu rozmowy, region UE, retencja nagrań i transkrypcji;
  - koszty: minuty Twilio plus ElevenLabs (zamiana mowy na tekst i z powrotem) plus model — do oszacowania przed decyzją; orientacyjnie (2026-11): ElevenLabs Agents ok. $0,08/min, Twilio PL ok. $0,01/min przychodzące + ok. $4/mies. za numer;
  - linki: [Twilio Voice – cennik PL](https://www.twilio.com/en-us/voice/pricing/pl), [Twilio Speech Recognition](https://www.twilio.com/en-us/speech-recognition).

## Open Questions

1. **Insight vs status quo** — dlaczego gotowe oprogramowanie dla wspólnot nie rozwiązuje tego problemu? (drugi akapit ## Vision & Problem Statement pozostaje TODO) — Owner: użytkownik. Block: nie (warto wiedzieć przed decyzją build-vs-buy).
2. **Kiedy zgłoszenie dostaje „Do oceny”, skoro kategoria ma domyślny priorytet?** — Jeżeli każda kategoria z listy ma domyślny priorytet, reguła „wyższy wygrywa” zawsze da wynik i „Do oceny” nigdy nie wystąpi. Do ustalenia: czy kategoria jest obowiązkowa, czy istnieje kategoria bez priorytetu (np. „Inne”) prowadząca do „Do oceny”. — Owner: użytkownik. Block: nie, ale wpływa na regułę domenową i US-01 (kryterium „brak trafień → Do oceny”).
3. **Wartości startowe** — lista kategorii z domyślnymi priorytetami, katalog słów kluczowych, czasy reakcji P1–P4 oraz termin oceny „Do oceny”. — Owner: użytkownik. Block: nie (potrzebne przed wdrożeniem, nie przed PRD).
4. **Zakres anonimizacji (FR-016, poza MVP)** — kto wywołuje anonimizację i co dokładnie zostaje po niej. — Owner: użytkownik. Block: nie (dotyczy wersji po MVP).
5. **Treść kart bezpieczeństwa (FR-021)** — które atrybuty katalogu dostają kartę, jaka jest treść każdej karty i kto ją zatwierdza (odpowiedzialność za poradę: zarządca / wspólnota). — Owner: użytkownik. Block: nie (potrzebne przed wdrożeniem, razem z wartościami startowymi z pkt 3).
