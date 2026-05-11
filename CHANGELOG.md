# 📜 Changelog - ITS Hunt
Tutte le modifiche significative al progetto, versione per versione.

## v3.1.0 — Staff Security & Anti-Duplicazione (2026-05-11)
### Aggiunto
- **Meccanica Blacklist Token (verify.html)**: Introdotta logica di salvataggio dei token scansionati nel `localStorage` del dispositivo dell'organizzatore.
- **Controllo "Gia Utilizzato"**: Se un QR code viene scansionato due volte dallo stesso dispositivo staff, la verifica fallisce con un avviso specifico e una nuova variante cromatica (ambra/marrone).
- **Feedback Caricamento**: Aggiunto un delay artificiale (800ms) nella verifica per simulare l'elaborazione dei dati e migliorare l'esperienza dello staff.

### Modificato
- **index.html (Hardening)**: Rimossa la possibilità per l'utente di auto-verificare il proprio QR. Il pulsante "Vai allo stand" e il relativo link sono stati eliminati; ora l'utente vede solo il QR da mostrare fisicamente allo staff.
- **Refactoring verify.html**: Ricostruzione completa del file con separazione delle responsabilità:
    - `isTokenAlreadyUsed()`: Controllo incrociato nel database locale.
    - `markTokenAsUsed()`: Registrazione post-consegna gadget.
    - `verificaToken()`: Logica di decodifica e validazione firma/soglia punteggio.

### Rimosso
- Pulsante di redirect alla verifica dalla dashboard utente per impedire manipolazioni manuali del token.

---

## v3.0.1 — Fix Bug Critico Retry & Verifica Soglia (2026-05-11)
### Fixato
- **[CRITICAL] Retry bloccato**: La funzione `domandaGiaRisolta()` ora controlla `progress[position] === true`. Precedentemente, anche un errore salvato (false) bloccava il re-scan.
- **[CRITICAL] Verifica keys**: Rimosso il check `keys !== EXPECTED_KEYS` da `verify.html` poiché incompatibile con la soglia 10/12 (non è obbligatorio tentare tutte le 12 domande per vincere).
- **BFCache Fix**: `resetProgress()` ora usa un timestamp (`index.html?t=...`) invece di `reload()` per forzare il refresh sui browser mobile iOS/Android.
- **Type Safety**: `WIN_THRESHOLD` convertito definitivamente in `number` per evitare ambiguità nel confronto con `parseInt(count)`.

---

## v3.0.0 — 12 Domande, Meccanica 10/12, Categorie ITS (2026-05-09) — BREAKING CHANGE
### Aggiunto
- **12 Domande specifiche**: Suddivise in categorie (Produzione, Sound, Videomaker, Fotografia, Game/VFX, Cyber Security).
- **Meccanica Soglia 10/12**: Introdotta la possibilità di vincere anche con 2 errori.
- **Score Board**: Visualizzazione in dashboard di risposte corrette ✅, errate ❌ e rimanenti 🔒.
- **Categorie**: Visualizzazione del settore di appartenenza della domanda in `collect.html`.

### Modificato
- **Formato Dati**: Il localStorage ora traccia sia `true` che `false` per permettere la visualizzazione degli slot rossi.
- **UI Dashboard**: Griglia espansa a 12 slot con feedback cromatico differenziato.

---

## v2.0.1 — Fix Token Base64url (2026-05-09)
### Fixato
- **[CRITICAL] URL Token Corruption**: Sostituito `btoa()` standard con **base64url encoding**. I caratteri `+`, `/` e `=` (padding) causavano errori di scansione; ora vengono sostituiti con caratteri safe (`-`, `_`).

---

## v2.0.0 — Refactor Sistema Domande (2026-05-09) — BREAKING CHANGE
### Aggiunto
- **Sistema Quiz**: Sostituita la raccolta lettere con domande a scelta multipla (4 opzioni).
- **verify.html**: Prima versione della pagina di verifica per lo staff.
- **Token System**: Generazione payload cifrato con `SALT + count + keys`.

---

## v1.x (Versioni Precedenti)
- **v1.4.0**: Fix sicurezza (rimozione debug info pubblica) e ottimizzazione animazioni CSS.
- **v1.3.0**: Parola segreta "Rossellini" (10 lettere) e 10 chiavi uniche.
- **v1.2.0**: Integrazione colori ufficiali ITS (Ciano/Viola) e offuscamento base64.
- **v1.1.0**: Creazione `test.html` e supporto protocollo `file://`.
- **v1.0.0**: Release iniziale (Sistema a 5 lettere, parola "CLOUD").