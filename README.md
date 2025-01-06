# TSP_tests

# Tipuri de Grafuri TSP Testate

În cadrul proiectului, am ales să generăm și să testăm **cinci** tipuri de grafuri pentru problema TSP. Fiecare tip de graf prezintă caracteristici și dificultăți diferite, care ajută la analizarea și compararea performanței dintre:

- **Held-Karp** – algoritm exact bazat pe programare dinamică (oferă soluția optimă, dar are complexitate exponențială),
- **Algoritm Euristic** (ex. multifragment, nearest neighbor, etc.) – oferă soluții aproximative rapid, dar poate fi păcălit de anumite structuri de cost.

---

## 1. Graf Complet Random

### Definiție
- Un **graf complet** în care **toate muchiile există** și **costurile** dintre două noduri sunt alese **aleator** dintr-un anumit interval (ex. \([1, 100]\)).  
- De obicei, se iau costurile simetrice (TSP simetric), dar se poate face și asimetric.

### Motivație
- Este un **caz generic**, lipsit de structură specială.  
- Ajută la testarea comportamentului de **bază** al algoritmilor, fără influențe geometrice sau pattern-uri de cost.

### Influența asupra analizei (Held-Karp vs. Euristic)
- **Held-Karp**: 
  - Găsește **soluția optimă** cu un cost de execuție exponențial.  
- **Euristic**: 
  - Poate obține soluții destul de bune în medie, dar poate eșua atunci când există costuri extreme (foarte mici sau foarte mari) care apar întâmplător.  
  - Ne permite să vedem *cât de mult* se abate euristica de la optim în condiții complet aleatorii.

---

## 2. Graf Euclidian

### Definiție
- Nodurile sunt **puncte în plan**, cu coordonate \((x_i, y_i)\) alese aleator, iar **costul** dintre două noduri este **distanța Euclidiană rotunjită**.

### Motivație
- Reflectă un **scenariu clasic** de TSP (livrări, trasee, rute) în care distanțele respectă geometria plană.  
- Se apropie cel mai mult de **aplicațiile reale** (de ex. logistică).

### Influența asupra analizei (Held-Karp vs. Euristic)
- **Held-Karp**: 
  - Va găsi exact **circuitul minim** (cel mai scurt drum care vizitează toate punctele).  
- **Euristic**: 
  - De multe ori, se descurcă **mai bine** decât în grafurile complet random, pentru că în cazul euclidian, nodurile apropiate în plan au și costuri mici.  
  - Rămâne totuși posibil să aibă un *gap* față de optim dacă există poziționări ciudate ale punctelor.

---

## 3. Graf Stea

### Definiție
- Există un **nod central** (de regulă nodul 0) cu **costuri mici** (1, 2 etc.) către toate celelalte noduri, iar costurile dintre **nodurile periferice** (1, 2, 3, ...) sunt **mari**.

### Motivație
- Modelează un **caz extrem**: un singur nod este foarte avantajos, iar restul conexiunilor sunt dezavantajoase.  
- Testează cât de mult se pot „păcăli” algoritmii euristici de existența unor *muchiile foarte ieftine* față de un anumit nod.

### Influența asupra analizei (Held-Karp vs. Euristic)
- **Held-Karp**: 
  - Găsește circuitul optim, **nu se lasă amăgit** de costurile mici către nodul central (nu îl include de mai multe ori).  
- **Euristic**: 
  - Tinde să selecteze excesiv muchiile către nodul central, riscând să formeze *sub-cicluri* înainte de a lega toate nodurile.  
  - Se poate obține un **circuit mult mai costisitor** față de optim dacă nu se gestionează atent condiția de a nu repeta nodul central.

---

## 4. Graf Bipartit

### Definiție
- Nodurile sunt împărțite în **două seturi**, \(A\) și \(B\).  
  - **Costuri mici** între un nod din \(A\) și un nod din \(B\).  
  - **Costuri mari** între nodurile din același set.

### Motivație
- Este o structură simplă, dar care **creează un pattern** de costuri (mic vs. mare) și testează cum se leagă seturile.  
- Poate duce la situații în care euristicile formează *sub-cicluri A-B-A-B* și nu includ toți membrii din seturi.

### Influența asupra analizei (Held-Karp vs. Euristic)
- **Held-Karp**: 
  - Determină optim ordinea de vizitare a nodurilor din A și B fără a crea sub-cicluri izolate.  
- **Euristic**: 
  - Poate fi tentat să ia **mereu muchii ieftine** (A-B), formând sub-cicluri și forțând ulterior **alegeri scumpe** pentru a închide toate nodurile într-un singur ciclu.  
  - Ajută la evidențierea potențialelor mari diferențe față de soluția optimă.

---

## 5. Graf cu Clustere

### Definiție
- Nodurile sunt grupate în **2 sau mai multe clustere**.  
  - **Costuri mici** (1...20) în interiorul aceluiași cluster.  
  - **Costuri mari** (50...100) între clustere.

### Motivație
- Reprezintă un **scenariu frecvent** în rețele de transport, unde „regiunile” sunt relativ apropiate în interior, dar separate prin costuri mari.  
- Ideal pentru a vedea dacă algoritmii pot evita să formeze *cicluri locale* într-un cluster înainte de a se conecta cu altele.

### Influența asupra analizei (Held-Karp vs. Euristic)
- **Held-Karp**: 
  - Va stabili cu precizie **când** și **cum** să iasă dintr-un cluster către altul, obținând circuitul optim.  
- **Euristic**: 
  - Tinde să selecteze **muchiile ieftine** din interiorul clusterelor, formând repede sub-cicluri locale.  
  - Apoi, conexiunile inter-cluster devin obligatorii și foarte costisitoare.  
  - Diferența dintre soluția euristică și cea exactă poate fi **substanțială** pe aceste instanțe.