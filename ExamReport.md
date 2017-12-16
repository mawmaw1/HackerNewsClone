# LSD Exam report

Rapporten er skrevet af:
- Kristian Oliver Nielsen
- Christopher Bob Borum
- Magnus Illum Gonoré Larsen
- Kristoffer Noga

## Indledning

Som en del af vores Large-System-Development-fag har vi udviklet en klon af HackerNews og implementeret så mange af de samme features som muligt. Vi har analyseret og studeret den rigtige version af HackerNews, forsøgt at danne os et overblik over systemet, redegjort for systemets entiteter, udformet use-cases og lavet sekvensdiagrammer. Ligeledes har vi på baggrund af vores analyse truffet valg om type af database, hvordan databasen skulle se ud og hvordan det data vi modtog fra simulatoren skulle gemmes i databasen. I den forbindelse har vi designet en lagdelt arkitektur og til sidst har vi givet systemet vores eget præg i form af en frontend med et unikt design.
Da systemet blev et MVP har vi siden hen forsøgt at vedligeholde, monitorere, logge og skalere systemet for at afspejle et system med tusindvis af afhængige brugere, hvor down-time og langsomme load-tider er katastrofale.
Opgaven har dermed bestået i at udvikle et system, hvilket vi har prøvet før, og at vedligeholde og skalere et live system, hvilket vi ikke har prøvet før.

## 1. Requirements, architecture, design and process

### 1.1. System requirements

#### Systemets formål:

At udvikle en Reddit-lignende platform, hvor brugere kan dele links eller billeder og generelt diskutere teknologi og software.

#### Scope of the system:

Brugerne opretter posts i form af links eller tekst-posts til debat eller generel deling af viden eller interessante emner. Sidens indhold bliver derefter up- eller downvoted af brugerne. Brugerne genererer karma-points når deres indhold bliver upvoted. Brugere af systemet kan kun downvote indhold, når de selv har indsamlet 500 karma-points.
For at bruge systemet opretter brugerne et login. Uden et login kan systemet kun bruges til at observere indhold, men man kan ikke interagere med siden på nogen måde.

#### Glossary:

- **Post**: Et stykke tekst, link eller billede, der lægger op til debat om et givent emne.
- **Upvote**: En bruger kan klikke en knap for at udtrykke, at han/hun synes godt om en kommentar eller post, og dermed giver karma til ejeren af kommentaren eller post’en.
- **Downvote**: En bruger kan klikke en knap for at udtrykke, at han/hun ikke synes godt om en kommentar eller post. (Det er kun muligt at downvote for brugere der selv har 500 karma-points)
- **Karma**: Point, der er opnået fra up- eller downvotes.
- **Comment**: En kommentar kan forekomme på en post for at diskutere et emne, eller som et svar på en anden allerede eksisterende kommentar. 

#### Functional requirements:
- Brugere skal logge ind for at få adgang til al funktionalitet på sitet.
- Når brugeren er logget ind kan de tilgå en oversigt over deres posts og kommentarer, og yderligere information af deres brug af systemet.
- Brugere kan tilgå en indbakke med beskeder fra andre brugere.
- Brugere kan se posts
- Brugere kan upvote og downvote (hvis de er berettiget) andre brugeres posts og kommentarer.

#### Nonfunctional requirements:

**Usability:**
Applikationen bør virke på de to seneste versioner af Chrome, Safari, Firefox og Edge. Siden bør skalere korrekt når det tilgås fra en telefon eller tablet.

**Reliability:**
Applikationen skal have en uptime på mindst 95%. Under maintenance og opdateringer bør indkommende posts blive lagt i en buffer og gemt så snart applikationen er oppe at køre igen.

**Performance:**
Når applikationen åbnes via en browser bør siden være renderet med synlige posts inden for 1,5 sekunder. Systemet skal være tilfredsstillende at bruge på forskelligt hardware. Gamle telefoner, tablet og computere bør være i stand til at bruge systemet uden for meget lag og lange load-tider.

**Supportability:**
Systemet skal være nemt at vedligeholde, servicere og teste.

