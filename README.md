Prosjektplan – Investlytics Hub
============================================

![](investlytics-hero.png)

Introduksjon og Målsetninger
----------------------------

Investlytics Hub er en planlagt plattform for kryptoanalyse med **hovedfokus på kryptovaluta-trading, sanntids markedsdata, integrasjoner mot sentraliserte og desentraliserte børser (CEX/DEX)** samt **AI-drevet analyse**. Plattformen skal være et alt-i-ett verktøy der tradere og investorer kan overvåke markedene i sanntid, analysere porteføljer på tvers av børser, og få prediktiv innsikt ved hjelp av kunstig intelligens (AI). I tillegg vil løsningen støtte visning av digitale eiendeler (f.eks. NFT-er) som en valgfri del.

**Hovedmål for Investlytics Hub:**

*   **Sanntids markedsdata:** Aggregere og vise kurser, ordrebok og volum i sanntid fra flere kryptobørser (CEX/DEX) i ett brukergrensesnitt.
    
*   **Trading og porteføljeoversikt:** Tilrettelegge for trading-relatert innsikt – som prisdiagrammer, tekniske indikatorer og personlig porteføljeoversikt på tvers av børser – slik at brukeren kan følge med på egen beholdning og ytelse.
    
*   **CEX/DEX-integrasjoner:** Integrere mot eksterne API-er fra sentraliserte børser (CEX) og desentraliserte børser (DEX) for å hente markedsdata og (etter hvert) støtte handel eller porteføljesynkronisering via API-nøkler/lommebøker.
    
*   **AI-drevet analyse:** Utnytte AI for prediktiv analyse og sentimentanalyse innen kryptomarkedet. For eksempel tilby prognoser om prisutvikling eller stemningsindikatorer basert på nyheter og sosiale medier. (En dedikert API-endepunkt, f.eks. /api/v1/ai/crypto\_predict, skal implementeres for å levere slike AI-baserte innsikter til frontenden.)
    
*   **Støtte for digitale eiendeler:** Inkludere grunnleggende visning av digitale eiendeler (NFT-er) knyttet til brukerens portefølje som en _tilleggsmulighet_. Denne delen holdes enkel i MVP, med potensiale for utvidelse senere, slik at kjernefokuset forblir på kryptotrading og analyse.
    

Prosjektomfang og MVP
---------------------

Denne prosjektplanen omfatter design og utvikling av Investlytics Hub fra konsept til lansering. **Minimumsproduktet (MVP)** vil konsentrere seg om kjernefunksjonene knyttet til kryptotrading og markedsdata, mens mer komplekse eller sekundære funksjoner utsettes til senere faser. Nedenfor presiseres omfanget for MVP vs. senere faser:

*   **Inkludert i MVP:**
    
    *   **Sanntids kursovervåkning:** Live oppdatering av kryptovalutakurser og enkle diagrammer for utvalgte tradingpar.
        
    *   **Markedssammendrag:** Vise nøkkeldata (f.eks. topp 10 kryptovalutaer, markedsverdi, 24t volum) innhentet fra en tjeneste som CoinGecko.
        
    *   **Basis CEX-integrasjon:** Hente data (og eventuelt brukerportefølje) fra minst én sentralisert børs via API (f.eks. Binance eller Coinbase Pro) for å demonstrere konseptet.
        
    *   **Enkel porteføljefunksjonalitet:** La brukeren registrere eller importere sin kryptoportefølje og se totalverdi, fordeling per valuta osv. (Evt. ved å koble en API-nøkkel til en børs for å hente balansen automatisk, eller manuelt legge inn beholdning).
        
    *   **AI-drevet innsikt (grunnleggende):** Integrere en første versjon av AI-analyse, f.eks. en prisprognose for en valgt kryptovaluta eller en enkel sentimentanalyse basert på nyhetsheadlines. Dette leveres via et API-endepunkt på backend (f.eks. /api/v1/ai/crypto\_predict) som benytter en forhåndstrent modell eller tredjeparts AI-tjeneste.
        
    *   **Visning av digitale eiendeler (NFT-er):** MVP’en vil inkludere mulighet for brukeren til å se en enkel oversikt over sine digitale eiendeler (f.eks. NFT-er) – for eksempel en liste over NFT-er knyttet til brukerens lommebok eller pNFT-prosjekt – men uten avansert funksjonalitet (kun visning av navn/bilde og eventuell verdi). Denne delen implementeres kun dersom tid tillater, og i så fall som en isolert modul slik at den ikke påvirker resten av systemet om den må deaktiveres.
        
*   **Utenfor MVP / fremtidige faser:**
    
    *   **Avanserte NFT-funksjoner:** Kjøp/salg av NFT-er, minting, full integrasjon med markedsplasser eller kompleks gallerivisning er utelatt fra MVP. Slike _digitale eiendel_\-funksjoner kan vurderes i senere faser dersom det blir prioritert, men MVP holder det på et minimum (kun enkel visning).
        
    *   **Flere børsintegrasjoner og handel:** Selv om MVP kobler til minst én børs for data (og eventuelt portefølje), vil støtten for **flere** CEX-er og desentraliserte børser (DEX-er) samt faktisk handelsutførelse via API (plassere kjøp/selg-ordrer) bli bygget ut gradvis etter MVP. Initialt fokuserer vi på datainnhenting og analyse, mens ordreutførelse og kompleks kontohåndtering på tvers av mange børser tas i senere faser.
        
    *   **Utvidet AI og modelltrening:** Eventuell utvikling av egne AI-modeller som krever trening på store datamengder, tuning av prediksjonsmodeller spesielt for kryptomarkedet, eller dyp integrasjon av sentimentanalyse (f.eks. analyse av Twitter i sanntid) vil bli utført etter MVP. I første omgang bruker vi eksisterende modeller og tjenester for å levere AI-funksjonalitet raskt.
        
    *   **Andre utvidelser:** Omfattende funksjoner som automatiserte trading-strategier, porteføljeoptimalisering, skattekalkulasjoner, eller mobilapp-utvikling (egen native app) er foreløpig utenfor MVP og nevnes som mulige oppfølgningsprosjekter senere.
        

Ved å avgrense omfanget som over sikrer vi at MVP leverer verdifull funksjonalitet raskt, samtidig som arkitekturen utformes med tanke på fremtidige utvidelser (AI-forbedringer, flere integrasjoner, digitale eiendeler etc.). Neste seksjoner beskriver arkitekturen og utviklingsplanen i detalj, tilpasset disse prioriteringene.

Arkitektur
----------

Den oppdaterte arkitekturen for Investlytics Hub er designet for å støtte sanntidsdata, eksterne integrasjoner og AI-analyse innenfor en modulær og skalerbar struktur. Løsningen følger en klient-tjener-modell med en webbasert frontend og en Python-basert backend. Backend tilbyr et REST API (samt WebSocket for sanntidsoppdateringer) som frontenden (og eventuelle andre klienter) bruker. I tillegg er det integrasjon mot eksterne datakilder (børs-APIer, on-chain APIer, AI-tjenester). Hovedkomponentene er:

*   **Frontend:** En webklient (single-page application) skrevet i et moderne rammeverk (f.eks. React) som kjører i nettleser. Denne kommuniserer med backend over HTTPS (REST API for forespørsler/response og WebSocket for løpende oppdateringer). Frontend-en presenterer dashbord, grafer, porteføljeinformasjon og AI-innsikter til brukeren i sanntid. (Eventuell mobilapp i fremtiden kan bruke de samme API-ene.)
    
*   **Backend API:** En serverapplikasjon skrevet i Python ved hjelp av FastAPI, som håndterer forretningslogikken. Backend tilbyr ulike moduler/endepunkter:
    
    *   **Marked/Trading-modul:** Henter sanntids markedsdata fra eksterne kilder (CEX/DEX APIer, CoinGecko m.fl.), prosesserer dem (f.eks. beregne indikatorer), og leverer til frontend. Kan inkludere WebSocket-endepunkter for push av live kurser.
        
    *   **Porteføljemodul:** Tar seg av alt relatert til brukerens portefølje – lagring av hvilke aktiva brukeren eier, innhenting av oppdatert verdi (pris) for disse, integrasjon mot brukerens konto på børser via API-nøkler (for automatisk oppdatering av beholdning), og eventuelt henting av brukerens **digitale eiendeler (NFT-er)** via en on-chain data API.
        
    *   **AI-modul:** Står for AI-drevet analyse, som prediktiv modellering og sentimentanalyse. Denne modulen kan kalle ut til tredjeparts AI-tjenester (f.eks. OpenAI API eller HuggingFace pipeline) eller bruke lokale forhåndstrente modeller for å generere prognoser. Resultatene eksponeres gjennom egne endepunkter (f.eks. /api/v1/ai/crypto\_predict) som frontend kan benytte for å vise AI-insikt.
        
    *   **Database lagring:** En database (f.eks. PostgreSQL) for lagring av vedvarende data – brukerkontoer, porteføljeopplysninger, historiske data som er nødvendige for analyser (f.eks. lagrede priser for å kjøre simuleringer) og eventuelle logger. Databaseabstraksjon håndteres via et ORM eller queries direkte i Python.
        
    *   **Autentisering og sikkerhet:** Håndtering av brukerpålogging, API-nøkler og tilgangskontroll. For eksempel kan JSON Web Tokens (JWT) benyttes for å sikre API-ene, og sensitive nøkkelverdier (børs-API-nøkler, etc.) krypteres i databasen.
        
*   **Eksterne integrasjoner:** Backend vil kommunisere med en rekke eksterne tjenester:
    
    *   **CEX APIer:** for å hente markedsdata (priser, orderbok) og brukerdata (balanser, transaksjoner) fra sentraliserte børser. Et bibliotek som CCXT kan brukes for å standardisere integrasjon mot mange børser.
        
    *   **DEX/on-chain data APIer:** for å hente on-chain informasjon, som token-beholdninger i en lommebok, transaksjoner på blokkjeden og NFT-data. Her planlegges bruk av tjenester som **Moralis** eller **Bitquery** som forenkler uthenting av blokkjede-data via API. Disse vil være viktige spesielt for porteføljefasen (Fase 4) der on-chain analytics inngår.
        
    *   **Markedsdata-aggregatorer:** f.eks. **CoinGecko API** for generelle kryptodata (priser, markedsverdi, historikk) som supplement til direkte børsintegrasjoner.
        
    *   **AI-tjenester:** f.eks. **OpenAI API** for å utføre språkbasert analyse (som nyhetssentiment) eller til og med for å generere prisprediksjoner basert på trenede modellert, samt mulighet for å integrere **HuggingFace-modeller** for sentimentanalyse av sosiale medier. Hvis nødvendig kan et ML-rammeverk som PyTorch brukes bak kulissene, men i første omgang benyttes eksterne AI-tjenester for rask implementering.
        

Arkitekturskisse som illustrerer samspillet mellom frontend, backend og eksterne komponenter:

