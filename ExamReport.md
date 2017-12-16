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





