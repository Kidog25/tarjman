# Tarjman

Interprete tascabile italiano ⇄ darija marocchino. Registri chi parla, l'app trascrive e traduce, e la frase in darija si può mostrare a schermo intero o far leggere ad alta voce dal telefono.

## Come funziona

- È un'unica pagina statica (`index.html`), senza build e senza server.
- La registrazione usa il microfono del browser (Web Audio). L'audio viene convertito in WAV mono a 16 kHz, con un massimo di 60 secondi.
- L'audio va direttamente dal telefono all'API di Gemini (`generateContent`). Una sola chiamata fa sia la trascrizione sia la traduzione, e restituisce un JSON con uno schema fisso. Prima dell'invio l'app taglia il silenzio all'inizio e alla fine della registrazione.
- La chiave API di Gemini la inserisce l'utente nelle impostazioni e resta nel `localStorage` del telefono. Nel codice non c'è nessun segreto, quindi il repository può essere pubblico.
- Per dare più contesto alla traduzione, a ogni richiesta vengono inviati anche gli ultimi 6 scambi della conversazione.

## Quante traduzioni si possono fare gratis

Il piano gratuito di Gemini non conta i token ma le richieste, separatamente per ogni modello. I limiti non sono pubblicati: quelli visti nel 2026 sono circa 15 al minuto e 500 al giorno per i modelli Flash-Lite, e 5 al minuto e 20 al giorno per i modelli Flash. Il conteggio riparte a mezzanotte, ora della California (le 9:00 in Italia con l'ora legale, le 8:00 con quella solare).

Per consumare meno:

- **Catena di modelli.** Con "Veloce" l'app usa prima i Flash-Lite (`gemini-3.5-flash-lite`, poi `gemini-3.1-flash-lite`), poi i Flash. Con "Più accurata" parte dai Flash. Quando un modello risponde 429 l'app legge nell'errore quale limite è finito. Se è quello giornaliero, salta quel modello fino al giorno dopo. Se è quello al minuto, lo salta per circa un minuto e, se tutti sono fermi, aspetta da sola e riprova. Un modello che non esiste più (404) viene saltato.
- **Modelli trovati da soli.** "Salva e prova" non fa più una traduzione di prova: chiede a Google l'elenco dei modelli (`models.list`), che controlla la chiave senza consumare traduzioni, e costruisce la catena con i modelli `gemini-X.Y-flash-lite` e `gemini-X.Y-flash` che la chiave può usare, dal più nuovo. L'elenco si aggiorna da solo ogni tre giorni.
- **Frasi pronte.** Circa 170 frasi comuni per una riunione di famiglia (saluti, a tavola, auguri, capirsi…), già tradotte in darija, con le forme giuste per uomo, donna o più persone. Non usano internet né la chiave. Se scrivi una di queste frasi, l'app la riconosce e non chiama Gemini.
- **Memoria.** Una frase scritta che è già stata tradotta, con le stesse impostazioni, viene ripresa dalla memoria (ultime 300). Il pulsante "Traduci con Gemini" la ritraduce comunque.
- Nelle impostazioni, "Consumo di oggi" mostra quante richieste ha fatto ogni modello e quali sono finiti.

Se servono più traduzioni, basta attivare la fatturazione su AI Studio: i limiti diventano molto più alti e una traduzione costa meno di un centesimo. Con il piano a pagamento, inoltre, Google non usa i testi per migliorare i suoi prodotti.

## Metterla online (GitHub Pages)

1. Carica tutti i file di questa cartella nella radice del repository.
2. Vai in Settings → Pages → Build and deployment → Source: "Deploy from a branch" → branch `main`, cartella `/ (root)` → Save.
3. Dopo circa un minuto l'app è su `https://<utente>.github.io/<repo>/`.

Il microfono funziona solo su `https://`. Se apri il file direttamente dal telefono o dal computer, il microfono non parte.

## Sull'iPhone

Apri l'indirizzo con Safari, poi Condividi → "Aggiungi alla schermata Home". Alla prima registrazione Safari chiede il permesso per il microfono.

## File

- `index.html`: tutta l'app (HTML, CSS e JS).
- `manifest.webmanifest`, `icon-192.png`, `icon-512.png`, `apple-touch-icon.png`: servono per installarla sulla schermata Home.