```mermaid
flowchart TD
    %% ─────────────────────────
    %% 1) User-facing layer
    %% ─────────────────────────
    subgraph UserFacing["Brukergrensesnitt"]
        FE["Web-app (React + TypeScript / Tailwind)<br/>• Recharts / D3 for grafer<br/>• Deploy: Vercel / Cloudflare Pages"]
    end

    %% ─────────────────────────
    %% 2) Core services
    %% ─────────────────────────
    subgraph CoreServices["Kjernetjenester"]
        BE["Backend API (FastAPI + SQLAlchemy)<br/>WebSockets • REST"]
        N8N["n8n Workflows<br/>Automatisering & varsler"]
    end

    %% ─────────────────────────
    %% 3) Data & AI layer
    %% ─────────────────────────
    subgraph DataAndAI["Data & Analyse"]
        DB[(PostgreSQL)]
        RD[(Redis cache)]
        SIM["Monte-Carlo engine<br/>(portert fra mc-simulations)"]
        AI["AI-modul<br/>(OpenAI / HF Transformers)"]
    end

    %% ─────────────────────────
    %% 4) External integrations
    %% ─────────────────────────
    subgraph External["Eksterne integrasjoner"]
        CEX["CEX / DEX-APIer<br/>(Binance, Coinbase,<br/>Uniswap, ccxt …)"]
        CG["CoinGecko / Market-data"]
        OC["On-chain-API<br/>(Moralis, Bitquery …)"]
        DA["Digital assets<br/>(NFT floor-price etc.)"]
        TV["TradingView / PineScript<br/>(strategi-kilde)"]
    end

    %% ─────────────────────────
    %% Connections
    %% ─────────────────────────
    FE  -- "REST + WS" --> BE

    BE  --> DB
    BE  --> RD
    BE  --> SIM
    BE  --> AI

    %%  ↔  External calls
    BE  -- "market-prices"  --> CG
    BE  -- "spot / futures trades" --> CEX
    BE  -- "on-chain metrics" --> OC
    BE  -- "minimal NFT data" --> DA
    BE  -- "import strategy code" --> TV

    %%  n8n orchestration
    N8N -- "Webhooks / cron" --> BE
    N8N -- "Fetch data" --> CG
    N8N -- "Trade-execution signal" --> CEX

    %%  Frontend direct wallet if needed (optional)
    FE  -. "wallet connect (Phantom / MetaMask)" .- OC

```

_Forklaring:_ Frontend-klienten (webappen) kommuniserer med FastAPI-backenden via HTTPS REST-kall for vanlige forespørsler (f.eks. hente porteføljedata) og eventuelt WebSocket for å motta sanntids kurser uten polling. Backenden henter data fra eksterne kilder: direkte fra børsers APIer for live trading-data og brukerens kontoinformasjon, fra on-chain APIer (som Moralis) for f.eks. NFT-oversikt eller transaksjonshistorikk, og fra markedsdata-APIer som CoinGecko for generell info. AI-modulen i backenden kan kalle ut til en AI-tjeneste (OpenAI, etc.) for å få f.eks. en prediksjon, som så sendes tilbake til frontend. Alle disse modulene samhandler med databasen ved behov (lagring av brukerdata, caching av nylige spørringsresultater, historiske priser for analyse etc.).

Arkitekturen er modulær slik at nye datakilder eller moduler (f.eks. flere AI-modeller, flere børser) kan legges til uten store omveltninger. FastAPI-valget gir også automatisk dokumentasjon av API-et (Swagger UI), noe som er nyttig for både utviklere og eventuelle tredjepartsintegrasjoner. Løsningen planlegges containerisert (Docker) for enkel distribusjon, og kan settes opp i skytjenester for skalering ved behov.

#### Rate Limiting

For å forhindre misbruk av API-et og sikre rettferdig ressursbruk er det implementert **rate limiting**. Dette betyr at det er satt en grense på hvor mange forespørsler en enkelt bruker eller IP-adresse kan sende til API-et per tidsenhet. Hvis grensen overskrides, vil systemet returnere en feilmelding (HTTP 429 "Too Many Requests") og midlertidig blokkere ytterligere kall. Rate limiting beskytter tjenesten mot **spam** og **Denial of Service-angrep**, samtidig som det sørger for stabil ytelse for alle brukere. Den nåværende konfigurasjonen begrenser for eksempel antall API-kall per bruker til et rimelig nivå (f.eks. X antall kall per minutt; verdien kan justeres basert på belastning). Implementasjonen er gjort på servernivå ved hjelp av et mellomvare som overvåker og teller forespørsler.

#### AI-cache

Mange av analysene og innsiktene i Investlytics Hub genereres ved hjelp av AI-modeller og komplekse beregninger. For å forbedre responstiden og redusere unødvendig belastning er det innført en **AI-cache**. Dette innebærer at resultatene fra tunge AI-spørringer blir mellomlagret i en hurtiglager for gjenbruk. Hvis en bruker gjentar en identisk forespørsel (f.eks. en bestemt porteføljerapport eller markedsanalyse) innenfor et kort tidsrom, kan systemet levere det bufrede resultatet umiddelbart i stedet for å kjøre AI-beregningen på nytt.

Cachingen er implementert slik at hver unike forespørsel (for eksempel definert av kombinasjonen av spørringsparametre eller AI-prompt) lagrer sitt resultat i minnet (eller en rask nøkkel/verdi-store) med en tidsbegrensning (TTL). Dersom de underliggende dataene (som markedspriser eller nyheter) oppdateres, vil relevante cachede resultater automatisk invaliders for å sikre at brukeren får oppdatert informasjon. **Fordelene** med AI-cachen er redusert responstid for brukeren, lavere kostnader (spesielt om eksterne AI-tjenester brukes og faktureres per kall), og mindre belastning på systemets ressurskrevende komponenter.

#### WebSocket-protokoll 
* **Endepunkt**  
  `wss://api.investlytics.io/ws/{channel}?token=<JWT>`
  *Eksempel:* `wss://api.investlytics.io/ws/prices?token=eyJhbGciOi...`

* **Meldingsformat (schema-validerte JSON-pakker)**  
  ```json
  {
    "type": "<event>",        // f.eks. "price_update", "auth_ok", "ping"
    "ts":   1717672405123,    // UNIX-tid i millisekunder
    "payload": { ... }        // Hendelsesspesifikt innhold
  }
  ``` 
Fullt schema ligger i /schemas/ws\_message.json.

*   **Autentisering** 
JWT (HS256 / RS256) sendes som token-query-parameter.– Ved gyldig token svarer serveren med{"type":"auth\_ok","ts":}– Ugyldig eller utløpt token → tilkobling lukkes med **close-code 4401**.
    
*   **Heartbeat / keep-alive** 
Server → klient: {"type":"ping","ts":} hvert **20 s**.Klient må svare innen **10 s** med{"type":"pong","ts":}.Mangel på “pong” eller manglende “ping” i **30 s** ⇒ server lukker forbindelsen (close-code 4000).
    
*   **Reconnect-logikk (eksponentiell back-off m. jitter)** 
1 s → 2 s → 4 s → 8 s → 16 s (maks) + ±0-0.5 s tilfeldig jitter.Etter **5 min** stabil forbindelse nullstilles back-off-tellen.
  
#### Støttede WebSocket-close-koder

| Kode | Betydning                 |
| ---- | ------------------------- |
| 1000 | Normal avslutning         |
| 4000 | Heartbeat time-out        |
| 4401 | Uautorisert / ugyldig JWT |
| 4408 | Ugyldig meldings-schema   |
| 4500 | Intern serverfeil         |
    
*   **Tillegg**
– permessage-deflate er aktivert for å redusere båndbredde. 
– Maks meldingsstørrelse: **64 KiB**; større meldinger gir close-code 4408. 

DevOps-strategi 
---------------

For å kunne levere endringer raskt og pålitelig, samt drifte Investlytics Hub på en stabil måte, er det utarbeidet en DevOps-strategi. Dette omfatter kontinuerlig integrasjon og utrulling av kode, samt overvåking av systemet i produksjon for å fange opp problemer tidlig. DevOps-praksisene som er innført bidrar til en **smidig utviklingsprosess** og høy **driftsstabilitet**.

#### CI/CD Pipeline 

Prosjektet benytter **Continuous Integration/Continuous Deployment (CI/CD)** for automatisk bygging, testing og utrulling av applikasjonen. Hver gang ny kode pushes til repoet:

1.  **Automatisk bygg og test:** Et CI-system (for eksempel GitHub Actions eller Jenkins) kjører en rekke skript som bygger prosjektet og kjører alle enhetstester og integrasjonstester. Dette verifiserer at ny kode ikke introduserer feil eller regressjoner. Koden gjennomgår også statisk analyse og linting for å sikre kvalitetsstandarder.
    
2.  **Kontinuerlig integrasjon:** Hvis testene passerer, blir endringene integrert og en ny byggeartifakt (f.eks. en docker-container eller et versjonsnummer) blir generert.
    
3.  **Deploy til miljøer:** Gjennom konfigurerte pipelines rulles applikasjonen automatisk ut. Vanligvis deployeres først til et **staging-miljø** der man kan gjøre slutt-testing i et produksjonslignende miljø. Når endringene er godkjent der, kan de promoteres til **produksjonsmiljøet**. Utrullingen kan være helautomatisert eller kreve manuell godkjenning basert på prosjektets behov.
    
4.  **Rollback og versjonshåndtering:** Pipeline-oppsettet inkluderer mekanismer for enkel rollback dersom en produksjonsutrulling skulle forårsake problemer. Alle versjoner er sporbare, og tidligere versjoner kan reinstalleres raskt ved behov.
    

Denne CI/CD-strategien, som er en forbedring av utviklingsprosessen, sikrer at nye funksjoner og feilrettinger kan leveres hyppig og med høy kvalitet. Den reduserer også risikoen for menneskelige feil ved utrulling, da det meste av prosessen er automatisert og forutsigbar.

#### Observabilitet og overvåking 

For å drifte Investlytics Hub proaktivt er det lagt stor vekt på **observabilitet**, det vil si applikasjonens evne til å gi innsikt i sin egen tilstand gjennom data. Dette omfatter logging, metrikk og alarmering:

*   **Logging:** Applikasjonen logger relevante hendelser og feil med tilstrekkelig detaljgrad. Viktige handlinger (som brukerinnlogging, dataspørringer, AI-kall) logges informativt, mens feil og unntak logges som advarsler eller kritiske feil. Loggene samles og aggregeres ved hjelp av et logg-rammeverk. I produksjon kan loggene sendes til en sentral loggtjeneste (f.eks. Elasticsearch/Kibana eller en skybasert loggtjeneste) for analyse og historikk.
    
*   **Metrikker:** Systemet samler inn driftsmetrikk som CPU- og minnebruk, responstid for API-kall, antall aktive brukere, antall AI-forespørsler, treffrater i AI-cachen osv. Disse metrikkene lagres og vises i sanntid på et dashboard (for eksempel Grafana, DataDog eller Azure Monitor). Ved å følge med på slike metrikk kan teamet oppdage ytelsesavvik eller kapasitetsbehov tidlig.
    
*   **Overvåking og varsling:** Det er konfigurert helsesjekker og overvåkingsrutiner som kontinuerlig sjekker at kjernefunksjonene er oppe. Dersom en tjeneste går ned eller en unormal situasjon oppstår (for eksempel vedvarende høy feilrate på API-et, eller ingen respons fra AI-tjenesten), utløses alarmer. Ansvarlige utviklere eller driftspersonell varsles umiddelbart via e-post, SMS eller Slack, slik at tiltak kan iverksettes før sluttbrukere merker stort.
    

