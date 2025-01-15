**Mașini paralele și modele de programare**

#### **Definiție generală**
Un calculator paralel constă din mai multe procesoare care comunică și cooperează pentru a rezolva probleme complexe. Modelele de programare determină cum interacționează aceste componente pentru a partaja resurse și date.

---

#### **Modele de programare pentru calculatoarele paralele**

1. **Modelul memoriei partajate**
   - Programele sunt compuse din **fire de execuție** care partajează variabile și comunică prin citire/scriere.
   - **Sincronizare**: Prin indicatori (*flags*), zavoare (*locks*), semafoare.
   - **Exemple de arhitecturi**:
     - **SMP (Symmetric Multiprocessors)**: Procesoare multiple care împart aceeași memorie fizică.
     - **Variante**:
       - **Memorie partajată distribuită**: Memoria este fizic distribuită, dar logic partajată (ex.: SGI Origin).
       - **Sisteme cu memorie locală**: Memoria cache înlocuită cu memorie locală (ex.: Cray T3E).

2. **Modelul transferului de mesaje**
   - Programele sunt o colecție de **procese** independente, fiecare având memorie proprie.
   - Comunicarea între procese se face prin mesaje explicite (*send*, *receive*).
   - **Exemple de arhitecturi**:
     - Multicalculatoare cu memorie distribuită (ex.: IBM SP2, Cray T3E).
   - **Instrumente de lucru**: Biblioteci standard precum **MPI** și **PVM**.

3. **Modelul paralelismului datelor**
   - Un **singur fir de control** aplică operații paralele asupra unui set mare de date.
   - **Exemple de arhitecturi**:
     - **Sisteme SIMD (Single Instruction Multiple Data)**: Procesoare multiple execută simultan aceeași instrucțiune pe date diferite (ex.: CM2, MASPAR).
     - Variante: Mașini vectoriale (un procesor cu mai multe unități funcționale).

4. **Modelul cluster de SMP-uri (CLUMP - Cluster of SMPs)**
   - **Definiție**: Este o rețea de SMP-uri, fiecare fiind un sistem cu memorie partajată.
   - **Caracteristici**:
     - Comunicarea între SMP-uri are loc prin **transfer de mesaje**, similar cu modelul multicalculatoarelor.
     - Combina avantajele memoriei partajate locale (în interiorul unui SMP) cu scalabilitatea transferului de mesaje între noduri.
   - **Exemple de arhitecturi**:
     - Millennium, IBM SPx, ASCI Red.
   - **Programare**:
     - Modele hibride: Memorie partajată în interiorul SMP-urilor și transfer de mesaje între SMP-uri.
   - **Utilizare**:
     - Frecvent în centre de date și supercomputere datorită capacității de a scala la niveluri mari.

---
### **Modele teoretice de calculatoare paralele**

#### **1. Modelul RAM (Random Access Machine)**
- **Definiție**: Modelul clasic pentru calcul secvențial.
- **Caracteristici**:
  - Presupune un singur procesor cu acces rapid la memorie.
  - Operațiile au un cost uniform, indiferent de locația memoriei.
- **Utilitate**:
  - Este punctul de plecare pentru modelele paralele.
  - Se folosește pentru evaluarea algoritmilor secvențiali.

---

#### **2. Modelul PRAM (Parallel Random Access Machine)**
- **Definiție**: Extinderea modelului RAM pentru calcul paralel.
- **Caracteristici**:
  - Multiple procesoare care partajează o memorie comună.
  - Accesul la memoria partajată este sincronizat.
  - **Timp de execuție**: Se consideră că toate operațiile de citire și scriere au un cost egal.
- **Tipuri de acces la memorie**:
  - **ER (Exclusive Read)**: Doar un procesor poate citi dintr-o locație de memorie într-un ciclu.
  - **EW (Exclusive Write)**: Doar un procesor poate scrie într-o locație de memorie într-un ciclu.
  - **CR (Concurrent Read)**: Mai multe procesoare pot citi simultan din aceeași locație de memorie.
  - **CW (Concurrent Write)**: Mai multe procesoare pot scrie simultan în aceeași locație de memorie.

---

