## Datamaskinen
I hjertet av datamaskinen finner vi et lite instruksjonssett som hovedsakelig anvender enkle matematiske eller logiske operasjoner. Over tid er det bygget et mangfold av tjenester og applikasjoner på toppen av disse intruksjonene. Hver gang vi sender en mail, ringer, spiller eller streamer en film, blir dette realisert blant annet fordi datamaskinen evner å legge sammen to tall i et forrykende tempo.

På den andre siden bygger mange oppgaver som mennesker løser med letthet på mer enn logikk og matematikk: Gjenkjenne personer i et bilde, føre en dialog skriftlig eller muntlig, eller velge maten man skal spise til lunsj. Når jeg står i kantinen og skal velge mellom salat eller en varmrett, går jeg for det jeg har lyst på. Det er ingen bevisst logisk prosess bakenfor som styrer valget; det er basert på erfaring fra mat jeg har spist tidligere, samt en god porsjon magefølelse. Maskinlæring handler om å presentere datamaskinen for ulike typer mat, og la den ut fra dette generalisere sine egne matpreferanser.

## Datadrevet læring
Maskinlæring handler om å lære fra data. Hvilke utfordringer bringer det med seg? Hvordan ser egentlig data ut?

### Semantikk og syntaks
I sin enkleste form, trenger datamaskinen å vite to ting for å kunne lære: _Eksempler_ på input og hva de _leder til_. _Læringsprosessen_ består i å forme en _modell_ som forklarer hvordan hvert eksempel fører til sin _konklusjon_. For å fortsette lunsjanalogien, kan et eksempel være `lasagne` med ønsket konklusjon `bedre enn salat`. Realiteten er likevel at datamaskinen ikke vet hva hverken `lasagne` eller `bedre enn salat` betyr. _Syntaktisk_ tillater maskinen bare tall, så på en eller annen måte må man representere retten lasagne med en rekke tallverdier. La oss for enkelhetens skyld si at vi gjør dette ved at hver smak i lasagnen representeres med et tall mellom `0.0` og `1.0`, avhengig av hvor dominant smaken er i retten. `bedre enn salat` kan på lignende vis representeres med `0` eller `1`, dersom retten er dårligere eller bedre enn salat.

###### Semantisk tolkning
|Eksempel|Konklusjon|
|---|---|
|Lasagne|Bedre enn salat|
|Laks|Bedre enn salat|
|Pulled pork|Bedre enn salat|
|Matpakke|Ikke bedre enn salat|

###### Syntaktisk uttrykk
|Eksempel|Konklusjon|
|---|---|
|[0.2, 0.1, 0.7, 0.1, 0.3]|1|
|[0.1, 0.2, 0.5, 0.2, 0.3]|1|
|[0.3, 0.2, 0.7, 0.1, 0.4]|1|
|[0.2, 0.2, 0.2, 0.2, 0.2]|0|

Ettersom data representeres med tall, ser vi at maskinen egentlig ikke bryr seg om den _semantiske_ tolkningen av `lasagne`; den forholder seg kun til en rekke verdier. Dette skiller seg fra tradisjonelle metoder, hvor man for eksempel hyrer inn en domeneekspert til å lage håndskrevne regler skreddersydd for hvert enkelt problem og domene. Ved å la datamaskinen lære fra data, blir regler automatisk funnet basert på mønstre og mer eller mindre latent informasjon i datasettet.

### Datakvalitet
Fordi maskinen lærer fra et datasett, betyr det at kvaliteten på datasettet er avgjørende for resultatet. Når læringsprosessen er avsluttet, sitter vi igjen med en modell som helst skal kunne forklare hele problemet. Dersom datasettet mangler eksempler fra store regioner av problemområdet, så vil heller ikke modellen være rustet til å jobbe i disse regionene. Eksempel: Hvis datasettet ikke inneholder vegetare lunsjretter, så vil modellen være mindre rustet til å gjøre et godt valg mellom disse og salat. Selv om det finnes noen hederlige unntak, blir i all hovedsak aldri modellens kvalitet bedre enn datasettets.

