# Joinware — sito GitHub Pages

Sito statico responsive pronto per GitHub Pages. Non contiene segreti. Catalogo, filtri, schede prodotto e carrello funzionano nel browser; il pagamento usa una piccola funzione esterna che crea una sessione Stripe Checkout.

## Prima della pubblicazione

1. In `data/products.js`, verificare il catalogo importato dal sito pubblico e inserire per ogni prodotto lo `stripePriceId` reale. I prezzi sono quelli pubblicati nelle varianti del catalogo Joinware al momento della generazione del pacchetto.
2. I testi di Termini e condizioni, Privacy policy, Cookie policy, Limitazioni di responsabilità, Limite al risarcimento, disclaimer e dati societari sono stati riportati dalla versione pubblica attuale. Non modificarli senza approvazione legale.
3. In `config.js`, inserire l’URL HTTPS dell’endpoint Stripe nel campo `checkoutEndpoint`.
4. Pubblicare il contenuto di questa cartella nella root del repository GitHub, quindi attivare **Settings → Pages → Deploy from a branch**.
5. Se si usa il dominio `www.joinware.it`, configurarlo nelle impostazioni Pages e aggiornare, se necessario, `robots.txt`, `sitemap.xml` e gli URL di successo/annullamento.

## Configurazione Stripe

1. Nel Dashboard Stripe creare un **Product** e un **Price** una tantum in EUR per ogni licenza. Copiare ogni `price_…` in `data/products.js`.
2. Distribuire `stripe-worker/worker.js` su Cloudflare Workers (o tradurre lo stesso endpoint per un altro servizio serverless).
3. Salvare `STRIPE_SECRET_KEY` come secret del Worker, mai in GitHub. Configurare inoltre `ALLOWED_ORIGIN=https://www.joinware.it` e `ALLOWED_PRICE_IDS` come lista separata da virgole dei Price ID vendibili.
4. Aggiungere in Stripe un webhook per `checkout.session.completed` verso un endpoint riservato alla gestione ordini, oppure usare le e-mail di pagamento riuscito del Dashboard per il flusso manuale. Verificare sempre il pagamento in Stripe prima di inviare la key.
5. Attivare le e-mail cliente, raccogliere l’indirizzo di fatturazione e impostare i dati fiscali richiesti. Provare prima in modalità test con le chiavi `sk_test_…`.

La chiave pubblicabile Stripe non è necessaria: il browser viene reindirizzato all’URL sicuro generato dalla Checkout Session. La chiave segreta resta esclusivamente nell’ambiente serverless.

## Test locale

Aprire la cartella con un piccolo server statico (non direttamente con `file://`) e controllare home, filtri, schede prodotto, carrello e responsive. Il checkout mostra un messaggio di configurazione finché `checkoutEndpoint` e i Price ID non sono impostati.
