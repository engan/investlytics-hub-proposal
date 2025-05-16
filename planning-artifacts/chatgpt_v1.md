Prosjektplan for Investlytics Hub
=================================

1\. Oversikt over arkitektur og teknologi
-----------------------------------------
![alt text](SkisseA.png)
_Figur: Forenklet arkitektur der en frontend-klient (React) kommuniserer med en backend-tjeneste (FastAPI) over HTTP._Investlytics Hub er planlagt som en fullstack webapplikasjon som kombinerer flere finans- og analysekomponenter. Arkitekturen er modulær, med en tydelig separasjon mellom frontend (klient) og backend (server). Under beskrives hver hovedkomponent og tilhørende teknologi i løsningen:

*   **Frontend (React + TailwindCSS + Recharts):** Frontend-applikasjonen bygges i React for en dynamisk og responsiv brukeropplevelse. TailwindCSS benyttes for rask UI-styling med et konsistent design uten å skrive mye egen CSS. Datavisualisering som grafer og diagrammer implementeres med Recharts, som gir et rikt bibliotek for interaktive finansgrafer (f.eks. kursutvikling, simuleringer). Frontend-komponenten vil kommunisere med backend gjennom REST API-er og WebSocket for sanntidsdata. Den kjører i nettleseren, og for MVP vil den være en enkel én-siders applikasjon (SPA) som viser frem de ulike funksjonsmodulene (strategi-backtesting, NFT-oversikt, simuleringer m.m.).
    
*   **Backend (Python FastAPI + WebSocket):** Backend-tjenesten er bygget med Python ved bruk av FastAPI, et moderne og høytytende web-rammeverk. FastAPI eksponerer REST API-endepunkter for frontendens forespørsler (f.eks. hente data, trigge simuleringer) og støtter WebSocket for å pushe sanntidsoppdateringer (som f.eks. oppdaterte prissignaler eller simuleringresultater) til frontenden. WebSocket-støtte er viktig for deler av applikasjonen som krever kontinuerlig dataflyt, som overvåkning av markeder eller streaming av resultater. Backend vil samle logikk for:
    
    *   **Trading-strategier:** Integrasjon av TradingView **PineScript**\-strategier. Ettersom PineScript kjøres på TradingView-plattformen, planlegges det å importere eller gjenskape utvalgte strategier i Python. For MVP benyttes et bibliotek som **Backtrader** (eller tilsvarende) for å backteste strategier basert på historiske data. FastAPI-backenden kan ta inn parametre (f.eks. en strategi-ID eller kode) og returnere backtest-resultater (ytelsesstatistikk, P/L, indikatorverdier) til frontend. Dette demonstrerer kandidatens evne til å koble et domene-spesifikt språk (PineScript) med Python.
        
    *   **Monte Carlo-simuleringer:** Kandidaten har allerede Monte Carlo-simuleringer deployert via Cloudflare Pages (antakelig som statiske analyser). Disse simuleringene integreres i backend slik at frontend kan forespørre en simulering av f.eks. porteføljebane eller risikoberegning. Enten kan simuleringene kjøres på nytt på server (f.eks. med NumPy/Pandas for rask generering av mange scenarier) eller så kan forhåndskjørte resultater eksponeres. Integrasjonen viser forståelse for statistisk analyse og hvordan presentere det via API. Resultatene sendes til frontend (f.eks. som datasett for grafer).
        
    *   **NFT-overvåking (Solana):** Backend vil også håndtere henting av NFT-porteføljeinformasjon. Ved integrasjon med **Solana**\-blokkjeden planlegges det en enkel kobling mot en tjeneste som **Phantom** wallet eller **solana.fm** API for å hente oversikt over hvilke Solana NFT-er en gitt wallet eier. For MVP forenkles dette til en leseoperasjon: brukeren angir sin Phantom-walletadresse i frontend, og backend kaller solana.fm (eller et annet offentlig endpoint) for å hente en liste over NFT-er (navn, bilder/metadata). Disse dataene sendes deretter til frontend for visning. Dette demonstrerer kandidatens erfaring med Web3/kriptoteknologi og integrasjon mot eksterne API-er.
        
    *   **n8n Workflow-automatisering:** **n8n** er en selvhostet workflow-automatiseringsplattform som kan integreres for datainnsamling og behandling. I arkitekturen fungerer n8n som et eksternt støtteverktøy: For eksempel kan n8n brukes til å periodisk hente markedsdata (aksjekurser, kryptopriser) fra tredjeparter, som igjen leveres til backend (via webhook eller ved at backend henter fra en delt database). For MVP planlegges en enkel n8n workflow, lokalt kjørt eller selvhostet, som henter nødvendig data (f.eks. daglige aksjekurser for en bestemt ticker) og gjør det tilgjengelig for videre prosessering. n8n integrasjonen viser kandidatens evne til å bruke low-code verktøy for å orkestrere dataflyt mellom systemer. (Merk: n8n vil ikke være direkte innebygget i koden, men eksistere side-om-side; integrasjonen kan dokumenteres i README og eventuelt eksporteres som JSON-fil i repoet.)
        
    *   **Database (senere fase):** For MVP antar vi at behovet for en database er minimalt (mye data kan hentes «live» eller fra filer). Men arkitekturen er forberedt for å koble til en database ved behov (f.eks. PostgreSQL eller MongoDB) for lagring av brukerdata, historiske resultater, etc. I MVP kan vi bruke enkle lagringsmetoder (filer eller in-memory) for prototyping.FastAPI-backenden kjører trolig med Uvicorn ASGI-server. All kildekode skrives i Python, strukturert i moduler for klart skille av funksjoner (f.eks. egne moduler for strategies, simulations, nft, api/routes etc.).
        
