# Połączenie MySQL z phpMyAdmin w Dockerze

## Opis
W tej notatce znajdziesz instrukcję dotyczącą połączenia bazy danych MySQL z phpMyAdmin w Dockerze, wykorzystując flagę `--link` oraz rozwiązanie problemu z logowaniem do phpMyAdmin.

## Tworzenie kontenerów MySQL i phpMyAdmin

1. **Uruchomienie kontenera MySQL**
 
 *docker run --name mysql-db -e MYSQL_ROOT_PASSWORD=root -d mysql:latest*

--name mysql-db – nadaje nazwę kontenerowi MySQL

-e MYSQL_ROOT_PASSWORD=root – ustawia hasło dla użytkownika root

-d – uruchamia kontener w tle

mysql:latest – pobiera i uruchamia najnowszą wersję MySQL

2. **Uruchomienie kontenera phpMyAdmin połączonego z MySQL**

*docker run --name my-phpmyadmin --link mysql-db -p 8080:80 -d phpmyadmin/phpmyadmin*

--name my-phpmyadmin – nadaje nazwę kontenerowi phpMyAdmin

--link mysql-db – łączy kontener phpMyAdmin z MySQL

-p 8080:80 – mapuje port 8080 lokalnego komputera na port 80 kontenera

-d phpmyadmin/phpmyadmin – uruchamia kontener phpMyAdmin

Po wykonaniu tych kroków phpMyAdmin będzie dostępny pod adresem http://localhost:8080.

## Problem z logowaniem do phpMyAdmin
Podczas logowania do phpMyAdmin może pojawić się problem z dostępem do MySQL. Zwykle wynika to z ograniczeń dostępu użytkownika MySQL.

**Rozwiązanie: Utworzenie nowego użytkownika**
1. Zaloguj się do kontenera MySQL

*docker exec -it mysql-db mysql -u root -p*
(Podaj hasło ustawione przy tworzeniu kontenera, np. root).


2. Utwórz nowego użytkownika z dostępem do wszystkich hostów

*CREATE USER 'admin'@'%' IDENTIFIED BY 'adminpassword';*

*GRANT ALL PRIVILEGES ON *.* TO 'admin'@'%' WITH GRANT OPTION;*

*FLUSH PRIVILEGES;*

'admin'@'%' – oznacza, że użytkownik może łączyć się z dowolnego hosta

'adminpassword' – hasło użytkownika (możesz ustawić własne)

GRANT ALL PRIVILEGES – przyznaje wszystkie uprawnienia


3. Teraz możesz zalogować się w phpMyAdmin

Użytkownik: admin

Hasło: adminpassword

Serwer: mysql-db (lub localhost jeśli phpMyAdmin działa w tym samym kontenerze)

Teraz phpMyAdmin powinien poprawnie łączyć się z bazą MySQL.

## Podsumowanie

Użycie --link pozwala na komunikację między kontenerami MySQL i phpMyAdmin.

Jeśli napotkasz problem z logowaniem, utwórz użytkownika MySQL z dostępem z dowolnego hosta ('%').

phpMyAdmin jest dostępny pod http://localhost:8080.

To podstawowa konfiguracja – w produkcji warto używać docker-compose i ograniczać dostęp do bazy danych tylko do konkretnych adresów IP.
