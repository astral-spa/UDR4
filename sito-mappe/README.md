# Sito della Rete UdR 4

Sito con mappa interattiva e quadri orari della rete (Schiaffini, Castelli Romani), generato dal GTFS. È un unico
file HTML autonomo (la libreria delle mappe Leaflet è inclusa): si apre con un doppio clic o si pubblica così com'è.
Grafica e funzioni sono le stesse dei siti delle altre UdR (base: UdR 11).

## Funzioni

- **Titolo cliccabile**: «Rete bus UdR 4» riporta alla mappa iniziale con tutte le linee (resta il giorno scelto).
- **Elenco linee per comune**: il comune è quello di `route_long_name` del GTFS, con la grafia ufficiale («Genzano» →
  Genzano di Roma, «Montecompatri» → Monte Compatri, «Rocca Di Papa» → Rocca di Papa, «Intercomunale» → Linee
  intercomunali; `AREE_NOMI` in testa a `build_data.py`). Il numero della linea è il codice senza la sigla del comune
  (`Albn1_sc_m` → **1 sc m**, `CmpnS_2B` → **S 2B**; le intercomunali `LA…` restano intere, `SIGLE_LINEA`).
- **Nome delle linee**: il GTFS non lo indica, per cui si ricava dai capolinea del percorso più frequente
  («Piazza Giuseppe Mazzini – Corso Giacomo Matteotti»); per le circolari, capolinea e fermata più lontana
  («Circolare P.zza Garibaldi – Via dei Fienili»). Con nomi di fermata definitivi nel GTFS cambiano da soli.
- **Calendari**: quelli del GTFS (`calendar.txt`) con le eccezioni di `calendar_dates.txt` (festività e vacanze
  scolastiche). Etichette in `ETICHETTE`: *Scolastico lunedì–venerdì*, *Scolastico lunedì–sabato*, *Non scolastico*,
  *Domenica e festivi*.
- **Giorno**: si sceglie una data; prima dell'avvio del servizio (30/11/2026) si apre sul primo giorno.
- **Mappa**, **quadro orario**, **Vicino a me** e **Bus in viaggio** come negli altri siti.
- **Tempo reale** (solo per il giorno di oggi, sul sito online): dal proxy (`REALTIME`; codice nel repository UDR11,
  cartella `proxy-realtime/`) ritardi, prossima fermata evidenziata e bus «In arrivo» alle fermate, abbinati col
  `trip_id` del GTFS. Nel proxy (Cloudflare) `AGENCY_ID` deve comprendere l'agenzia dell'UdR 4 (nel GTFS
  `UdR4Schiaffini`). Se il proxy non risponde restano gli orari programmati.
- **Statistiche visite**: GoatCounter (account `andreaantonini`, senza cookie), percorso fisso `/UDR4/`.

**Nomi delle fermate**: «Via Olivella, Albano Laziale (1)» diventa «Via Olivella (1)» con il comune Albano Laziale
sotto il nome (anche le frazioni, per esempio Cecchina); le fermate senza comune nel nome prendono quello della
fermata più vicina entro 3 km (`NOMI_COMUNI`, `FRAZIONI`). **Fermate principali**: il GTFS non ha `timepoint`, per
cui sono i capolinea, la prima fermata in ogni comune, i nodi serviti da almeno 4 altre linee e una fermata almeno
ogni 5 minuti di viaggio.

## File

- `template.html`: grafica e funzioni del sito, con i segnaposto per i dati.
- `logo.png`: logo Astral Infomobilità al centro della testata (incluso nella pagina).
- `icona.png`: icona del sito (logo Unità di Rete, 180×180) per la scheda del browser e la schermata Home del
  telefono, inclusa nella pagina; per cambiarla basta sostituire il file.
- `build_data.py`: legge il GTFS e scrive il sito completo in `sito-mappe/index.html` (pubblicato online) e
  `index.html` nella radice del repo.

Online (GitHub Pages) viene pubblicato **solo** `index.html`.

## Pubblicazione (la prima volta)

In *Settings → Pages*, alla voce *Build and deployment → Source* scegliere **GitHub Actions**. Poi in *Actions*
aprire *Pubblica sito mappe* e premere *Run workflow* (oppure basta il prossimo commit su `main`).
L'indirizzo del sito sarà https://astral-spa.github.io/UDR4/ (o il dominio proprio, es. udr4.astralspa.it).

## Aggiornare con un nuovo GTFS

**Da GitHub**: *Add file → Upload files*, trascina il nuovo zip GTFS e fai *Commit changes* su `main`: il sito si
rigenera e ripubblica in un paio di minuti. Si può caricare in anticipo: ogni GTFS vale dalla data di avvio del suo
calendario (la `start_date` più bassa di `calendar.txt`); i vecchi zip si cancellano quando il loro periodo è passato.

**Dal computer** (serve solo Python 3): `python3 sito-mappe/build_data.py [percorso/GTFS.zip]`