Tilsammen gir disse observabilitets-tiltakene et detaljert bilde av hvordan Investlytics Hub kjører i praksis. **Forbedringen** gjør det mulig å reagere raskt på problemer og å forstå systemets oppførsel, noe som er kritisk for å oppnå høy oppetid og god brukeropplevelse over tid.

Sikkerhet
---------

Sikkerhet har høy prioritet i Investlytics Hub, gitt den sensitive naturen til finansielle data og brukertilliten som kreves. Prosjektet har derfor implementert flere **sikkerhetstiltak** og forbedringer for å beskytte både systemet og brukerne:

*   **HTTPS overalt:** All kommunikasjon mellom frontend-klienten og serveren skjer over kryptert HTTPS. Dette forhindrer avlytting og «man-in-the-middle»-angrep ved overføring av data (inkludert JWT og andre sensitive data).
    
*   **Sikker databasebruk:** Databaseforespørsler gjøres ved hjelp av sikre metoder (f.eks. ORM eller parametrisert SQL) for å unngå SQL-injeksjon og sikre dataintegritet.
    
*   **Passord og hashing:** Brukerpassord lagres ikke i klartekst. Ved registrering hashes passord med en sterk enveis hash-funksjon (som bcrypt eller Argon2) kombinert med salt, slik at brukerinformasjon er beskyttet selv om databasen skulle kompromitteres.
    

I tillegg til disse generelle tiltakene er det gjennomført spesifikke forbedringer på sikkerhetsfronten:


#### JWT-håndtering 

Applikasjonen benytter **JSON Web Tokens (JWT)** for autentisering og autorisasjon av brukere. JWT brukes til å sikre at kun gyldige, autentiserte brukere får tilgang til sine data og sensitive funksjoner i Investlytics Hub. Som en del av forbedringene er JWT-håndteringen gjort mer robust og sikker:

*   **Sikker signering:** Alle JWT er signert med en sterk hemmelig nøkkel (eller et asymmetrisk nøkkelpar) på serversiden. Nøkkelen holdes hemmelig i konfigurasjonen (se _Nøkkelhåndtering_ under Sikkerhet) slik at tokenene ikke kan forfalskes.
    
*   **Utløp og fornying:** Hvert JWT har et relativt kort levetid (exp claim) for å begrense hvor lenge en token er gyldig. Dette reduserer risikoen hvis en token skulle komme på avveie. Det er implementert støtte for **refresh tokens** slik at brukeren kan få nye JWT når de gamle utløper uten å måtte logge inn på nytt, alt håndtert på en sikker måte.
    
*   **Validering:** Backend validerer JWT i alle innkommende forespørsler for beskyttede endepunkter. Ugyldige eller utløpte token avvises med 401 Unauthorized. Valideringen kontrollerer signaturen, utløpstiden og nødvendige adgangsrettigheter (f.eks. roller eller tillatelser kodet inn i tokenens payload).
    
Ved å bruke JWT på denne måten får vi en **stateless** autentiseringsmekanisme som er skalerbar (serveren trenger ikke holde styr på sesjoner i minne) samtidig som sikkerheten ivaretas. Forbedringene sikrer at JWT-basert innlogging er i tråd med bransjens beste praksis, og integreres med frontend-klienten ved at token lagres trygt (for eksempel i en httpOnly cookie eller sikret lagring) for å forhindre XSS/angrep som forsøker å stjele token (jf. neste seksjon om sikkerhet).

#### XSS-beskyttelse 

For å beskytte brukerne mot **Cross-Site Scripting (XSS)**\-angrep er applikasjonen gjennomgått og herdet mot injeksjon av ondsinnet skript. XSS-angrep skjer typisk ved at angripere får lagt inn skript eller ondsinnet HTML-innhold som kjøres i andre brukeres nettlesere. Investlytics Hub unngår dette ved å:

*   **Sanitere brukerinput:** All data som brukerne sender inn (for eksempel via skjemaer, søkefelt eller kommentarer) blir validert og renset. Eventuelle farlige tegn eller skript-tagger fjernes eller kodes om før data lagres eller vises.
    
*   **Escape output:** Brukerinnhold som vises i grensesnittet rendres på en trygg måte. Vi sørger for å ikke injisere rå HTML/JS fra brukerdata direkte i siden. Frontendrammeverket og serveren benytter kontekstuell escaping, slik at spesialtegn vises som tekst og ikke tolkes som kode.
    
*   **Content Security Policy (CSP):** Applikasjonen definerer en streng CSP header som begrenser hvor skript og innhold kan lastes fra. Dette gjør det vanskeligere for injisert kode å kontakte eksterne nettsteder eller kjøre uautoriserte funksjoner.
    

Disse tiltakene sammen bidrar til at risikoen for XSS-sårbarheter er minimert. Forbedringen adresserer et kritisk sikkerhetsaspekt slik at brukerne kan stole på at applikasjonen ikke vil kjøre uventet eller skadelig kode i nettleseren deres.

#### Nøkkelhåndtering 

Sikker håndtering av sensitive nøkler og hemmeligheter (som API-nøkler, JWT-signeringsnøkler, databasenøkler osv.) er essensielt for plattformens integritet. Investlytics Hub har innført klare retningslinjer og mekanismer for **nøkkelhåndtering**:

*   **Ingen nøkler i kildekode:** Hemmelige nøkler lagres aldri direkte i repoet eller i selve kildekoden. I stedet benyttes konfigurasjonsfiler (som ikke sjekkes inn) eller miljøvariabler til å levere nødvendige nøkler ved oppstart.
    
*   **Sikre lagringssteder:** I produksjonsmiljø kjøres applikasjonen med nøkler hentet fra et sikkert lager, for eksempel en **Secrets Manager** (som Azure Key Vault, AWS Secrets Manager eller lignende), der tilgang er strengt kontrollert. Dette begrenser eksponeringen av hemmelig data.
    
*   **Begrenset tilgang:** Kun autoriserte personer og tjenester har tilgang til sensitive nøkler. Tilgangsstyring (IAM) og revisjonsspor er på plass slik at all bruk av nøkler kan spores og gamle nøkler kan roteres ved behov.
    
*   **Kryptering:** Nøkler og sensitive konfigurasjonsdata holdes kryptert i hvilende tilstand der det er mulig. For eksempel kan konfigurasjonsfiler være kryptert på server, og dekrypteres først ved oppstart av applikasjonen.
    

Gjennom disse tiltakene reduseres faren for at kompromitterte nøkler fører til sikkerhetsbrudd. Forbedringen sikrer at selv om kildekoden er åpen eller tilgjengelig for flere utviklere, forblir de mest sensitive nøklene beskyttet og håndtert på en forsvarlig måte.

Teknologistakk
--------------

Prosjektet vil benytte en moderne teknologistack, valgt for å oppfylle kravene til sanntid, dataintegrasjoner og AI-analyse:

*   **Frontend:** _React_ (JavaScript/TypeScript) for å bygge en responsiv enkelt-side webapplikasjon. React gir god dynamisk grensesnittoppdatering, og har et stort økosystem (f.eks. Redux for state management om nødvendig, og biblioteker for grafer som D3.js eller Chart.js for finansdiagrammer). Alternativt/utfyllende kan rammeverk som _Next.js_ benyttes for server-side rendering om det er behov. Styling gjøres med et UI-bibliotek (f.eks. Material UI eller Tailwind CSS) for konsistente komponenter. (Merk: Andre rammeverk som Angular eller Vue.js kunne også vært brukt; arkitekturen tillater det, men React antas her som preferanse.) For fremtidig mobilstøtte vurderes _Flutter_ eller _React Native_ slik at kode kan gjenbrukes til native-apper, men i første omgang fokuserer vi på en webbasert klient som også fungerer på mobile enheter via responsivt design.
    
*   **Backend:** _Python_ med _FastAPI_ som web-rammeverk. FastAPI gir høy ytelse (basert på Uvicorn/ASGI) og støtter asynkrone operasjoner, noe som er viktig når vi skal gjøre mange I/O-kall mot eksterne API-er samtidig (unngå blocking). Python ekosystemet muliggjør også enkel integrasjon av dataanalyse og AI-biblioteker. Noen viktige bibliotek og verktøy på backend:
    
    *   **FastAPI & Pydantic:** For å definere API-ruter og datastrukturer (Pydantic-modeller) klart og typesikret.
        
    *   **SQLAlchemy eller Tortoise ORM:** For å kommunisere med SQL-databasen på en praktisk måte (modellere bruker, portefølje osv. som Python-klasser).
        
    *   **CCXT:** Bibliotek som støtter et stort antall kryptobørser sine APIer under en felles grensesnitt. Dette vil forenkle implementasjon av CEX-integrasjoner (hente tickere, legge inn ordre, etc.) uten å måtte håndkode hver børs sitt API.
        
    *   **Web3.py eller Moralis SDK:** Ved behov for direkte interaksjon med blokkjeder (for DEX-data eller NFT), kan disse brukes. Alternativt brukes rene HTTP-kall til Moralis/Bitquery APIer om de dekker behovet (f.eks. REST/GraphQL spørringer).
        
    *   **AI/ML-biblioteker:** I første omgang vil vi benytte eksterne AI-tjenester, men Python gir mulighet til å integrere libs som HuggingFace Transformers (for NLP/sentimentanalyse), scikit-learn (for enklere prediktive modeller), eller PyTorch/TensorFlow (for dypere modeller) dersom det blir aktuelt å kjøre egne modeller.
        
    *   **Databehandling:** NumPy, Pandas for håndtering av datasett (f.eks. beregne indikatorer, kjøre simuleringer). For Monte Carlo-simulering kan f.eks. NumPy brukes til å generere mange scenarier. Matplotlib/Plotly kan brukes server-side for å generere grafer om noe skal forhåndsprosesseres, men mest sannsynlig sendes data til frontend for visualisering der.
        
    *   **Andre verktøy:** Uvicorn ASGI server for å kjøre FastAPI, Celery eller APScheduler for bakgrunnsjobber (f.eks. jevnlig hente kursoppdateringer eller nyheter i bakgrunnen), Redis for caching (kan f.eks. cache siste priser eller API-svar for å redusere responstid og belastning på eksterne APIer).
        
*   **Database:** _PostgreSQL_ (foretrukket) som hoveddatabase for lagring av persisterte data. Postgres er robust og godt støttet i Python-miljøet. Alternativt kan en skylagringstjeneste eller NoSQL vurderes for spesielle data (f.eks. historiske pris-ticks i stort volum kunne lagres i en tidsserie-database som InfluxDB, men sannsynligvis holder Postgres eller et cachelag). Brukerdata, porteføljeobjekter og referanser til digitale eiendeler lagres her.
    
*   **Integrasjons-APIer:** Vi benytter tredjeparts API-er for å slippe å håndtere alt selv:
    
    *   _CoinGecko API_ – for fri tilgang til prisinformasjon, markedsoversikter og historiske data uten behov for API-nøkkel.
        
    *   _Børs-APIer_ – f.eks. Binance API for sanntidskurser og brukers portfolio (via API-nøkkel), Coinbase API, Kraken etc., med CCXT for å forenkle bruken. Websocket-feeds fra disse kan også brukes for realtime oppdatering (f.eks. Binance WebSocket for trade ticks).
        
    *   _Moralis_ – for raskt oppsett mot flere blokkjeder; kan gi token- og NFT-balanser for en lommebok, historikk av transaksjoner m.m. via enkle spørringer.
        
    *   _Bitquery_ – for mer avanserte on-chain spørringer via GraphQL, dersom vi trenger dypere blockchain-analyser (f.eks. on-chain volum for en gitt coin, eller spore hvordan midler flytter seg).
        
    *   _OpenAI API_ – for å hente AI-basert analyse, f.eks. la GPT-4 oppsummere sentiment eller predikere ut ifra nyheter. Eller _HuggingFace Inference API_ for å kjøre sentimentmodell på kryptorelaterte tekster. Slike tjenester krever nøkkel og koster per bruk, så de brukes der det gir mest verdi.
        
