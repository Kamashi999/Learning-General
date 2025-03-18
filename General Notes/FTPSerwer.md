# Konfiguracja FTP na Fedora z użyciem vsftpd

To jest krótka instrukcja konfiguracji serwera FTP na systemie Fedora przy użyciu **vsftpd**. Zawiera również informacje o konfiguracji SELinux, który może wpływać na dostępność usług FTP, a także szczegóły ustawień w pliku konfiguracyjnym **vsftpd.conf**.

## Krok 1: Instalacja vsftpd

Aby zainstalować **vsftpd** na systemie Fedora, uruchom poniższe polecenie:

```
sudo dnf install vsftpd
```
Po zakończeniu instalacji włącz i uruchom usługę vsftpd:

```
sudo systemctl enable vsftpd
sudo systemctl start vsftpd
```

## Krok 2: Konfiguracja vsftpd
Plik konfiguracyjny vsftpd.conf znajduje się w katalogu /etc/vsftpd/. Poniżej znajduje się przegląd ustawień w pliku konfiguracyjnym vsftpd.conf, które zostały odkomentowane i które wpływają na działanie serwera FTP.

1. anonymous_enable=NO

Ta linia wyłącza dostęp anonimowy do serwera FTP. Oznacza to, że użytkownicy muszą posiadać konto, aby się zalogować.

2. local_enable=YES

Pozwala lokalnym użytkownikom na logowanie się do serwera FTP. Jeżeli masz utworzonych użytkowników w systemie, będą oni mogli się logować i przesyłać pliki na serwer.

3. write_enable=YES

Włącza możliwość zapisu plików przez użytkowników, co pozwala na przesyłanie plików na serwer FTP. Musisz mieć tę opcję włączoną, jeśli chcesz, aby użytkownicy mogli przesyłać pliki.

4. local_umask=022

Określa domyślną maskę uprawnień dla plików tworzonych przez użytkowników FTP. Maska 022 oznacza, że pliki będą miały uprawnienia 644 (czyli właściciel może je edytować, a reszta użytkowników tylko odczytywać).

5. dirmessage_enable=YES

Włącza wyświetlanie komunikatów powitalnych przy wchodzeniu do katalogu. Może to być przydatne, aby pokazać użytkownikom informacje o katalogu, w którym się znajdują.

6. xferlog_enable=YES

Włącza logowanie operacji przesyłania plików (upload i download), co pozwala na śledzenie aktywności serwera FTP.

7. connect_from_port_20=YES

Aktywuje przesyłanie danych przez port 20 (ftp-data). Jest to wymagane w tradycyjnej konfiguracji FTP.

8. chroot_local_user=YES

Oznacza, że lokalni użytkownicy będą "zatrzymani" (chroot) w swoich katalogach domowych. Dzięki temu nie będą mieli dostępu do innych części systemu.

9. listen_ipv6=YES

Konfiguruje serwer FTP do nasłuchiwania na połączenia przychodzące na adresach IPv6. Jeśli masz IPv6 w swojej sieci, to ustawienie będzie wymagane. Jeśli nie, możesz to wyłączyć, ustawiając listen=YES (na IPv4).

10. pam_service_name=vsftpd

Określa nazwę usługi PAM (Pluggable Authentication Module), która jest używana do autentykacji użytkowników. Dzięki temu można zintegrować autentykację systemową.

11. userlist_enable=YES

Włącza listę użytkowników, którzy mają dostęp do serwera FTP. Oznacza to, że tylko użytkownicy zdefiniowani w pliku /etc/vsftpd.userlist będą mogli się zalogować.

12. userlist_deny=NO

Ustawienie to oznacza, że wszyscy użytkownicy z pliku /etc/vsftpd.userlist będą mieli dostęp do serwera FTP. Jeśli ustawisz YES, tylko użytkownicy nie z listy będą mieli dostęp.

13. user_sub_token=$USER

Określa zmienną, która wskazuje, że katalogi użytkowników będą podstawiane w ścieżce. Dzięki temu możesz ustawić katalog domowy w zależności od nazwy użytkownika.

14. local_root=/home/$USER

Ta linia ustawia katalog domowy użytkownika na /home/$USER, co oznacza, że użytkownicy będą mieli dostęp tylko do swoich katalogów domowych.

15. pasv_enable=YES

