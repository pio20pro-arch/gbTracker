# gbTracker

`gbTracker` to windowsowa aplikacja tray napisana w C# / .NET 8, która pokazuje ile internetu zostało na kontach dostawców internetu mobilnego w Polsce.

Architektura projektu jest przygotowana pod przyszłą obsługę kolejnych operatorów, takich jak Orange czy Plus, ale obecnie aplikacja wspiera wyłącznie konta nju mobile w dwóch trybach logowania:

- `nju (subskrypcja)`
- `nju (abonament, na kartę, internet mobilny)`

## Najważniejsze funkcje

- wspólny tray dla wielu kont i wielu metod logowania
- obsługa wielu kont `subscription`
- obsługa wielu kont `prepaid`
- grupowanie numerów w menu według metody logowania
- główna ikonka pokazująca łączną sumę danych dla zaznaczonych pozycji
- dodatkowe ikonki per numer dla danych krajowych i roamingowych
- opcjonalne kolorowanie tła ikonek na podstawie numeru telefonu
- logowanie przez standardowe okno WinForms
- autostart z Windows
- zapis danych logowania w Windows DPAPI
- logi HTTP z opcją minimalnych logów i maskowania sekretów
- automatyczne sprawdzanie `API key` dla `nju (subskrypcja)` przy starcie
- automatyczne sprawdzanie `userAgent` przy starcie
- pozycja `Donate` w menu
- informacja o wersji w menu oraz sprawdzanie nowych release na GitHubie

## Obsługiwane typy kont

### `nju (subskrypcja)`

Aplikacja loguje się do API nju, pobiera grupy i produkty, a następnie sumuje pakiety danych dla aktywnych numerów.

### `nju (abonament, na kartę, internet mobilny)`

Aplikacja loguje się przez web flow, pobiera stronę stanu konta i parsuje dostępne pakiety danych.

## Wymagania

- Windows 10 lub Windows 11
- .NET SDK 8.0 lub nowszy

## Build i uruchomienie

```powershell
dotnet build ".\VSCODE Workspace.sln"
dotnet run --project ".\gbTracker\gbTracker.csproj"
```

## Menu aplikacji

Po kliknięciu prawym przyciskiem na ikonę tray zobaczysz:

- listę numerów pogrupowaną według metody logowania
- `Zaloguj`
- `Logs`
- `Tools`
- `Autostart z Windows`
- `Wyjdz`
- `Version: 1.0.0`
- `Donate`

### `Zaloguj`

Menu `Zaloguj` zawiera:

- `nju (subskrypcja)`
- `nju (abonament, na kartę, internet mobilny)`

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

## Konfiguracja

Plik konfiguracyjny jest zapisywany tutaj:

`%AppData%\gbTracker\config.json`

Przykładowe pola:

- `version`
- `refreshIntervalSeconds`
- `hideSecretsInLogs`
- `minimalLogs`
- `colorizeIcons`
- `njuSubscriptionApiKey`
- `userAgent`
- `perNumberTrayIconEnabled`
- `releaseCheck`

## Logi

Logi są zapisywane tutaj:

`%AppData%\gbTracker\logs\`

Przy włączonej opcji `Ogranicz logi do minimum` log zawiera tylko podstawowe informacje, w tym requestowany URI.

Przy wyłączonej opcji minimalnych logów log zawiera rozszerzone szczegóły requestów i odpowiedzi.

Przy włączonej opcji `Ukrywaj sekrety` aplikacja maskuje między innymi:

- hasła
- tokeny
- `API key`
- wybrane pola logowania

## Przechowywanie danych logowania

Dane logowania są przechowywane lokalnie z użyciem Windows DPAPI w jednym wspólnym bezpiecznym pliku:

`%AppData%\gbTracker\credentials.sec`

## Zdalna konfiguracja

Przy starcie aplikacja sprawdza aktualne wartości:

- `https://raw.githubusercontent.com/pio20pro-arch/gbTracker/main/njuSubscriptionApiKey.json`
- `https://raw.githubusercontent.com/pio20pro-arch/gbTracker/main/user-agent.json`

Jeśli pojawi się nowa wartość, lokalny config jest aktualizowany automatycznie.

## Donate

Jeśli projekt jest przydatny, możesz wesprzeć go tutaj:

`https://buycoffee.to/gbTracker`

## Wersjonowanie

Aktualna wersja startowa:

`1.0.0`

Aplikacja sprawdza dostępność nowszych release na GitHubie i może oznaczyć to w menu przy wersji.

## Status projektu

Obecnie aplikacja działa z kontami nju mobile. W kolejnych wersjach może zostać rozszerzona o obsługę kolejnych dostawców internetu mobilnego w Polsce bez przebudowy wspólnego shellu tray.