*   **Utviklingsverktøy og miljø:** Koden versjonshåndteres på _GitHub_. Vi setter opp _CI/CD_\-pipelines (f.eks. GitHub Actions) for å automatisk kjøre tester og kunne deploye til et staging-miljø. Docker brukes for å pakke applikasjonen slik at både utviklingsmiljø og produksjonsmiljø er konsistente. Dokumentasjon håndteres i markdown og genereres evt. med verktøy (se egen seksjon om dokumentasjon).
    

Denne teknologistakken gir et godt utgangspunkt for å dekke alle krav. Valgene vektlegger velprøvde rammeverk med stor brukerbase (lett å finne støtte og utviklere), og tjenester som lar oss implementere avanserte funksjoner (AI, on-chain data) uten å gjenoppfinne hjulet.

Faser og milepæler (Roadmap)
----------------------------

Prosjektet deles inn i klare faser for å sikre strukturert fremdrift. Hver fase har definerte milepæler og leveranser. Under følger en roadmap med faser fra konsept til lansering, inkludert en ekstra dedikert fase (Fase 4) for kryptoporteføljeanalyse som ønsket. Milepæler er uthevet for å markere viktige delmål (f.eks. ferdigstillelse av MVP).

#### Fase 0: Konseptualisering og Oppsett 

<details style="margin-bottom: .8em;">
<summary><strong>⬅ Åpne for arkitekturdiagram, fase 0</strong></summary>

```mermaid
flowchart TD
    UI["React-basert frontend"] --> API[/"FastAPI\nbackend"/]
    API --> DB[(Database)]
    API -. integrasjon .-> N8N["n8n workflows"]
```
_Forklaring_: Her representerer UI brukergrensesnittet (React-app), API er FastAPI-backenden, DB er databaseserveren, og N8N er n8n-verktøyet for arbeidsflyter. Pilen fra UI til API viser at frontenden kaller backend-API’et. Backenden skriver til DB (pilen til databasen). Den stiplete linjen fra API til N8N indikerer at backenden trigges/integreres med n8n-workflows (for eksempel via webhooks eller planlagte jobber). Dette diagrammet er nå syntaktisk gyldig og gir et klart bilde av arkitekturen i fase 0.   
</details> 

**Mål:** Legge grunnlaget for prosjektet med klart design, arkitektur og utviklingsmiljø.

*   **Krav og design:** Verifisere kravene (spesielt fokusområdene: trading, sanntidsdata, AI, minimal NFT) og justere opprinnelig plan til ny fokus. Definere use-cases og brukerhistorier for kjernefunksjonene (f.eks. _"Som bruker vil jeg se oppdaterte priser fra flere børser i et dashboard..."_).
    
*   **Arkitekturdesign:** Oppdatere systemarkitekturen (tegn nye diagrammer, definer modul-inndeling) basert på krav. Dette inkluderer også å bestemme dataflyt: hvordan data går fra eksterne APIer gjennom backend til frontend, og hvor AI-modellen kommer inn i flyten. (Arkitekturdiagram utarbeides – se eksempel ovenfor.)
    
*   **Teknologivalg bekreftes:** Gjennomgå teknologistakken beskrevet over og gjøre endelige valg for frontend-rammeverk, database, hosting, etc. (Fase 1 forventes å konkludere med f.eks. at _React + FastAPI + Postgres_ brukes, samt hvilke eksterne API-nøkler som trengs for videre utvikling).
    
*   **Repository oppsett:** Initialisere GitHub-repositoriet for prosjektet. Etablere grunnstruktur (se **GitHub-struktur** nedenfor) og sette opp kontinuerlig integrasjon (CI) for å kjøre tester og linth etter hvert.
    
*   **Proof-of-Concepts:** Utvikle enkle prototyper for høy-risiko elementer for å redusere usikkerhet: f.eks. et raskt skript for å hente data fra en børs-API via CCXT, teste et kall mot OpenAI API for å se responstid/kost, eller en liten React-app som viser data fra CoinGecko. Disse POC-ene sikrer at i senere faser kan vi bygge med trygghet på disse integrasjonene.
    
*   **Milestone:** _Prosjektplan og arkitektur godkjent._ (Etter fase 1 skal vi ha en “spesifikasjon” og plan klar. Dette dokumentet – oppdatert prosjektplan – er en del av leveransen her. Utviklingsmiljø og repo er klart, slik at koding kan starte.) 

#### Fase 1: Grunnleggende MVP-funksjonalitet (Sanntidsdata & UI)

**Mål:** Bygge kjernen av MVP – sanntids markedsdatafunksjoner og grunnleggende brukergrensesnitt for tradingoversikt.

<details style="margin-bottom: .8em;">
<summary><strong>⬅ Åpne for sekvensdiagram, fase 1</strong></summary>

```mermaid
sequenceDiagram
    participant User as Frontend
    participant API  as FastAPI
    participant CG   as CoinGecko

    User->>API: GET /api/v1/pricefeed
    activate API
    API->>CG:   Hent topp N priser
    CG-->>API:  JSON med priser
    API-->>User: JSON relay
    deactivate API
    User->>User: Oppdater ticker-komponent
```    

</details> 

*   **Backend – markedstjenester:** Implementere modul for markedsdata. Start med integrasjon mot en offentlig kilde (CoinGecko) for å trekke ut en liste av kryptovalutaer, priser og evt. historisk data for diagram. Sett opp endepunkter i FastAPI for “get prices”, “get market summary” osv. Implementer også en WebSocket-endepunkt for live oppdatering av pris (f.eks. kringkaste nye priser til klienter hvert 5. sekund eller når endringer skjer). Dette kan gjøres ved at backenden jevnlig henter oppdateringer fra f.eks. en børs WebSocket og pusher ut til tilkoblede klienter.
    
*   **Frontend – dashbord:** Starte utvikling av React-applikasjonen. Lage en grunnleggende navigasjon og et dashboard som viser: en kursticker eller liste over utvalgte kryptovalutaer med pris som oppdateres i sanntid, og et enkelt diagram (candlestick chart) for en valgt valuta. Bruke et chart-bibliotek for å vise f.eks. siste 24 timers prisutvikling. Test at WebSocket-forbindelsen mottar oppdateringer og at UI oppdateres uten refresh.
    
*   **Brukeropplevelse:** Selv om full brukerpålogging ikke er i fokus ennå, sett opp grunnlag for senere (for eksempel klargjør Redux store eller context for å holde brukertilstand). Foreløpig kan MVP-funksjonene i fase 2 være tilgjengelige uten innlogging (offentlige data).
    
*   **Tekniske indikatorer:** Implementere et par enkle tekniske indikatorer i backenden (eller frontend) for demonstrasjon, f.eks. beregne 7-dagers og 30-dagers glidende snitt av prisen og vis dette på diagrammet, eller vise en indikator som Relative Strength Index (RSI) under chartet. Dette gir verdi til tradere og tester datahåndtering.
    
*   **Testing underveis:** Skriv enhetstester for funksjoner som beregner indikatorer, og integrasjonstest for API-kall (f.eks. kall /api/v1/prices og se at formatet er korrekt). Kjør disse via CI.
    
*   **Milestone:** _MVP kjernefunksjoner implementert:_ Systemet kan vise sanntidspriser og enkle grafer i frontend med data fra backend. Dette demonstreres evt. internt for å verifisere at sanntidskjeden (fra ekstern API til UI) fungerer.
    
#### Fase 2: Brukerportefølje, AI-analyse og digitale eiendeler (MVP fullføring)

<details style="margin-bottom: .8em;">
<summary><strong>⬅ Åpne for sekvensdiagram, fase 2</strong></summary>

```mermaid
flowchart LR
    ExternalAPI[Ekstern data-kilde] -->|finansdata| Backend[(FastAPI backend)]
    Backend -->|lagrer data| DB[(Database)]
    Backend -->|sender data| Frontend[React frontend]
``` 

_Forklaring_: Dette flytdiagrammet (ikke i originalplanen) viser et mulig scenario i fase 2: en Ekstern data-kilde (f.eks. en børs-API) leverer finansdata til backenden. Backend prosesserer og lagrer data i DB, og sender nødvendige data videre til Frontend for visning til brukeren. Diagrammet ville understøtte teksten i fase 2 ved å gi leseren en rask visuell forståelse av hvordan kjernefunksjonen (dataflyt fra ekstern kilde til visning) fungerer. Om fase 2 ikke involverer eksterne datakilder, er et slikt diagram mindre relevant – da er det riktig at ingen diagram er inkludert.

<strong>Sekvensdiagrammet som beskriver API-flyten med DB-cache og CEX-henting.</strong>

```mermaid
sequenceDiagram
    participant User     as "Frontend-klient"
    participant API      as "FastAPI backend"
    participant DB       as "PostgreSQL"
    participant CEX      as "Binance API via CCXT"

    User  ->> API : GET /api/v1/portfolio (JWT)
    activate API
    API   ->> DB  : SELECT saldo WHERE user_id

    alt Cache hit
        DB  -->> API : saldo
    else Cache miss
        DB  -->> API : (tom)
        API ->> CEX : hent kontobalanse
        CEX -->> API : JSON balanse
        API ->> DB  : UPSERT balanse
    end

    API  -->> User : JSON portefølje
    deactivate API
    User ->> User : tegn doughnut-graf
``` 

</details> 

**Mål:** Utvide MVP med bruker-spesifikke funksjoner: porteføljeforvaltning, AI-drevet analysefunksjoner og enkel integrasjon av digitale eiendeler (NFT-visning). Etter denne fasen anses MVP som komplett.

*   **Brukerautentisering og profil:** Implementere registrering/innlogging (f.eks. ved hjelp av FastAPI Users-biblioteket eller egen JWT-løsning). Dette tillater at brukere kan ha sine egne porteføljeopplysninger lagret sikkert. Frontend får en enkel innloggingsside og brukerstatus i UI.
    
*   **Porteføljefunksjonalitet:** Utvikle backend-endepunkter for CRUD-operasjoner på brukerens portefølje (legg til kryptovaluta du eier, antall, kjøpspris etc.). Knytte dette til databasen (e.g. en PortfolioItem modell som kobles til User). Frontend får et porteføljeskjermbilde der brukeren kan se en liste over sine eiendeler, total porteføljeverdi (utregnet basert på siste priser) og prosentvis fordeling. Legg også til mulighet for å _importere_ portefølje automatisk via API-nøkler: f.eks. brukeren oppgir API-nøkkel til Binance, og systemet henter deres saldo automatisk. (I MVP kan vi støtte minst én slik integrasjon for demo, mens flere kommer senere.)
    
*   **CEX-integrasjon (brukerdata):** Som nevnt over, integrere mot en valgt CEX API for å hente kontobalanse og kanskje siste transaksjoner for bruker. Dette kan implementeres som en bakgrunnsjobb som kjører ved innlogging eller på forespørsel. CCXT-biblioteket brukes her for å forenkle, eller direkte REST-kall om nødvendig. Sikre at API-nøklene lagres kryptert i DB og **aldri** eksponeres til frontend.
    
