## Wazuh - Notatka Instalacyjna

## Źródła

Filmik na YouTube, z którego korzystałem

https://www.youtube.com/watch?v=v_6VWB-_wtw

Oficjalna dokumentacja QuickStart Wazuh

https://documentation.wazuh.com/current/quickstart.html

## Instalacja Wazuh

Podczas instalacji Wazuh automatycznie generuje użytkownika administracyjnego oraz bardzo skomplikowane hasło. Warto je zapisać lub zmienić na bardziej przystępne.

## Logowanie do Wazuh

Po zakończonej instalacji można zalogować się do interfejsu webowego Wazuh, wpisując w przeglądarce internetowej adres IP serwera Linux, na którym został zainstalowany Wazuh. Domyślnie działa on na porcie 443, czyli przykładowy adres logowania wygląda tak:
```
https://<adres_IP_serwera>
```

## Podstawowe funkcjonalności

- **Zarządzanie agentami** – Możemy podłączać wielu agentów do jednego menadżera.

- **Monitorowanie zagrożeń** – System ocenia poziom zagrożenia, im wyższy poziom, tym większe ryzyko.

- **Zarządzanie logami** – Pozwala na analizę i monitorowanie zdarzeń w systemie.

## Wazuh jako system SIEM

Wazuh jest oparty na SIEM (Security Information and Event Management), co oznacza, że pozwala na zbieranie, analizowanie i korelowanie logów z różnych źródeł w celu wykrywania zagrożeń bezpieczeństwa. SIEM umożliwia:

**Centralizację logów** – zbieranie danych z różnych systemów i aplikacji,

**Analizę w czasie rzeczywistym** – wykrywanie anomalii i podejrzanych działań,

**Zarządzanie incydentami** – automatyczne wykrywanie zagrożeń i alertowanie administratorów,

**Spełnianie wymagań zgodności** – ułatwia audyt i zgodność z regulacjami (np. GDPR, PCI-DSS).

## Obsługiwane systemy operacyjne

Wazuh Menadżer może być zainstalowany na systemach Linux, takich jak:

Ubuntu

Amazon Linux

CentOS

Red Hat

**Uwaga:** Wazuh Manager nie może być zainstalowany na systemie Windows.