#### **3. Variante ale modelului PRAM**
- **EREW-PRAM (Exclusive Read Exclusive Write)**:
  - Cel mai restrictiv model.
  - Fără acces simultan la citire sau scriere.
- **CREW-PRAM (Concurrent Read Exclusive Write)**:
  - Permite citirea simultană, dar nu și scrierea simultană.
  - Conflictele de scriere sunt evitate prin mecanisme de excludere mutuală.
- **ERCW-PRAM (Exclusive Read Concurrent Write)**:
  - Permite scrierea concurentă, dar citirea este exclusivă.
- **CRCW-PRAM (Concurrent Read Concurrent Write)**:
  - Cel mai puternic model.
  - Scrierea simultană este permisă, dar trebuie să fie gestionată printr-o regulă:
    - **COMMON-PRAM**: Toate procesoarele scriu aceeași valoare.
    - **ARBITRARY-PRAM**: Una dintre valorile scrise este aleasă arbitrar.
    - **MINIMUM-PRAM**: Se memorează valoarea minimă scrisă.
    - **PRIORITY-PRAM**: Se aplică o funcție asociativă, cum ar fi suma, pentru toate valorile scrise.

---
### **Tipuri de calculatoare paralele**

#### **1. Calculatoare în bandă de asamblare (Pipeline Processors)**
- **Definiție**: Procesoarele sunt organizate în etape secvențiale (pipeline), unde fiecare etapă execută o parte dintr-o instrucțiune.
- **Funcționare**:
  - Execuția instrucțiunilor este suprapusă în timp.
  - Un pipeline eficient reduce timpul de execuție pentru fiecare instrucțiune.
- **Exemple**:
  - Procesoare cu pipeline-uri simple sau multiple (de ex.: RISC).
- **Utilizare**:
  - În aplicații care necesită un flux constant de date, cum ar fi procesarea de semnale.

---

#### **2. Calculatoare de masive (Array Processors)**
- **Definiție**: Utilizează mai multe procesoare pentru a efectua operații identice pe seturi diferite de date.
- **Caracteristici**:
  - Procesoarele sunt organizate într-o matrice.
  - Execută operații SIMD (Single Instruction Multiple Data).
- **Exemple**:
  - Cray-1, Maspar.
- **Aplicații**:
  - Prelucrarea imaginilor, grafica computerizată, simulări științifice.

---

#### **3. Sisteme multiprocesor**
- **Definiție**: Sisteme cu mai multe procesoare care partajează o memorie comună.
- **Tipuri**:
  - **UMA (Uniform Memory Access)**:
    - Timpul de acces la memoria partajată este uniform pentru toate procesoarele.
    - Exemple: Sistemele cu magistrale sau comutatoare.
  - **NUMA (Non-Uniform Memory Access)**:
    - Timpul de acces la memorie depinde de localizarea fizică a memoriei.
    - Exemple:
      - **CC-NUMA**: Cache-Coherent NUMA.
      - **NC-NUMA**: Non-Coherent NUMA.
  - **COMA (Cache-Only Memory Access)**:
    - Memoria partajată este distribuită între procesoare sub formă de cache.

---

#### **4. Calculatoare în flux de date (Dataflow Computers)**
- **Definiție**: Execuția instrucțiunilor este condusă de fluxul de date, nu de secvența programului.
- **Caracteristici**:
  - Instrucțiunile sunt declanșate numai când datele necesare sunt disponibile.
- **Exemple**:
  - Manchester Dataflow Computer.
- **Aplicații**:
  - Probleme cu flux de date mare și execuție neregulată.

---

#### **5. CMP (Chip Multiprocessors)**
- **Definiție**: Integrează mai multe procesoare (core-uri) pe un singur cip.
- **Caracteristici**:
  - Fiecare nucleu rulează independent un fir de execuție.
  - Paralelism la nivel de fir (TLP - Thread-Level Parallelism).
- **Exemple**:
  - Intel Duo, AMD Opteron dual-core, Niagara SPARC.
- **Aplicații**:
  - Servere, calcul personal, centre de date.

---

#### **6. Sisteme sistolice VLSI**
- **Definiție**: Sistemele procesează fluxuri de date utilizând rețele de procesoare interconectate.
- **Caracteristici**:
  - Datele curg prin rețea și sunt procesate la fiecare etapă.