Fordi vi ønsker å eksponere modellen for alle mulige typer eksempler, er det som regel en sammenheng mellom størrelsen på datasettet og kvaliteten på modellen. Jo mer data dess bedre.

Jeg erkjenner at lunsjanalogien min har noen svakheter. For eksempel ville det vært rart å ha med matpakke hjemmefra, for så å kjøpe salat til lunsj. Et potensielt større problem, er at selv om salat kan være godt, er som regel det varme alternativet mer fristende. Datasettet vil følgelig ha et vesentlig større antall eksempler på mat som er bedre enn salat, enn motsatt. For noen læringsalgoritmer kan dette resultere i modeller som tror all mat er bedre enn salat. Når det er sagt, finnes det måter å jobbe seg rundt problemet på. En enkel metode er å la maskinen lære fra retter som ikke er bedre enn salat hyppigere enn andre retter.

### Overtrening
Man skal være forsiktig med å lære for lenge av datasettet, da dette kan føre til overtrening. Ved _overtrening_ har modellen blitt så spesialisert på hvert eksempel i datasettet, at det minste avvik vil forvirre den. Dette vil man erfare når man anvender modellen senere. Man kan redusere problemet ved å trene fra et subsett av datasettet, og evaluere mot resten.

Vanlig praksis er å dele datasettet inn i tre deler: Trenings-, validerings- og testsett. Modellen lærer fra _treningssettet_. I tillegg ønsker man å holde av litt data for å evaluere modellen når læringen er avsluttet. _Testsettet_ dekker det behovet. Det er viktig at modellen ikke har lært direkte fra testsettet, ellers kan man ikke måle om modellen klarer å generalisere.

Å separere trening og testing er nok for å finne ut om modellen er overtrent, men hvordan kan vi forhindre at modellen blir overtrent i utgangspunktet? Løsningen er å ha et _valideringssett_ som modellen blir evaluert mot med jevne mellomrom under læring. Dersom ytelsen mot valideringssettet går ned, vet vi at modellen er i ferd med å bli overtrent. Man stopper da læringen og evaluerer en siste gang mot testsettet. Avhengig av algoritmen man bruker, finnes det ulike tiltak mot overtrening. Et valideringssett er en universal måte å overvåke og redusere problemet.

Undertrening er en lignende situasjon, hvor man stanser læringsprosessen for tidlig. Modellen har da ikke tilegnet seg nok erfaring til å generalisere godt nok. Dersom modellen aldri klarer å lære fra treningssettet, så er som regel enten datakvaliteten for lav, eller læringsalgoritmen misbrukt.