*   **AI-prediktiv analyse:** Implementere den første AI-funksjonen. For eksempel, et endepunkt /api/v1/ai/crypto\_predict?asset=BTC som returnerer en korttidsprisprognose for Bitcoin (BTC). For å realisere dette raskt kan vi benytte en tredjeparts AI-tjeneste: f.eks. sende siste 7 dagers prisdata til en OpenAI modell med en prompt om å fortsette trenden, eller bruke en enklere statistisk modell (ARIMA eller regressjon) server-side for beregning. Poenget er å demonstrere AI-drevet innsikt. Resultatet kan være: "Forventet pris i morgen: X USD (opp Y%) med Z% konfidens". Frontend presenterer dette kanskje som en widget ved siden av prisdiagrammet.
    
*   **AI-basert sentiment:** (Hvis tid i fase 3) Alternativt eller i tillegg, integrere en enkel sentimentanalyse for en spesifikk kryptovaluta basert på nyheter. F.eks. backend henter siste nyhetsoverskrifter eller twitter-meldinger om Bitcoin, sender dem til en sentimentanalysemodell (via HuggingFace API eller en enkel nltk-basert løsning) og returnerer en samlet sentiment score (positiv/nøytral/negativ). Dette kan vises i UI som en indikator (f.eks. "Markedsstemning: Nøytral"). Denne oppgaven kan også flyttes til fase 4 dersom den kompliserer MVP, men nevnes her som en del av AI-drevet analyse.
    
*   **Digitale eiendeler (NFT) integrasjon:** Knytte brukerens eventuelle digitale eiendeler til plattformen. Siden brukeren har et tidligere pNFT-prosjekt, kan vi forenklet anta at de har en lommebokadresse med NFT-er eller en API fra det prosjektet. Vi implementerer en modul som bruker Moralis API (eller pNFT-prosjektets API hvis tilgjengelig) for å hente en liste over NFT-er for en gitt bruker (f.eks. via lommebokadresse lagret på profilen). Frontend får en seksjon som viser disse digitale eiendelene i porteføljesiden, f.eks. som små kort med bilde og navn. Det legges ikke opp til trading av disse NFT-ene, kun visning. Denne funksjonen gjøres optional – hvis brukeren ikke har NFT-er å koble til, kan delen skjules. (Teknisk: et endepunkt /api/v1/portfolio/nft som returnerer NFT-oversikt).
    
*   **Oppdatert dokumentasjon:** Underveis i fase 3 oppdateres dokumentasjon for alle nye API-endepunkter (FastAPI genererer automatisk docs, men vi lager også lesbar dokumentasjon). Brukerhåndtering, AI-endepunkt og NFT-integrasjon beskrives for fremtidig referanse.
    
*   **Milestone:** **MVP ferdigstilt** – Investlytics Hub v1 kan lanseres i lukket beta. Den oppfyller hovedmålene: en bruker kan logge inn, se sin portefølje (inkl. ev. NFT-er) oppdatert med sanntidspriser, få noen AI-baserte innsikter, og alt presentert i et brukervennlig dashboard. Kompleks NFT-funksjonalitet og avansert AI er ikke med, men plattformen er funksjonell og demonstrerer konseptet godt.
    

#### Fase 3: Krypto­portefølje­analyse & on-chain data (utvidelse etter MVP)

<details style="margin-bottom: .8em;">
<summary><strong>⬅ Åpne for sekvensdiagram, fase 3</strong></summary>

```mermaid
sequenceDiagram
    participant Bruker
    participant Frontend
    participant Backend
    participant ML as AI/Analysemodul
    participant DB as Database

    Bruker->>Frontend: Starter analyse (f.eks. laster opp porteføljedata)
    Frontend->>Backend: Sender forespørsel med data (API-kall)
    Backend->>DB: Lagrer rådata
    Backend->>ML: Kaller AI-modul for analyse
    ML-->>Backend: Returnerer analyseresultater
    Backend-->>DB: Lagrer resultater (valgfritt)
    Backend-->>Frontend: Sender analyseresultater (respons til frontend)
    Frontend-->>Bruker: Viser resultater i dashbordet
```    

_Forklaring_: Sekvensdiagrammet over viser stegene når en Bruker initierer en analyse gjennom frontenden. Frontenden kaller Backend (FastAPI) med input-data. Backenden lagrer data i databasen og sender data videre til en AI/Analysemodul (f.eks. en maskinlæringsmodell eller beregningskomponent). AI-modulen prosesserer data og returnerer resultatet til backenden. Backenden kan lagre resultatet i DB og sender så svaret tilbake til Frontend, som presenterer det for brukeren. Dette diagrammet gjør det enklere å følge med på de mange delene som er involvert i fase 3. Det er plassert i riktig fase (fase 3) siden det er her den avanserte analysen implementeres. Ved å inkludere dette sekvensdiagrammet i planen forbedres leserens forståelse av fase 3 betydelig.
</details> 

**Mål:** Dedikert fase for å implementere dypere porteføljeanalyse ved hjelp av Monte Carlo-simulering, visualiseringer, samt integrere on-chain data og kryptospesifikke indikatorer. (Dette er en ny foreslått fase for å forsterke analyse-delen av plattformen.)

*   **Monte Carlo-simulering:** Utvikle funksjonalitet for å simulere fremtidige utfall for brukerens portefølje. Dette innebærer å ta historiske avkastningsdata for hver kryptovaluta i porteføljen (f.eks. daglig prisendring over X år, hentet via CoinGecko API) og bruke Monte Carlo-metoden til å generere et stort antall tilfeldige scenarioer for fremtidig prisutvikling. Summér porteføljens verdi i hvert scenario over en gitt tidshorisont (f.eks. 1 år frem). Beregn og lagre statistikk fra simuleringen: forventet avkastning, standardavvik (risiko), sannsynlighet for tap v. gevinst, Value-at-Risk, osv.
    
*   **Visualisering av simulering:** Presentere resultatene for brukeren på en intuitiv måte. Frontend kan vise en graf for det forventede spennet av porteføljeverdi over tid (f.eks. et bånd som viser 10%-90% percentil av simuleringene), eller en histogram som viser fordeling av sluttverdier. Dette gir brukeren innsikt i potensiell risiko og reward. Backend kan forberede aggregerte data mens frontend håndterer selve graftegningen.
    
*   **On-chain data integrasjon:** Bygge videre på NFT-integrasjonen og hente inn flere blokkjede-data for analyseformål. For hver kryptovaluta i porteføljen, kan vi inkludere on-chain metrikk som en del av informasjonsbildet. Eksempler: antall aktive adresser, transaksjonsvolum på kjeden, hash-rate (for proof-of-work), staking-grad (for proof-of-stake), store holder-distribusjon (“whale”-aktiviteter). Ved å benytte **Bitquery** eller lignende kan vi hente slike data. Disse indikatorene vises i porteføljeanalyseseksjonen for avanserte brukere som vil forstå fundamentale trender.
    
*   **Kryptospesifikke indikatorer:** I tillegg til generelle tekniske indikatorer, implementere indikatorer unike eller populære i kryptosammenheng. Dette kan inkludere f.eks. **Crypto Fear & Greed Index** (som kan hentes via et API), **Bitcoin dominans** (BTC markedsandel, beregnet ut fra data), eller on-chain-indikatorer som **NVT Ratio** (forhold mellom markedsverdi og transaksjonsvolum på nettverket). Legge disse til dashboardet eller porteføljesiden som ekstra widgets/grafer.
    
*   **Frontend funksjonell utvidelse:** Utvide porteføljesiden i UI til å ha en egen fane for “Advanced Analysis” der overnevnte resultater og indikatorer vises. Sørge for at brukeropplevelsen fortsatt er god ved mye data – kanskje med toggles for å vise/skjule avansert info for de som vil ha det.
    
*   **Ytelsesoptimalisering:** Monte Carlo-simuleringer kan være tunge (tusenvis av iterasjoner). Vurder optimaliseringer: bruke NumPy vektoriserte beregninger, kjøre beregningene asynkront (slik at API-et ikke blokkerer), eller utføre dem på forhånd (f.eks. trigget når bruker oppdaterer porteføljen sin). Kanskje implementer en cache på resultater som oppdateres periodisk eller ved vesentlige endringer.
    
*   **Milestone:** _Avansert porteføljeanalyse tilgjengelig._ Etter fase 4 vil brukeren kunne dykke dypt i porteføljens risiko og ytelse. Vi har nå differensiert Investlytics Hub som mer enn en tracker – det er et verktøy for analyse og prognose. Denne milepælen markerer en utgivelse (v1.5) til brukerne som er interessert i avansert funksjonalitet.
    

#### Fase 4: Avansert AI og videreutvikling

<details style="margin-bottom: .8em;">
<summary><strong>⬅ Åpne for sekvensdiagram, fase 4</strong></summary>

```mermaid
flowchart TD
  trigger1[Planlagt trigger n8n] --> n8nWF[n8n workflow]
  n8nWF -->|API-kall| extAPI[Ekstern finans API]
  extAPI -->|data| n8nWF
  n8nWF -->|webhook| backend[FastAPI backend]
  backend -->|lagrer| db[(Database)]
  backend -->|varsler| frontend[Frontend React]
```    

_Forklaring_: Dette diagrammet viser en mulig automatiseringsflyt i fase 4: En trigger i n8n starter en workflow (f.eks. tidsstyrt hver natt). n8n workflow kaller en Ekstern API for å hente finansdata. Eksternt system sender data tilbake til n8n, som deretter kommuniserer med Backend (f.eks. via et API-endepunkt eller webhook i FastAPI). Backenden lagrer de nye dataene i DB og kan samtidig oppdatere Frontend eller sende et varsel slik at brukeren får ferske data. Et slikt diagram er nyttig for å vise kompleks samhandling mellom flere systemer. Dersom planen beskriver noe lignende i fase 4, ville dette diagrammet vært riktig plassert der. Hvis fase 4 derimot er mindre kompleks (f.eks. kun enkel integrasjon uten automasjon), klarer man seg kanskje uten et eget diagram.
</details> 

**Mål:** Bygge videre på AI-funksjonaliteten og andre forbedringer post-MVP, samt begynne å inkludere eventuelle utsatte funksjoner (som utvidet digital eiendel-håndtering eller flere integrasjoner).

*   **AI-modeller v2:** Basert på tilbakemeldinger fra MVP, forbedre AI-prediksjonene. Dette kan innebære å trene en mer spesialtilpasset modell for kryptopris-forutsigelser. For eksempel samle historiske data for de topp N valutaene og trene en tidsserie-modell (LSTM, Prophet eller en Bayesian model) som kjøres periodisk for å gi prognoser. Alternativt, integrere en tredjepartstjeneste som tilbyr ferdige krypto-prediksjoner hvis det finnes. I tillegg utvide /api/v1/ai/crypto\_predict endepunktet til å kunne gi prognoser på flere tidshorisonter (kortsiktig/dagsbasis og lengre sikt).
    
