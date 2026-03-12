# gbTracker

`gbTracker` to windowsowa aplikacja tray napisana w C# / .NET 8, która działa jako licznik GB i pokazuje, ile internetu zostało na Twoich kontach u dostawców internetu mobilnego.

Projekt jest przygotowany pod przyszłą obsługę kolejnych operatorów, takich jak Plus, a obecnie wspiera konta nju mobile i Orange w następujących trybach logowania:

- `nju (subskrypcja)`
- `nju (abonament, na kartę, internet mobilny)`
- `orange (na kartę)`

## ✨ Najważniejsze funkcje

- licznik GB w trayu Windows
- wspólny tray dla wielu kont i wielu metod logowania
- grupowanie numerów w menu według metody logowania
- główna ikonka pokazująca łączną sumę danych dla zaznaczonych pozycji
- dodatkowe ikonki per numer dla danych krajowych i roamingowych
- opcjonalne kolorowanie tła ikonek na podstawie numeru telefonu
- możliwość zmiany rozmiaru i koloru cyfr w ikonkach
- logowanie przez standardowe okno WinForms
- autostart z Windows
- zapis danych logowania w Windows DPAPI
- logi HTTP z opcją minimalnych logów i maskowania sekretów
- automatyczne sprawdzanie aktualnego `API key` dla `nju (subskrypcja)` przy starcie
- automatyczne sprawdzanie aktualnego `userAgent` przy starcie

## 📱 Obsługiwane typy kont

### `nju (subskrypcja)`

Aplikacja loguje się do API nju, pobiera grupy i produkty, a następnie sumuje pakiety danych dla aktywnych numerów.

### `nju (abonament, na kartę, internet mobilny)`

Aplikacja loguje się przez web flow, pobiera stronę stanu konta i parsuje dostępne pakiety danych.

### `orange (na kartę)`

Aplikacja loguje się przez web flow Orange, a przy pierwszym logowaniu może poprosić o kod SMS i zapisanie urządzenia jako zaufanego. Dane o pakiecie internetu są następnie parsowane z widoku konta.

## 🧰 Wymagania

- Windows 10 lub Windows 11

## 🚀 Uruchomienie

`gbTracker` jest udostępniany jako gotowy skompilowany plik `.exe`.

Aby uruchomić aplikację:

1. pobierz paczkę z plikiem `gbTracker.exe`
2. uruchom `gbTracker.exe`
3. po starcie aplikacja pojawi się w zasobniku systemowym Windows

Przy pierwszym uruchomieniu aplikacja tworzy własny plik konfiguracyjny i pobiera wymagane pliki konfiguracyjne z repozytorium projektu.

## 🖱️ Menu aplikacji

Po kliknięciu prawym przyciskiem na ikonę tray zobaczysz:

- listę numerów pogrupowaną według metody logowania
- `Zaloguj`
- `Logs`
- `Tools`
- `Autostart z Windows`
- `Wyjdz`

### `Zaloguj`

Menu `Zaloguj` zawiera:

- `nju (subskrypcja)`
- `nju (abonament, na kartę, internet mobilny)`
- `orange (na kartę)`

Obie opcje pozwalają dodawać wiele kont.

### `Logs`

Menu `Logs` zawiera:

- `Ogranicz logi do minimum`
- `Ukrywaj sekrety`

Przy pierwszym uruchomieniu obie opcje są domyślnie włączone i zapisywane do configu.

### `Tools`

Menu `Tools` zawiera:

- `Zmien API key dla nju subskrypcja`
- `Wyloguj konto`
- `Wyloguj wszystkie`
- `Koloruj ikonki`
- `Rozmiar cyfr w ikonkach`
- `Rozmiar tla ikonek`
- `Zmien kolor tekstu`

## ⚙️ Konfiguracja

Plik konfiguracyjny jest zapisywany tutaj:

`%AppData%\gbTracker\config.json`

Przykładowe pola:

- `version`
- `refreshIntervalSeconds`
- `hideSecretsInLogs`
- `minimalLogs`
- `colorizeIcons`
- `iconTextSizeOffset`
- `iconBackgroundSizeOffset`
- `iconTextColorArgb`
- `njuSubscriptionApiKey`
- `userAgent`
- `perNumberTrayIconEnabled`
- `releaseCheck`

## 📝 Logi

Logi są zapisywane tutaj:

`%AppData%\gbTracker\logs\`

Przy włączonej opcji `Ogranicz logi do minimum` log zawiera tylko podstawowe informacje, w tym requestowany URI.

Przy wyłączonej opcji minimalnych logów log zawiera rozszerzone szczegóły requestów i odpowiedzi.

Przy włączonej opcji `Ukrywaj sekrety` aplikacja maskuje między innymi:

- hasła
- tokeny
- `API key`
- wybrane pola logowania

## 🔐 Przechowywanie danych logowania

Dane logowania są przechowywane lokalnie z użyciem Windows DPAPI w jednym wspólnym bezpiecznym pliku:

`%AppData%\gbTracker\credentials.sec`

## 🌐 Zdalna konfiguracja

Przy starcie aplikacja sprawdza aktualne wartości:

- `https://raw.githubusercontent.com/pio20pro-arch/gbTracker/main/njuSubscriptionApiKey.json`
- `https://raw.githubusercontent.com/pio20pro-arch/gbTracker/main/user-agent.json`

Jeśli pojawi się nowa wartość, lokalny config jest aktualizowany automatycznie.
