# PRD — Strona z zaproszeniem na randkę

## 1. Cel projektu

Jednorazowa, mobilna strona internetowa służąca do zaproszenia Natalii na randkę. Strona jest humorystyczna — przycisk odmowy celowo utrudnia użytkownikowi kliknięcie, by zachęcić go do zaakceptowania zaproszenia.

---

## 2. Odbiorcy

- Urządzenia: głównie smartfony (mobile-first), responsywna na desktopie
- Jedyna osoba docelowa: Natalia

---

## 3. Widoki i przepływ aplikacji

### 3.1 Widok główny (`/`)

**Treść:**

> Hej Natalia, co powiesz na randkę w niedzielę?

**Elementy UI:**
- Wycentrowany tekst zaproszenia (duże, czytelne na mobile)
- Dwa przyciski na dole ekranu:
  - `Zgadzam się` — przycisk akceptacji (stały, wyróżniony wizualnie)
  - `Sorry, ale nie mogę.` — przycisk odmowy (zachowanie opisane w sekcji 3.3)

---

### 3.2 Widok potwierdzenia (po kliknięciu „Zgadzam się")

**Wyzwalacz:** kliknięcie przycisku `Zgadzam się`

**Zachowanie:** przejście do nowego widoku (pełna strona lub overlay) z komunikatem:

> No to jakoś tak 17:00, 18:00, 19:00, bądźmy w kontakcie.

**Elementy UI:**
- Tekst potwierdzenia wycentrowany na ekranie
- Opcjonalnie: animacja (np. serduszka, konfetti) — mile widziane

---

### 3.3 Zachowanie przycisku „Sorry, ale nie mogę." — maszyna stanów

Przycisk odmowy ma cztery stany zależne od liczby kliknięć:

| Stan | Kliknięcie # | Zachowanie |
|------|-------------|------------|
| **0** | — | Przycisk widoczny, normalny |
| **1** | 1. | Przycisk „trzęsie się" (animacja shake). Pojawia się dymek/tooltip: *„Jesteś pewna?"* |
| **2** | 2. | Dymek znika, pojawia się komunikat przy przycisku: *„Przemyśl to."* |
| **3+** | 3. i każde kolejne | Przy każdym kliknięciu przycisk teleportuje się w losowe miejsce na ekranie (ucieka). Animacja ruchu. |

**Szczegóły techniczne zachowania:**

- **Animacja shake (stan 1):** krótka animacja CSS `keyframes` — szybkie przesunięcia w osi X, czas ~500 ms.
- **Tooltip/dymek:** stylizowany dymek (CSS `::after` lub element absolutny), znika po ~2 s lub przy ponownym kliknięciu.
- **Komunikat „Przemyśl to." (stan 2):** wyświetlany jako tekst pod/obok przycisku lub w tooltipie.
- **Uciekający przycisk (stan 3+):** pozycja absolutna / `fixed`, losowe `top` i `left` wyliczane w JS tak, by przycisk pozostał w granicach widocznego ekranu (`window.innerWidth`, `window.innerHeight`). Płynne przejście CSS (`transition: top 0.3s, left 0.3s`).

---

## 4. Stos technologiczny

| Warstwa | Technologia |
|---------|-------------|
| Markup | HTML5 |
| Styling | CSS3 (Tailwind CSS lub vanilla CSS) |
| Logika | Vanilla JavaScript (bez frameworków) |
| Hosting | Dowolny static hosting (np. GitHub Pages, Vercel, Netlify) |

> Brak backendu — wszystko działa client-side.

---

## 5. Wymagania niefunkcjonalne

- **Mobile-first:** viewport `<meta name="viewport" content="width=device-width, initial-scale=1">`, wszystkie interakcje dotykowe działają poprawnie
- **Szybkość:** strona ładuje się w < 2 s (brak zewnętrznych zależności poza ewentualnym fontem)
- **Brak persistencji:** stan przycisku odmowy resetuje się przy odświeżeniu strony
- **Dostępność:** przyciski mają odpowiedni rozmiar dotyku (min. 44×44 px)

---

## 6. Estetyka i UX

- Styl: przyjemny, ciepły, lekko romantyczny — pastelowe kolory, zaokrąglone przyciski
- Czcionka: czytelna, lekka (np. Google Fonts: *Lato*, *Nunito*, *Poppins*)
- Przycisk `Zgadzam się`: wyróżniony kolorem (np. różowy lub czerwony)
- Przycisk odmowy: szary lub stonowany, wizualnie mniej zachęcający
- Animacje: płynne, niezbyt przesadzone — mają bawić, nie irytować

---

## 7. Poza zakresem (out of scope)

- Logowanie, konta użytkowników
- Wysyłanie powiadomień / e-maili
- Wielojęzyczność
- Wersja desktopowa jako priorytet
- Panel administracyjny

---

## 8. Kryteria akceptacji

- [ ] Strona poprawnie wyświetla się na smartfonie (iOS Safari, Android Chrome)
- [ ] Kliknięcie `Zgadzam się` pokazuje ekran potwierdzenia z właściwym tekstem
- [ ] Pierwsze kliknięcie odmowy wywołuje animację shake i dymek „Jesteś pewna?"
- [ ] Drugie kliknięcie odmowy pokazuje „Przemyśl to."
- [ ] Trzecie (i każde kolejne) kliknięcie odmowy powoduje ucieczkę przycisku w losowe miejsce
- [ ] Uciekający przycisk nie wychodzi poza krawędzie ekranu