*   **Kommunikasjon og integrasjon:** Frontend og backend kommuniserer hovedsakelig via REST API-kall (HTTP) for forespørsler som ikke krever øyeblikkelig respons, og via WebSocket for sanntidsdata eller kontinuerlige oppdateringer. For eksempel kan en WebSocket-kanal /ws/simulation strømme progressive resultater fra en Monte Carlo-simulering slik at frontend kan oppdatere en graf iterativt. Tilsvarende kan en kanal /ws/market sende sanntids markedsprisoppdateringer dersom integrert. Bruk av websockets demonstrerer kompetanse med to-veis kommunikasjon i webapplikasjoner.
    
*   **Cloudflare Pages / Hosting:** Siden kandidaten allerede har erfaring med Cloudflare Pages (hvor simuleringene er deployert), kan den statiske frontend bygges og deployes via Cloudflare Pages for gratis hosting og CDN-fordeler. Backend (FastAPI) kan kjøre på en cloud-tjeneste (for MVP kanskje Heroku, Railway eller en enkel VM/container). Alternativt kjøres alt lokalt for demoformål. GitHub Actions kan settes opp for CI/CD slik at pushes til main trigger deploy av frontend (og eventuelt backend) – dette viser DevOps-forståelse.
    
*   **GitHub som plattform:** Hele prosjektet er åpent kildekode og lever på GitHub. Repoet vil inneholde all kode (frontend, backend, workflows) samt dokumentasjon. GitHub benyttes også for prosjektorganisering – Issues for arbeidsoppgaver, Milestones for milepæler (f.eks. MVP ferdig), Projects kan evt. brukes som kanban-board. Denne strukturen gjør det lett for andre utviklere og rekrutterere å følge med på fremdrift og bidra ved behov. Koden versjonshåndteres med git, og kandidatens erfaring med GitHub sikrer god commit-historikk, bruk av pull requests for endringer, og generelt god prosjektstruktur.
    

**Kort oppsummert arkitektur:** React-frontenden kommuniserer med FastAPI-backenden via et definert API. Backenden integrerer eksterne komponenter (n8n for dataflyt, PineScript/backtrader for strategier, Solana API for NFT-data) og kjører algoritmer som Monte Carlo. Resultatet er et "hub" som samler alt i en helhetlig løsning. Arkitekturen er utformet for å være utvidbar – nye datakilder eller funksjoner kan legges til ved å legge til nye moduler/tjenester, og kommunikasjonen skjer via veldefinerte grensesnitt (HTTP/JSON eller websockets). Dette gir et solid utgangspunkt for MVP samtidig som det demonstrerer bredden i kandidatens teknologikompetanse.

2\. Roadmap for MVP (Proof of Concept)
--------------------------------------

For å levere en MVP (Minimum levedyktig produkt) innen rimelig tid og med høy kvalitet, deles utviklingen opp i etapper med konkrete milepæler. Nedenfor er en foreslått roadmap med faser, oppgaver og estimert fremdrift. Hver fase bygger på den forrige, slik at vi kontinuerlig har en fungerende applikasjon som utvides gradvis.

