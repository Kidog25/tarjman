# Tarjman

Interprete tascabile italiano ⇄ darija marocchino. Registri chi parla, l'app trascrive e traduce, e la frase in darija si può mostrare a schermo intero o far leggere ad alta voce dal telefono.

## Come funziona

- È un'unica pagina statica (`index.html`), senza build e senza server.
- La registrazione usa il microfono del browser (Web Audio). L'audio viene convertito in WAV mono a 16 kHz, con un massimo di 60 secondi.
- L'audio va direttamente dal telefono all'API di Gemini (`generateContent`, modello `gemini-2.5-flash`). Se quel modello non esiste più, l'app passa da sola a `gemini-flash-latest`. Una sola chiamata fa sia la trascrizione sia la traduzione, e restituisce un JSON con uno schema fisso.
- La chiave API di Gemini la inserisce l'utente nelle impostazioni e resta nel `localStorage` del telefono. Nel codice non c'è nessun segreto, quindi il repository può essere pubblico.
- Per dare più contesto alla traduzione, a ogni richiesta vengono inviati anche gli ultimi 6 scambi della conversazione.

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