*   **Utvidet sentimentanalyse:** I fase 4 kan vi integrere sanntids sentimentanalyse fra sosiale medier og nyheter på en dypere måte. For eksempel koble til Twitter API (eller tjenester som TheTie, Stockgeist) for å få sentiment-score for omtalene av ulike coins, og vise en “sentiment trend” over tid. Implementere en modul som periodisk trekker sentiment-data og lagrer, slik at man kan se korrelasjon mellom sentiment og pris. AI kan brukes her til å filtrere ut støy og analysere tekst.
    
*   **Flere integrasjoner (CEX/DEX):** Utvide støtte til flere kryptobørser basert på brukernes behov. F.eks. legge til Coinbase, Kraken på CEX-siden, og Uniswap, SushiSwap data på DEX-siden. Dette øker dekningsgraden. Også vurdere integrasjon mot Decentralized Finance (DeFi) protokoller analytics via APIer hvis relevant (f.eks. låne-rater, yield farming info), som en mulig utvidelse av plattformens data.
    
*   **Digital eiendel-utvidelse:** Hvis det er ønskelig strategisk, kan noen mer avanserte NFT-funksjoner implementeres her. For eksempel, la brukeren se markedsverdiestimater for sine NFT-er (hente gulvpris fra OpenSea API eller Moralis), eller integrere deler av pNFT-prosjektet mer dypt (f.eks. importere metadata som beskrivelser, gi mulighet til å dele NFT-porteføljen offentlig). Fortsatt vil dette være sekundært i prioritet, men mulig nå som hovedfunksjonene er robuste.
    
*   **Brukerfeedback og UX iterasjoner:** Samle data fra betabrukere av MVP og fase 4 funksjoner. Forbedre grensesnitt, ytelse og brukevennlighet der det trengs. Kanskje noen funksjoner for personalisering: brukeren kan lage egne overvåkningslister, sette opp prisalarmer (via e-post/varsel), osv., som “nice-to-have” forbedringer i denne fasen.
    
*   **Skaleringsforberedelser:** Gjør eventuelle arkitektoniske justeringer for skalering før full lansering. Dette kan inkludere å flytte visse komponenter til mikrotjenester hvis nødvendig (f.eks. en egen service for AI-beregninger hvis det er krevende), lasttesting av systemet med mange samtidige brukere og optimalisering (legge inn caching-lag med Redis/memory caching der det monner, sørge for at WebSocket-håndtering er effektiv, etc.).
    
*   **Milestone:** _Full funksjonalitet implementert._ Investlytics Hub har nå både kjernefunksjonene og “wow-faktor” funksjoner (avansert AI, sentiment, bred integrasjon) på plass i en stabil versjon 2.0. Vi er klare for siste finpuss, testing og deretter lansering.
    
#### Fase 5: Testing, kvalitetssikring & lansering

<details style="margin-bottom: .8em;">
<summary><strong>⬅ Åpne for sekvensdiagram, fase 5</strong></summary>

```mermaid
sequenceDiagram
    participant Dev   as Utvikler
    participant GH    as GitHub Repo
    participant CI    as GitHub Actions Runner
    participant Reg   as Docker Registry
    participant Prod  as Cloud Host (K8s / VM)

    Dev->>GH: git push main
    activate GH
    GH-->>CI: Trigger workflow .yaml
    activate CI
    CI->>CI: npm test (frontend) / pytest (backend)
    CI-->>Reg: Bygg & push docker-images (fe/be)
    CI-->>Prod: kubectl apply / deploy update
    Prod-->>CI: Health-check OK
    CI-->>GH: Status = ✔ green (shield badge)
    CI-->>Dev: Slack/Discord «Deploy suksess»
    deactivate CI
```    
</details> 

**Mål:** Siste fase for å sikre kvalitet, dokumentasjon og en vellykket lansering av produktet.

*   **Grundig testing:** Utover de løpende testene, kjør omfattende systemtester på hele plattformen. Inkludér ende-til-ende tester der man simulerer en brukerreise (logg inn, legg til portefølje, se data, kjør en simulering, osv.). Test også feilscenarier (tapsfri håndtering av når eksterne APIer feiler eller gir treg respons – systemet skal håndtere det grasiøst). Penetrasjons- og sikkerhetstesting gjennomføres for å sikre at sensitive data er beskyttet (f.eks. test at API-nøkler ikke lekker, at autorisering fungerer).
    
*   **Ytelsestesting:** Simuler høy last (mange samtidige WebSocket-klienter, hyppige API-kall) for å avdekke flaskehalser. Tun backenden (f.eks. juster Uvicorn workers, databaseindekser) for å tåle forventet bruk.
    
*   **Bugfiksing og siste justeringer:** Triager alle gjenstående bugs fra issue-trackeren og lukk kritiske før launch. Poler brukergrensesnittet basert på siste testtilbakemeldinger.
    
*   **Dokumentasjonsferdigstilling:** Fullfør **dokumentasjonsstrategi** (se under). Sikre at README og eventuell brukerhåndbok er oppdatert for lanseringsversjonen. Lag “Hvordan komme i gang” guider for nye brukere. For utviklere, oppdater eventuelle arkitekturdokumenter for å matche endelig implementasjon (slik at fremtidige bidragsytere har lett for å forstå systemet).
    
*   **Lanseringsplan:** Forbered distribusjon til produksjonsmiljø. Set up cloud-infrastruktur (f.eks. AWS/GCP/Azure eller Heroku/Vercel avhengig av valg) med nødvendige tjenester: database, backend API-server, frontend hosting. Utfør en prøvedeploy (soft launch) for et begrenset antall brukere for en siste verifisering. Planlegg en lanseringsdato og eventuelt markedsføringsstøtte.
    
*   **Lansering:** Rull ut Investlytics Hub til alle planlagte brukere. Monitorer nøye i startfasen for å fange opp eventuelle uforutsette problemer (bruk logging og monitoreringsverktøy aktivt). Sørg for supportkanaler for nye brukere.
    
*   **Milestone:** **Offisiell lansering** av Investlytics Hub. Prosjektet går deretter inn i driftsfase med kontinuerlig forbedring basert på brukernes tilbakemeldinger.

### Gantt-diagram — visuell roadmap
    
Gantt-oversikt (tentativ tidslinje) for faser og milepæler beskrevet over:

```mermaid
%%{init:{ "gantt":{ "leftPadding":140,"rightPadding":50 } }}%%
gantt
    title Investlytics Hub – Roadmap (≈ 12 uker)
    dateFormat  YYYY-MM-DD

    section Fase 0 – Grunnoppsett
    Repo & Docker           :done,    task0, 2025-06-02, 7d

    section Fase 1 – Markedsdata
    Live priser API         :active,  task1, after task0, 10d
    Dashboard UI            :         task2, after task1, 7d

    section Fase 2 – Portefølje
    Auth + CEX-sync         :         task3, after task2, 14d

    section Fase 3 – AI + Digital assets
    AI prognose             :crit,    task4, after task3, 10d
    NFT visning (lett)      :         task5, after task4, 5d

    section Fase 4 – Avansert analyse
    Monte Carlo             :         task6, after task5, 10d
    On-chain metrics        :         task7, after task6, 7d

    section Fase 5 – Finish & Launch
    Test / polish / deploy  :milestone, task8, after task7, 10d


```
_(Tidslinjen over er et estimat for illustrasjon. Faktiske datoer og varigheter kan justeres etter behov og ressurser.)_

#### Etiketter til Issues og Pull Requests i GitHub

| **Kategori**       | **Eksempel-label**                                                                 |
|--------------------|-----------------------------------------------------------------------------------|
| **Type**           | `type:feature`, `type:bug`, `type:docs`, `type:refactor`                          |
| **Modul**          | `module:frontend`, `module:backend`, `module:n8n`, `module:simulation`,          <br> `module:trading`, `module:digital-assets`, `module:ai` |
| **Prioritet**      | `priority:high`, `priority:medium`, `priority:low`                                |
| **Status (valgfritt)** | `status:todo`, `status:in-progress`, `status:review`, `status:done`         |
| **Andre**          | `Good first issue`, `help wanted`                                                |

#### Milepæler

- **Milestone 0: Konsept & Grunnoppsett**  
  Krav, arkitektur, repo + Docker/CI-skjelett (fase 0).

- **Milestone 1: Sanntidsdata + Dashboard**  
  CoinGecko-feed, WebSocket-push, React-dashboard (fase 1).

- **Milestone 2: Portefølje & AI-MVP**  
  JWT-auth, CEX-sync, `/ai/crypto_predict`, enkel NFT-visning (fase 2).

- **Milestone 3: Avansert Porteføljeanalyse**  
  Monte Carlo, on-chain-metrics, advanced-tab (fase 3).

- **Milestone 4: AI v2 & Integrasjoner**  
  Bedre modeller, sentiment 2.0, flere CEX/DEX-integrasjoner, valgfri NFT-boost (fase 4).

- **Milestone 5: QA, Deploy & Launch**  
  E2E-tester, lasttest, produksjonsdeploy (fase 5).

GitHub-struktur
---------------

Prosjektet vil ha en ryddig struktur i GitHub-repositoriet, slik at frontend, backend, dokumentasjon og verktøy er klart separert. Strukturen legger til rette for samarbeid og kontinuerlig integrasjon. Under er et forslag til mappestruktur:

