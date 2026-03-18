# Narzędzia Transparentności Politycznej: Polska vs USA i Przewodnik Implementacji

## Szybka Odpowiedź na Pierwotne Pytanie

**Nie, nie można znaleźć baz danych pokazujących, jak miasta/gminy głosowały nad konkretnymi ustawami.**

W Polsce **miasta i gminy nie głosują nad ustawami**. Polska jest demokracją parlamentarną — tylko Sejm (460 wybranych posłów) głosuje nad ustawami. Można śledzić, jak głosowali poszczególni posłowie — i są do tego bazy danych.

---

## Część 1: Obecna Infrastruktura Transparentności w Polsce

### Co jest dostępne w Polsce

| Źródło danych | Co zawiera | URL |
|---------------|------------|-----|
| **Oświadczenia majątkowe** | Deklaracje majątkowe posłów i urzędników (wiele odręcznych, niespójne) | sejm.gov.pl, cba.gov.pl |
| **Jawne Partie** | Darowizny i umowy partii | jawnepartie.pl |
| **Jawne Finanse** | Beneficjenci Funduszu Sprawiedliwości | jawnefinanse.pl |
| **KRS** | Rejestracja NGO + podstawowe dane finansowe | rejestrkrs.pl |
| **Sprawozdania OPP** | Sprawozdania organizacji pożytku publicznego | sprawozdaniaopp.niw.gov.pl |
| **Głosowania Sejmu** | Głosy posłów indywidualnie | sejm-stats.pl, SejmTracker |

**Problem**: Dane są fragmentaryczne, w niespójnych formatach, brak automatycznego łączenia, brak śledzenia powiązań rodzinnych.

### Czego brakuje w Polsce

| Luka | Status w Polsce | Co trzeba zbudować |
|------|-----------------|---------------------|
| **Ścieżka NGO → Polityk** | Brak śledzenia | Mapowanie finansowania NGO → politycy |
| **Rejestr lobbingu** | Słaby/brak danych | Rejestracja kto lobbyuje kogo |
| **Konflikt interesów** | Fragmentaryczne | Ujednolicone porównanie |
| **Przejrzystość własności mediów** | Częściowe | Kto jest właścicielem mediów |
| **Własność beneficjentów** | Wdrażane (2026+) | Śledzenie rzeczywistych właścicieli |

---

## Część 2: Śledzenie Pieniędzy Politycznych w USA (Czego brakuje Polsce)

### Narzędzia, które istnieją w USA

| Narzędzie | Co robi | URL |
|-----------|---------|-----|
| **OpenSecrets** | Darowizny kampanijne, lobbingu, ciemne pieniądze, śledzenie AIPAC | opensecrets.org |
| **MapLight** | **Koreluje głosy z pieniędzmi** — pokazuje kto finansował każdą ustawę | maplight.org |
| **LobbyView** | Wizualizacja sieci grup interesu | lobbyview.org |
| **FEC** | Surowe dane Federalnej Komisji Wyborczej | fec.gov |

### Kluczowa innowacja USA: Korelacja Głosów i Pieniędzy

Dane MapLight o stanowiskach wobec ustaw śledzą:
- Które organizacje popierają lub przeciwstawiają się każdej ustawie
- Ich źródła finansowania
- Jak to koreluje z głosowaniem legislatorów

To jest model, którego Polska jeszcze nie ma.

---

## Część 3: Co Możliwe z Obecnymi AI/Narzędziami

### 1. Detector Anomalii Wzorców Głosowania

**Wysiłek: NISKI** | **Dane: Już dostępne**

```
Wejście: API głosowań Sejmu → ML clustering → Wyjście: Odbiegający posłowie
```

| Komponent | Narzędzie |
|-----------|-----------|
| Dane głosowań | sejm-votes (GitHub: michalpazur/sejm-votes) |
| Klasteryzacja | sklearn, podstawowe statystyki |
| Wizualizacja | D3.js / Python Plotly |

