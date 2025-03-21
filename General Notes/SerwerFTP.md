## Serwer FTP
służy on do wymiany danych pomiędzy użytkownikami w sieci. (W Fedorze FTP = vsftpd).
FTP działa na porcie 21.

ftp (nazwa sieci) - łączenie.

get (nazwa pliku) - wyciąganie pliku.
put (nazwa pliku) - przesyłanie pliku.
lcd /home/user - pozwala poruszać się po lokalnym środowisku jeżeli jesteśmy podłączeni do serwera FTP.

## Linux Fedora komendy

- systemctl ... - zarządzanie usługami, można np włączyć ftp.
- journalctl ... - logi systemowe.
- useradd (nazwa) - dodaje użytkownika.
- usermod -aG (nazwa grupy) - dodaje użytkownika do grupy (-a powoduje to że użyt. nie traci poprzednich grup).
- passwd (nazwa użytkownika) - zmienia hasło.
- groupadd (nazwa grupy) - tworzy grupę.
- chown (nazwa nowego właściciela) (plik/folder) - zmienia własciciela pliku.
- chmod XYZ (nazwa pliku/folderu) - zmienia uprawnienia (X - własciciela, Y - grupy, Z - wszystkich).
- ls -a - pokazuje zawartość katalogu.
- cat (nazwa pliku) - wyświetla zawartość pliku.
- nano (nazwa pliku) - edytowanie pliku.
- touch (nazwa) - tworzy plik.
- cp (nazwa pliku/folderu) (docelowe miejsce gdzie ma zostać skopiowane) - kopiowanie.
- mv (nazwa pliku/folderu) (docelowe miejsce gdzie ma zostać przeniesione) - przenoszenie (ewentualnie gdy użyjemy mv test.txt test2.txt, zmieni nam nazwe).

## Pojęcia ogólne

shell - powłoka, która jest pośrednikiem w komunikacji Użytkownik - System operacyjny.

bash - pozwala na wywoływanie skryptów (np. echo "Hello Bash").

## Dodatkowe spostrzeżenia
Problemem w stworzeniu serwera FTP na Fedorze, okazał się firewall ze strony głównie Fedory. Na Windowsie wystarczyło dodać porty 20-21 jako otwarte + zakres 1024-1048 aby
wszystko działało. Również wazna była konfiguracja (nano /etc/vsftpd/vsftpd.conf).

Fedora używa firewalld, należy popracować nad konfiguracją przez terminal tej usługi.

Przy konfiguracji serwera Ftp czy Apache, warto zwrócić uwagę na "sestatus / setenforce 0" i firewall, one zazwyczaj blokują działanie aplikacji.

## Połączenie (aktywne/pasywne):

## Aktywne 
Klient inicjuje połączenie na port 21 (kontrolne).

Serwer odpowiada i próbuje nawiązać połączenie zwrotne do klienta na losowy port >1024.

Problem: Jeśli klient jest za NAT-em, firewallem lub VPN-em, połączenie zwrotne może zostać zablokowane.


## Pasywne 
Klient inicjuje połączenie na port 21 (kontrolne).

Klient prosi serwer o port do przesyłania danych.

Serwer zwraca losowy port >1024, na który klient się łączy.

Połączenie wychodzące zawsze inicjuje klient, więc działa lepiej przez NAT/firewalle.