```text
investlytics-hub/
├── .vscode/                        # VS Code-workspace (launch, settings, anbefalte extensions)
│
├── frontend/                       # React-(evt. Next.js) SPA
│   ├── public/
│   └── src/
│       ├── assets/                 # Ikoner, bilder, fonter
│       ├── components/
│       │   ├── common/             # Buttons, inputs, modals …
│       │   ├── charts/             # Recharts/D3.js wrappers
│       │   ├── dashboard/          # Kurs-ticker, pris-widgets, AI-cards
│       │   └── digital-assets/     # komponenter for NFT-visning
│       ├── contexts/               # Global state (Theme, Auth, WebSocket ctx)
│       ├── hooks/                  # Custom hooks: usePrices, useAIInsight …
│       ├── layouts/                # BasicLayout, AuthLayout, SettingsLayout
│       ├── pages/                  # Route-nivå: Dashboard, Portfolio, AI, DigitalAssets
│       ├── services/               # REST & WS-klienter (axios, socket.io, signalR)
│       ├── styles/                 # Tailwind config, globale SCSS/vars
│       ├── utils/                  # f.eks. formatters, math helpers
│       ├── App.tsx
│       └── main.tsx              
│   ├── tests/                      # Jest / React-Testing-Library
│   ├── .eslintrc.js
│   └── vite.config.ts
│
├── backend/                        # FastAPI-applikasjon
│   ├── app/
│   │   ├── main.py                 # create_app(), router-registrering
│   │   ├── core/                   # config.py, security.py, logging.py
│   │   ├── api/
│   │   │   ├── deps.py
│   │   │   └── v1/
│   │   │       ├── endpoints/
│   │   │       │   ├── market_data.py        # priser, orderbook
│   │   │       │   ├── portfolio.py          # CRUD + balansesync fra CEX-API
│   │   │       │   ├── ai_crypto.py          # /ai/crypto_predict, /ai/sentiment
│   │   │       │   ├── simulation.py         # Monte-Carlo endepunkt
│   │   │       │   ├── digital_asset_min.py  # minimal NFT-oversikt
│   │   │       │   └── health.py
│   │   │       └── schemas.py
│   │   ├── db/
│   │   │   ├── database.py
│   │   │   ├── base_class.py
│   │   │   └── models/
│   │   │       ├── user.py
│   │   │       ├── portfolio.py
│   │   │       ├── trade.py
│   │   │       └── digital_asset.py          # NFT-objekt (valgfritt)
│   │   ├── services/
│   │   │   ├── exchanges/                    # CEX- & DEX-klienter (ccxt wrappers)
│   │   │   │   ├── binance_service.py
│   │   │   │   └── coinbase_service.py
│   │   │   ├── onchain/                      # Moralis / Bitquery wrappers
│   │   │   │   └── solana_service.py
│   │   │   ├── ai/                           # OpenAI / HF-integrasjon
│   │   │   │   └── ai_service.py
│   │   │   ├── ai_service.py                 # OpenAI / HF-integrasjon
│   │   │   ├── simulation_service.py         # Monte-Carlo-logikk
│   │   │   ├── indicator_service.py          # TA-Lib, on-chain metrics
│   │   │   └── cache_service.py              # Redis helpers
│   │   └── worker/                           # Celery/APS-jobs (pris-polling, sync)
│   ├── tests/                                # PyTest enhet + integrasjon
│   │   └── api_v1/
│   ├── Dockerfile
│   ├── pyproject.toml / requirements.txt
│   └── .env.example
│
├── workflows/                    # n8n (eller alternativt: GitHub Actions Workflows)
│   ├── flows/
│   │   ├── daily_market_update.json
│   │   ├── crypto_sentiment_ingest.json
│   │   └── trade_execution_signal.json      # (senere fase)
│   └── README.md
│
├── simulations/                  # Valgfritt – Python-/Rust-/WASM-kode portert fra mc-simulations
│
├── scripts/                      # Dev&ops hjelpeskript
│   ├── setup_dev.sh
│   ├── run_dev.sh
│   └── initial_data.py
│
├── infrastructure/               # thin README-link
│   ├── docker-compose.yml        # DB, Redis, Backend, Frontend, n8n
│   ├── k8s/                      # K8s manifester / Helm chart (valgfritt)
│   └── ci-cd/                    # GitHub Actions-workflow-yaml
│
├── docs/                         # Arkitektur, API & brukerhjelp
│   ├── architecture/
│   ├── api/
│   └── pnft-background.md        # Kort om pNFT-prosjektet & digital-asset-kobling
│
├── .gitignore
├── README.md
├── CONTRIBUTING.md
├── ROADMAP.md
└── LICENSE
```

