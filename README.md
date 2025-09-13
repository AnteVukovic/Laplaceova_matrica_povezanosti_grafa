# Laplaceova matrica povezanosti grafa

Cilj ovog projekta je definirati i analizirati Laplaceovu matricu povezanosti grafa, proučiti njezina svojstva te ispitati povezanost s Brouwerovom pretpostavkom o spektru Laplaceove matrice grafa.

## Biblioteke

* __NetworkX__ - za kreiranje grafova i upravljanje njima
* __NumPy__ - za rad s matricama i algebarske operacije
* __SciPy__ - za algebarske operacije

## Pokretanje

Nakon što se otvori datoteka u kojoj se nalazi .ipynb dokument u VisualStudioCode-u, potrebno je odabrati kernel ili stvoriti novi klikom na gumb Select kernel u gornjem desnom kutu:

![kernel](./images/kernel.png)

te Python enviroments > Create Python enviroment > Venv.

Nakon toga unutar dokumenta treba dodati novu ćeliju koda s sljedećom naredbom:

```python
%pip install networkx numpy scipy
```

Nakon pokretanja te ćelije klikom na gumb Execute cell može se redom pokretati ostale ćelije.

## Funkcije

Prvo je potrebno definirati funkciju koja računa Laplaceovu matricu povezanosti za dani graf

```python
def LapConMatrix(G: nx.Graph):
    nodes = list(G.nodes())
    n = len(nodes)
    P = np.zeros((n, n), dtype=int)
    for i in range(n):
        for j in range(i+1, n):
            u, v = nodes[i], nodes[j]
            num_paths = len(list(nx.node_disjoint_paths(G, u, v)))
            P[i, j] = num_paths
            P[j, i] = num_paths
    sums = np.sum(P, axis=1)
    D = np.diag(sums)
    LC = D - P
    return LC, P
```

Na koordinate (i, j) i (j, i) matrice P postavlja se broj vršno disjunktnih puteva između i-tog i j-og vrha grafa. 

Klasična je Brouwerova hipoteza za Laplaceovu matricu zadana kao

Slika

gdje je m broj bridova grafa. Pri modifikaciji za Laplaceovu matricu povezanosti, m će predstavljati ukupan broj vršno disjunktnih puteva u grafu, a binomni će se koeficijent promijenti u t+3 povrh 4.

Za provjeru Brouwerowe hipoteze koristi se funkcija

```python
def sve(G: nx.Graph):
    LC, P = LapConMatrix(G)
    evals, evecs = eigh(LC)

    evl = np.round(evals[::-1], 4)
    evc = np.round(evecs, 4)
    evl.sort()
    dict={}
    for eval in evl:
        if eval in dict:
            dict[eval]+=1
        else:
            dict[eval]=1
    return(brouwer(evl, P))
```

Ta funkcija koristi pomoćnu funkciju brouwer definiranu kao

```python
def brouwer(evals, P):
    sumsv = 0
    m = np.sum(np.triu(P, 1))
    p_max = np.max(P)
    for i in range(len(evals)):
        sumsv += evals[i]
        if sumsv > (m + comb(i+4, 4)):
            return False
    return True
```
## Testiranje

Na primjeru potpunog multipartitnog grafa sa skupovima veličina 1, 2 i 3 provjerava se broj vršno disjunktnih puteva između nultog i petog vrha. To ćemo dobiti kao

```python
len(list(nx.node_disjoint_paths(G, 0, 5)))
```

Slika

Dobija se da između nultog i petog vrha danoga grafa postoje 3 vršno disjunktna puta.

Slika

Nakon toga može se provjeriti valjanost hipoteze za određene primjere grafova. Ovo je prikaz obje strane nejednakosti hipoteze u svakom koraku za potpuni graf sa 6 vrhova.

Slika

| Suma prvih t svojstvenih vrijednosti   | m(G) + (t+3 povrh 4) |
| -------- | ------- |
| 0  | 76    |
| 30 | 80     |
| 60    | 90    |
| 90  | 110   |
| 120  | 145    |
| 150  | 201    |

Testiranje hipoteze provodilo se i za druge posebne vrste grafova.

Binarno stablo:

Slika

Kružni ljestvasti graf:

Slika

Mrežni graf(dimenzija 3x3x3):

Slika

Trivijalni graf:

Slika

Modificirana Brouwerova hipoteza za Laplaceovu matricu povezanosti vrijedila je za svaki od navedenih grafova te za sve ostale na kojima je se testiranje provodilo.
