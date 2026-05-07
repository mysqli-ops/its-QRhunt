# 🔍 ITS Hunt - Caccia al Tesoro QR

Sistema di caccia al tesoro basato su QR Code per eventi ITS.

## 📁 File

| File | Descrizione |
|------|-------------|
| `index.html` | Dashboard principale - la pagina che i partecipanti tengono aperta |
| `collect.html` | Pagina di raccolta invisibile - raggiunta tramite QR code |

## 🚀 Come usarlo

### 1. Configura la parola segreta

In **entrambi i file**, modifica la costante `CORSO`:

```javascript
const CORSO = ["C", "L", "O", "U", "D"];  // La tua parola
```

### 2. Genera i QR Code

Usa un generatore di QR Code (come [qr-code-generator.com](https://www.qr-code-generator.com)) con questi URL:

| QR | URL |
|----|-----|
| QR 1 | `https://tuosito.it/collect.html?pos=0&key=a1b2` |
| QR 2 | `https://tuosito.it/collect.html?pos=1&key=c3d4` |
| QR 3 | `https://tuosito.it/collect.html?pos=2&key=e5f6` |
| QR 4 | `https://tuosito.it/collect.html?pos=3&key=g7h8` |
| QR 5 | `https://tuosito.it/collect.html?pos=4&key=i9j0` |

> ⚠️ **Importante**: Cambia le chiavi (`key`) in produzione! Sono nel dizionario `KEYS` in `collect.html`.

### 3. Attiva i Quiz (opzionale)

In `collect.html`, imposta:

```javascript
const QUIZ_ATTIVO = true;
```

E configura le domande:

```javascript
const QUIZ = {
    "0": { question: "Quanti bit ha un byte?", answer: "8" },
    "1": { question: "Che protocollo usa la porta 80?", answer: "http" },
    // ...
};
```

## 🔒 Sicurezza

- Le chiavi (`key`) impediscono di indovinare le lettere cambiando solo `pos`
- Ogni QR ha una chiave univoca
- Il progresso è salvato in `localStorage` (persiste nel browser)

## 🎨 Personalizzazione

- Modifica i colori nelle variabili CSS
- Cambia le animazioni
- Aggiungi un timer o una classifica
- Integra con un backend per salvare i punteggi

## 📝 Note

- Il sistema funziona **completamente offline** (solo frontend)
- Per un sistema multi-utente, serve un backend con database
- Il pulsante "Reset" in basso a destra è solo per debug
