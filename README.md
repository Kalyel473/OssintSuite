# ⚡ OSINT Suite

**Unified Passive Intelligence Collection Framework**  
Python · CLI · 15 módulos · Relatório HTML/JSON/CSV

> ⚠️ **USO EXCLUSIVO PARA FINS LEGAIS E AUTORIZADOS.**  
> Respeite a [LGPD](https://www.planalto.gov.br/ccivil_03/_ato2018-2022/2018/lei/l13709.htm), o [Marco Civil da Internet](http://www.planalto.gov.br/ccivil_03/_ato2011-2014/2014/lei/l12965.htm) e os Termos de Serviço de cada plataforma.

---

## 📦 Instalação

```bash
git clone https://github.com/seu-usuario/osint-suite
cd osint-suite
pip install -r requirements.txt
```

Para suporte a Tor (módulo `--darkweb`):
```bash
sudo apt install tor && sudo service tor start
```

---

## ⚙️ Configuração

Na primeira execução, o arquivo `config/settings.yaml` é criado automaticamente.  
Preencha as API keys que desejar — módulos sem key funcionam em modo degradado:

```yaml
github_token:      "ghp_..."        # github.com/settings/tokens
shodan_api_key:    "..."            # account.shodan.io
virustotal_key:    "..."            # virustotal.com/gui/my-apikey
abuseipdb_key:     "..."            # abuseipdb.com/account/api
otx_api_key:       "..."            # otx.alienvault.com/api
wigle_api_name:    "..."            # wigle.net/account
wigle_api_token:   "..."
hibp_api_key:      "..."            # haveibeenpwned.com/API/Key
censys_api_id:     "..."            # search.censys.io/account/api
censys_api_secret: "..."
telegram_bot_token: "..."           # Alertas (opcional)
telegram_chat_id:   "..."
```

Variáveis de ambiente também são suportadas (prefixo `OSINT_`):
```bash
export OSINT_GITHUB_TOKEN="ghp_..."
export OSINT_VT_KEY="..."
```

---

## 🚀 Uso

### Listar módulos
```bash
python main.py --list
```

### Módulo único
```bash
python main.py exemplo.com.br --cert
python main.py usuario123      --social
python main.py 45.33.32.156    --threat
python main.py 11987654321     --phone
python main.py MinhaEmpresa    --git
```

### Múltiplos módulos
```bash
python main.py exemplo.com.br --cert --email --asn --cloud --git
```

### Varredura completa
```bash
python main.py exemplo.com.br --all --output all
```

### Com Tor
```bash
python main.py keyword --darkweb --tor
```

### Análise de dump de credenciais
```bash
# Indexar dump
python main.py /path/to/dump.txt --breach

# Buscar por domínio no DB
python main.py empresa.com.br --breach
```

---

## 🧩 Módulos

| Flag        | Módulo                  | Descrição                                        |
|-------------|-------------------------|--------------------------------------------------|
| `--social`  | SocialTracer            | Username em 50+ plataformas (async)              |
| `--email`   | EmailHarvester          | Coleta emails + MX + HIBP breach check           |
| `--brpeople`| BRPeopleOSINT           | CNPJ, Diário Oficial, JusBrasil, LinkedIn BR     |
| `--phone`   | PhoneOSINT              | Operadora, DDD, Anatel, QuemMeLigou              |
| `--asn`     | ASNMapper               | ASNs, prefixes BGP, peers (BGPView, RIPE)        |
| `--cloud`   | CloudFinder             | Buckets S3 / Azure Blob / GCS expostos           |
| `--cert`    | CertSpy                 | Subdomínios via crt.sh, CertSpotter, Censys      |
| `--threat`  | ThreatIntelAggregator   | VT, AbuseIPDB, OTX, Shodan, ThreatFox + risk score |
| `--darkweb` | DarkWebMonitor          | Menções em .onion via Ahmia (+ Tor opcional)     |
| `--breach`  | BreachAnalyzer          | Indexa e busca dumps de credenciais (SQLite)     |
| `--geo`     | GeoOSINT                | EXIF GPS → endereço + mapa HTML (folium)         |
| `--wigle`   | WiGLEMapper             | SSID/BSSID → localização + mapa de calor         |
| `--git`     | GitLeakHunter           | 30+ regex patterns de secrets em repos públicos  |
| `--brand`   | BrandWatcher            | Reddit, Gists, Pastebin, Google News + Telegram  |
| `--wayback` | WaybackMiner            | URLs históricas + categorização + check ativo    |

---

## 📊 Relatórios

```bash
--output html    # Relatório interativo (padrão)
--output json    # JSON estruturado
--output csv     # Planilha flat
--output all     # Gera os 3 formatos
```

Todos os relatórios são salvos em `reports/`.

---

## 🏗️ Estrutura do Projeto

```
osint-suite/
├── main.py                  # Entry point / CLI
├── requirements.txt
├── config/
│   ├── __init__.py
│   └── settings.py          # Loader YAML + env vars
├── core/
│   ├── __init__.py
│   └── base.py              # BaseModule (HTTP, rate limit, retry, Tor)
├── modules/
│   ├── __init__.py
│   ├── social_tracer.py
│   ├── email_harvester.py
│   ├── br_people.py
│   ├── asn_mapper.py
│   ├── cloud_finder.py
│   ├── cert_spy.py
│   ├── threat_intel.py
│   ├── dark_web.py
│   ├── breach_analyzer.py
│   ├── geo_osint.py
│   ├── wigle_mapper.py
│   ├── phone_osint.py
│   ├── git_leak.py
│   ├── brand_watcher.py
│   └── wayback_miner.py
├── utils/
│   ├── __init__.py
│   └── report.py            # HTML / JSON / CSV generator
└── reports/                 # Output dir (auto-criado)
```

---

## 🔒 Aviso Legal

Este software é fornecido **apenas para fins educacionais, de pesquisa de segurança e pentests autorizados**.  
O uso indevido pode violar leis brasileiras e internacionais, incluindo:

- **LGPD** (Lei 13.709/2018)
- **Marco Civil da Internet** (Lei 12.965/2014)
- **Código Penal Brasileiro** — Art. 154-A (invasão de dispositivo informático)

O autor não se responsabiliza pelo uso inadequado desta ferramenta.
