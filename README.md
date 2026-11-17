# Presenze FIRE – versione 3

Web app per registrare entrate e uscite leggendo i QR code delle tessere, anche **senza rete**. Più telefoni inviano le scansioni a un foglio Google, che calcola in tempo reale le presenze e le ore per l'attestato.

## Novità della versione 3 (app 2.1.1)

- **Reinvia tutte le scansioni (app 2.1.1):** in ⚙ → Scansioni, per recuperare scansioni mancanti nel foglio senza creare doppioni.
- **Correzione importante (script 3.1):** con più telefoni che inviavano nello stesso momento, alcune scansioni potevano essere sovrascritte nel foglio, anche se il telefono le dava per inviate. Ora lo script completa ogni scrittura prima di passare all'invio successivo e conferma al telefono solo le scansioni che risultano davvero nel foglio. Quelle non confermate restano "da inviare" e il telefono le rimanda.

- **Arrotondamento senza orari da programma:** se in Config mancano inizio e fine del giorno, le ore vengono comunque arrotondate ai 5 minuti, senza tetto. Prima il tetto era la prima e l'ultima scansione del giorno, e chi era entrato per primo e uscito per ultimo non veniva arrotondato.
- **Durate prefissate per l'attestato:** per esempio 6 ore. Se al totale mancano al massimo 20 minuti (valore impostabile), il totale diventa 6:00. La colonna **Note** di Presenze lo segnala.
- **Controllo QR:** si passano le tessere una dopo l'altra. L'app dice subito se numero, nome e cognome corrispondono all'Elenco e se il QR è già stato letto. Il foglio **Controllo QR** mostra chi non è stato letto, i QR letti più volte, i QR con un nome diverso (vecchi) e i numeri non in elenco.
- **Nomi dall'app:** per un QR con il solo numero, l'app permette di inserire cognome e nome, anche senza rete. Lo script li scrive nell'Elenco e tiene traccia nel foglio **Assegnazioni**.
- **Prove sullo stesso foglio:** il comando **Archivia e svuota le scansioni di prova** prepara il foglio per l'evento vero.

## Novità della versione 2

