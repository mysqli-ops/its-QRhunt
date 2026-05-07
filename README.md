# 🔍 ITS Hunt - Caccia al Tesoro QR

Sistema di caccia al tesoro basato su QR Code per eventi ITS Open Day.
I partecipanti scansionano QR code nascosti per svelare una parola segreta, lettera per lettera.

---

## 📁 File del progetto

| File | Descrizione |
|------|-------------|
| `index.html` | **Dashboard principale** — la pagina che i partecipanti tengono aperta durante la caccia |
| `collect.html` | **Pagina di raccolta invisibile** — raggiunta tramite scan dei QR code, salva le lettere trovate |
| `test.html` | **Pagina di debug** — genera QR code al volo, simula scan, mostra stato progresso |

---

## 🚀 Come usarlo

### 1. Configura la parola segreta

In **tutti e tre i file**, modifica la costante offuscata:

```javascript
const _e = "Um9zc2VsbGluaQ==";  // btoa("Rossellini")
const CORSO = atob(_e).split('');
```

Per cambiare parola:
```javascript
btoa("NUOVA_PAROLA")  // → incolla il risultato in _e
```

### 2. Genera le chiavi di sicurezza

In `collect.html` e `test.html`, il dizionario `KEYS` contiene le chiavi univoche per ogni posizione:

```javascript
const KEYS = {
    "0": "k9mP2xQv",
    "1": "Lw7nR4tY",
    // ... una chiave diversa per ogni lettera
};
```

**Regole:**
- Ogni chiave deve essere **univoca** (non ripetuta)


### 3. Genera i QR Code

Usa un generatore di QR Code online (es. [qr-code-generator.com](https://www.qr-code-generator.com)) con questi URL:

```
https://TUO-SITO.github.io/its-QRhunt/collect.html?pos=0&key=k9mP2xQv
https://TUO-SITO.github.io/its-QRhunt/collect.html?pos=1&key=Lw7nR4tY
...
```

Ogni QR contiene:
- `pos` = posizione della lettera nella parola (0-based)
- `key` = chiave di sicurezza che valida il QR

### 4. Deploy su GitHub Pages

1. Carica i file in un repository GitHub
2. Attiva GitHub Pages in **Settings → Pages**
3. Il sito sarà live in 1-2 minuti

---

## 🔒 Sicurezza

- **Chiavi univoche**: ogni QR ha una chiave diversa
- **Offuscamento base64**: la parola non appare in chiaro nel sorgente
- **localStorage namespaced**: `its_hunt_2026_openday`

---

## 🧪 Test in locale

Apri `test.html` nel browser. Ti permette di:
- Vedere i QR code generati al volo
- Simulare gli scan con un click
- Sbloccare tutte le lettere istantaneamente
- Resetare il progresso
- Monitorare lo stato del localStorage

---

## 📝 Note tecniche

- **100% frontend**
- **Nessun backend necessario**: il progresso è salvato nel `localStorage` del browser
- **Cross-tab sync**: la dashboard si aggiorna automaticamente
- **Responsive**: funziona su desktop, tablet e smartphone

---

## 🆘 Debug

Se qualcosa non funziona:
1. Apri `collect.html` con i parametri nell'URL e controlla la console (F12)
2. Verifica che `pos` e `key` siano corretti
3. Controlla che `localStorage` contenga `its_hunt_2026_openday`
4. Usa il pulsante "Reset Progresso" in `test.html` per ripartire da zero

---

*Creato per l'Open Day ITS 2026*
