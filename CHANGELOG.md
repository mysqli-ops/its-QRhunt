# 📜 Changelog - ITS Hunt

Tutte le modifiche significative al progetto, versione per versione.

---

## v3.3.1 — Fix Token Stabile (2026-05-12)

### Fixato
- **Token instabile tra sessioni** (`index.html`): `generateToken()` includeva `Date.now()` nel payload — il token cambiava ad ogni nuova sessione anche per lo stesso dispositivo, rendendo inutile la blacklist in `verify.html` e rompendo la coerenza del QR mostrato. **Rimosso `timestamp` dal payload**
- **`verify.html` si aspettava 5 parti** ma con timestamp rimosso il payload è a 4 parti: aggiornato `parts.length !== 4` e destructuring a `[salt, count, sortedKeys, deviceId]`

### Modificato
- Payload token: da `SALT:count:sortedKeys:timestamp:deviceId` (5 parti) a `SALT:count:sortedKeys:deviceId` (4 parti)
- Token ora **deterministico per dispositivo**: stesso progresso + stesso dispositivo = stesso token tra sessioni

### Nota
La blacklist `its_hunt_claimed_rewards` in `verify.html` è locale al dispositivo dello staff. Se lo staff usa più telefoni o cancella il browser, un token già usato può passare come nuovo — limitazione nota e accettabile per l'uso previsto.

---

## v3.3.0 — Token Univoco per Dispositivo (2026-05-11)

### Aggiunto
- Device ID persistente: chiave `its_hunt_device_id` in localStorage, generato al primo scan QR
- `getDeviceId()` in `collect.html` e `index.html`: genera 5 caratteri alfanumerici casuali se non esiste
- Timestamp nel token: `Date.now()` per unicità *(rimosso in v3.3.1)*
- Payload token: `SALT:count:sortedKeys:timestamp:deviceId` (5 parti)
- Meccanica blacklist token in `verify.html`: `localStorage its_hunt_claimed_rewards` traccia token già scannerizzati
- Controllo "GIA UTILIZZATO": QR scannerizzato due volte dallo stesso dispositivo staff mostra avviso specifico
- Stato visivo `body.used`: gradiente ambra/marrone per distinguere token già riscosso
- `isTokenAlreadyUsed(t)` e `markTokenAsUsed(t)` in `verify.html`

### Modificato
- `index.html`: `generateToken()` include `deviceId` nel payload; ripristinata generazione QR via qrcodejs
- `verify.html`: decodifica formato 5 parti; messaggi "VALIDO: CONSEGNA GADGET" / "GIA UTILIZZATO" / "NON VALIDO"; stamp "Registro Stand Produzione"

---

## v3.2.0 — Debug Nascosto & Rimozione Accesso Verify (2026-05-11)

### Aggiunto
- Parametro segreto `?debug=1` per mostrare il pulsante Reset Progresso in `index.html`
- Sezione debug nascosta di default (`display: none`)

### Modificato
- `index.html`: pulsante reset spostato in sezione debug; rimosso pulsante CTA "🏆 Vai allo stand Produzione"; rimossa `openVerify()`

### Rimosso
- Accesso diretto a `verify.html` dalla UI utente

---

## v3.1.0 — Blocco 2 Tentativi & Schema Dati (2026-05-11)

### Aggiunto
- Schema dati `progress[pos] = {correct: bool, attempts: n}`: tracciamento tentativi per posizione
- Meccanica blocco dopo 2 errori: al 2° errore la domanda è bloccata permanentemente anche via re-scan
- Messaggio "Ti rimane 1 tentativo!" dopo primo errore
- Messaggio "Tentativi esauriti!" dopo secondo errore con redirect automatico
- Migrazione automatica vecchio formato booleano → nuovo formato oggetto

### Modificato
- `collect.html`: `salvaRisposta(position, corretta, tentativi)`; `domandaGiaRisolta()` controlla `entry.correct === true OR entry.attempts >= 2`
- `index.html`: `getProgress()` migra vecchio formato; `renderGrid()` legge nuovo schema
- `test.html`: progress display con 🚫 per domande bloccate; `simulateAll()` scrive `{correct: true, attempts: 1}`