| Fase | Innhold og delmål | Estimert tidsramme |
|------|--------------------|---------------------|
| **Fase 1: Oppstart** | - Prosjektoppsett: Opprette GitHub-repo, sette opp grunnstruktur for frontend (React-app) og backend (FastAPI-app).<br>- Grunnleggende konfigurasjon: Integrere TailwindCSS i React, konfigurere linter/formatter, konfigurere uvicorn/FastAPI.<br>- Verifisering: Kjøre "Hello World" frontend som treffer et enkelt API-endepunkt (f.eks. /api/health) på backend og får svar. | 1 uke |
| **Fase 2: Grunnleggende MVP UI & API** | - UI skjelett: Lage hovedkomponenter i frontend (f.eks. hoveddashboard-side med seksjoner for “Strategier”, “Simuleringer”, “NFT”). Enkle navigasjonslenker eller faner hvis nødvendig.<br>- API-stubber: Definere hoved-API-endepunkt i FastAPI (uten full logikk ennå), f.eks. /api/backtest, /api/simulation, /api/nftportfolio. Returner mock-data eller tomme svar for nå.<br>- WebSocket test: Implementere et eksempel WebSocket-endepunkt (f.eks. /ws/echo) som klienten kan koble til for å teste sanntidskommunikasjon (f.eks. sende en melding og få echo tilbake). | 1–2 uker |
| **Fase 3: Dataflyt og n8n integrasjon** | - n8n Workflow: Sette opp en enkel n8n-instans (lokalt eller Docker) med en workflow som henter eksempeldata (f.eks. laster ned historiske priser for en valgt aksje fra en API) periodisk eller manuelt.<br>- Backend-integrasjon av data: La FastAPI eksponere et endepunkt (f.eks. /api/prices/{ticker}) som enten henter data fra n8n (via webhook) eller direkte kaller en datakilde. For MVP kan vi forenkle ved at backend selv henter data fra en API hvis n8n kompliserer, men hensikten er å demonstrere konseptet med bruk av n8n.<br>- Visning i frontend: Lage en enkel komponent i React som kan forespørre prisdata og vise det i en graf (Recharts-line chart). Dette forbereder grunnlaget for backtesting (man må ha prisdataserie). | 1 uke |
| **Fase 4: PineScript backtesting (PoC)** | - Strategiimport: Velge en enkel TradingView PineScript-strategi som kandidaten har erfaring med. Implementere en tilsvarende logikk i Python, f.eks. ved å bruke Backtrader: definere en strategi (kjøp/selg regler) og mate inn prisdata (fra fase 3).<br>- API for backtest: Implementere /api/backtest endepunktet i FastAPI. Requesten kan inneholde valg av strategi (ID eller navn) og evt. parametre, og backenden kjører backtest (f.eks. Backtrader-run) på et definert datasett. Returner resultater: f.eks. total avkastning, Sharpe ratio, og gjerne serie av porteføljeverdi over tid.<br>- Frontend visning: Oppdatere “Strategier” seksjonen i frontend til å kunne sende en backtest-forespørsel (bruk gjerne et skjema der bruker velger en forhåndsdefinert strategi og kanskje ticker/tidsrom), og motta resultat. Vise resultatet grafisk (f.eks. porteføljekurve via Recharts line chart) og tekstlig (f.eks. "Total avkastning: X%").<br>- Validering: Sørge for at backtesten kjører innen rimelig tid (noen sekunder). Denne fasen demonstrerer full flyt: frontend input -> backend kjøring av algoritme -> resultat tilbake. | 2 uker |
| **Fase 5: Monte Carlo-simulering** | - Integrasjon av simulering: Gjenbruke eksisterende MC-simuleringer. Hvis de er implementert i JavaScript (fra Cloudflare Pages), vurdere å portere til Python for enklere integrasjon, eller inkludere dem som et modul i frontend.<br>- API for simulering: Implementere /api/simulation som kjører en Monte Carlo-simulering (f.eks. generere 1000 tilfeldige utfall av en portefølje over 1 år basert på historisk avkastningsfordeling). Returnere aggregerte data som kan vises (f.eks. percentiler for sluttverdi, eller sannsynlighet for ulike utfall).<br>- Frontend visning: I “Simuleringer” seksjonen, legge til en knapp for å starte simulering. Etter kjøring, vise et diagram (f.eks. histogram av utfallsfordeling eller en fanegraf som viser beste/verste case). Recharts kan benyttes for histogram eller områdetabell.<br>- WebSocket streaming (hvis tid): Hvis simuleringen er lang, vurder å sende progressive resultater over WebSocket slik at frontend kan oppdatere løpende. Hvis ikke, kan simulering kjøres raskt nok i ett kall. | 1 uke |
| **Fase 6: NFT-portefølje** | - Solana API integrasjon: Implementere /api/nft/{wallet} endepunkt. Bruk en tjeneste som Solana.fm API eller offisiell Solana JSON-RPC for å hente alle NFT tokens for en gitt wallet-adresse. Parse de viktigste data (token navn, ev. bilde-URI).<br>- Frontend UI: I “NFT” seksjonen, lage et input-felt for wallet-adresse (eller en knapp "Koble Phantom" dersom mulig i nettleser). For MVP kan vi be brukeren lime inn adresse. Ved submit, kalle backend /api/nft/{wallet} og vise en liste over NFT-er: for hver NFT vise navn og eventuelt bilde eller link. Enkel styling med Tailwind, kanskje en grid av kort.<br>- Validering: Håndtere feil (ugyldig adresse, ingen NFT-er) på en brukervennlig måte. Denne funksjonen viser enda en teknologi-integrasjon (blokkjede) i applikasjonen. | 1 uke |
| **Fase 7: Avsluttende polish og dokumentasjon** | - Testing og fikser: Gjennomgå MVP-funksjonalitetene, teste ende-til-ende. Fikse eventuelle kritiske bugs. Sørge for at applikasjonen kjører konsistent (f.eks. backend håndterer parallelle forespørsler greit, frontend ikke har åpenbare feil i konsollen).<br>- UI/UX forbedringer: Legge til små forbedringer som gjør demoen penere – f.eks. bedre fargevalg med Tailwind, responsive layout hvis nødvendig, hover-effekter, etc. (Begrense tiden her da funksjonalitet er viktigst, men litt polish gjør et godt inntrykk.)<br>- Dokumentasjon: Fullføre all dokumentasjon for repoet (README.md med oversikt og instruksjoner, CONTRIBUTING.md, ROADMAP.md oppdatert i henhold til faktisk implementasjon).<br>- Milestone: MVP ferdig: Tagge en release (f.eks. v0.1.0) i GitHub. Eventuelt deploye MVP: bygge og laste opp frontend til Cloudflare Pages (eller alternativt Vercel/Netlify) og kjøre backend på en enkel server så rekrutterere kan se en live demo. | 1 uke |

**Totalt estimat:** ca. 6–7 uker for én utvikler (kandidaten alene) å gå fra oppstart til ferdig MVP, gitt deltid/normal progresjon. Tidsplanen er fleksibel, men rekkefølgen prioriterer en tidlig fungerende kjerne (fase 1-2) og deretter inkrementell tillegg av funksjoner.

Hver fase korresponderer med en **milepæl (Milestone)** i GitHub (se seksjon 4), slik at progresjon lett kan følges. Dersom visse deler tar lengre tid, kan de parallelliseres noe (f.eks. kan NFT-delen utvikles uavhengig av PineScript-delen, i den grad ressurser tillater).

3\. GitHub-repo struktur og navnekonvensjoner
---------------------------------------------

