📜 Changelog - ITS Hunt
Tutte le modifiche significative al progetto, versione per versione.
v1.0.0 — Release Iniziale (2026-05-07)
Aggiunto

    index.html — Dashboard con griglia di slot, progress bar, messaggio di completamento
    collect.html — Pagina di raccolta con validazione chiavi e salvataggio localStorage
    Sistema di slot bloccati/sbloccati con animazioni CSS (lucchetti 🔒 e lettere neon)
    Supporto base per quiz opzionali (struttura preparata, disattivata di default)
    Redirect automatico dalla pagina di raccolta alla dashboard

Modificato

    Palette colori iniziale: verde #00ff88 e azzurro #00ccff
    Namespace localStorage: its_hunt_progress
    Parola di default: CLOUD (5 lettere)
    Chiavi di sicurezza semplici: a1b2, c3d4, e5f6, g7h8, i9j0

v1.1.0 — Fix & Stabilizzazione (2026-05-07)
Aggiunto

    test.html — Pagina di debug con generatore QR integrato, simulazione scan, reset progresso
    Supporto protocollo file:// per test in locale (fix redirect)
    Log di debug visibile in console per collect.html
    Pulsante "Reset Progresso (Debug)" nella dashboard

Modificato

    Fix redirect collect.html → index.html per funzionare anche con file://
    Aggiunta info debug nel messaggio di errore

v1.2.0 — Colori ITS & Sicurezza (2026-05-08)
Aggiunto

    Offuscamento base64 della parola segreta: const _e = "Q0xPVUQ="; const CORSO = atob(_e).split('');
    Namespace localStorage specifico per l'evento: its_hunt_2026_openday
    Event listener visibilitychange sulla dashboard per aggiornamento cross-tab
    Colori ufficiali ITS: ciano #00aeef e viola #662d91

Modificato

    Palette colori completa: da verde/azzurro a ciano/viola ITS
    Gradienti header, progress bar, slot sbloccati, pulsanti quiz
    Animazione fadeInUp spostata da .completion-message a .completion-message.show
    setInterval ridotto a ruolo di fallback cross-tab (primario ora è visibilitychange)

Rimosso

    Colori precedenti: #00ff88, #00ccff, #00cc6a

v1.3.0 — Parola "Rossellini" & Chiavi Uniche (2026-05-08)
Aggiunto

    Supporto per parole più lunghe: da 5 a 10 lettere
    10 chiavi di sicurezza tutte uniche e più robuste

Modificato

    Parola segreta: da CLOUD (5 lettere) a Rossellini (10 lettere)
    Offuscamento base64 aggiornato: _e = "Um9zc2VsbGluaQ=="
    Chiavi di sicurezza:
        Prima: a1b2, c3d4, e5f6, g7h8, i9j0 (5 chiavi, alcune duplicate)
        Dopo: k9mP2xQv, Lw7nR4tY, Bj3hF8sD, Za5eW1qU, Xc6gV0iO, Np8rT3lK, Hm2jF7bZ, Vd4wQ9sA, Gy1kL6xC, Rf5tE2nM (10 chiavi, tutte uniche)
    Progress bar: da 0 / 5 a 0 / 10 lettere
    Grid dashboard: adattata a 10 slot

Rimosso

    Chiavi duplicate nel dizionario KEYS
    Supporto implicito per lunghezza fissa di 5 lettere

v1.3.1 — Documentazione (2026-05-08)
Aggiunto

    README.md aggiornato con istruzioni complete per setup, deploy e test
    CHANGELOG.md con storico completo delle versioni

Modificato

    Documentazione allineata alla versione corrente (v1.3.0)

v1.4.0 — Fix Sicurezza & Ottimizzazione Animazioni (2026-05-08)
Fixato

    [SECURITY] Debug info rimossa dall'output pubblico (collect.html): la funzione mostraErrore() non espone più pos, key e il risultato di validaKey() nell'HTML visibile
    Flag DEBUG aggiunto in collect.html: le info di debug sono ora condizionate a const DEBUG = false; true solo in locale

Modificato

    glowPulse applicata solo all'ultimo slot sbloccato (index.html): rimossa l'animazione infinita da tutti gli slot .unlocked, aggiunta classe .last-found applicata dinamicamente via JS all'ultimo indice presente nel progress — evita 10 animazioni CSS simultanee su mobile
    completion-message rimossa su reset (index.html): aggiunto else { msg.classList.remove('show') } nel renderGrid() per gestire il caso in cui il localStorage venga ridotto da un'altra tab

Rimosso

    Costante _k = 42 inutilizzata in collect.html (residuo del prototipo XOR)

v2.0.0 — Refactor Sistema Domande (2026-05-09) — BREAKING CHANGE
Il sistema è stato completamente refactorato. I file v1.x non sono retrocompatibili.
Aggiunto

    Sistema domande a scelta multipla in collect.html: 4 opzioni per domanda, selezione + conferma, feedback visivo
    verify.html — nuovo file: pagina di verifica. Scannerizza il QR generato su index.html e mostra ✅ verde se il token è valido, ❌ rosso se non lo è
    Token di verifica: generato con btoa(SALT + ':' + count + ':' + sortedKeys) dove SALT = "ITS2026OD"
    QR di verifica: generato dinamicamente su index.html quando tutte le domande sono risolte, puntante a verify.html?token=TOKEN
    Struttura dati QUIZ: dizionario principale con question, options (array 4 stringhe), answer (indice corretto)
    Domande per test:
        Anno fondazione ITS Rossellini
        Settore tecnologico principale
        Sede principale
        Significato acronimo ITS
        Figura del cinema dedicata all'istituto

