Abstract

A C implementation of Huffman coding for satellite communication networks. 
This project constructs optimal binary trees for efficient data encoding/decoding,
converts binary trees to multi-way trees for hierarchical routing, and computes
distances between nodes.

Implementation:
# Structuri de date utilizate:
Am ales structuri cu pointeri pentru a evita copieri inutile
si pentru a lucra eficient.

Astfel, structurile de date folosite sunt:
- satelit - nod in arbore binar, cu frecventa, nume si doi copii
- MinHeap - implementata pe un vector de pointeri la sateliti,
           permite accesarea rapida de elemente si extragerea
           eficenta a elementului minim
- Coada - coada statica cu capacitate fixa
- multicai - nod cu orice numar de copii de tip satelit


# Functii de initializare si utilitare
- conversie() - converteste un numar salvat in forma de char in int
- prelucrareString() - elimina caracterul '\n' de la finalul unui sir
- citireSateliti() - citeste datele despre sateliti si le salveaza in *sateliti[]


# Functii de eliberare memorie
- freeTree() - elibereaza arborele binar prin parcurgere post-ordine
- freeMulticai() - elibereaza arborele multicai recursiv


# 1
- concatenare() - intoarce un nou sir creat prin concatenarea a altor doua
- newNode() - creaza nod frunza pentru arbore

Pentru Min-Heap:
- initializareHeap()- initializeaza un Min-Heap gol
- cmpSat() - compara doi sateliti dupa frecventa, apoi lexical dupa nume
- siftUp() - restaureaza proprietatea de Min-Heap prin urcarea nodului cu indexul k
- siftDown() - restaureaza proprietatea de Min-Heap prin coborarea nodului cu indexul k
- inserareHeap() - adauga un nod in Min-Heap
- extrageMin() - scoate nodul cu frecventa minima (radacina) si rearanjeaza structura

- creareArbore() - construieste un arbore Huffman dintr-un vector de sateliti cu
                ajutorul Min-Heap-ului

Functiile tipice pentru cozi:
-  creareCoada(), eVida(), enqueue(), dequeue()

- parcurgerePeNivel() - afiseaza arborele binar nivel cu nivel


# 2
- decodificare() - parcurge arborele binar pe baza codului dat si scrie
                  numele satelitului corespunzator


# 3
- codificare() - cauta recursiv codul pentru nodul cu numele dat si il stocheaza in 'cod'


# 4
Am implementat prin doua moduri, al doilea bazandu-se pe observatia
prefixului comun al codului dar fiind mai ineficient pentru ca presupune 
doua parcurgeri ale arborelui si compararea prefixului, primul fiind un
mod clasic de implementare care parcurge arborele o singura data.

- cautareParinte() - metoda recursiva directa
- cautareParinte2() - pe baza codurilor binare: calculeaza cod1 si cod2,
                     gaseste prefix comun, coboara de la root


# 5
Am convertit arborele binar intr-un arbore multicai folosind conversieMulticai(),
apoi pentru fiecare legatura citita din fisier am gasit nodul parinte (de legatura)
cu cautareMulticai() si am adaugat noii copii cu addCopil(), iar in final am folosit
distanta() pentru a calcula numarul de muchii dintre doua noduri.

- newMulticai() - creeaza un nod pentru arborele multicai
- addCopil() - adauga un copil la un nod multicai, extinzand vectorul daca e nevoie
- conversieMulticai() - transforma un arbore binar de sateliti intr-un arbore multicai
                       cu aceeasi structura si valori
- cautareMulticai() - cauta un nod dupa nume intr-un arbore multicai
- cautareParinteMulticai() - cauta cel mai apropiat parinte comun prin parcurgere
                            recursiva intr-un arbore multicai
- determinareAdancime() - calculeaza recursiv adancimea unui nod
- distanta() - calculeaza numarul de muchii dintre doua noduri prin formula:
              adancime(nod1) + adancime(nod2) – 2 * adancime(parinte)


# Main
-  Se deschid fisierele de intrare si iesire cu
  fopen(argv[2], "r") si fopen(argv[3], "w")  
-  Se citeste numarul de sateliti din prima linie
  si se converteste cu conversie()
-  Se aloca vector de pointeri la sateliti: 
  satelit **sateliti = malloc(sizeof(satelit*) * nrSat)  
-  Se apeleaza citireSateliti() pentru a popula vectorul,
  apoi creareArbore() pentru a construi arborele
-  In functie de argv[1] se executa comanda corespunzatoare:  
   -c1: parcurgerePeNivel()
   -c2: citire coduri + decodificare()  
   -c3: citire nume + codificare() 
   -c4: citire nume + cautareParinte()  
   -c5: conversieMulticai() + citire legaturi + distanta()  
-  Dupa c1, c2, c3, c4 se elibereaza arborele binar cu freeTree()  
-  Dupa c5 se elibereaza arborele multicai cu freeMulticai()
-  Se elibereaza vectorul sateliti si se inchid fisierele cu fclose()  