En ryddig repository-struktur og klare navnekonvensjoner gjør det enklere for andre å navigere koden og bidre. Under er forslag til hvordan GitHub-repoet for Investlytics Hub organiseres:

**Repo-navn:** investlytics-hub (konsistent og enkelt). Repoet vil være offentlig og under kandidatens GitHub-konto._(Alternativt kan man dele det i to repos, investlytics-hub-frontend og investlytics-hub-backend, men for MVP er det hensiktsmessig å samle alt i ett monorepo for enkelhets skyld.)_

**Hoved-branch:** main brukes som produksjons/stabil branch. Under utvikling kan en dev-branch brukes for å samle opp features før de merges til main, men siden dette er et MVP-prosjekt av begrenset omfang, kan man også jobbe direkte med feature branches som merges til main via pull requests når de er klare.

**Branch-navn konvensjoner:** Bruk beskrivende navn for feature branches, prefiks som indikerer type:

*   feature/ for nye funksjoner (f.eks. feature/pinescript-backtesting).
    
*   fix/ for bugfiks (f.eks. fix/socket-connection-bug).
    
*   docs/ for rene dokumentasjonsendringer (f.eks. docs/update-readme).
    

**Mappestruktur i repoet:**

```text 
investlytics-hub/
├── frontend/              # React frontend kildekode
│   ├── public/            # Offentlige filer (f.eks. index.html, favicon)
│   └── src/               # React source (komponenter, hooks, services)
│       ├── components/    # Gjenbrukbare React-komponenter (grafer, tabeller, etc.)
│       ├── pages/         # Sider/utsikt-komponenter (Dashboard, NFT-oversikt, etc.)
│       ├── styles/        # TailwindCSS konfigurasjon eller ekstra CSS
│       └── ...            # etc. (React app oppsett filer, index.jsx, App.jsx)
├── backend/               # FastAPI backend kildekode
│   ├── app/               # Python-pakker for appen
│   │   ├── main.py        # Oppstart av FastAPI-appen, inkluderer routes
│   │   ├── api/           # API routes definert, f.eks. api/routes/
│   │   ├── core/          # Kjernefunksjonalitet (strategi-logikk, simuleringer, etc.)
│   │   ├── models/        # Evt. datamodeller/schemaer (Pydantic-modeller)
│   │   └── services/      # Tjenestelag, f.eks. integrasjon mot solana API, etc.
│   └── requirements.txt   # Python-avhengigheter
├── n8n/                   # (Valgfritt) Exporterte n8n workflows eller noter
├── simulations/           # Monte Carlo-simulering kode eller data (hvis portert til Python)
├── .github/               # GitHub config (issue templates, workflows for CI/CD)
├── README.md
├── CONTRIBUTING.md
└── ROADMAP.md
```

_Bemerk:_ Over er en mulig struktur. Det viktigste er å holde frontend og backend adskilt, og dokumentasjon for seg. Hvis prosjektet vokser kan man vurdere en mer sofistikert monorepo-oppsett (f.eks. med verktøy som Yarn workspaces eller pnpm for å håndtere flere pakker), men for MVP er det ikke nødvendig.

**Navnekonvensjoner i kode:**

*   Bruk engelsk som kodespråk (variabelnavn, funksjoner) for konsistens.
    
*   Fil- og mappenavn i kildekoden i kebab-case eller snake\_case etter hva som er idiomatisk (React komponentfiler typisk CamelCase.jsx for komponenter, mens øvrige filer i lowercase).
    
