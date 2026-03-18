# Transparentność Polityczna na Poziomie UE: Infrastruktura i Implementacja Techniczna

## Podsumowanie Wykonawcze

**UE ma znacznie lepszą infrastrukturę transparentności niż Polska**, ale pozostają poważne luki. Ten dokument analizuje, co istnieje na poziomie UE i co można osiągnąć za pomocą obecnych narzędzi/AI.

---

## Część 1: Dane o Głosowaniach Instytucji UE

### 1.1 Parlament Europejski - Doskonałe Dane

| Zasób | Co oferuje | URL |
|-------|------------|-----|
| **HowTheyVote.eu** | Pełne rejestry głosowań MEP, wyszukiwanie | howtheyvote.eu |
| **MEPWatch** | Przejrzystość głosowań, śledzenie buntowników | mepwatch.eu |
| **ParlTrack** | Sprawy, głosowania, komitety, eksporty JSON | parltrack.eu |
| **Portal Otwartych Danych UE** | 339 zbiorów danych | data.europarl.europa.eu |
| **Oficjalne Głosowania EP** | Głosowania imienne | europarl.europa.eu/plenary |

**Kluczowe Funkcje**:
- Głosy poszczególnych MEP przy każdym głosowaniu
- Przynależność do grup politycznych
- Podział według kraju
- Dane historyczne (2004-obecnie)
- Śledzenie "buntu" (głosowanie przeciwko linii partyjnej)
- **Dostęp do JSON/API** (ParlTrack)

**Statystyki**:
- 23 157+ głosowań śledzonych
- 1 271 MEP w bazie
- 7 lat aktywnych danych

---

### 1.2 Rada Europejska / Rada UE - Ograniczona Przejrzystość

| Aspekt | Status |
|--------|--------|
| **Głosowanie Większością Kwalifikowaną** | Wyniki publiczne (tak/nie) |
| **Głosy Poszczególnych Ministrów** | NIE ujawniane |
| **Rejestry głosowań według kraju** | Dostępne od grudnia 2009 |
| **Kto jak głosował** | Tylko końcowy wynik |

**Luka**: W przeciwieństwie do PE, nie można zobaczyć:
- Który konkretny minister z każdego kraju głosował
- Szczegółowy podział stanowisk państw członkowskich
- Indywidualne uzasadnienie głosowania kraju

**URL**: https://www.consilium.europa.eu/en/documents-publications/public-register/votes/

---

### 1.3 Komisja Europejska - Protokoły Spotkań

- Ogólne podejście publiczne
- Stanowiska poszczególnych komisarzy niesledzone
- Oceny skutków publikowane

---

## Część 2: Przejrzystość Lobbowania w UE

### 2.1 Rejestr Przejrzystości UE (OBOWIĄZKOWY od września 2025)

| Metryka | Wartość |
|---------|---------|
| Zarejestrowane organizacje | 13 000+ |
| Co pokazuje | Kto lobbyuje, ile, na jakie kwestie |
| Obowiązkowy | TAK (od września 2025) |
| Dane finansowe | Roczny budżet na lobbing |
| Śledzone kwestie | Obszary polityki |

**Co jest Wymagane**:
- Nazwa i adres organizacji
- Obszar działalności
- Liczba pracowników ds. affairs UE
- Klienci (jeśli pośrednik)
- Budżet/obrót
- Interesy polityczne

**Luki**:
- Brak wymogu ujawniania konkretnych spotkań
- Problemy z jakością (samoocena)
- 12% aktywności poza-UE powiązanych z zarejestrowanymi lobbystami

**URL**: https://transparency-register.europa.eu

---

### 2.2 Konflikty Interesów MEP

| Dane | Status |
|------|--------|
| Deklaracja Interesów Finansowych | Wymagana na początku mandatu |
| Aktywności poza-parlamentarne | Raportowane (1 678 razem) |
| Przedziały dochodów | Publiczne (ale niejasne: 1-5 tys. EUR) |
| Znalezione problemy | 20% niedokładnych/niekompletnych |

**Problemy Zidentyfikowane przez Transparency International UE**:
- 42% zatrudnienia poza brakuje kluczowych informacji
- Brak listy zakazanych aktywności o wysokim ryzyku konfliktu interesów
- Do 36 MEP potencjalnie czerpiących nieujawnione dochody
- Tylko 12% powiązanych z rejestrem lobbystów

**URL**: https://www.europarl.europa.eu/meps/en/{MEP_ID}/declarations

---

## Część 3: Finansowanie Partii Politycznych w UE