- **Presenze:** il foglio è unico. Contiene tutte le persone dell'Elenco e, in fondo, i numeri non in elenco. Si aggiorna in tempo reale (ogni 15 secondi se ci sono novità) ed è protetto da scrittura.
- **Ore per l'attestato:** si calcolano dalle coppie entrata/uscita, limitate agli orari di **inizio e fine da programma**, impostabili per 3 giorni. Il totale è arrotondato **ai 5 minuti per eccesso**, ma mai oltre la durata della giornata.
- **Correzioni con traccia:**
  - foglio **Correzioni** per gli eventi mancanti (per esempio l'uscita) e per annullare scansioni sbagliate;
  - colonna **Ore corrette** nell'Elenco.
- **QR nel formato `numero|nome|cognome`.** Restano validi `numero|nome cognome` e il solo `numero`.
- **Numeri di riserva:** righe dell'Elenco con il numero e senza nome. L'app le mostra in blu. Per assegnarle basta scrivere il nominativo nell'Elenco.
- **App:**
  - si collega inquadrando il QR di configurazione, anche su iPhone;
  - avvisa se l'orologio del telefono è sbagliato;
  - ha il pulsante "Invia file (email, WhatsApp…)";
  - se l'evento scelto viene tolto dal foglio, chiede di sceglierne un altro.

## Cosa c'è nello zip

| Cartella | Dove va |
|---|---|
| `1-foglio-Google/` – `Codice.gs`, `Logica.gs` | Da incollare nello script del foglio Google (Estensioni → Apps Script) |
| `2-GitHub/` – la web app | Da caricare nel repository GitHub (cartella principale) |
| `3-prova/` – `elenco_di_prova.csv`, `qr_di_prova_v2.pdf` | **Solo sul computer**: contiene nomi, non va pubblicata |

---

## A0. Aggiornare dalla versione 2

1. **Script.** In Estensioni → Apps Script sostituisci tutto il contenuto di `Codice.gs` e di `Logica.gs`, poi salva.
2. **Fogli.** Ricarica il foglio e scegli **Presenze → 1. Prepara / aggiorna fogli**. Vengono aggiunti il foglio Assegnazioni e, in Config, le voci **Durate attestato (ore)** e **Durate attestato aggiunta massima (minuti)**. Le spiegazioni in Config vengono aggiornate.
3. **Nuova versione dello script:** Esegui il deployment → Gestisci deployment → matita → **Nuova versione**. Senza questo passo i telefoni continuano a usare lo script vecchio, e i nomi inseriti dall'app non arrivano.
4. **GitHub.** Carica i file della cartella `2-GitHub/` sovrascrivendo quelli esistenti. Sono cambiati `index.html`, `app.js`, `sw.js` e `README.md`.
5. **Telefoni.** Apri l'app con la rete, anche due volte. In ⚙, in fondo, deve comparire **Versione app 2.1.1**.

## A. Aggiornare dalla versione 1

1. **Script.** In Estensioni → Apps Script sostituisci tutto il contenuto di `Codice.gs` e di `Logica.gs` con i file nuovi, poi salva.
2. **Fogli.** Ricarica il foglio Google e scegli **Presenze → 1. Prepara / aggiorna fogli**. Google chiederà di nuovo l'autorizzazione, perché lo script ora può programmare l'aggiornamento automatico. L'avviso finale elenca cosa è stato cambiato:
   - colonne nuove nell'Elenco;
   - arrotondamento a 5 minuti;
   - nuove voci Giorno in Config;
   - nuovo foglio Correzioni;
   - foglio "Non in elenco" tolto;
   - Presenze e Scansioni protetti.
3. **Eventi.** Per la prova lascia solo due righe: `entrata | Entrata | entrata` e `uscita | Uscita | uscita`. La colonna "Sessione" non serve più e puoi cancellarla.
4. **Config.** Compila Giorno 1 **data** (gg/mm/aaaa), **inizio** e **fine** da programma (hh:mm).
5. **Nuova versione dello script.** Vai in Esegui il deployment → Gestisci deployment, clicca la matita, scegli **Nuova versione** ed esegui il deployment. L'indirizzo `/exec` non cambia.
6. **GitHub.** Carica i file della cartella `2-GitHub/` sovrascrivendo quelli esistenti (Add file → Upload files). GitHub Pages si aggiorna in un paio di minuti.
7. **Telefoni.** Apri l'app con la rete. Si aggiorna da sola; a volte servono due riaperture. In ⚙, in fondo, deve comparire **Versione app 2.0.0**.
8. **Ripartire puliti (facoltativo).** Le scansioni di prova della versione 1 restano nel foglio Scansioni. Il proprietario del file può cancellarle (righe dalla 2 in giù) e lanciare **Elabora adesso**. Sui telefoni, a scansioni inviate, scegli ⚙ → Cancella dati del telefono, poi Aggiorna elenco.

## B. Prima installazione

1. **Foglio Google**, con l'account di lavoro:
   - crea il foglio e apri Estensioni → Apps Script;
   - incolla `Codice.gs` e aggiungi un file Script `Logica` con `Logica.gs`, poi salva;
   - ricarica il foglio e scegli **Presenze → 1. Prepara / aggiorna fogli** (all'autorizzazione: Avanzate → Vai a … → Consenti).
2. **Elenco.** Inserisci Numero, Cognome, Nome, eventualmente le righe di riserva (solo numero) e le colonne Ore corrette e Motivo correzione, che restano vuote. Incollando dopo il punto 1, gli zeri iniziali (001) restano.
3. **Eventi e Config:** come ai punti A.3 e A.4.
4. **Pubblicazione dello script:**
   - Esegui il deployment → Nuovo deployment → **App web**, con "Esegui come: **Me**" e "Chi può accedere: **Chiunque**";
   - copia l'URL che finisce con `/exec` in Config, alla voce **Indirizzo script**.
5. **GitHub:**
   - crea un repository pubblico e carica i file di `2-GitHub/`;
   - vai in Settings → Pages → *Deploy from a branch*, branch `main`, cartella `/ (root)`;
   - copia l'indirizzo **…github.io/…** (non quello github.com) in Config, alla voce **Indirizzo app**.

## C. Collegare un telefono

1. Nel foglio scegli **Presenze → 2. Mostra link di configurazione**: compare un QR.
2. **Android**, la prima volta: inquadra il QR con la fotocamera del telefono e apri il link in **Chrome**, poi dal menu ⋮ scegli **Installa app**.
3. **iPhone**: apri l'indirizzo dell'app in Safari, poi Condividi → **Aggiungi alla schermata Home**, e apri l'app dall'icona.
4. Nell'app tocca **Tocca per iniziare** e inquadra il QR di configurazione con la fotocamera dell'app. Conferma e dai un nome al telefono.

Per collegare il telefono a un altro foglio basta inquadrare il QR di configurazione di quel foglio. Prima però vanno inviate le scansioni in sospeso.

## D. Uso dell'app

Tocca l'evento (Entrata/Uscita), poi inquadra le tessere.

| Segnale | Significato |
|---|---|
| bip breve | QR letto |
| **verde** | In elenco: registrata |
| **giallo** | Già letto per questo evento oggi: registrata di nuovo |
| **blu** | Numero di riserva non ancora assegnato: registrata |
| **arancione** | Numero non in elenco: registrata (in Presenze compare in fondo) |
| **rosso** | QR estraneo o nessun evento scelto: **non** registrata |

Su Android ogni esito ha anche una vibrazione diversa. Su iPhone ci sono solo suono e colore.

- **Inserisci cognome e nome:** dopo la lettura di un QR con il solo numero (riserva in blu, oppure numero non in elenco senza nome), sotto l'esito compare il pulsante **Inserisci cognome e nome**. Il nome resta sul telefono, viene usato subito e parte verso il foglio con il prossimo invio. Vedi § F2.

- **Barra rossa in alto:** l'orologio del telefono è sfasato di oltre 2 minuti rispetto a Google. Imposta data e ora automatiche.
- **Annulla ultima:** annulla l'ultima scansione del telefono. Non la cancella: lascia traccia nel foglio.
- **Invio al foglio:** automatico ogni 30 secondi e al ritorno della rete; manuale con il pulsante **Invia**.
- **Impostazioni (⚙):**
  - **Invia file (email, WhatsApp…)**: manda il CSV delle scansioni del telefono; senza rete il messaggio parte quando la rete torna;
  - **Salva file nel telefono**: lo stesso file nei Download, come copia di sicurezza;
  - **Reinvia tutte le scansioni al foglio**: se nel foglio manca qualche scansione che il telefono dà per inviata. Il foglio scarta quelle che ha già e aggiunge solo le mancanti; alla fine l'app dice quante ne ha aggiunte;
  - aggiornamento dell'elenco e ultime scansioni.

## E. Il foglio Presenze

Contiene una riga per ogni persona dell'Elenco, nello stesso ordine, poi i numeri **non in elenco** (in arancione) con cognome e nome presi dal QR.

| Colonna | Contenuto |
|---|---|
| Numero, Cognome, Nome, Stato | Stato: vuoto, `riserva` oppure `NON IN ELENCO` |
| Entrata, Uscita | Orari. Se ci sono più letture: Entrata 1, Entrata 2… In **azzurro** quelli inseriti a mano. Con più giorni le colonne iniziano con la data |
| Minuti calcolati, Ore calcolate | Calcolo automatico, già arrotondato |
| Ore corrette | Dall'Elenco, se compilate (in giallo) |
| **Ore attestato**, **Minuti attestato** | Ore corrette se presenti, altrimenti quelle calcolate |
| Anomalie | In rosso: entrata senza uscita, uscita senza entrata, due entrate o due uscite di fila |
| Note | Per esempio "portato a 6:00 (+15 min)" quando si applica una durata prefissata |

**Regole del calcolo,** per ogni giorno:
- **Intervalli:** ogni entrata apre un intervallo, la successiva uscita lo chiude.
- **Limiti:** se in Config ci sono inizio e fine del giorno, ogni intervallo è limitato tra questi orari: chi entra prima dell'inizio conta dall'inizio, chi esce dopo la fine conta fino alla fine. Se mancano, nessun limite.
- **Letture doppie:** due entrate (o due uscite) di fila a meno di 10 minuti valgono come una sola, e conta la prima. Oltre i 10 minuti conta comunque la prima, ma la riga viene segnalata tra le anomalie.
- **Dati mancanti:** un'entrata senza uscita non conta finché non si aggiunge l'uscita in **Correzioni**.
- **Arrotondamento:** il totale del giorno è arrotondato per eccesso a 5 minuti. Se inizio e fine sono in Config, non oltre la durata della giornata; altrimenti senza tetto. Con più giorni si sommano.
- **Durate prefissate** (facoltative): in Config, **Durate attestato (ore)** per esempio `6` oppure `6; 12` oppure `7:30`, e **Durate attestato aggiunta massima (minuti)**, per esempio `20`. Sul totale di tutti i giorni: se alla durata più vicina sopra il totale mancano al massimo quei minuti, il totale diventa quella durata. Per esempio 5:45 diventa 6:00, mentre 5:35 resta 5:35. Non si supera mai la durata del programma, se tutti i giorni hanno inizio e fine in Config. Le Ore corrette nell'Elenco restano sempre come le scrivi.

**Ordinare o filtrare senza modificare:** usa **Dati → Crea visualizzazione filtro**.

## F. Correzioni

Le scansioni originali non si toccano mai. Nel foglio **Correzioni** ogni riga è una correzione, e la colonna **Esito** la scrive lo script.

| Numero | Data | Ora | Evento | ID da annullare | Motivo |
|---|---|---|---|---|---|
| 4 | 12/11/2026 | 17:30 | Uscita | | uscita non letta, confermata da … |
| | | | | 3f2a…-… | scansione fatta per errore |

- La **data** si può omettere se c'è un solo giorno.
- L'**evento** va scritto come in Eventi, con il codice o l'etichetta.
- Per **annullare** una scansione copia il suo ID dal foglio Scansioni.
- Per correggere direttamente le **ore** di una persona compila nell'Elenco **Ore corrette** (`3:30` oppure `3,5`) e **Motivo correzione**.

## F2. Nomi inseriti dall'app (foglio Assegnazioni)

Quando l'app invia un cognome e nome per un numero:
- se nell'Elenco la riga del numero ha Cognome e Nome vuoti, lo script li scrive lì;
- se il numero non è nell'Elenco, aggiunge una riga in fondo;
- se la riga ha già un nome, **non lo sovrascrive** e annota il conflitto.

Ogni invio resta nel foglio **Assegnazioni** (protetto), con ora, telefono e **Esito**: "scritto nell'Elenco (riga 12)", "aggiunto all'Elenco", "NON scritto: nell'Elenco c'è già …". Se due telefoni assegnano lo stesso numero a persone diverse, vale il primo arrivato e il secondo compare come conflitto da risolvere a mano.

## F3. Controllo QR (tessere stampate)

Serve prima dell'evento per controllare che tutte le tessere siano state stampate, che non ci siano QR doppi e che non ci siano QR vecchi, cioè con un numero abbinato a un altro nome.

1. Nel foglio **Eventi** aggiungi la riga `controllo | Controllo QR | controllo`. Sui telefoni, dopo Aggiorna elenco, compare il pulsante **Controllo QR**.
2. Scegli **Controllo QR** e passa le tessere una dopo l'altra, anche con più telefoni.
3. Le letture di controllo **non contano per le presenze**.

| App | Significato |
|---|---|
| **verde** OK | Numero in elenco e nome uguale. Va bene anche un QR con il solo numero o un numero di riserva |
| **giallo** GIÀ LETTO | Questo telefono ha già letto lo stesso numero: tessera passata due volte o QR stampato due volte |
| **rosso** NOME DIVERSO | Il nome nel QR non è quello dell'Elenco: tessera vecchia, da ristampare |
| **arancione** NON IN ELENCO | Il numero non c'è nell'Elenco |

Nel foglio **Controllo QR** (protetto, aggiornato come Presenze) c'è una riga per ogni persona dell'Elenco con Esito, Letture, Nome nel QR, Telefoni e Ultima lettura. A destra c'è un riepilogo: in elenco, letti, non letti, letti più volte, nome diverso, non in elenco.
- **non letto:** tessera mancante, oppure non ancora passata;
- **letto N volte:** lettura ripetuta. Il foglio conta anche le letture di telefoni diversi, che l'app non può vedere.

Il confronto dei nomi ignora maiuscole, spazi doppi e ordine delle parole.

A controllo finito togli la riga `controllo` da Eventi, così il pulsante sparisce dai telefoni. Il foglio Controllo QR resta.

## F4. Fare una prova sullo stesso foglio

Non serve un ambiente di prova separato.
1. Fai le prove sul foglio vero, con i telefoni veri.
2. Prima dell'evento scegli **Presenze → Archivia e svuota le scansioni di prova**. Scansioni, Correzioni e Assegnazioni vengono copiati in fogli di archivio nascosti (Visualizza → Fogli nascosti) e svuotati.
3. Elenco, Eventi e Config restano come sono. Se nelle prove i telefoni hanno inserito nomi nell'Elenco, il comando li elenca e chiede se toglierli. Con **Sì**:
   - i numeri di riserva tornano senza nome;
   - le righe aggiunte dai telefoni vengono cancellate;
   - le righe in cui nel frattempo il nome è stato cambiato a mano non vengono toccate, e il messaggio finale le elenca da controllare.
   Le righe si cercano per numero, quindi va bene anche se l'Elenco nel frattempo è stato riordinato.
4. Su ogni telefono: ⚙ → **Cancella dati del telefono** (solo dopo aver inviato tutto), poi **Aggiorna elenco**. Altrimenti il telefono ricorda le letture di prova e segnala "già letto".

In alternativa puoi fare **File → Crea una copia** del foglio per avere un ambiente separato. La copia però va pubblicata di nuovo (nuovo indirizzo /exec in Config) e i telefoni vanno collegati con il suo QR di configurazione.

## G. Aggiornamento in tempo reale

Il menu **Presenze** offre tre modi:
- **Elabora adesso**: aggiornamento immediato con un riepilogo (presenti ora, non in elenco, anomalie…).
- **Apri pannello (aggiorna ogni 15 secondi)**: finché il pannello resta aperto sul computer del proprietario, Presenze si aggiorna ogni 15 secondi se ci sono novità, e il pannello mostra i contatori.
- **Avvia aggiornamento automatico (ogni minuto)**: funziona anche a pannello chiuso e aggiorna subito all'arrivo dei dati dai telefoni. A fine evento scegli **Ferma aggiornamento automatico**. Google limita il tempo di calcolo giornaliero degli script automatici (90 minuti con un account personale, 6 ore con Workspace), ma i giri senza novità costano quasi nulla.

## H. Protezione

**Presenze**, **Scansioni**, **Assegnazioni** e **Controllo QR** sono protetti: può modificarli solo il proprietario del file, cioè lo script. L'Elenco resta modificabile: lo script ci scrive solo i nomi inviati dall'app nelle righe vuote. Condividi il file con i colleghi in sola lettura, oppure come editor se devono compilare Elenco e Correzioni.

Il pannello va aperto dal proprietario.

## Problemi frequenti

- **"Chiave non valida":** ricollega il telefono inquadrando di nuovo il QR di configurazione.
- **Il link di configurazione apre GitHub:** in Config, alla voce Indirizzo app, c'è l'indirizzo github.com; serve quello …github.io.
- **Nessun evento:** controlla il foglio Eventi, poi nell'app scegli ⚙ → Aggiorna elenco.
- **Modifiche allo script senza effetto:** serve la **Nuova versione** del deployment (punto A.5).
- **App non aggiornata:** chiudila e riaprila con la rete (anche due volte). La versione è indicata in ⚙.

## Limiti di questa versione

- È stata provata con fotocamera simulata, un server di prova e un ambiente Google simulato. La versione 2 è stata usata con telefoni e foglio veri; le novità della versione 3 vanno ancora provate dal vero.
- Controllo QR: l'app vede solo le letture fatte da lei stessa; quelle di tutti i telefoni insieme sono nel foglio Controllo QR.
- iPhone: senza vibrazione; i dati vanno inviati o esportati entro pochi giorni.
- Attestati PDF ed email: da fare, in un file separato.
