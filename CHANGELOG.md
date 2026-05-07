# 📜 Changelog - ITS Hunt

## v1.0.0 — Release Iniziale (2026-05-07)

### Aggiunto
- `index.html` — Dashboard con griglia di slot, progress bar, messaggio di completamento
- `collect.html` — Pagina di raccolta con validazione chiavi e salvataggio localStorage
- Sistema di slot bloccati/sbloccati con animazioni CSS (lucchetti 🔒 e lettere neon)
- Supporto base per quiz opzionali (struttura preparata, disattivata di default)
- Redirect automatico dalla pagina di raccolta alla dashboard

### Modificato
- Palette colori iniziale: verde `#00ff88` e azzurro `#00ccff`
- Namespace localStorage: `its_hunt_progress`
- Parola di default: `CLOUD` (5 lettere)
- Chiavi di sicurezza semplici: `a1b2`, `c3d4`, `e5f6`, `g7h8`, `i9j0`

---

## v1.1.0 — Fix & Stabilizzazione (2026-05-07)

### Aggiunto
- `test.html` — Pagina di debug con generatore QR integrato, simulazione scan, reset progresso
- Supporto protocollo `file://` per test in locale (fix redirect)
- Log di debug visibile in console per `collect.html`
- Pulsante "Reset Progresso (Debug)" nella dashboard

### Modificato
- Fix redirect `collect.html` → `index.html` per funzionare anche con `file://`
- Aggiunta info debug nel messaggio di errore

---

## v1.2.0 — Colori ITS & Sicurezza (2026-05-08)

### Aggiunto
- **Offuscamento base64** della parola segreta: `const _e = "Q0xPVUQ="; const CORSO = atob(_e).split('');`
- **Namespace localStorage** specifico per l'evento: `its_hunt_2026_openday`
- **Event listener `visibilitychange`** sulla dashboard per aggiornamento cross-tab
- Colori ufficiali ITS: ciano `#00aeef` e viola `#662d91`

### Modificato
- Palette colori completa: da verde/azzurro a ciano/viola ITS
- Gradienti header, progress bar, slot sbloccati, pulsanti quiz
- Animazione `fadeInUp` spostata da `.completion-message` a `.completion-message.show`
- `setInterval` ridotto a ruolo di fallback cross-tab (primario ora è `visibilitychange`)

### Rimosso
- Colori precedenti: `#00ff88`, `#00ccff`, `#00cc6a`

---

## v1.3.0 — Parola "Rossellini" & Chiavi Uniche (2026-05-08)

### Aggiunto
- Supporto per parole più lunghe: da 5 a 10 lettere
- 10 chiavi di sicurezza **tutte uniche** e più robuste

### Modificato
- Parola segreta: da `CLOUD` (5 lettere) a **`Rossellini`** (10 lettere)
- Offuscamento base64 aggiornato: `_e = "Um9zc2VsbGluaQ=="`
- Chiavi di sicurezza:
  - **Prima**: `a1b2`, `c3d4`, `e5f6`, `g7h8`, `i9j0` (5 chiavi, alcune duplicate)
  - **Dopo**: `k9mP2xQv`, `Lw7nR4tY`, `Bj3hF8sD`, `Za5eW1qU`, `Xc6gV0iO`, `Np8rT3lK`, `Hm2jF7bZ`, `Vd4wQ9sA`, `Gy1kL6xC`, `Rf5tE2nM` (10 chiavi, tutte uniche)
- Progress bar: da `0 / 5` a `0 / 10` lettere
- Grid dashboard: adattata a 10 slot

### Rimosso
- Chiavi duplicate nel dizionario `KEYS`
- Supporto implicito per lunghezza fissa di 5 lettere

---

## v1.3.1 — Documentazione (2026-05-08)

### Aggiunto
- `README.md` aggiornato con istruzioni complete per setup, deploy e test
- `CHANGELOG.md` con storico completo delle versioni

### Modificato
- Documentazione allineata alla versione corrente (v1.3.0)

---

## v1.4.0 — Fix Sicurezza & Ottimizzazione Animazioni (2026-05-08)

### Fixato
- **[SECURITY] Debug info rimossa dall'output pubblico** (`collect.html`): la funzione `mostraErrore()` non espone più `pos`, `key` e il risultato di `validaKey()` nell'HTML visibile — era un oracolo di validazione utilizzabile per brute force degli URL
- **Flag `DEBUG`** aggiunto in `collect.html`: le info di debug sono ora condizionate a `const DEBUG = false`; impostare `true` solo in locale

### Modificato
- **`glowPulse` applicata solo all'ultimo slot sbloccato** (`index.html`): rimossa l'animazione infinita da tutti gli slot `.unlocked`, aggiunta classe `.last-found` applicata dinamicamente via JS all'ultimo indice presente nel progress — evita 10 animazioni CSS simultanee su mobile
- **`completion-message` rimossa su reset** (`index.html`): aggiunto `else { msg.classList.remove('show') }` nel `renderGrid()` per gestire il caso in cui il localStorage venga ridotto da un'altra tab

### Rimosso
- Costante `_k = 42` inutilizzata in `collect.html` (residuo del prototipo XOR)

---

## 📋 Legenda

| Tag | Significato |
|-----|-------------|
| **Aggiunto** | Nuove funzionalità, file o componenti |
| **Modificato** | Cambiamenti a funzionalità esistenti |
| **Rimosso** | Funzionalità, file o opzioni eliminate |
| **Fixato** | Correzioni di bug (documentati nelle versioni specifiche) |
| **Deprecato** | Funzionalità mantenute ma sconsigliate per uso futuro |