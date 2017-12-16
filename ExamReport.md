# LSD Exam report

Rapporten er skrevet af:
- Kristian Oliver Nielsen
- Christopher Bob Borum
- Magnus Illum Gonoré Larsen
- Kristoffer 'Gruppens største penis' Noga

## Indledning

Som en del af vores Large-System-Development-fag har vi udviklet en klon af HackerNews og implementeret så mange af de samme features som muligt. Vi har analyseret og studeret den rigtige version af HackerNews, forsøgt at danne os et overblik over systemet, redegjort for systemets entiteter, udformet use-cases og lavet sekvensdiagrammer. Ligeledes har vi på baggrund af vores analyse truffet valg om type af database, hvordan databasen skulle se ud og hvordan det data vi modtog fra simulatoren skulle gemmes i databasen. I den forbindelse har vi designet en lagdelt arkitektur og til sidst har vi givet systemet vores eget præg i form af en frontend med et unikt design.
Da systemet blev et MVP har vi siden hen forsøgt at vedligeholde, monitorere, logge og skalere systemet for at afspejle et system med tusindvis af afhængige brugere, hvor down-time og langsomme load-tider er katastrofale.
Opgaven har dermed bestået i at udvikle et system, hvilket vi har prøvet før, og at vedligeholde og skalere et live system, hvilket vi ikke har prøvet før.

## 1. Requirements, architecture, design and process

### 1.1. System requirements

#### Systemets formål 

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

**Functional requirements:**
- Brugere skal logge ind for at få adgang til al funktionalitet på sitet.
- Når brugeren er logget ind kan de tilgå en oversigt over deres posts og kommentarer, og yderligere information af deres brug af systemet.
- Brugere kan tilgå en indbakke med beskeder fra andre brugere.
- Brugere kan se posts
- Brugere kan upvote og downvote (hvis de er berettiget) andre brugeres posts og kommentarer.