**Co robi:**
- Klastruje posłów według wzorców głosowania
- Oznacza tych, którzy odbiegają od linii partyjnej
- Pokazuje "oczekiwane" vs rzeczywiste głosowanie
- Wyróżnia zmieniających stanowisko przy kluczowych głosowaniach

---

### 2. Parser Oświadczeń Majątkowych z AI

**Wysiłek: ŚREDNI** | **Dane: Skany z sejm.gov.pl**

Wiele deklaracji majątkowych jest odręcznych. AI to rozwiązuje:

| Komponent | Narzędzie |
|-----------|-----------|
| OCR | **Transkribus** (specjalnie trenowany dla polskiego pisma) |
| Parsowanie | GPT-4o / Claude (ekstrakcja strukturyzowanych danych z tekstu) |
| Przechowywanie | PostgreSQL + pgvector (wyszukiwanie podobieństw) |

**Co robi:**
- Wgraj skan PDF → strukturalny JSON
- Ekstrahuj: nieruchomości, oszczędności, akcje, firmy
- Śledź zmiany majątku rok do roku
- Oznacz nagłe wzrosty majątku

**Koszt**: ~50-100€/mc za kredyty API + Transkribus

---

### 3. Graf Sieci Firm KRS

**Wysiłek: NISKI** | **Dane: Dostępne dumpy KRS**

| Komponent | Narzędzie |
|-----------|-----------|
| Dane KRS | Synteza.pro (komercyjne) lub scrape z gov.pl |
| Grafowa baza danych | Neo4j / PostgreSQL |
| Wizualizacja | Cytoscape.js, Gephi, D3 |

**Co robi:**
- Mapuj: Osoba → Firma (właściciel/zarząd) → Firma → Osoba
- Znajdź: Wszystkie firmy powiązane z posłem X
- Wykryj: Sieci spółek-krzeseł, cykliczne własności

**Istniejący przykład**: Offensive OSINT (offensiveosint.io) już robi wizualizację "Spółki posłów"

---

### 4. Dashboard Porównawczy

**Wysiłek: ŚREDNI** | **Dane: Już publiczne, tylko niepołączone**

| Komponent | Narzędzie |
|-----------|-----------|
| Głosowania posłów | API Sejmu |
| Deklaracje majątkowe | Twój pipeline OCR |
| Darowizny partyjne | API Jawne Partie |
| Własność firm | KRS |
| Kontrakty rządowe | BZP / GUS dane przetargowe |

**Co robi:**
- Szukaj: "Pokaż wszystkie firmy należące do posłów, którzy głosowali TAK nad ustawą X"
- Połącz: Ta NGO → otrzymała fundusze UE → powiązana z posłem Y → głosowała Z
- Alert: Firma posła właśnie wygrała kontrakt rządowy

---

### 5. Analiza Mediów i Przemówień

**Wysiłek: NISKI-ŚREDNI** | **Dane: Transmisje Sejmu**

| Komponent | Narzędzie |
|-----------|-----------|
| Transmisje | sejm.gov.pl (wypowiedzi) |
| NLP | GPT-4o / Claude (podsumowania, sentyment) |
| Modelowanie tematów | BERTopic |

**Co robi:**
- Podsumowuje przemówienia posłów
- Śledzi kto mówi o jakich tematach
- Wykrywa zmianę retoryki w czasie

---

## Część 4: Rekomendowana Architektura

```
┌─────────────────────────────────────────────────────────────┐
│                   POLITYK / OSOBA PUBLICZNA                 │
├─────────────────────────────────────────────────────────────┤
│  Rejestr głosowań   │  Oświadczenia   │  Sieć rodzinna     │
│  (Sejm)            │  Majątkowe      │  (ręczne + KRS)    │
└────────┬─────────────┴────────┬─────────┴────────┬───────────┘
         │                    │                  │
         ▼                    ▼                  ▼
┌─────────────────────────────────────────────────────────────┐
│               CENTRALNA BAZA KORELACJI                     │
│  - Łącz: Poseł → Firmy → NGO → Finansowanie → Głosowania  │
│  - Mapowanie relacji rodzinnych                            │
│  - Wykrywanie anomalii (nagły majątek, odbiegające głosy) │
└────────────────────┬──────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────┐
│                   PUBLICZNY DASHBOARD                       │
│  - Szukaj według posła, NGO, firmy                         │
│  - Wizualizacja "przepływu wpływów"                       │
│  - Alerty o anomaliach                                     │
└─────────────────────────────────────────────────────────────┘
```