### 3.1 Partie na Poziomie UE

| Autorytet | Co Śledzą |
|-----------|-----------|
| **APPF** (Urząd ds. Partii Politycznych UE) | Finansowanie UE partii/fundacji |
| **Listy darowizn** | Wszystkie darowizny powyżej 12 000 EUR ujawnione |
| **Roczne sprawozdania** | Pełne sprawozdania finansowe |

**Finansowanie UE Partii (2025)**:
- Łącznie przydzielone: ponad 567 mln EUR rocznie
- Najwięksi beneficjenci: EPP, S&D, Renew, Zieloni/EFA

**URL**: https://www.appf.europa.eu

---

### 3.2 Luka w Przejrzystości (Dziennikarstwo Śledcze)

**Kluczowe Ustalenie** (Follow The Money, 2024):
- **74% darowizn politycznych w UE ukrytych przed opinią publiczną**
- Prawie 1 mld EUR darowizn w latach 2019-2022
- Niemcy i Francja najgorsi
- Kraje Europy Środkowo-Wschodniej bardziej przejrzyste

**Co Jest Ukryte**:
- Tożsamość darczyńców poniżej progu
- Mechanizmy finansowania transgranicznego
- "Charytatywne" pojazdy wykorzystywane do darowizn politycznych

**Źródło**: https://www.ftm.eu/transparency-gap

---

## Część 4: Co Można Osiągnąć na Poziomie UE (AI/Narzędzia)

### 4.1 Detector Anomalii Głosowania - NAJŁATWIEJSZE

**Dostępność Danych**: Doskonała

| Komponent | Narzędzie |
|-----------|-----------|
| Dane głosowań | ParlTrack JSON API |
| Klasteryzacja | sklearn |
| Wizualizacja | D3.js |

**Co Już Istnieje**:
- HowTheyVote już to robi!
- MEPWatch pokazuje "buntowników"
- Śledzi odchylenia od linii partyjnej

**Dodatkowa Wartość**: Można dodać:
- Analiza koalicji między krajami
- Zmiany wzorców głosowania w czasie
- Klasteryzacja tematyczna

---

### 4.2 Śled Powiązań MEP-Lobbysta

**Dostępność Danych**: Częściowa (istnieją luki)

| Komponent | Narzędzie |
|-----------|-----------|
| Deklaracje MEP | europarl.europa.eu/meps |
| Rejestr lobbystów | transparency-register.europa.eu |
| Krzyżowe odniesienie | Ręczne + NLP |

**Co Można Zbudować**:
- Powiązanie aktywności poza-UE MEP z zarejestrowanymi organizacjami lobbyjnymi
- Śledzenie głosowań w sprawach, gdzie istnieją konflikty
- Oznaczanie potencjalnych drzwi obrotowych (byli MEP -> lobbing)

**Obecna Luka**: Tylko 12% aktywności MEP powiązanych z rejestrem lobbystów

---

### 4.3 Korelacja Pieniądze-Głosy w UE

**Wyzwanie**: Bardziej złożone niż USA/Polska, ponieważ:
- Zaangażowane wiele instytucji
- Rejestr lobbystów brakuje danych o spotkaniach
- Głosowania Rady są nieprzejrzyste

**Co Można Zrobić**:
- Głosowania PE + stanowiska rejestru lobbystów (do konkretnych spraw)
- Mapowanie organizacji wspierających/przeciwnych ustawom do wzorców głosowania MEP
- Śledzenie trilogów Komisji PE i Rady

**Narzędzia**:
- Model MapLight mógłby się Apply do EP
- Stanowiska lobbystów z konsultacji publicznych
- Poprawki komisji EP

---

### 4.4 Sieć Finansowania Partii UE

**Źródła Danych**:
- Darowizny APPF (powyżej 12 tys. EUR publiczne)
- Fundacje partii UE
- Finansowanie partii krajowych (różni się według kraju)

**Co Można Zbudować**:
- Sieć przepływów finansowania partii UE
- Powiązanie darowizn ze stanowiskami politycznymi
- Śledzenie darowizn transgranicznych

**Wyzwanie**: 74% ukrytych = poważna luka

---

## Część 5: Implementacja Techniczna

### 5.1 Źródła Danych UE (Gotowe do Użycia)

| Źródło | Typ | Format | URL |
|--------|-----|--------|-----|
| ParlTrack | Głosowania, sprawy | JSON | parltrack.eu |
| HowTheyVote | Głosowania MEP | CSV | github.com/HowTheyVote/data |
| Otwarte Dane UE | 339 zbiorów | Różne | data.europarl.europa.eu |
| Rejestr Przejrzystości | Lobyści | API | transparency-register.europa.eu |
| Głosowania Rady | Wyniki legislacyjne | HTML | consilium.europa.eu |

