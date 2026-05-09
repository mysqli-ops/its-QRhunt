# 🔍 ITS Hunt - Caccia al Tesoro QR

Sistema di caccia al tesoro basato su QR Code per eventi ITS Open Day.
I partecipanti scansionano QR code nascosti, rispondono a domande a scelta multipla, e ottengono un QR di verifica da mostrare al banchetto per ritirare il gadget.

---

## 📁 File del progetto

| File | Descrizione |
|------|-------------|
| `index.html` | **Dashboard partecipante** — mostra il progresso domande e genera il QR di verifica finale |
| `collect.html` | **Pagina domanda** — raggiunta tramite scan del QR nascosto, mostra la domanda a scelta multipla |
| `verify.html` | **Pagina di verifica** — aperta dal banchetto dopo scan del QR finale; mostra ✅ verde o ❌ rosso |
| `test.html` | **Pagina di debug** — genera QR, simula scan, mostra stato progresso (**non deployare su GitHub Pages**) |

---

## 🚀 Come usarlo

### 1. Configura le domande

In `collect.html`, modifica il dizionario `QUIZ`:

```javascript
const QUIZ = {
    "0": {
        question: "Testo della domanda",
        options: ["Opzione A", "Opzione B", "Opzione C", "Opzione D"],
        answer: 0  // indice (0-based) della risposta corretta
    },
    // una entry per ogni domanda...
};
```

Per aggiungere una domanda: aggiungi una entry in `QUIZ` e una corrispondente in `KEYS` (vedi sotto). Aggiorna anche `TOTAL_QUESTIONS` in `index.html` e `EXPECTED_COUNT` in `verify.html`.

### 2. Configura le chiavi di sicurezza

In `collect.html` e `test.html`, il dizionario `KEYS` associa ogni posizione a una chiave univoca:

```javascript
const KEYS = {
    "0": "k9mP2xQv",
    "1": "Lw7nR4tY",
    // una chiave per ogni domanda, tutte univoche
};
```

### 3. Aggiorna le costanti di scala

Quando cambia il numero di domande, aggiorna questi tre valori:

| File | Costante | Valore attuale |
|------|----------|----------------|
| `index.html` | `TOTAL_QUESTIONS` | `5` |
| `verify.html` | `EXPECTED_COUNT` | `5` |

`EXPECTED_KEYS` in `verify.html` è generato automaticamente da `EXPECTED_COUNT` — non va modificato manualmente.

### 4. Genera i QR Code

Usa un generatore di QR Code online (es. [qr-code-generator.com](https://www.qr-code-generator.com)) con questi URL:

```
https://TUO-SITO.github.io/its-QRhunt/collect.html?pos=0&key=k9mP2xQv
https://TUO-SITO.github.io/its-QRhunt/collect.html?pos=1&key=Lw7nR4tY
...
```

Ogni QR contiene:
- `pos` = indice della domanda (0-based)
- `key` = chiave di sicurezza che valida il QR

In alternativa, usa `test.html` per generarli e stamparli direttamente.

### 5. Deploy su GitHub Pages

1. Carica `index.html`, `collect.html`, `verify.html` nel repository (**non** `test.html`)
2. Attiva GitHub Pages in **Settings → Pages**
3. Il sito sarà live in 1-2 minuti
4. Aggiorna gli URL dei QR con il dominio GitHub Pages definitivo

---

## 🔒 Sicurezza

- **Chiavi univoche**: ogni QR ha una chiave diversa — un QR non può essere riusato per un'altra domanda
- **Token base64url**: il QR di verifica finale codifica il progresso in un token URL-safe firmato con `SALT`
- **`DEBUG = false`**: nessuna info sensibile esposta nell'HTML pubblico di `collect.html`
- **`localStorage` namespaced**: `its_hunt_2026_openday`

---

## 🧪 Test in locale

Apri `test.html` nel browser. Permette di:
- Visualizzare i QR code di tutte le domande
- Simulare la scansione di ogni QR con un click
- Risolvere tutte le domande istantaneamente ("Risolvi TUTTO")
- Aprire `verify.html` con token generato dal progresso attuale
- Resettare il progresso
- Monitorare lo stato del localStorage in tempo reale

---

## 📝 Note tecniche

- **100% frontend** — nessun backend, nessun server
- **Progresso in localStorage**: persistente nel browser del partecipante
- **Cross-tab sync**: la dashboard si aggiorna via `visibilitychange` + `setInterval` fallback
- **Token di verifica**: `btoa(SALT + ':' + count + ':' + sortedKeys)` con encoding base64url (URL-safe)
- **Responsive**: funziona su desktop, tablet e smartphone

---

## 🆘 Debug

Se qualcosa non funziona:
1. Apri `collect.html?pos=0&key=k9mP2xQv` e controlla la console (F12)
2. Verifica che `pos` e `key` corrispondano esattamente a quelli in `KEYS`
3. Controlla che `localStorage` contenga la chiave `its_hunt_2026_openday`
4. Usa "Reset Progresso" in `test.html` per ripartire da zero
5. Per testare `verify.html`, usa il pulsante "Apri Verify (con token)" in `test.html` dopo aver risolto tutte le domande

---

*Creato per l'Open Day ITS Rossellini 2026*