Aktywuje tryb pasywny FTP. Tryb pasywny jest konieczny, gdy serwer FTP znajduje się za zaporą sieciową (firewallem).

16. pasv_max_port=10100

Ustawia maksymalny port, na którym serwer FTP będzie nasłuchiwał w trybie pasywnym.

17. pasv_min_port=10090

Ustawia minimalny port dla trybu pasywnego.

18. userlist_file=/etc/vsftpd.userlist

Określa ścieżkę do pliku, w którym znajdują się użytkownicy FTP. Plik /etc/vsftpd.userlist powinien zawierać listę użytkowników, którzy mają dostęp do serwera FTP.

## Krok 3: Tworzenie użytkownika FTP i ustawienie uprawnień katalogu

1. Aby utworzyć użytkownika, który będzie używany przez serwer FTP, wykonaj poniższe polecenie:
```
sudo useradd ftpuser
```

2. Ustawienie hasła dla użytkownika
```
sudo passwd ftpuser
```

3. Ustawienie odpowiednich uprawnień katalogu
```
sudo chown ftpuser:ftpuser /home/ftpuser
```
- Zmień uprawnienia katalogu, aby użytkownik mógł w nim przebywać, ale nie mógł modyfikować plików w katalogu głównym:
```
sudo chmod 755 /home/ftpuser
```

4. Zakończenie konfiguracji

Po wykonaniu powyższych kroków, użytkownik ftpuser będzie gotowy do logowania się na serwerze FTP, a katalog domowy będzie miał odpowiednie uprawnienia do pracy w trybie chroot.

## Krok 4: Konfiguracja SELinux

Jeśli SELinux jest włączony, należy wykonać kilka dodatkowych kroków, aby upewnić się, że serwer FTP będzie działał poprawnie:

1. Włącz dostęp do katalogów domowych użytkowników FTP:

```
sudo setsebool -P ftp_home_dir 1
```
2. Włącz pełny dostęp do plików:
```
sudo setsebool -P allow_ftpd_full_access 1
```
3. Zezwól na połączenia FTP przez określony port:
```
sudo semanage port -a -t ftp_port_t -p tcp 21
```

## Krok 5: Restartowanie usługi vsftpd
Po zakończeniu edycji pliku konfiguracyjnego vsftpd.conf oraz skonfigurowaniu SELinux, należy zrestartować usługę vsftpd:
```
sudo systemctl restart vsftpd
```
## Krok 6: Sprawdzenie statusu serwera FTP
Po zrestartowaniu serwera, możesz sprawdzić status usługi, aby upewnić się, że działa poprawnie:
```
sudo systemctl status vsftpd
```

## Krok 7: Testowanie połączenia FTP
Możesz teraz spróbować połączyć się z serwerem FTP z użyciem klienta FTP, np. FileZilla, podając odpowiednie dane logowania.

## Krok 8: Konfiguracja firewalla

Aby umożliwić połączenia FTP na Twoim serwerze Fedora, musisz odpowiednio skonfigurować zaporę firewalld. Oto jak to zrobić:

1. Zezwól na połączenia na portach FTP (20, 21 oraz porty pasywne)
W przypadku FTP będziesz musiał otworzyć standardowy port 21 oraz porty pasywne, które zostały ustawione w pliku konfiguracyjnym vsftpd.conf (w Twoim przypadku to porty od 10090 do 10100).

Aby to zrobić, użyj poniższych komend:
```
# Zezwól na połączenia FTP na standardowy port 21 oraz porty pasywne (10090-10100)
sudo firewall-cmd --zone=public --add-port=20-21/tcp --permanent
sudo firewall-cmd --zone=public --add-port=10090-10100/tcp --permanent

# Zastosuj zmiany
sudo firewall-cmd --reload
```
2. Sprawdź status firewalla
Aby upewnić się, że zapora jest poprawnie skonfigurowana, sprawdź status firewalla:\
```
sudo firewall-cmd --list-all
```
Powinna się pojawić lista otwartych portów, w tym porty 20, 21 oraz 10090-10100.

3. Weryfikacja połączenia
Po tych zmianach, jeśli zapora była główną przeszkodą, serwer FTP powinien teraz poprawnie nasłuchiwać na odpowiednich portach. Możesz spróbować połączyć się z serwerem FTP z zewnętrznego komputera lub używając lokalnego klienta FTP.
