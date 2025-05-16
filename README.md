# Vaultwarden Self-Hosted (Fly.io)

Dette prosjektet inneholder konfigurasjonen du trenger for å kjøre en selvhostet [Vaultwarden](https://github.com/dani-garcia/vaultwarden) (et Bitwarden-kompatibelt passordhvelv) på Fly.io med HTTPS og eget domene.

---

## ✨ Funksjoner

* 🔐 Vaultwarden-server kjører via [Fly.io](https://fly.io)
* 🔒 HTTPS aktivert med Let's Encrypt
* 🌍 Tilpasset domene: [https://vault.vangenwood.com](https://vault.vangenwood.com)
* 🔑 2FA aktivert med Authenticator-app
* 🌙 Auto-sleep for å redusere kostnader (via `auto_stop_machines`)
* 🦆 DuckDNS-støtte eller egendefinert DNS (eks. Domeneshop)

---

## 🗂️ Filer

| Fil                  | Beskrivelse                            |
| -------------------- | -------------------------------------- |
| `fly.toml`           | Konfigurasjonsfil for Fly.io           |
| `docker-compose.yml` | Lokal testkjøring (valgfritt)          |
| `duckdns-update.sh`  | Shellscript for å oppdatere DuckDNS-IP |
| `README.md`          | Du er her ✅                            |

---

## 🚀 Deploy

```bash
fly deploy
```

> PS: Krever at du har logget inn med `fly auth login` og initialisert med `fly launch`.

---

## 🧠 Arkitektur og beslutninger (ADR)

### ADR-001: Hostingplattform

**Beslutning:** Vaultwarden ble satt opp på [Fly.io](https://fly.io) for å få en lavkost, skybasert løsning med støtte for HTTPS, volumer og auto-sleep.

**Alternativer vurdert:**

* Vercel (ikke egnet for langlevende apper)
* Render (begrenset gratisplan)
* Lokal Docker (ikke tilgjengelig uten port forwarding)

### ADR-002: TLS og domenevalg

**Beslutning:** Bruker eget domene `vault.vangenwood.com` administrert hos Domeneshop.

**DNS-konfig:**

* Type A → `66.241.124.223`
* Let's Encrypt brukt via Fly.io auto-cert

**Alternativ:** DuckDNS (ble brukt i initielle tester)

### ADR-003: Vaultwarden-konfig

**Beslutning:** Miljøvariabler settes via `fly.toml` for `DOMAIN`, `ROCKET_PORT`, `ROCKET_ADDRESS`, `ADMIN_TOKEN` og `WEBSOCKET_ENABLED`.

**Valg:** `ROCKET_ADDRESS = "0.0.0.0"` og `ROCKET_PORT = "8080"` for å støtte offentlig tilgjengelighet.

---

## 📸 Skjermbilder

### Vaultwarden Admin Dashboard

![Vaultwarden Admin Screenshot](https://raw.githubusercontent.com/mortnev/vaultwarden-server/main/assets/vaultwarden-admin.png)

### Vault: Hvelv og innlogging

![Vaultwarden Vault Screenshot](https://raw.githubusercontent.com/mortnev/vaultwarden-server/main/assets/vaultwarden-vault.png)

---

## 💡 Forbedringsforslag

1. **Legg til backup-strategi**

   * Automatisk `db.sqlite3` eksport til GitHub repo, S3 eller lignende.

2. **Konfigurer SMTP**

   * For e-postbekreftelser og kontogjenoppretting

3. **Oppdater README med diagram/skisser**

   * F.eks. arkitekturdiagram via mermaid eller draw\.io

4. **Opprett ADR-mappe**

   * Samle alle arkitekturvalg under `docs/adr/`

5. **Legg inn GitHub Actions**

   * For validering og test av `fly.toml`, evt. Docker builds

---

## 🛠️ Kommandoer

```bash
fly auth login
fly deploy
fly secrets set ADMIN_TOKEN="<argon2id>" DOMAIN="https://vault.vangenwood.com"
fly ssh console -a vaultwarden-makkamy
```

---

Lisens: MIT © Mortne Vangen