---

## Część 5: Szybki Stack Wygranej (MVP)

### Faza 1 (Tydzień 1-2): Wykrywanie Anomalii Głosowania
1. Scrape głosowań Sejmu → Baza SQLite
2. Oblicz linię bazową partyjną
3. Oznacz posłów odbiegających >20% od partii
4. Prosty web dashboard (Streamlit / Flask)

**Wynik**: "Którzy posłowie odeszli od linii swojej partii?"

### Faza 2 (Tydzień 3-4): Powiązania Firm
1. Dodaj dane firm KRS dla każdego posła
2. Mapuj firmy → kontrakty rządowe
3. Dodaj darowizny z Jawne Partie

**Wynik**: "Kto ma firmy wygrywające kontrakty?"

### Faza 3 (Miesiąc 2): Śledzenie Majątku
1. Pipeline OCR dla oświadczeń majątkowych
2. Śledzenie majątku rok do roku
3. Wykrywanie anomalii (nagły wzrost majątku)

**Wynik**: "Kto niespodziewanie się wzbogacił?"

---

## Częście 6: Istniejące Narzędzia do Połączenia

| Czego szukasz | Istniejące polskie narzędzie |
|---------------|------------------------------|
| Rejestry głosowań posłów | sejm-stats.pl |
| Darowizny partyjne | Jawne Partie |
| Fundusz Sprawiedliwości | Jawne Finanse |
| Rejestr firm | Synteza.pro, Transparent Data |
| Finanse NGO | Sprawozdania OPP |
| Orzeczenia sądów | orzeczenia.ms.gov.pl |

---

## Część 7: Co NIE jest Łatwe (Wymaga Zmian Instytucjonalnych)

| Luka | Dlaczego |
|------|----------|
| Śledzenie lobbingu w czasie rzeczywistym | Brak obowiązkowego rejestru |
| Własność beneficjentów (do 2026+) | Jeszcze nie wymagane |
| Automatyczne łączenie rodzin | Brak publicznej bazy rodzin |
| Przepływ pieniędzy NGO → Polityk | Fragmentaryczne, brak jednego źródła |

---

## Podsumowanie

Polska ma **trochę** danych, ale są one fragmentaryczne i niepołączone. Systemy USA (szczególnie MapLight) pokazują co jest *możliwe* — ale wymagałoby znacznego inżynierowania danych do odtworzenia w Polsce ze względu na luki prawne/instytucjonalne.

**Zacznij od detektora anomalii głosowania** — to najłatwiejszy pierwszy projekt:
1. Dane są czyste (API Sejmu)
2. Algorytm jest prosty (odległość od centroidy partyjnej)
3. Wartość jest natychmiastowa
4. Brak problemów prawnych (dane publiczne)

---

## Zasoby

### Polskie Źródła Danych
- Głosowania Sejmu: https://sejm-stats.pl
- Oświadczenia majątkowe: https://sejm.gov.pl/sejm10.nsf/page.xsp/formularzeosw
- Darowizny partyjne: https://jawnepartie.pl
- Dane firm KRS: https://synteza.biz
- Sprawozdania NGO: https://sprawozdaniaopp.niw.gov.pl

### USA (inspiracja)
- OpenSecrets: https://opensecrets.org
- MapLight: https://maplight.org
- LobbyView: https://lobbyview.org

### Narzędzia Techniczne
- Transkribus (OCR dla pisma): https://transkribus.org
- Neo4j (baza grafowa): https://neo4j.com
- D3.js (wizualizacja): https://d3js.org

---

*Aktualizacja: Marzec 2026*
