## Po co nam TCP/IP

Jest potrzebne, aby istniało porozumienie między dystrybutorami różnych serwerów/routerów, czy nawet otwieranie poprzez różne systemy operacyjne pozwala na osiągnięcie takiego samego efektu poprzez zbiór protokołów

## TCP/IP
Jest to zbiór protokołów dzięki którym łączymy się z internetem, W jego skład wchodzą protokoły TCP i UDP (ten drugi jest do lżejszych plików).

Posiadamy 4 warstwy:
Warstwa aplikacji - odpowiada za odbiór żądania od użytkownika
Warstwa transportowa - rozbija żądanie na mniejsze części
Warstwa internetowa - planuje trase i przypisuje adresy komputer (...) -> router (...) -> serwer (...)
Warstwa dostępu do internetu - sumuje wszystkie czynności i wysyła poprzez medium (np. kabel ethernet)

Użytkownik - enkapsulacja

---> Serwer - dekapsulacja (proces odwrotny, od warstwy dostępu do aplikacji)

## Pojęcia

NAT - tłumaczy adresy prywatne na publiczne, umożliwiając komunikację z internetem.

DNS - konwertuje www.wp.pl na adres np. 123.123.123.123, dzięki czemu nie trzeba wpisywać adresu.

DHCP - pozwala na dynamiczne przydzielenie adresu IP w sieci.

Firewall - filtruje ruch sieciowy, blokując potencjalnie niebezpieczne lub nieautoryzowane połączenia.


Serwer FTP - jest to serwer plików głównie tworzony na potrzeby łatwego przesyłania plików między komputerami w jednej sieci