*   Git commit meldinger: Følg konvensjonell stil, f.eks. _“Add: Basic FastAPI server with health endpoint”_ eller bruk konvensjoner som _feat:_, _fix:_ i begynnelsen. Korte og beskrivende, og knytt til Issue-nummer dersom relevant (e.g. “feat: implement backtest API (closes #12)”).
    
*   Versjonering: Tagge versjoner ved viktige milepæler (v0.1.0 for MVP). SemVer kan brukes hvis det gir mening når utvidelser kommer.
    

**GitHub-integrasjoner:**

*   Aktiver GitHub Issues (for oppgavehåndtering), Projects (Kanban board for oversikt, valgfritt) og Actions (for CI/CD pipeline – f.eks. automatiske tester eller deploy av frontend).
    
*   Sett opp beskyttede branch-regler på main (krav om PR og godkjenning før merge, selv om kandidaten er eneste utvikler; demonstrerer beste praksis).
    
*   Bruk GitHub Wiki eller docs/ mappe hvis mer omfattende dokumentasjon skal legges til (utenom det som dekkes i README).
    

Denne strukturen og konvensjonene vil gjøre det tydelig for rekrutterere og andre utviklere hvordan prosjektet er satt sammen, og viser at kandidaten er vant med profesjonell bruk av GitHub.

4\. GitHub Issues og Milestones
-------------------------------

For å organisere arbeidet effektivt brukes GitHub Issues til å opprette oppgaver/featurerequests og Milestones til å gruppere dem etter mål (f.eks. MVP Milestone). Hver Issue beskriver en avgrenset oppgave med klar akseptansekriterier, og merkes med labels for kategori og prioritet. Under er forslag til labels, samt eksempler på utformede issues og milepæler.

#### Labels og deres betydning

Det foreslås å bruke følgende labels på GitHub for å kategorisere og prioritere issues:

| Label         | Beskrivelse                                                                                                                                |
|---------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| `frontend`    | Oppgaver relatert til frontend-klienten (React, UI, grafikk).                                                                              |
| `backend`     | Oppgaver for backend-tjenesten (FastAPI, database, API-logikk).                                                                            |
| `simulation`  | Oppgaver knyttet til Monte Carlo-simuleringer eller beregninger.                                                                             |
| `n8n`         | Oppgaver for integrasjon eller bruk av n8n workflow-automatisering.                                                                        |
| `AI`          | (Fremtidig) Oppgaver som involverer AI/ML eller avansert algoritmer. (Ikke prioritert i MVP, men label er klar for eventuell utvidelse). |
| `docs`        | Dokumentasjonsoppgaver (README, docstrings, wiki etc.).                                                                                    |
| `infra`       | Infrastruktur, devops eller konfigurasjonsrelaterte oppgaver (CI/CD, deployment).                                                          |
| `high priority`| Kritisk oppgave som har høy prioritet (ofte krav for MVP eller blokkerer fremdrift).                                                       |
| `nice to have`| Oppgave som er ønskelig men ikke essensiell. Kan utsettes til etter MVP hvis tiden ikke strekker til.                                      |
| `help wanted` | Oppgaver der bidrag fra andre ønskes eller trengs (fint å ha for å invitere open-source bidrag).                                             |
| `in progress` | (Brukes gjerne via Projects/Kanban) Indikerer at noen jobber med oppgaven nå.                                                              |
| `blocked`     | Oppgaven er blokkert av en ekstern faktor eller en annen issue som må løses først.                                                          |

Labels som `frontend`, `backend`, `simulation` osv. kategoriserer hva slags arbeid det er, mens `high priority`, `nice to have` angir viktighet. Man kan kombinere flere labels per issue (f.eks. en oppgave kan ha `frontend` + `high priority`). Dette gir rask filtrering – f.eks. se alle `high priority` som må med i MVP, eller alle `n8n` oppgaver.

### Eksempler på GitHub Issues

Nedenfor er noen eksempler på issues som vil finnes i prosjektets plan. Hver issue er formulert med tittel, beskrivelse, akseptansekriterier og et estimat på arbeidsmengde. (Estimater er veiledende og kan justeres underveis.)

*   **Issue:** _Opprett React-prosjekt og integrer TailwindCSS_
*   **Labels:** frontend, high priority
*   **Beskrivelse:** Starte et nytt React-prosjekt for frontend og sette opp TailwindCSS for styling. Dette inkluderer å initialisere prosjektet (f.eks. med Create React App eller Vite), installere TailwindCSS, og bekrefte at styling virker.
    **Akseptansekriterier:**
    
    *   Prosjektet bygger og kjører lokalt (kjøre npm start uten feil).
        
    *   TailwindCSS er konfigurert (postCSS/oppsett) og man kan se at en Tailwind-klasse faktisk påvirker et element på skjermen (f.eks. en
        
        vises med blå bakgrunn).
        
    *   Kode og avhengigheter commitet til repo (frontend/-mappen opprettet). 
       **Estimat:** ca. 4 timer (oppsett og testing).
        
*   **Issue:** _Implementer grunnleggende FastAPI-backend med helsesjekk-endepunkt_
*   **Labels:** backend, high priority
*   **Beskrivelse:** Sette opp en minimal FastAPI-applikasjon. Lage et endepunkt /api/health som returnerer f.eks. JSON {"status": "ok"} for å verifisere at serveren kjører. Dette vil danne grunnlag for videre backend-utvikling.
*   **Akseptansekriterier:**
    
    *   FastAPI kjører lokalt (uvicorn starter uten feil).
        
    *   HTTP GET til /api/health returnerer 200 OK med forventet innhold.
        
    *   Kodestruktur følger prosjektets konvensjoner (for eksempel at main.py oppretter app = FastAPI() og inkluderer routere).
    *   **Estimat:** 2 timer (enkelt oppsett).
        
*   **Issue:** _n8n workflow for å hente testdata (f.eks. aksjekurser)_
*   **Labels:** n8n, backend
*   **Beskrivelse:** Lage en enkel n8n workflow som henter aksjekurser for en gitt ticker fra en offentlig API (f.eks. Alpha Vantage eller Yahoo Finance) og gjør dataene tilgjengelige for backend. Workflowen kan trigges manuelt i MVP. Inkluder dokumentasjon på hvordan kjøre workflowen.
    **Akseptansekriterier:**
    
    *   n8n workflow-fil (JSON) er sjekket inn i n8n/-mappen med instruksjoner.
        
    *   Workflowen henter f.eks. siste 30 dager med kurser for en hardkodet ticker (eller konfigurerbar) og lagrer resultatet i et format backend forstår (f.eks. en JSON-fil eller via webhook).
        
    *   Backend kan integreres ved å enten lese JSON-filen eller eksponere et webhook-endepunkt som workflowen kan sende data til (f.eks. /api/prices/update).
        **Estimat:** 1 dag (inkl. testing mot ekstern API og håndtering av nøkler/rate limits).
        
*   **Issue:** _Backtesting av PineScript-strategi via Backtrader_
*   **Labels:** backend, simulation, high priority
*   **Beskrivelse:** Implementere logikken for en spesifikk trading-strategi i Python for backtesting. Strategien kan være en forenklet versjon av en PineScript-strategi kandidaten har laget (f.eks. basert på bevegelige snitt crossover). Bruke Backtrader biblioteket til å kjøre backtesting på historiske priser.
    **Akseptansekriterier:**
    
    *   En Backtrader “Strategy” klasse er implementert for strategien (kjøps-/salgslogikk).
        
    *   Funksjon eller modul i core/strategies/ som tar inn prisdata (f.eks. som liste av OHLC-verdier) og returnerer resultat: total avkastning %, antall handler, og daglig porteføljebalanse.
        
    *   FastAPI-endepunkt /api/backtest?strategy=&ticker= kobles til denne funksjonen: den henter nødvendig data (fra n8n eller en intern kilde), kjører strategien og returnerer resultatene (som JSON).
        
    *   Enhetstest eller manuelt testet med et lite datasett for å verifisere at logikken gjør som forventet (f.eks. kjøper når den skal).
        **Estimat:** 2–3 dager (inkluderer koding av strategi og integrasjon).
        
*   **Issue:** _Vis NFT-portefølje i dashboard_
*   **Labels:** frontend, backend, nice to have
*   **Beskrivelse:** Bygge funksjonalitet for å vise NFT-er tilhørende en brukeradresse i appen. Backend-delen innebærer å hente NFT-data fra Solana (via solana.fm API eller annet), frontend-delen innebærer UI-komponenter for å vise disse dataene.
    **Akseptansekriterier:**
    
    *   Backend-endepunkt /api/nft/{wallet} returnerer en liste med NFT-objekter (minst navn eller symbol, evt. bildeURI).
        
    *   Frontend har et inputfelt for walletadresse og en "Hent NFT" knapp. Ved submit kalles APIet og resultatene (liste av NFTer) vises.
        
    *   Hver NFT vises med navn og evt. et bilde/thumb. (Kan bruke en enkel  med URL fra data eller en placeholder hvis ingen bilde).
        
    *   Feilhåndtering: ugyldig adresse gir feilmelding til bruker, og tom liste håndteres pent (f.eks. "Ingen NFT funnet").
        **Estimat:** 1–2 dager (inkluderer både frontend og backend del, samt testing med en faktisk walletadresse).
        

I tillegg til disse eksemplene vil det finnes en rekke andre issues, for eksempel "_Designjusteringer for dashboard_", "_Skrive README med instruksjoner_", "_Konfigurer CI/CD workflow på GitHub Actions_", etc., som vil få docs eller infra labels. Hver issue skal ha klare kriterier slik som over, slik at man alltid vet når oppgaven er "Done". Estimater hjelper med planlegging, men er ikke absolutte.

### Milestones og milepæler

**Milestones** brukes for å gruppere relaterte issues mot et større mål. For MVP anbefales disse milepælene:

*   **Milestone: MVP Oppstart ferdig** – Inneholder Fase 1 og 2 issues (grunnleggende oppsett). Fullført når basis frontend+backend kommuniserer.
    
*   **Milestone: MVP Data & Backend** – Inneholder Fase 3 og 4 (dataflyt via n8n, og backtest-funksjonaliteten). Fullført når en enkel strategi kan backtestes og vises.
    
*   **Milestone: MVP Fullført (v0.1)** – Inneholder Fase 5, 6, 7 issues, alt som trengs for at MVP produktet skal være komplett (simulering, NFT, dokumentasjon, finpuss). Når denne er nådd, tagges versjonen og MVP anses som levert.
    

Hver Milestone på GitHub vil ha en beskrivelse som oppsummerer målsettingen (f.eks. "Denne milepælen dekker alt som trengs for en demonstrasjonsklar MVP av Investlytics Hub."). Prosent fullført øker ettersom issues lukkes, noe som gir en tydelig visuell indikasjon på fremdriften for alle interessenter.

Vedlikehold av issues og milepæler gjøres jevnlig: hvis noe utsettes eller omprioriteres, kan issues flyttes mellom milepæler. Kandidaten (og eventuelle bidragsytere) vil ha jevnlige avsjekker mot roadmap-en for å sikre at prosjektet holder kursen.

5\. Dokumentasjonsstruktur (norsk)
----------------------------------

God dokumentasjon er kritisk, spesielt siden prosjektet skal vise frem kandidatens kompetanse til rekrutterere, utviklere og investorer. All skriftlig dokumentasjon vil være på norsk (kodekommentarer kan være engelsk for bredere utviklerforståelse, men hoveddokumenter på norsk). Følgende filer og struktur planlegges:

#### README.md

Repoets **README.md** er ofte det første folk ser. Vi strukturerer denne som et levende dokument som gir full oversikt over prosjektet på en kortfattet måte. Forslag til inndeling av README:

*   **Tittel og Introduksjon:** En kort beskrivelse av Investlytics Hub, prosjektets mål og hvorfor det er laget (demonstrere kompetanse innen fintech, automasjon, web-utvikling). Få frem at det er et åpne kildekodeprosjekt og inviter leseren til å utforske.
    
*   **Arkitektur Oversikt:** Sammendrag av arkitekturen med en enkel diagram (kan være embed av figur eller ASCII-diagram) og forklaring av komponenter (React frontend, FastAPI backend, etc., i korte trekk). Dette er på mange måter en kondensert versjon av seksjon 1 i denne planen, men mer spisset mot hva som er implementert per nå.
    
*   **Funksjonalitet (MVP-omfang):** Bullet-liste over hva MVP-versjonen av Investlytics Hub kan gjøre. For eksempel:
    
    *   _Viser historiske prisgrafer for utvalgte tickere._
        
    *   _Kjører backtesting av en enkel trading-strategi og visualiserer resultatet._
        
    *   _Simulerer fremtidige porteføljemuligheter med Monte Carlo._
        
    *   _Henter og viser NFT-er fra en Solana wallet._(Og gjerne notere at dette er for demoformål, ikke en ferdig produktløsning.)
        
*   **Hvordan komme i gang (Installasjon og kjøring):** Steg-for-steg på norsk for å kjøre prosjektet lokalt. Dette inkluderer:
    
    1.  Krav (Node.js, Python 3.x, eventuelle API-nøkler for datakilder om nødvendig).
        
    2.  Klone repoet.
        
    3.  Instruksjoner for å starte backend: f.eks. cd backend && pip install -r requirements.txt && uvicorn app.main:app.
        
    4.  Instruksjoner for å starte frontend: f.eks. cd frontend && npm install && npm start.
        
    5.  Hvordan åpne appen i nettleser (default http://localhost:3000 etc.).
        
    6.  (Hvis relevant, hvordan starte n8n or legge inn API-nøkler - eventuelt linke til separat doc for det).
        
*   **Bruk av applikasjonen:** Kort forklare hvordan man bruker UI-en når den kjører. F.eks. "For å kjøre en backtest, gå til Strategi-seksjonen, velg strategi X og trykk 'Kjør'.", "For å se NFTer, skriv inn din Solana-adresse..." osv. Dette for at en ikke-utvikler rekrutterer også kan prøve uten å være usikker.
    
*   **MVP Begrensninger:** En ærlig liste over hva som **ikke** er med i MVP som en fullverdig produkt normalt ville hatt. F.eks. "Autentisering/brukerpålogging er ikke implementert", "Kun en strategi er tilgjengelig i backtesting", "Data hentes kun for én forhåndsdefinert ticker" etc. Dette setter forventninger riktig og viser at kandidaten er klar over hva som mangler, selv om det ikke er del av demoen.
    
*   **Videre planer:** En teaser om hva som kan komme (som kanskje peker til ROADMAP.md for detaljene).
    
*   **Bidrag:** En kort note ala "Bidra gjerne! Se CONTRIBUTING.md for hvordan du kan involvere deg."
    
*   **Lisens:** Oppgi lisens (f.eks. MIT License) for prosjektet.
    

README skal gi nok info til at både tekniske og ikke-tekniske lesere skjønner prosjektets formål og hvordan de kan kjøre det. Den holdes relativt kort, og linker til mer detaljert info i de andre filene ved behov.

#### CONTRIBUTING.md

**CONTRIBUTING.md** beskriver hvordan eksterne (eller kandidaten selv, ved senere anledning) kan bidra til prosjektet. Siden prosjektet er kandidatens eget showcase, forventes ikke store bidrag utenfra i startfasen, men å ha et slikt dokument signaliserer profesjonalitet og åpenhet.

En skisse til innhold:

*   **Introduksjon:** Takke for interessen i å bidra, og en setning om at prosjektet setter pris på forslag, feilmeldinger, nye funksjoner osv.
    
*   **Hvordan sette opp prosjektet lokalt:** Henvisning til README for grunnleggende oppsett. Evt. tilleggsinfo for utviklingsmodus, som at man bør kjøre frontend og backend samtidig, evt. hvordan kjøre tester hvis slikt finnes.
    
*   **Retningslinjer for kode:** Beskriv kodestil (f.eks. PEP8 for Python, bruk av Prettier/Eslint for JS). Nevn om det er noen arkitektoniske prinsipper (f.eks. "hold logikk i backend så frontend forblir presentasjonslag").
    
*   **Git workflow:** Forklar at man bør forke repoet, lage en branch, committe med beskrivende meldinger, og lage en pull request. Nevn at PRer skal knytte til en issue hvis aktuelt, og at alle CI-tester må passere (dersom CI er satt opp).
    
*   **Issue tracker:** Oppfordre til å åpne issues for bugs eller forespørsler. Beskriv gjerne hvordan man skriver en god issue (kort beskrivelse, steg for å reprodusere bug, forslag til løsning).
    
*   **Code of Conduct:** Referer til en enkel Code of Conduct (man kan inkludere en CODE\_OF\_CONDUCT.md eller bare nevne at man forventer en respektfull tone).
    
*   **Kontakt:** Oppgi evt. kontaktinfo (e-post eller GitHub discussions) for dem som vil diskutere større endringer før de koder.
    

CONTRIBUTING.md skal gjøre det lett for en potensiell bidragsyter å komme i gang og forstå hvordan prosjektet drives, samtidig som den viser at kandidaten kjenner til samarbeidspraksiser i open source.

#### ROADMAP.md

**ROADMAP.md** gir en mer detaljert oversikt over planlagte funksjoner og milepæler fremover. Den bygger på roadmap fra seksjon 2, men kan oppdateres etter hvert som MVP skrider frem. Strukturforslag:

*   **Innledning:** Kort om at dette dokumentet sporer planene for utvikling av Investlytics Hub. Forklarer at roadmap er inndelt i MVP og videre utvikling.
    
*   **Milepæler og leveranser:** En punktliste eller tabell over kommende milepæler. For eksempel:
    
    *   _MVP 0.1 – Core funksjonalitet:_ (ferdig Q2 2025) – Inneholder grunnleggende dashboard med strategitest, simulering, NFT-visning.
        
    *   _Versjon 0.2 – Utvidet data & strategier:_ (planlagt Q3 2025) – Legge til flere strategier, bedre UI, realtid datafeeds.
        
    *   _Versjon 0.3 – Brukerhåndtering:_ – Legge til brukerprofiler, lagring av resultater, osv.(Disse er eksempler, kan justeres etter faktisk plan.)
        
*   **Detaljert roadmap for MVP:** Gjenbruk gjerne innhold fra seksjon 2 i noe kortere form. Kan listes som Fase 1, 2, 3... med de viktigste deliverables per fase. Hak av bokser (- \[x\] / - \[ \]) kan brukes om man vil vise hva som er gjort.
    
*   **Fremtidige idéer:** Egen seksjon for ideer som ikke er planlagt i konkrete versjoner ennå, men som vurderes (f.eks. "Mobilapp", "Støtte for Ethereum", etc.). Dette signaliserer visjoner uten å forplikte dem til en tidslinje.
    
*   **Oppdateringer:** Noter at roadmap vil bli oppdatert jevnlig. Når MVP er levert, vil nye mål settes basert på tilbakemeldinger og ambisjoner.
    

ROADMAP.md er nyttig for interessenter som vil vite "hva kommer videre?" uten å måtte grave gjennom alle issues. Det skal vise at kandidaten tenker langsiktig og har en plan utover bare å hacke sammen en MVP.

6\. Videreutvikling etter MVP
-----------------------------

Selv om MVP fokuserer på kjernefunksjonalitet for demonstrasjon, finnes det en rekke muligheter for å videreutvikle Investlytics Hub til et mer robust og omfattende verktøy. Noen forslag til fremtidige utvidelser:

*   **Flere trading-strategier og brukerdefinerte strategier:** Legge til et bibliotek av ferdige strategier (både tekniske indikator-baserte og kanskje AI-baserte strategier). La brukeren selv definere en enkel strategi gjennom et GUI eller ved å importere sin egen PineScript og automatisere oversettelsen til Python. Dette vil virkelig demonstrere dybde i kompetanse på tradingalgoritmer.
    
*   **Sanntidsdata og multiple markeder:** Integrere en sanntids datafeed for aksjer/krypto (via websockets eller streaming API) slik at strategier kan kjøres “live” på paper trading. Støtte flere markeder utover bare det ene som MVP dekker, f.eks. ulike aksjeindekser, råvarer eller krypto-par.
    
*   **Porteføljeforvaltning:** Utvide NFT-delen til en mer generell porteføljeoversikt som inkluderer både tradisjonelle aktiva (aksjer/fond) og digitale aktiva (krypto, NFT). Dette kan innebære integrasjon mot meglere eller børser via API for å hente faktiske beholdninger.
    
*   **Brukerautentisering og lagring:** Implementere innlogging for at brukere kan lagre sine innstillinger, strategivalg og resultater. For eksempel bruke OAuth for GitHub/Google for enkel innlogging. Lagring av data i en database (post-MVP kan en f.eks. legge til PostgreSQL) slik at en bruker kan komme tilbake og se sine tidligere simuleringer eller backtests.
    
*   **Forbedret UI/UX:** Inkludere mer interaktiv visualisering, f.eks. annotering av grafer (markere kjøp/salg på grafen), drag-and-drop komponenter i dashboardet, mørk/lys-tema bytte, osv. Dette vil gjøre verktøyet mer profesjonelt og attraktivt å bruke.
    
*   **AI-analyse og anbefalinger:** Introdusere AI-baserte komponenter (jf. AI-label) – for eksempel en modul som bruker maskinlæring for å forutsi trender eller en chatbot-assistent som kan svare på finansspørsmål ved å bruke dataene i huben. Dette kan innebære integrasjon med eksisterende ML-biblioteker eller API-er (TensorFlow/PyTorch for prediktive modeller, GPT-baserte assistenter for Q&A). Slik funksjonalitet vil imponere ved å vise at kandidaten kan kombinere AI med finans.
    
*   **Utvidet blokkjede-støtte:** Støtte flere blokkjeder for krypto/NFT-portefølje, f.eks. Ethereum (Metamask-integrasjon) slik at Investlytics Hub blir en bredere plattform for digital assets analytics. Også overvåking av DeFi-posisjoner kunne være en nisje å utforske.
    
*   **Mobilvennlig/egen app:** Om behov, lage en forenklet mobilfrontend (React Native eller Flutter) som konsumerer de samme API-ene, slik at investeringsanalysene kan sees “on-the-go”. Alternativt sikre at web-appen er fullresponsiv for mobilbruk.
    
*   **Produksjonsklar drift:** Containerisere applikasjonen (Docker) for enklere deploy. Sette opp kontinuerlig utrulling (CD) og monitoring. Dette steget handler mer om å vise DevOps-ferdigheter: f.eks. bruk av Kubernetes på sikt, logging/overvåkning verktøy (ELK-stack, Prometheus) for en produksjonssetting.
    
*   **Community-building:** Hvis prosjektet får interesse, legge til en GitHub Pages docs side eller en liten prosjektwebsite som presenterer prosjektet, bloggposter om utviklingsprosessen, etc. Dette er ikke en kode-funksjon, men vil vise engasjement og evne til å bygge et community rundt et open-source prosjekt.
    

Videreutviklingsforslagene over kan prioriteres basert på tilbakemelding fra brukere og interessenter etter MVP. Planen viser at kandidaten har et langsiktig blikk: MVP er bare et fundament, og det finnes en visjon for hvordan Investlytics Hub kan vokse i funksjonalitet og verdi. Dette demonstrerer ikke bare teknisk kunnskap, men også produktforståelse og evne til å planlegge for fremtiden.