### Rimosso
- Schema `progress[pos] = true/false`

---

## v3.0.1 — Fix Bug Critico Retry & Verifica Soglia (2026-05-11)

### Fixato
- **[CRITICAL] Retry bloccato dopo errore via re-scan** (`collect.html`): `domandaGiaRisolta()` ora controlla `progress[position] === true`
- **[CRITICAL] Verifica `keys` incompatibile con meccanica soglia** (`verify.html`): rimosso check `keys !== EXPECTED_KEYS`
- **`EXPECTED_COUNT` stringa** (`verify.html`): `"10"` → `WIN_THRESHOLD = 10` number
- **Persist post-reset su mobile**: `location.reload()` → `window.location.replace('index.html?t=' + Date.now())`
- **`<base>` duplicato** rimosso da `collect.html` e `test.html`

---

## v3.0.0 — 12 Domande, Meccanica 10/12, Categorie ITS (2026-05-09) — BREAKING CHANGE

### Aggiunto
- 12 domande divise per categorie: Produzione (3), Sound (2), Videomaker (2), Fotografia (1), Game e VFX (2), Cyber Security (2)
- Meccanica soglia 10/12
- Score board con contatori corrette/errate/rimanenti
- Slot rossi ❌ per risposte errate
- `simulateMixed()` in `test.html`
- Categoria tag in `collect.html`

### Modificato
- `collect.html`: `QUIZ` a 12 entry con `category`; `KEYS` a 12 chiavi; salva `true/false`
- `index.html`: `TOTAL_QUESTIONS = 12`, `WIN_THRESHOLD = 10`; griglia tricolore; token include `correctCount`
- `verify.html`: verifica soglia con `parseInt`
- `test.html`: 12 slot ✅/❌/🔒

---

## v2.0.1 — Fix Token Base64url (2026-05-09)

### Fixato
- **[CRITICAL]** `btoa()` produceva `+`, `/`, `=` — problematici in URL. Sostituito con base64url
- `toBase64Url()` in `index.html` e `test.html`; `fromBase64Url()` in `verify.html`

---

## v2.0.0 — Refactor Sistema Domande (2026-05-09) — BREAKING CHANGE

### Aggiunto
- Sistema domande a scelta multipla; `verify.html`; token base64url; QR di verifica

### Rimosso
- `CORSO`, `_e`, sistema parola segreta, `QUIZ_ATTIVO`

---

## v1.4.0 — Fix Sicurezza & Ottimizzazione Animazioni (2026-05-08)

### Fixato
- **[SECURITY]** `mostraErrore()` non espone più `pos`, `key`, `validaKey()` nell'HTML pubblico
- `DEBUG = false` in `collect.html`

### Modificato
- `glowPulse` solo sull'ultimo slot; `completion-message` rimossa su reset cross-tab

---

## v1.3.1 — Documentazione (2026-05-08)

### Aggiunto
- `README.md` e `CHANGELOG.md` con storico completo

---

## v1.3.0 — Parola "Rossellini" & Chiavi Uniche (2026-05-08)

### Modificato
- Parola segreta: `CLOUD` → `Rossellini`; 10 chiavi uniche; progress bar a 10 lettere

---

## v1.2.0 — Colori ITS & Sicurezza (2026-05-08)

### Aggiunto
- Offuscamento base64 parola segreta; namespace `its_hunt_2026_openday`; `visibilitychange`; colori `#00aeef` / `#662d91`

---

## v1.1.0 — Fix & Stabilizzazione (2026-05-07)

### Aggiunto
- `test.html`; supporto `file://`; log debug console

---

## v1.0.0 — Release Iniziale (2026-05-07)

### Aggiunto
- `index.html`, `collect.html`; sistema slot bloccati/sbloccati; redirect automatico

---

## 📋 Legenda

| Tag | Significato |
|-----|-------------|
| **Aggiunto** | Nuove funzionalità, file o componenti |
| **Modificato** | Cambiamenti a funzionalità esistenti |
| **Rimosso** | Funzionalità, file o opzioni eliminate |
| **Fixato** | Correzioni di bug |