- **Exemple**:
  - Sisteme pentru calcularea convoluțiilor (ex.: rețele liniare pentru filtrarea digitală).
- **Aplicații**:
  - Procesarea semnalelor și algoritmi cu volum mare de date.

---
Iată un rezumat detaliat despre **performanțele calculatoarelor paralele**, incluzând **performanța medie aritmetică/geometrică**, **rata de execuție medie armonică** și **paralelismul mediu**, conform informațiilor din curs.

---

### **Performanțe ale calculatoarelor paralele**

#### **1. Gradul de paralelism (DOP - Degree of Parallelism)**
- **Definiție**: Numărul de procesoare utilizate simultan la un moment dat.
- **Profilul paralelismului**:
  - Grafic care arată DOP în funcție de timp.
  - Determină eficiența utilizării procesoarelor și zonele în care acestea sunt subutilizate.

---

#### **2. Paralelismul mediu (Average Parallelism)**
- **Definiție**: Gradul mediu de paralelism al unui algoritm.
- **Formula**:
  \[
  A = \frac{\int_{t_1}^{t_2} DOP(t) \, dt}{t_2 - t_1}
  \]
  - Pentru un caz discret:
    \[
    A = \frac{\sum_{i=1}^n DOP_i \cdot t_i}{\sum_{i=1}^n t_i}
    \]
  unde:
  - \( t_i \): Intervalul de timp în care gradul de paralelism este constant (\( DOP_i \)).
- **Exemplu**:
  Dacă un algoritm are \( DOP = 2 \) timp de 5 secunde, \( DOP = 3 \) timp de 3 secunde, și \( DOP = 4 \) timp de 2 secunde:
  \[
  A = \frac{(2 \cdot 5) + (3 \cdot 3) + (4 \cdot 2)}{5 + 3 + 2} = \frac{31}{10} = 3.1
  \]

---

#### **3. Factorul câștig de viteză (Speedup - \( S \))**
- **Definiție**: Câștigul de viteză obținut prin utilizarea unui sistem paralel comparativ cu unul secvențial.
- **Formula**:
  \[
  S = \frac{T(1)}{T(n)}
  \]
- **Limitări**:
  - Performanța ideală (\( S = n \)) este rar atinsă din cauza porțiunilor secvențiale și a costurilor de sincronizare (Legea lui Amdahl).

---

#### **4. Eficiența (Efficiency - \( E \))**
- **Definiție**: Măsura utilizării eficiente a resurselor.
- **Formula**:
  \[
  E = \frac{S}{n}
  \]
- **Interpretare**:
  - Dacă \( E = 1 \), utilizarea resurselor este maximă (ideal).
  - \( E \) scade odată cu creșterea numărului de procesoare dacă suprafața secvențială rămâne constantă.

---

#### **5. Rata de execuție medie**
##### **a. Performanța medie aritmetică**
- **Formula**:
  \[
  R_a = \frac{1}{n} \sum_{i=1}^n R_i
  \]
  unde:
  - \( R_i \): Rata de execuție pentru fiecare program \( i \).
  - \( n \): Numărul total de programe.
- **Ponderi**: Dacă fiecare program are o pondere \( f_i \):
  \[
  R_a = \sum_{i=1}^n f_i \cdot R_i
  \]

##### **b. Performanța medie geometrică**
- **Formula**:
  \[
  R_g = \left( \prod_{i=1}^n R_i \right)^{1/n}
  \]
- **Ponderi**: Dacă fiecare program are o pondere \( f_i \):
  \[
  R_g = \prod_{i=1}^n R_i^{f_i}
  \]

##### **c. Rata de execuție medie armonică**
- **Formula**:
  \[
  R_h = \frac{n}{\sum_{i=1}^n \frac{1}{R_i}}
  \]
- **Ponderi**: Dacă ponderile sunt \( f_i \):
  \[
  R_h = \frac{1}{\sum_{i=1}^n \frac{f_i}{R_i}}
  \]
- **Interpretare**:
  - Se folosește când timpul de execuție este critic.
  - Inversul timpului mediu.

---