---

## Część 6: Porównanie Polska vs UE

| Wymiar | Polska | UE (PE) | UE (Rada) |
|--------|--------|---------|-----------|
| **Głosy indywidualne** | Tak | Tak (doskonale) | Nie |
| **Deklaracje majątkowe** | Odręczne, słabe | Niejasne zakresy | ND |
| **Rejestr lobbystów** | Brak | Obowiązkowy | Obowiązkowy |
| **Finansowanie partii** | Częściowe | Tak (poziom UE) | ND |
| **Dostęp do API** | Częściowy | Dobry | Ograniczony |
| **Jakość danych** | Niska | Wysoka | Średnia |

---

## Część 7: Projekty Priorytetowe

### Priorytet 1: Połącz Dane Głosowań PE z Rejestrem Lobbystów
- **Dlaczego**: Najbardziej realna luka
- **Dane**: Oba publicznie dostępne
- **Wysiłek**: Średni
- **Wartość**: Wysoka - pokazuje pieniądze->wpływ->głosy

### Priorytet 2: Popraw Analizę Deklaracji MEP
- **Dlaczego**: 20% niekompletnych, 12% powiązanych z rejestrem lobbystów
- **Dane**: Dostępne ale nieustrukturyzowane
- **Wysiłek**: Średni (OCR + NLP)
- **Wartość**: Wysoka

### Priorytet 3: Rzecznictwo dla Przejrzystości Głosowania Rady
- **Dlaczego**: Stanowiska indywidualnych ministrów ukryte
- **Dane**: Wymagałoby zmian instytucjonalnych
- **Wysiłek**: ND (rzecznictwo)
- **Wartość**: Wysoka

### Priorytet 4: Sieć Finansowania Partii UE
- **Dlaczego**: 74% ukrytych
- **Dane**: Częściowe z APPF
- **Wysiłek**: Wysoki
- **Wartość**: Średnia

---

## Część 8: Zasoby

### Niezbędne Źródła Danych UE
| Źródło | URL |
|--------|-----|
| ParlTrack (głosowania/sprawy) | https://parltrack.eu |
| HowTheyVote | https://howtheyvote.eu |
| MEPWatch | https://mepwatch.eu |
| Portal Otwartych Danych UE | https://data.europarl.europa.eu |
| Rejestr Przejrzystości | https://transparency-register.europa.eu |
| Wyniki Głosowań Rady | https://consilium.europa.eu |
| APPF (Finansowanie Partii) | https://www.appf.europa.eu |

### Dziennikarstwo Śledcze
| Źródło | Fokus |
|--------|-------|
| Follow The Money (FTM) | Śledztwo o finansowaniu partii UE |
| VSquare | Przepływy pieniędzy Europa Wschodnia |
| Transparency International UE | Analiza ujawniania przez MEP |

### Stack Techniczny
| Komponent | Narzędzie |
|-----------|-----------|
| Pipeline danych głosowań | Scraper ParlTrack |
| NLP dla deklaracji | GPT-4o / Claude |
| Baza grafowa | Neo4j |
| Wizualizacja | D3.js / Cytoscape |
| OCR (jeśli potrzeba) | Transkribus |

---

## Podsumowanie: UE vs Polska

| Aspekt | Polska | UE (PE) | UE (Rada) |
|--------|--------|---------|-----------|
| **Można śledzić głosy indywidualne?** | Tak | Tak | Nie |
| **Przejrzystość lobbowania?** | Brak | Obowiązkowy | Obowiązkowy |
| **Deklaracje majątkowe?** | Słaba jakość | Niejasne zakresy | ND |
| **Łatwo budować narzędzia?** | Średnio | Łatwo | Trudno |

**Kluczowy Wniosek**: **Parlament Europejski jest w rzeczywistości łatwiejszy do analizy niż parlamenty narodowe**, ponieważ:
1. Lepsze dane ustrukturyzowane
2. Dostępne JSON API
3. Rejestr lobbystów obowiązkowy
4. Istnieją aktywne narzędzia społeczeństwa obywatelskiego

**Główne luki w UE**:
- Przejrzystość Rady (głosy indywidualnych ministrów)
- Jakość deklaracji MEP (niejasne, niekompletne)
- Przejrzystość finansowania partii (74% ukryte)

---

*Aktualizacja: Marzec 2026*