![Feilrate ved trening og validering](https://raw.githubusercontent.com/tork/blog/master/machine-learning/resources/early-stop.png)

Diagrammet over illustrerer problemet med overtrening. Vi ser at feilraten på treningssettet (blå kurve) går stabilt nedover etterhvert som læringsprosessen går sin gang. Når `Tid` er rundt `4`, ser vi at feilrate ved validering (oransje kurve) begynner å øke. I dette vendepunktet kan man avslutte læring og forhindre overtrening.

## Læringsalgoritmen
Generalisering fra data kan oppnås på flere måter, og forskjellige læringsalgoritmer vil skape ulike modeller. Ofte dikterer datasettet hvilken form for maskinlæring vi anvender. En vanlig måte å dele inn algoritmer er supervised, unsupervised og reinforcement learning.

### Supervised learning
For å løse lunsjproblemet vårt, ville det mest naturlige vært å brukt _supervised learning_. Det innebærer at man for hvert eksempel i datasettet legger ved hvilken klasse eksempelet egentlig tilhører (`bedre enn salat` eller `ikke bedre enn salat`). Læringsalgoritmen vil så bruke denne informasjonen til å se etter fellestrekk i eksempler som tilhører samme klasse, og generalisere ut fra disse. Trekkene kan være enkle, som at retter med smak av kjøtt som regel er bedre enn salat, eller mer komplekse som at retter med al dente pasta og rik tomatsaus er bedre enn salat. _Nevrale nettverk_, _support vector machines_ og _k nearest neighbors_ er eksempler på algoritmer som kan benytte supervised learning.

### Unsupervised learning
Motstykket til supervised learning, er _unsupervised learning_. Her har man et datasett av eksempler, men vet ikke hva disse skal lede til. Datamaskinen må selv finne interessante mønstre i datasettet, og dele eksemplene inn etter disse. Sagt på en annen måte, så vil datamaskinen samle sammen de eksemplene som ligner mest på hverandre. Vi kan altså la maskinen lære hvilken mat som er bedre enn salat, uten at vi trenger å fortelle den hvilken mat dette er. Utfordringen med å gjøre noe slikt, er at de kategoriene modellen lærer seg med all sannsynlighet representerer noe annet enn en sammenligning mot salat. Unsupervised learning lar læringsalgoritmen bestemme kategoriene, og uten videre analyse er alt vi kan si at modellen har gruppert datasettet på en eller annen måte som er naturlig for den. I 2012 slapp Google en unsupervised algoritme løs på YouTube, hvilket blant annet [resulterte i en kattedetektor](https://www.wired.com/2012/06/google-x-neural-network/).

Det virker kanskje håpløst å forholde seg til en slik "tilfeldig" inndeling, men i praksis er det sjelden man trenger å vite noe om kategoriene. Eksempelvis kan vi bruke unsupervised learning til å gruppere søkeresultater etter type innhold, eller foreslå lignende produkter i en nettbutikk; man trenger ikke vite hvilken kategori produktet man foreslår tilhører, så lenge likheten er stor. _Restricted Boltzmann machines_ og _k means clustering_ er eksempler på algoritmer som støtter unsupervised learning.

### Reinforcement learning
_Reinforcement learning_ handler om å lære i situasjoner hvor man ikke vet utfallet før man har utført en rekke aksjoner. For eksempel kan man lage en modell som velger råvarer, kutter opp og tilbereder disse. Læringsalgoritmen vet ikke om det den holder på med er bra eller dårlig, før retten er ferdig og noen har smakt på den. Dersom retten ble god, kan læringsalgoritmen tilpasse modellen slik at lignende råvarer og tilberedelse leder til noe godt. I motsatt fall lærer modellen å ikke gjøre det igjen.

Denne formen for maskinlæring er populær innen robotikk, hvor roboten lærer seg å utføre ulike fysiske aksjoner i en fysisk verden. I enkle tilfeller gjøres reinforcement learning med tabeller og noen matematiske formler, men i oppgaver med et større antall aksjoner og utfall benyttes ofte et nevralt nettverk som del i læringsprosessen.

### Klassifisering og regresjon
Til nå har vi vurdert oppgaven å gruppere eksempler inn i kategorier. I noen tilfeller ønsker man ikke en _diskret_ inndeling, men heller en _kontinuerlig_ verdi; noen retter kan være åpenbart bedre enn salat, mens andre er bare litt bedre. Vi kan lage en modell som viser i hvilken grad retten er bedre enn salat, representert på en skala fra `-1.0` til `1.0`. Når man ønsker en kontinuerlig verdi av modellen, kaller vi det _regresjon_.

For modellering av tidsserier benyttes ofte regresjon, fordi tidsserier gjerne består av reelle tall. En modell kan da læres til å gi neste verdi i serien, gitt de `n` foregående. Mange algoritmer kan lære regresjon, blant annet nevrale nettverk og support vector machines.

## Avslutningsvis
Vi har vært overordnet innom flere konsepter innen maskinlæring. En ting er å lese om det, men hvis du er interessert anbefaler jeg å prøve deg frem på egen hånd. Det er først når man prøver å anvende en konkret læringsalgoritme man blir tvunget til å ta stilling til ulike valg og utfordringer forbundet med maskinlæring. For eksempel kan k nearest neighbors være godt sted å starte for supervised learning, eller k means clustering for unsupervised learning. Dette er intuitive modeller som man gjerne kan implementere selv.

Neste gang du er i tvil om hva du skal ha til lunsj, kan du trøste deg med at det er en datamaskin et sted der ute som spiser salat hver dag og har en bunke gamle matpakker i sekken.
