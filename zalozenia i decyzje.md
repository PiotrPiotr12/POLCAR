I. Założenia

1. Architektura multi-tenant: Korzystamy z jednej wspólnej bazy danych z jednym schematem dla wszystkich użytkowników. Umożliwia to uproszczenie zarządzania danymi i obniżenie kosztów.
2. Rosnąca liczba użytkowników: System musi być skalowalny i obsługiwać szybko rosnącą liczbę użytkowników, potencjalnie aż do dziesiątek tysięcy.
3. Role użytkowników: Użytkownicy mogą być pracownikami lub managerami, przy czym pracownicy są przypisani do managerów w stosunku 100:1.
4. Zarządzanie zadaniami: Każdy pracownik ma przypisane zadania. Managerowie mogą przeglądać zadania swoich podwładnych oraz historię zmian tych zadań.
5. Udostępnianie zadań: Pracownicy mają możliwość udostępniania swoich zadań innym pracownikom, aby ułatwić współpracę.
   
II. Decyzje Architektoniczne

1. Pojedyncza baza danych z jednym schematem:

* Uproszczone zarządzanie: Zarządzanie jedną bazą danych i jednym schematem jest prostsze i bardziej ekonomiczne niż zarządzanie wieloma bazami danych.
* Separacja tenantów: Każdy użytkownik ma przypisany identyfikator tenant (TenantID), co umożliwia logiczne oddzielenie danych różnych tenantów w jednej bazie danych.
  
2. Struktura tabel:

* Tabela Users: Przechowuje informacje o wszystkich użytkownikach (zarówno pracownikach, jak i managerach) oraz ich relacje (kolumna ManagerID określa, który manager zarządza danym pracownikiem).
* Tabela Tasks: Przechowuje zadania, które są przypisane do poszczególnych użytkowników.
* Tabela TaskHistory: Rejestruje historię zmian zadań, co pozwala na audytowanie operacji na zadaniach.
* Tabela TaskShares: Przechowuje informacje o zadaniach udostępnionych innym pracownikom.
  
3. Indeksy:

* Wydajność: Stworzenie indeksów na kluczowych kolumnach tabel, takich jak TaskID, AssignedTo, UserID i ChangeDate, poprawia wydajność operacji odczytu i zapisu.
  
4. Triggery:

* Automatyczny audyt: Trigger na tabeli Tasks automatycznie rejestruje wszelkie zmiany (dodanie, aktualizacja, usunięcie) w tabeli TaskHistory, zapewniając pełną historię operacji na zadaniach.
  
5. Procedury składowane:

* Automatyzacja operacji: Procedury do dodawania użytkowników, zadań, aktualizacji zadań, udostępniania zadań oraz przeglądania zadań przez managerów automatyzują procesy i zapewniają spójność oraz efektywność działania systemu.
  
6. Podsumowanie
   
* System jest zaprojektowany z myślą o skalowalności, aby obsługiwać szybko rosnącą liczbę użytkowników i zadań. Użycie jednej bazy danych z jednym schematem upraszcza zarządzanie i redukuje koszty. Struktura tabel, indeksy oraz triggery są zoptymalizowane, aby zapewnić wysoką wydajność operacyjną, a procedury składowane automatyzują i ułatwiają najważniejsze operacje na danych.