#### **6. Legea lui Amdahl**
- **Definiție**: Câștigul de viteză este limitat de porțiunile secvențiale ale programului.
- **Formula**:
  \[
  S = \frac{1}{(1 - f) + \frac{f}{n}}
  \]
  unde:
  - \( f \): Fracția paralelizabilă a programului.
  - \( 1-f \): Fracția secvențială.
  - \( n \): Numărul de procesoare.
- **Consecințe**:
  - Creșterea \( n \) are un efect diminuat dacă \( f \) este mic.
  - Limita superioară pentru \( S \) este \( \frac{1}{1-f} \).

---

#### **7. Limitările calculului paralel**
1. **Timpul de sincronizare (\( t_s \))**:
   - Costul coordonării între procese.
2. **Overhead-ul (\( t_o \))**:
   - Timp suplimentar pentru administrarea taskurilor paralele.
3. **Granularitatea (\( t \))**:
   - Raportul dintre timpul de execuție al unei sarcini și timpul de sincronizare.
4. **Numărul de taskuri vs. numărul de procesoare (\( N/n \))**:
   - Performanța depinde de raportul dintre numărul de taskuri și numărul de procesoare disponibile.

---
### **Pașii pentru crearea unui program paralel**

Crearea unui program paralel implică transformarea unui algoritm secvențial într-unul care poate fi executat simultan pe mai multe procesoare, cu scopul de a reduce timpul de execuție. Procesul se desfășoară în mai mulți pași esențiali:

---

#### **1. Descompunerea**
- **Definiție**: Spargerea algoritmului în **taskuri independente** care pot fi executate simultan.
- **Tipuri de descompunere**:
  - **Descompunere pe domeniu de date**:
    - Datele sunt împărțite între procese.
    - Exemplu: Împărțirea unei matrice în submatrici pentru procesare paralelă.
  - **Descompunere pe funcționalitate**:
    - Operațiile diferite ale algoritmului sunt atribuite diferitelor procese.
    - Exemplu: Un proces calculează suma, altul produsul.
- **Obiectiv**: Maximizarea sarcinilor independente pentru a crește paralelismul.

---

#### **2. Asignarea**
- **Definiție**: Distribuirea taskurilor către procese sau fire de execuție.
- **Tipuri de asignare**:
  - **Statică**:
    - Taskurile sunt alocate proceselor la începutul execuției.
    - Potrivit pentru aplicații cu sarcini de lucru previzibile.
  - **Dinamică**:
    - Taskurile sunt alocate proceselor în timpul execuției, pe măsură ce resursele devin disponibile.
    - Utilă pentru aplicații cu sarcini de lucru neregulate.
- **Obiectiv**: Echilibrarea încărcării între procese pentru a evita situațiile de subutilizare.

---

#### **3. Orchestrarea**
- **Definiție**: Controlarea comunicării, accesului la date și sincronizării între procese.
- **Elemente cheie**:
  - **Accesul la date**:
    - Memorie partajată: Procesele accesează variabile comune.
    - Transfer de mesaje: Datele sunt transmise între procese prin mesaje.
  - **Comunicarea între procese**:
    - Prin mesaje (ex.: *send*, *receive*).
    - Prin citirea și scrierea datelor partajate.
  - **Sincronizarea**:
    - Evitarea accesului concurent la resurse prin utilizarea de **zavoare (locks)**, **senzori (flags)** sau **sincroane**.
  - **Planificarea taskurilor**:
    - Determinarea ordinii în care taskurile sunt executate.
- **Obiectiv**: Reducerea costurilor de comunicare și sincronizare.

---

#### **4. Maparea**
- **Definiție**: Atribuirea proceselor la procesoarele fizice din sistemul hardware.
- **Tipuri de mapare**:
  - **Statică**:
    - Procesoarele sunt împărțite în subseturi, iar fiecare subset este dedicat unui proces.
    - Exemple: Algoritmi de tip *domain decomposition*.
  - **Dinamică**:
    - Sistemul de operare alocă dinamic procesele la procesoare în timpul execuției.
    - Exemple: Algoritmi de tip *work stealing*.
- **Obiectiv**: Maximizarea utilizării procesoarelor și reducerea timpului total de execuție.

---