#### Logical Data Model:

Systemets logical data model ser således ud.

![alt text](https://github.com/mawmaw1/HackerNewsClone/blob/master/doc/img/Logical%20Data%20Model.png)

Systemet er egentlig forholdsvis simpelt set ud fra denne model. Vi har forholdsvis få entiteter i systemet, og dette er også en af grundene til vi har valgt en NoSQL-database, da der er ret få relationer entiteterne imellem. Havde systemet været mere kompleks, eller havde business-logikken krævet flere entiteter havde en relationel database nok været foretrukket. Det er også værd at påpege at entiteterne har ret få attributter, og igen siger det noget om systemets ’relative’ simple opbygning.

#### Use-case model:

Use-case modellen fortæller noget om systemets aktører, og hvilke aktioner de hver især kan udføre. Således kan det ses på modellen, at systemet er forholdsvis afgrænset, hvis man besøger systemet som gæst. Der er dog alligevel et stort antal af brugere som vil besøge sitet som guest, og som ikke har nogen interesse i at interagere med systemet. Er man dog bruger kan det ses, at man har langt flere aktioner.

![alt text](https://github.com/mawmaw1/HackerNewsClone/blob/master/doc/img/Use%20Case%20Diagram.png)

#### Use cases

Ud fra vores use-case model har vi udarbejdet en række use cases. En use case uddyber en aktion skitseret i use-case modellen. Hver en cirkel i use-case modellen har derfor en tilhørende use case beskrivelse. Hver use case følger den samme mønster og har en række underpunkter, der skal være med til at forstå en use case.
Her ses use case for Login:

#### Login

- **Name**: Login
- **Scope**: System under design (SuD)
- **Level(Goal)**: To enable restricted content
- **Primary Actor**: The previously registered user
- **Precondition**: The user is on the site but not logged in
- **Main success scenario**: The user successfully logs in
- **Success Guarantees**: The user is logged in and can view restricted content
- **Extension**: NONE
- **Special Reqs.**: NONE

Grundet rapportens korte længde kan de resterende use cases ses [her](https://github.com/mawmaw1/HackerNewsClone).

#### Aktører:

**Bruger:**
En person der har valgt at registrere sig på siden. Brugeren driver indholdet af applikationen ved at indsende indhold og ved at up- eller downvote sidens indhold samt kommentere på posts eller andre kommentarer.

**Gæst:**
En person der besøger siden. En gæst ser eller læser sidens indhold, men interagerer på ingen måde med siden.

#### Sekvensdiagram:

Her ses et sekvensdiagram for en af de mange use cases. I det her tilfælde er det sekvensdiagrammet for use case’en View post.

![alt text](https://github.com/mawmaw1/HackerNewsClone/blob/master/doc/img/Sekvens%20Diagram.png)

Sekvensdiagrammet illustrerer det givne flow i applikationen når en bruger vælger at se en post. Det kan med andre ord ses, hvordan systemet går fra frontenden til backenden.

### 1.2 Development Proces

Under udviklingen af systemet har vi gjort brug af flere Scrum-processer blandet med en smule XP-praktikker.
Scrum er noget vi efterhånden har en del erfaring med, og det er specielt brugbart i udviklingen af systemer, hvor man er en del af et mindre team, og hvor krav og ændringer opstår ofte under processen. Da netop dette var de forhold vi befandt os under, var det oplagt at bruge Scrum.
Kravene var sådan set kendt på forhånd, og da vi ikke agerede med en product owner var vi i mindre grad udsat for kravændringer. Det var en lidt utraditionel måde at bruge Scrum på, da vi under dette forløb ikke havde en product owner som så systemet udvikle sig over tid. I stedet havde vi skrevet et requirement-dokument selv, og måtte arbejde ud fra dette. Vi havde alligevel flere gange undervejs ændringer i krav, eller tilføjelser til requirements i takt med at systemet blev større og mere komplekst. Vi har med andre ord selv ageret product owner under forløbet.
 
Vi prøvede så vidt muligt at arbejde i sprints. Det fungerede dog ikke altid, og sprints overlappede ofte hinanden, fordi vi netop ikke havde en product owner at lave weekly reviews med. Sprints hjalp os alligevel med at fokusere på specifikke dele af systemet, da vi under hvert sprint havde en række use cases vi prøvede at færdiggøre.
Da dette forløb som beskrevet ikke var sat sammen med en product owner betød det også, at flere af Scrums dele ikke var med i dette forløb. Således havde vi ikke retrospectives, daily scrum meetings, weekly reviews eller estimering af opgaver.
Alle disse aspekter skal helst finde sted, hvis et system skal udvikles efter Scrums’ principper. Der er heller ingen tvivl om, at vi havde arbejdet mere struktureret, hvis vi havde været bedre til at bruge alle Scrum-principperne.
 
Da Scrum er procesorienteret er XP et godt supplement, da det er produktorienteret. Scrum og XP komplimenterer derfor hinanden, og det ses derfor også tit brugt sammen.
XP-praktikkerne er også nogle vi har brugt mange gange efterhånden, så de sidder tæt under huden, og vi bruger dem derfor ofte uden at tænke over det. Fra XP-praktikkerne kan det nævnes at vi bl.a. har brugt: refactoring, continuous integration, pair programming, collective code ownership, coding standards og en smule testing (test driven development).
 
Et alternativ til den agile arbejdsproces og Scrum kunne være UP. I UP arbejder man med den iterative and incremental arbejdsproces. UP er i modsætningen til Scrum en arbejdsproces, hvor man ikke i lige så høj grad kan være agil og lave ændringer undervejs. UP kan ses som mellemledet mellem den agile arbejdsproces og vandfaldsmodellen. UP gør brug af fire faser kaldet inception, elaboration, construction og transition. Det betyder også, at man inden for hver af de fire faser arbejder iterativt, og dermed inden for hver fase kan nå at lave ændringer og træffe beslutninger. Typisk vil man dog have svært ved at ændre på de faser som er færdige og som allerede har været gennem iterationer.
Da UP oftes bruges til mellemstore eller store systemer gav det ikke mening at benytte denne arbejdsproces.

### 1.5. Software implementation
Vores produkt blev implementeret i forskellige iterationer, og vi gik efter det vigtigste hvilket var at have vores backend klar til at modtage posts, der kunne blive indsat i databasen. Allerede der løb vi ind i et problem der gik på at, de POST kald vi fik ikke havde en header der beskrev at data typen var JSON. Vi fik løst dette problem ved at lave et middleware i Express det skulle parse alt det data i HTTP requests, som JSON.

Efter en uges tid hvor vi har taget imod posts, opdagede vi at vores query til at hente de seneste posts var blevet markant langsommere, og kun blev mere langsom i takt med hvor mange posts vi havde. Efter noget troubleshooting fandt vi frem til at databasen nok skulle løbe igennem alle posts for at finde frem til et “parent” post der matchede, hvilket er et stort performance issue når der er flere end 1.000.000 posts. Vi fik løst dette problem ved at indeksere `post_parent` igennem mongoose, ved at sætte et index parameter.
```javascript
post_parent: { type: Number, index: true }
```
Det andet felt `hanesst_id` var allerede indexeret indekseret igennem mongoose, idet vi havde specificeret at dette felt skulle være unikt i denne kollektion.


På frontenden havde de valgte teknologier overordnet set en positiv effekt på udviklingen. Opsætning af Webpack var i første omgang lidt omstændig, men da vi fik styr på konfigureringen kørte det ret glat. Det var nemt at sætte en build process op i Jenkins, der transpilede og bundlede vores kildekode til produktions-klar filer. Derudover havde frontenden en overskuelig struktur og blev ikke for kompleks eller rodet. 

## 2. Maintenance and SLA status

### 2.1. Hand-over
Vores gruppe modtog et 14 sider langt hand-over dokument, som i detaljeret grad beskrev deres system. Dokumentet var både en beskrivelse af systemet som en helhed, men også i mere detaljeret grad en forklaring af de enkelte dele. I dokumentet var der henvisninger til Git-repositories, IP-adresser til del-komponenterne i deres system, samt information om deres Jenkins Build & Deploy opsætning.
Gruppen havde lavet et beskrivende diagram, som flot viste hvorledes hele deres ’Continuous Integration & Continuous Delivery’-opsætning fungerede, for både deres backend og frontend. Det gjorde det nemt at danne sig et overblik over, hvad der kom til at ske, når et nyt bygge-job blev eksekveret.
Da gruppen havde valgt, at vi skulle have adgang til deres Jenkins-server, havde de fint beskrevet deres forskellige bygge-job. Gruppens udviklere benyttede sig af webhooks til at starte nye byg, og derfor ville de gerne have at vi som operatører skulle holde øje med om byg fejlede, og i så fald melde tilbage. Derfor var det godt, at de havde dokumentation omkring deres forskellige byg med i deres hand-over.
Det var i dokumentet beskrevet, hvad vi som operatører havde adgang til, og hvad det var meningen vi skulle monitorere. Det var muligt at få et overblik over deres forskellige komponenters logs, via et bygge-job. Det kunne måske have været gjort nemmere, ved at have et dedikeret komponent til at vise logs i virkelig tid, fremfor at skulle starte et nyt job for at kunne se logs. Udover dette, var der i deres hand-over en masse viden omkring hvordan deres komponenter internt kommunikerede, hvilket også var med til at øge forståeligheden af deres komplette system.
Sammenlagt var det et hand-over i høj kvalitet, og de havde tydeligt klargjort hvad formålet for os som operatører var, og vi havde derfor nemt ved at sætte os ind i rollerne som operatører.

#### 2.2. Service-level agreement
I samarbejde med den gruppe vi skulle være operatører for, blev der udarbejdet en SLA som begge grupper var enige om.
I denne SLA var begge gruppers ansvar beskrevet, for både udviklerne og operatørerne. Den metric som var vigtigst for gruppe A som vi monitorerede, var at deres systems oppetid blev holdt over 95%. Såfremt denne metric ikke blev overholdt, skulle vi som operatører melde det til udviklerne, som efterfølgende ville sørge for at udbedre dette. Til at starte med havde vi talt med gruppen om at denne metric skulle holdes over 99%, da vi mente at nedetid burde holdes på et absolut minimum. Vi blev dog efterfølgende enige med gruppen om, at dette var for ambitiøst.
Dokumentet indeholdt herudover en oversigt over gruppens forskellige del-komponenter i deres system, samt de dertilhørende IP-adresser.
Her følger SLA mellem gruppe A og B:

![alt text](https://github.com/mawmaw1/HackerNewsClone/blob/master/doc/img/Screen%20Shot%202017-12-16%20at%2015.57.11.png)

### 2.3. Maintenance and reliability
Efter at have indgået en SLA med den gruppe vi skulle monitorere, sørgede vi for mindst en gang dagligt at kontrollere gruppens delkomponenter, for dermed at sikre at der ikke var opstået problemer. Vi kontrollerede deres visuelle monitoreringssystem Grafana for at sikre at oppetiden var over 95%, undersøgte om vi fik et svar på deres /alive backend REST-api sti, samt besøgte deres frontend for at finde ud af hvorvidt der skulle være problemer der.
Helt generelt har deres system kørt mere eller mindre problemfrit, og der har derfor ikke været meget at rapportere til gruppen.
Vi oplevede dog en dag at deres system kørte helt ekstremt langsomt, og det tog op til 20-25 sekunder at indlæse posts på deres frontend. Da problemet blev opdaget til en undervisningstime, gjorde vi gruppen opmærksomme på problemet med det samme. I den forbindelse oprettede vi ikke et GitHub issue, men i tilfælde af at vi havde opdaget det på et andet tidspunkt, ville vi have oprettet et GitHub issue tilsvarende nedenstående billede, som blot er et eksempel på hvordan det kunne have foregået.

![alt text](https://github.com/mawmaw1/HackerNewsClone/blob/master/doc/img/Screen%20Shot%202017-12-16%20at%2015.58.20.png)

I den forbindelse oplevede vi at udviklerne tog problemet seriøst, for da gruppen var blevet gjort opmærksomme på problemet, sørgede de for at løse det indenfor den tidsfrist på 24 timer, som var angivet i deres SLA. Da vi ikke oplevede at det aftalte metric blev overskredet, og da deres system udover det angivne problem har været helt pålideligt, har det været en fornøjelse at monitorere deres system.
 
 
## 3. Discussion
### 3.1. Technical discussion
Med henblik på teknisk design, valgte vi en kombination af bekendte og nye teknologier. Teknologier som Node og MongoDB, som vi på forhånd har erfaring med, var relativt nemme for os at sætte op, og skabte et udviklingsmiljø som vi kunne trives i. Teknologier som Jenkins og Webpack krævede lidt mere opsætning, men de gjorde også en mærkbar forskel med hensyn til hvor nemt det blev at deploye ny kode. Express og Vue (frameworks) hjalp med at sætte nogle faste rammer op omkring kode-strukturen, og abstraherede samtidig nogle af de mest gængse problemer væk, så vi kunne fokusere på det større billede. Docker strømlinede deployment, og gjorde at vi undgik en del problemer omkring opsætning af serveren. Overordnet set var vi meget tilfredse med den tekniske del, og har fået kendskab til en masse redskaber der hjælper med at løse mange af de problemstillinger man står overfor, når man har med et større system at gøre.
I teorien var det en god idé at monitorere hinanden, grupperne imellem, men rent praktisk faldt det en smule til jorden, da systemerne som sådan bare kørte uden større problemer. Derfor var der ikke så meget for os at lave som operatører, og i virkeligheden havde vi nok en større interesse i at monitorere og forbedre vores eget system, fremfor at monitorere andres systemer.


### 3.2. Group work reflection & Lessons learned
Inden vi gik i gang med projektet havde vi styr på udviklingen af systemer, da vi har prøvet det mange gange før. Vi manglede dog viden omkring, hvordan man håndterer store systemer, skalerer, monitorerer og generelt holder et stort system live størstedelen af tiden. Gennem projektet og de mange delafleveringer sidder vi tilbage med følelsen af at vi nu er klædt godt på til at udvikle og vedligeholde store systemer. Yderligere har det givet os et nyt perspektiv på softwareudvikling, da det viser sig, at man ikke bare kan kode et system og så aflevere det. Der findes en mindst lige så lang periode efter systemet er færdigt, hvor det skal vedligeholdes og monitoreres, og dette har sine helt egne problemstillinger og udfordringer som vi ikke før har mødt. Vi har nu erfaring med ting som Prometheus, og Grafana som vi tror på har en kæmpe værdi for os som udviklere, og erfaring med Kibana så vi langt nemmere og effektivt kan finde eventuelle fejl i en kæmpe mængde af log-entries. 

Yderligere har vi været ekstremt tilfredse med Jenkins og generelt CI/CD, da vi tidligere manuelt har deployet. En af fordelene ved at kunne deploye så nemt, er at man kan deploye oftere, og på den måde undgår massive deploys, der laver dramatiske ændringer i applikationen. Små releases er lettere at håndtere, og resulterer i at der er færre bugs der finder vej til produktion. 
De forskellige roller (project manager, architect, devops etc.), har vi ikke gjort brug af, da vi ikke følte det gjorde så meget for vores gruppe, idet vi har gennemgået de fleste ting som gruppe og ikke ladt den individuelle person stå for en kæmpe del af projektet.

## Konklusion:
Vi føler at projektet, og udviklingsprocessen i sin helhed, har udvidet vores horisont omkring store systemer. Vi har fået en masse viden omkring redskaber og deres anvendelse, samt designvalg, hvilket har gjort os bedre rustet til at udvikle og vedligeholde et system der vokser eksponentielt. Ligesom min penis.




