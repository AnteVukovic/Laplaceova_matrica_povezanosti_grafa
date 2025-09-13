# Laplaceova matrica povezanosti grafa

Cilj ovog projekta je definirati i analizirati Laplaceovu matricu povezanosti grafa, proučiti njezina svojstva te ispitati povezanost s Brouwerovom pretpostavkom o spektru Laplaceove matrice grafa.

## Biblioteke

* __NetworkX__ - za kreiranje grafova i upravljanje njima
* __NumPy__ - za rad s matricama i algebarske operacije
* __SciPy__ - za algebarske operacije

## Pokretanje

Nakon što se otvori datoteka u kojoj se nalazi .ipynb dokument u VisualStudioCode-u, potrebno je odabrati kernel ili stvoriti novi klikom na gumb Select kernel u gornjem desnom kutu:

Slika

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