Modificato

    collect.html: da pagina di raccolta lettere a pagina quiz interattiva
        UI: 4 bottoni opzione + bottone conferma + feedback ✅/❌
        Logica: nessun redirect automatico, retry illimitato su risposta errata
        Risposta corretta → salva progress[pos] = true → redirect a dashboard dopo 2s
    index.html: da dashboard lettere a dashboard domande
        Griglia: 5 slot con 🔒 (non trovato) / ✅ (risolto)
        Progress bar: X / 5 domande risolte
        Completamento: genera QR di verifica + messaggio "Mostra questo QR al banchetto Prod"
        glowPulse solo sull'ultimo slot risolto (classe .last-found)
    test.html: aggiornato per sistema domande
        simulateAll(): progress[i] = true invece di progress[i] = letter
        Progress display: ✅/🔒 invece di lettera/?
        KEYS ridotto a 5 chiavi (rimossi indici 5-9)
        Link a verify.html aggiunto

Rimosso

    CORSO e tutta la logica lettere da tutti i file
    _e (offuscamento base64) — non più necessario senza parola segreta
    QUIZ_ATTIVO — le domande sono sempre attive
    Sistema "parola segreta" — sostituito da "completa tutte le domande per il gadget"
    10 chiavi — ridotto a 5 chiavi (una per domanda)

v2.0.1 — Fix Token Base64url (2026-05-09)
Fixato

    [CRITICAL] Token base64 corrotto negli URL: il token btoa() produceva caratteri = (padding base64) che venivano mal interpretati da alcuni QR scanner e browser. Sostituito con base64url encoding che rimuove +, /, = — caratteri sicuri per URL
    index.html: funzione toBase64Url() sostituisce + → -, / → _, rimuove = padding
    verify.html: funzione fromBase64Url() ripristina padding e caratteri standard prima di atob()
    test.html: pulsante "Apri Verify" ora genera token dinamico dal progresso attuale invece di aprire verify.html senza token

Rimosso

    encodeURIComponent() nel passaggio token → URL (non più necessario con base64url)


  v3.0.0 — 12 Domande, Meccanica 10/12, Categorie ITS (2026-05-09) — BREAKING CHANGE
Il sistema è stato espanso da 5 a 12 domande con meccanica soglia 10/12 per vincere. Il localStorage precedente non è compatibile (formato dati cambiato: da progress[pos] = true a progress[pos] = true/false per tracciare anche le risposte errate).
Aggiunto

    12 domande complete divise per categorie ITS:
        Produzione (3 domande): documenti scena, figura produzione, acronimo ODG
        Sound (2 domande): Ennio Morricone, Foley Artist
        Videomaker (2 domande): Regola dei Terzi, Panoramica
        Fotografia (1 domanda): Temperatura colore Kelvin
        Game e VFX (2 domande): Motion Capture, Unreal Engine
        Cyber Security (2 domande): Vulnerability Assessment, Firewall
    Meccanica soglia 10/12: per vincere servono almeno 10 risposte corrette su 12
    Score board in index.html: mostra conteggio separato di corrette ✅, errate ❌, e rimanenti 🔒
    Visualizzazione risposte errate: slot rossi con ❌ nella griglia dashboard
    Call to action in schermata vittoria: pulsante "🏆 Vai allo stand Produzione" con link a verify
    simulateMixed() in test.html: simula 10 corrette / 2 errate per testare la soglia di vittoria
    Categoria tag in collect.html: mostra la categoria della domanda (Produzione, Sound, ecc.)

Modificato

    collect.html:
        QUIZ espanso a 12 entry con categorie
        KEYS espanso a 12 chiavi uniche
        TOTAL_QUESTIONS = 12
        Salva progress[pos] = true (corretta) o progress[pos] = false (errata)
        Retry illimitato: risposta errata → reset selezione, nessun blocca
    index.html:
        TOTAL_QUESTIONS = 12
        WIN_THRESHOLD = 10
        Griglia 12 slot con colori: verde (corretto), rosso (errato), grigio (non tentato)
        Token di verifica include conteggio corrette nel payload
        Schermata completamento differenziata: vittoria (≥10) vs sconfitta (<10)
        QUIZ ridotto a label categoria per non duplicare dati
    verify.html:
        EXPECTED_COUNT = "10" (minimo risposte corrette)
        TOTAL_QUESTIONS = 12
        EXPECTED_KEYS generato dinamicamente da TOTAL_QUESTIONS
        Messaggi specifici per conteggio insufficiente
    test.html:
        TOTAL_QUESTIONS = 12
        WIN_THRESHOLD = 10
        Progress display: 12 slot con ✅/❌/🔒
        Pulsante "Simula 10 corrette / 2 errate"

Rimosso

    5 domande originali generiche sostituite da 12 domande specifiche ITS
    Sistema "tutte le domande devono essere corrette" sostituito da "almeno 10/12"