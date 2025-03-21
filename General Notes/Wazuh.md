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

## Instalacja agenta Wazuh na Fedorze i Ubuntu

### Pobieranie agenta

1. Na **dashboardzie Wazuh** dodaj nowego agenta.
2. Wybierz odpowiedni pakiet w zależności od systemu:
   - **Fedora**: `.rpm`
   - **Ubuntu**: `.deb`
3. Podaj adres **Wazuh Managera**.

### Instalacja agenta na Fedorze (DEB)

Najpierw upewnij się, że masz `dpkg`:

```bash
sudo apt update && sudo apt install -y dpkg
```
Pobierz i zainstaluj agenta:

```bash
wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.11.1-1_amd64.deb && sudo WAZUH_MANAGER='192.168.0.24' WAZUH_AGENT_NAME='Ubuntu' dpkg -i ./wazuh-agent_4.11.1-1_amd64.deb
```

Dodaje agenta do managera o ip 192.168.0.24 (adres ip lokalny managera), o nazwie Ubuntu.

### Konfiguracja agenta (Jeżeli automatycznie nie zostanie dodany)

Edytuj plik konfiguracyjny, aby dodać adres menedżera Wazuh:

```bash
sudo nano /var/ossec/etc/ossec.conf
```

Dodaj lub edytuj sekcję:

```xml
<manager>
  <address>192.168.0.24</address>
</manager>
```

Zapisz i zamknij plik.

### Uruchamianie agenta

```bash
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent
```

### Wyłączenie SELinux na czas instalacji (opcjonalnie)

Jeśli SELinux blokuje instalację, tymczasowo go wyłącz:

```bash
sudo setenforce 0
```

Po instalacji włącz go ponownie:

```bash
sudo setenforce 1
```

### Sprawdzanie logów agenta

Aby monitorować działanie agenta:

```bash
sudo tail -f /var/ossec/logs/ossec.log
```
lub poprzez dashboard

# Wazuh + ELK - All-in-One Deployment

##  Wprowadzenie

**Wazuh** to open-source'owy system SIEM (Security Information and Event Management), który pozwala na monitorowanie bezpieczeństwa w czasie rzeczywistym. 

**ELK Stack** (Elasticsearch, Logstash, Kibana) jest używany do przechowywania, przetwarzania i wizualizacji logów.

Ten poradnik opisuje **instalację all-in-one**, czyli uruchomienie **Wazuh + ELK na jednej maszynie**.

**Pełna dokumentacja**: [Wazuh + ELK All-in-One Deployment](https://documentation.wazuh.com/4.5/deployment-options/elastic-stack/all-in-one-deployment/index.html)

---

## 🔹 Co to jest ELK i dlaczego warto go doinstalować do Wazuh?

### **Czym jest ELK?**
**ELK** to zestaw narzędzi:
- **Elasticsearch** – przechowuje i indeksuje logi
- **Logstash** – przetwarza logi przed wysłaniem do Elasticsearch
- **Kibana** – wizualizuje logi i pozwala na analizę danych

### **Dlaczego warto używać ELK z Wazuh?**
**Łatwiejsza analiza logów** – zamiast przeglądać surowe logi, można używać czytelnych dashboardów w Kibana.  
**Szybkie wykrywanie incydentów** – ELK pozwala na błyskawiczne filtrowanie i wyszukiwanie zdarzeń.  
**Automatyczne alerty** – możliwość ustawienia powiadomień na podejrzane aktywności.  
**Wykresy i statystyki** – lepsze zrozumienie zagrożeń w systemie.  