_Forklaring:_ Mappene **backend/** og **frontend/** holder adskilt kodebasen for henholdsvis backend-API og frontend-applikasjonen. Under backend er koden ytterligere strukturert etter “separation of concerns” (API, services, models, etc.), noe som gjør det enklere å navigere – f.eks. all kode for eksterne integrasjoner (CEX/DEX API, AI API) ligger under services/ kanskje som egne moduler (exchange\_service.py, ai\_service.py etc.). Frontend-delen følger en typisk React-prosjektstruktur med komponenter og sider.

Mappen **docs/** brukes til å holde prosjektets dokumentasjon i Markdown/tekst-form – dette kan inkludere arkitekturdoc (f.eks. denne planen, oppdaterte diagrammer), ADR (Architecture Decision Records) hvis vi bruker det, samt eventuelt en API-dokumentasjon eksport. Ved å ha dette i repo, versjonshåndteres også dokumentasjonen.

**Infrastructure/**\-mappen (navn kan endres til devops/ eller deploy/) inneholder alt som har med oppsett av miljø å gjøre, som Docker-filer for containerization, Kubernetes konfigurasjoner hvis vi går den veien, og CI/CD oppsett for automatisering. Dette gir en sentral plass å oppdatere ved endringer i devops-oppsett.

Denne strukturen er ment å balansere klar separasjon med oversiktlighet. Den kan justeres etter behov, men utgjør et godt utgangspunkt. I GitHub vil vi også benytte **Issues** for å spore oppgaver/bugs, **Projects/kanban** for fremdrift pr. fase, og Wiki (hvis ønskelig) for brukerdokumentasjon eller FAQ.

Dokumentasjonsstrategi
----------------------

God dokumentasjon er kritisk for et vellykket prosjekt, spesielt gitt den brede funksjonaliteten (trading, AI, integrasjoner). Vår strategi omfatter flere nivåer av dokumentasjon, kontinuerlig oppdatert i takt med utviklingen:

*   **Kode- og API-dokumentasjon:** Vi benytter FastAPIs innebygde støtte for dokumentasjon av API-endepunkter. Dette betyr at alle endepunkter har tydelige Pydantic-skjemaer og beskrivelser, slik at det genereres en interaktiv dokumentasjon (Swagger UI / OpenAPI) på f.eks. /docs endepunktet i kjørende backend. Utviklere og testere kan her se hvilke kall som finnes og prøve dem ut. I tillegg skrives det rikelig med docstrings og kommentarer i koden (Python og JavaScript) for å forklare ikke-opplagte deler, spesielt i kompliserte algoritmer som Monte Carlo-simuleringen eller AI-integrasjonen. Vi kan vurdere å bruke et verktøy som Sphinx eller Doxygen til å generere et mer lesbart utviklerdokument fra koden, men siden teamet er lite kan det holde med velholdte docstrings og markdown-filer.
    
*   **Prosjektdokumentasjon:** I docs/ mappen (nevnt over) opprettholdes dokumenter som beskriver systemets arkitektur og designvalg. Denne prosjektplanen danner grunnlaget. Etter hvert som vi gjør endringer (f.eks. velger en annen strategi for AI-modul, eller legger til nye faser), så loggfører vi det her. Dette sikrer at vi alltid har en “single source of truth” for hvordan systemet er tenkt å fungere. Diagrammer (som arkitekturdiagrammet, sekvensdiagrammer for dataflyt, etc.) lagres også her – gjerne i kildeform (Mermaid eller draw.io) slik at de lett kan oppdateres og versjonshåndteres.
    
*   **Brukerdokumentasjon:** For sluttbrukere av Investlytics Hub trengs en enkel guide. Dette kan være i form av en **README** eller Wiki-side som forklarer hvordan man oppretter konto, legger til sin første portefølje, tolker de ulike grafene og AI-prognosene, etc. Siden mye av appen er selvforklarende GUI, kan dokumentasjonen fokusere på konseptene (f.eks. forklare hva Monte Carlo-simulering betyr for brukeren, eller hvordan AI-prediksjonen bør tolkes, med disclaimers om at det ikke er finansråd). Vi vil også inkludere en FAQ-seksjon for vanlige spørsmål, f.eks. om datakilder og oppdateringsfrekvens. Denne brukerdokumentasjonen kan legges på prosjektets nettside eller som en markdown-fil i repo som deployes til en static site via e.g. GitHub Pages.
    
*   **Vedlikehold av dokumentasjon:** Dokumentasjon er en kontinuerlig prosess. Teamet forplikter seg til å oppdatere releaselogger og dokumentasjon hver gang nye funksjoner tilføyes (f.eks. når fase 4 fullføres, skal den avanserte porteføljeanalysen dokumenteres både for brukere og i teknisk API-dokumentasjon). For å sikre dette kan vi innføre en praksis der en feature branch ikke anses som “ferdig” før tilhørende dokumentasjon er oppdatert. CI-pipelinen kan også sjekke at f.eks. OpenAPI-skjemaet er synkronisert med koden.
    
*   **Kommunikasjon og kunnskapsoverføring:** Utover skrevne dokumenter, holdes jevnlige møter eller opptak der komplekse deler gjennomgås (f.eks. en gjennomgang av AI-modulen sin arkitektur). Oppsummeringer fra disse kan legges til i docs/ om nødvendig.
    

Til slutt vil vi sørge for at all dokumentasjon lagres sammen med koden (evt. i GitHub Wiki for enklere brukerdoku) slik at det ikke blir liggende lokalt hos enkeltpersoner. Dette gjør det enklere for nye utviklere å sette seg inn i prosjektet og for brukere å finne informasjon. Med en helhetlig dokumentasjonsstrategi som dette unngår vi at den nye fokuseringen på kryptotrading og AI skaper forvirring – alt skal være tydelig beskrevet, fra toppnivå målsetninger ned til hver API-rute.

Sekvensdiagram – eksempel dataflyt
----------------------------------

For å binde sammen de ulike delene av systemet, viser vi avslutningsvis et eksempel på dataflyten i en typisk forespørsel i Investlytics Hub. Under er et sekvensdiagram som illustrerer hvordan en forespørsel om AI-basert prisprognose for en kryptovaluta forløper gjennom systemet:

```mermaid
sequenceDiagram
    participant Bruker as Bruker (Frontend)
    participant API as FastAPI Backend
    participant AI as Ekstern AI-tjeneste
    Bruker->>API: Kall til `/api/v1/ai/crypto_predict?asset=BTC`
    activate API
    API->>MarketAPI: Hent siste pris- og trenddata for BTC (for kontekst)
    API-->>AI: Sender data (historikk, nyheter) for prediksjon
    activate AI
    AI-->>API: Returnerer prognose (f.eks. forventet % endring)
    deactivate AI
    API->>DB: Lager prediksjonsresultat i DB (for logging)
    API-->>Bruker: JSON-respons med AI-basert prisprognose
    deactivate API
    Bruker->>Bruker: Viser prognose i UI (f.eks. "BTC +5% neste 24t")
``` 

I dette eksemplet ser vi at frontend-brukeren utløser en AI-analyse via REST API-et. Backenden (API) henter først relevant markedsdata (kanskje trengs for kontekst), deretter kaller en ekstern AI-tjeneste. Svaret lagres og sendes tilbake til frontend som presenterer det for brukeren. Lignende flyt vil gjelde for andre operasjoner: f.eks. når brukeren åpner porteføljesiden vil frontend kalle et endepunkt /api/v1/portfolio hvorpå backenden henter siste priser, brukerens beholdninger fra DB (eller børs via API), kanskje NFT-listen via Moralis, og sammenstiller et svar. Disse sekvensene er planlagt nøye for å gi rask respons til brukeren til tross for mange bevegelige deler.

Risikovurdering og tiltak 
-------------------------

Selv med solide tekniske løsninger er det viktig å erkjenne potensielle risikoer knyttet til bruken av Investlytics Hub, samt beskrive hvordan disse risikoene håndteres. Nedenfor følger en risikovurdering av noen sentrale utfordringer, sammen med tiltak for å mitigere dem:

#### Narrativ oversikt

*   **Modellfeil og unøyaktige analyser:** Plattformen benytter AI-modeller og algoritmer for å generere investeringsanalyser. Det er en iboende risiko for at modellene kan ta feil eller gi misvisende anbefalinger, enten grunnet begrensninger i treningsdata, bias eller uforutsette markedsforhold. **Tiltak:** Vi adresserer dette ved omfattende testing og validering av modellene (f.eks. backtesting mot historiske data for å se hvordan modellens anbefalinger hadde slått ut). Videre presenteres AI-analysene med passende **forbehold og transparens** – brukerne informeres om at analysene er generert av statistiske modeller som har usikkerhet. Vi oppfordrer til at brukeren anvender egen kritisk sans og eventuelt konsulterer en finansrådgiver for endelige beslutninger. Kontinuerlig oppdatering og forbedring av modellene basert på ny data og tilbakemeldinger bidrar også til å redusere denne risikoen over tid.
    
*   **Feilaktige markedsantakelser:** Investlytics Hub kan gjøre antakelser om markedet (f.eks. at historiske trender fortsetter, eller at visse korrelasjoner holder). Hvis markedet oppfører seg annerledes enn antatt – for eksempel ved plutselige krakk, regulatoriske endringer eller andre **svart svane**\-hendelser – kan anbefalingene være suboptimale eller gale. **Tiltak:** Systemet er designet for å være oppdatert med **sanntidsdata**, slik at analyser alltid bruker fersk informasjon. Vi inkluderer scenarioanalyser og stress-testing der det er relevant, for å illustrere hvordan porteføljer kan påvirkes under ekstreme forhold. Igjen tydeliggjøres det i brukergrensesnittet at alle prognoser har usikkerhet. Teamet overvåker markedsforholdene og vil justere modellene og antakelsene raskt dersom det oppdages systematiske avvik.
    
*   **Data-integritet og kildepålitelighet:** Investlytics Hub er avhengig av eksterne datakilder for markedspriser, finansnyheter osv. Feil eller forsinkelser i disse kildene kan påvirke plattformens output (for eksempel gi mangelfulle analyser). **Tiltak:** Vi benytter flere velrenommerte datakilder og har mekanismer for å kryssjekke data der det er mulig. Systemet har også innbygd håndtering for manglende data – hvis en kilde svikter, varsles det om problemet (jf. observabilitet) og eventuelt byttes til en alternativ kilde. Viktige beregninger har også valideringsregler som fanger opp åpenbart gale verdier og ignorerer dem.
    
*   **Brukerfeil og misbruk av innsikt:** Det finnes en risiko for at brukere feiltolker presenterte data eller ignorerer advarsler, noe som kan lede til dårlige investeringsbeslutninger. Alternativt kan ondsinnede aktører prøve å misbruke plattformen (f.eks. ved å trekke ut store datamengder eller lete etter sårbarheter). **Tiltak:** For det første har vi lagt inn pedagogiske forklaringer og kontekst rundt nøkkeltall og indikatorer i grensesnittet, slik at de blir enklere å forstå riktig. Videre er det implementert robuste **sikkerhetsmekanismer** (beskrevet tidligere: rate limiting, autentisering, mm.) som forhindrer scraping, misbruk og uautorisert tilgang. Brukervilkårene spesifiserer også at informasjonen som gis ikke er investeringsråd og at brukeren selv er ansvarlig for sine handlinger.
    

Gjennom denne risikovurderingen og de tilhørende tiltakene forsøker vi å sikre at Investlytics Hub forblir en nyttig og trygg tjeneste, selv i møte med usikkerhet og potensielle feil.

#### Kvantitativ matrise
| #   | Risiko                                              | Sannsynl. | Konsekv. | Score* | Tiltak / kontroll                                                         | Beredskap |
|-----|-----------------------------------------------------|-----------|----------|-------|---------------------------------------------------------------------------|-----------|
| T-1 | Eksterne API (nedetid / rate-limit / kontrakt-endring) | Høy       | Høy      | 9     | Redis-cache, retry + circuit-breaker,<br>versjonspinne mot schema          | Bytt leverandør, “degraded-mode” banner |
| T-2 | Lekkasje av API-nøkler / JWT                        | Lav       | Kritisk  | 8     | Secrets-manager, AES-kryptert lagring,<br>tvungen key-rotasjon             | Tvangslogg ut brukere, GDPR-varsel |
| T-3 | AI-prognoser gir «false confidence»                 | Medium    | Medium   | 6     | 95 % CI + disclaimer,<br>benchmark mot naive modeller                     | Slå av modulen, rollback modell |
| O-1 | Cloud-kostnader ved trafikk-peak                    | Medium    | Medium   | 6     | Autoscaling caps, kostnad-dashboard i Grafana                             | Flytte budsjett fra mindre kritisk prosjekt |
| P-1 | Scope-creep / forsinkelser                          | Høy       | Medium   | 6     | Sprint-backlog, MoSCoW-prioritering                                       | Kutte/later-tagge nice-to-have |
| S-1 | Regulatorisk endring (MiCA, GDPR, KYC)              | Lav       | Høy      | 5     | Følge høringer, modulær KYC-pipeline                                      | Pausere handel, kun analyse |
   ```markdown
   * Risikoscore = Sannsynlighet (1–3) × Konsekvens (1–3)
   ```

#### Forklaring:
1. **Kolonner**: Tabellen har seks hovedkolonner: `#`, `Risiko`, `Sannsynlighet`, `Konsekvens`, `Risikoscore*`, `Tiltak / kontroll`, og `Beredskapsplan`.
2. **Radene**: Informasjonen er strukturert rad for rad, med hver risiko som en egen rad.
3. **Lister inni celler**: For kolonnene `Tiltak / kontroll` og `Beredskapsplan` har jeg brukt stjerne (`*`) for å markere punkter i listen, slik at det blir lett å lese.

Overholdelse og juridisk etterlevelse
-------------------------------------

I utvikling og drift av Investlytics Hub tas det hensyn til gjeldende lover, forskrifter og etiske retningslinjer for både datahåndtering og finansiell informasjon. Nedenfor beskrives sentrale områder for compliance og hvordan prosjektet etterlever dem:

*   **Personvern (GDPR):** Tjenesten overholder prinsippene i EUs personvernforordning (GDPR). Personlige opplysninger om brukerne (som navn, e-post, porteføljeopplysninger) samles kun inn med gyldig samtykke og brukes utelukkende for definerte formål innen plattformen. Brukere har rett til innsyn i, eksport av og sletting av sine data. Data lagres sikkert og anonymiseres eller pseudonymiseres der det er mulig. Vi har opprettet interne rutiner for håndtering av personopplysninger, inkludert databehandleravtaler med eventuelle tredjepartsleverandører.
    
*   **Finansiell rådgivning og ansvarsfraskrivelse:** Investlytics Hub tilbyr analyse og informasjon, men er **ikke en autorisert finansiell rådgiver**. For å etterleve juridiske krav og bransjenormer, inkluderer vi tydelige ansvarsfraskrivelser i produktet. Det presiseres at innsikten som gis er generell og ikke tatt frem av en menneskelig rådgiver med kjennskap til den enkeltes situasjon. Brukerne gjøres oppmerksomme på at de selv er ansvarlige for investeringsbeslutninger de tar, og at de bør rådføre seg med en profesjonell rådgiver dersom nødvendig. Dette er viktig for å unngå å komme i konflikt med regelverk som krever konsesjon for individuell investeringsrådgivning.
    
*   **Sikkerhetsstandarder og -sertifiseringer:** Ved håndtering av finansielle data og personopplysninger legger vi vekt på å følge anerkjente sikkerhetsstandarder. Om nødvendig kan det gjennomføres **sikkerhetsrevisjoner** eller sertifiseringer (f.eks. ISO 27001 for informasjonssikkerhet) for å dokumentere at vi følger beste praksis. Systemet er også i stor grad designet etter **OWASP-retningslinjer** for å unngå vanlige sårbarheter.
    
*   **Åpen kildekode og lisenser:** (Hvis relevant) Prosjektet benytter enkelte open source-biblioteker og verktøy. Vi sørger for å overholde lisensvilkårene for disse, og kildekoden for Investlytics Hub som helhet er lisensiert under en åpen/kildekode-lisens (f.eks. MIT eller Apache 2.0), noe som er angitt i repositoriets LICENSE-fil. All bruk av tredjepartskode er kreditert, og vi følger oppdateringer for å unngå lisensbrudd.
    

Gjennom disse tiltakene og retningslinjene sikrer vi at Investlytics Hub drives i tråd med juridiske krav og etiske forventninger. Compliance-arbeidet er en kontinuerlig prosess; vi følger med på endringer i lovverket og tilpasser rutiner deretter, slik at både brukernes rettigheter og prosjektets omdømme ivaretas.

### Ikke-funksjonelle hensyn (må ivaretas allerede i MVP)

| **Tema**               | **Detaljer / anbefalte tiltak**                                                                 |
|------------------------|-----------------------------------------------------------------------------------------------|
| **Feilhåndtering**     | - Detaljert, strukturert logging i backend (INFO / WARN / ERROR)                               <br> - Håndterer tap av eksterne API elegant (time-outs, retries, circuit-breaker) <br> - Frontend viser brukervennlig feilmelding (f.eks. «Tjenesten er utilgjengelig, prøv igjen senere») i stedet for å krasje |
| **Sikkerhet**          | - Input-validering via Pydantic                                                               <br> - Stram CORS-policy i produksjon (kun godkjente domener) <br> - Hemmelige nøkler kun på server (.env / secret-store), aldri til klienten <br> - Klar for JWT / OAuth2 når brukerprofiler innføres <br> - Se seksjonen «Rate Limiting» for detaljer |
| **Skalerbarhet**       | - Horisontal skalering av backend bak load-balancer                                            <br> - DB-indekser + replikering når lesevolum øker <br> - Aggressiv Redis-cache for tunge eller hyppige kall <br> - n8n på egen node ved mange workflows <br> - Asynkrone jobber (Celery / RQ) for CPU-tunge prosesser |
| **Brukeropplevelse (UX)** | - Responsivt design (desktop + mobil)                                                       <br> - Lys/mørk temavelger <br> - Interaktive grafer (zoom, hover-tooltip) <br> - Drag-and-drop-oppsett av dashboard-widgets (post-MVP) <br> - Løpende insanking av brukerfeedback |
| **Modularitet / kodehygiene** | - Nye features = egne moduler / tjenester                                                <br> - Unngå duplisering – felles util-pakker <br> - Streng PR-regler og code-review for isolerte endringer |

### Mulige utvidelser **etter** MVP

| **Område**               | **Hva kan legges til - eksempel på verdikjøring**                                                                 |
|--------------------------|-----------------------------------------------------------------------------------------------------------------|
| **Brukerprofiler**       | Full registrering / OAuth 2.0 (Google, GitHub). Lagre foretrukne aktiva, egne simuleringer, tilpasset dashboard-layout. |
| **Flere trading-strategier** | Større strategibibliotek, GUI-bygger for regler, på sikt PineScript-parser som genererer Python-kode automatisk. |
| **Sanntidsdata & live / paper-trading** | WebSocket-feed fra børs = sanntidsgrafter; «paper trading»; valgfri integrasjon mot Pionex API m.m. for ekte handler. |
| **Porteføljeforvaltning** | Samlet oversikt over aksjer, krypto og NFT; spore P/L og risiko; daglige/ukentlige rapporter. |
| **Utvidet aktivastøtte** | Flere markeder (fond, råvarer …) og blokkjeder (Ethereum NFT, DeFi-posisjoner). |
| **Al-forbedringer**      | Pris-prediksjon, anomaly-detsksjon, Al-chat-assistent som svarer på porteføljespørsmål basert på egne data. |
| **Mobilopplevelse**      | Fullt responsiv webapp eller egen React-Native-app som bruker samme REST / WebSocket-API. |
| **Produksjons-DevOps**   | Kubernetes, blue-green deploy, observabilitet (Prometheus + Grafana, ELK-stack). |
| **Community / plugin-økosystem** | Offentlig API + plugin-arkitektur; GitHub Pages-side + blogg for å bygge et åpent utviklermiljø. |

----
<details>
<summary><strong>Seksjon for tillegg</strong></summary>

Sett teksten inn her

</details>