# [CS-308] Summary Calcul Quantique

[TOC]

$$
\newcommand{\bra}[1]{<#1|}
\newcommand{\ket}[1]{|#1>}
\newcommand{\bracket}[2]{\, <#1|#2> \,}
$$

# 1.	Circuits classiques

Le but d'un **circuit classique** est de faire un "calcul". Par exemple, on prend une fonction booléenne $f: \mathbb F_2^n \rightarrow \mathbb F_2^m$. Les éléments de l'espace $\mathbb F_2^n$ est une suite de $0$ et de $1$ à $n$ arguments. On a donc $(x_1, ..., x_n) \rightarrow f(x_1, ..., x_n)=(y_1, ..., y_m)$ tel que $x_i, y_i \in \{0, 1\}$ sont des bits classiques.

:information_source: $\mathbb F_2^n$ est appelé l'**hypercube de hamming à $n$ arguments**

Peut-on contruire un circuit calculant automatiquement les valeurs de $f$ pour chaque entrée?



Exemples simples de fonctions et circuits associés:

- La **porte logique** `NOT` est associée à la fonction

$$
f(x) = \begin{cases}1 & \text{si } x = 0 \\ 0 & \text{si } x = 1\end{cases}
$$
​		:information_source: $f$ vaut aussi $1-x\mod 2$ ou $1 \oplus x \mod 2$ 

- La porte logique `COPY` (ou `FANOUT`) prend 1 valeur en entrée et 2 valeurs en sortie ($\mathbb F_2 \rightarrow \mathbb F_2^2$). Voici sa table de vérité 

$$
\begin{cases}
	0 & \rightarrow & (0, 0) \\
  1 & \rightarrow & (1, 1)
\end{cases}
$$


> *Définition*: Un **circuit booléen** est un graphe acyclique, dirigé, avec $n$ bits d'entrée et $m$ bits de sortie. Les **noeuds** du graphe (vertices) sont les portes logiques et les **liens** (edges) sont les liens entre les portes.

> *Théorème de Emil Post*: Toute fonction booléenne $f: \mathbb F_2^n \rightarrow \mathbb F_2^m$ peut être représenté par un graphe booléen. De plus, ce graphe booléen est réalisé grâce aux portes {`NOT`, `AND`, `OR`, `COPY`}
>
> :information_source: On dit que l'ensemble {`NOT`, `AND`, `OR`, `COPY`} est un **ensemble de portes universel** car on peut réaliser n'importe que circuit booléean avec ces portes seulement



Pour les circuits quantiques, les portes `AND`, `OR` et `COPY` ne se généralisent pas car elles ne sont pas **réversibles** (e.g. s'il on a $x_1 \and x_2 = 1$, on ne peut pas retrouver les valeurs de $x_1$ et $x_2$ en ne regardant que la sortie). La porte `COPY` est logiquement inversible (on peut facilement reconstruire l'entrée), mais l'inverse *efface* un q-bit $\implies$ dissipe de la chaleur (au sens physique) $\implies$ n'est pas compatible avec les principes de la physique quantique

> *Théorème*: Une fonction booléenne peut être représentée par un circuit booléen (graphe acyclique dirigé) contenant uniquement les portes réversibles {`NOT`, `CNOT` (appelée **contrôle-not**), `CCNOT` (appelée **contrôle-contrôle-not** ou **porte de Toffoli**)}
>
> :information_source: Cet ensemble est un autre ensemble de portes universel. De plus, chaque porte est reversible!

$$
\begin{cases}
NOT 	& x \rightarrow \bar x \\
CNOT 	& (x, y) \rightarrow (x, y \oplus x) \\
CCNOT & (x, y, z) \rightarrow (x, y, z \oplus (x \and y))
\end{cases}
$$

Dans une porte `CNOT` (on parle aussi de `XOR` contrôlé), le bit $x$ est appelé le **bit de contrôle**, tandis que le bit $y$ est appelé "**target bit**". La porte `CCNOT` revient à faire deux portes `CNOT`avec deux bits de contrôle ($x$ et $y$) et un target bit $z$

La porte `COPY` peut être exprimée de façon réversible grâce à une porte `CNOT` (mettre le target bit à $0$). De même, une porte `AND` peut être réalisée de façon réversible grâce à une porte de Toffoli (mettre le target bit à $0$). Finalement, on peut faire de même avec la porte `OR` en mettant le target bit à $1$ et en inversant les bits de contrôles

Puisqu'on peut réaliser `OR`, `AND` et `COPY` grâce à `NOT`, `CNOT` et `CCNOT`, on voit d'après le théorème de Post que toute fonction booléenne peut être réalisée par un circuit booléen *de façon réversible*.



# 2.	Algèbre linéaire en notation de Dirac

On travaille dans des espaces vectoriels de dimensions $2^n$ (où $n$ représente le nombre de q-bits) sur le corps $\C$ munis de produits scalaires. On appelle celà un **espace d'Hilbert** $H$

La **notation de Dirac** pour un vector colonne $\vec \psi$ vaut $\ket{\psi}$ (appelé "**ket**") $= \begin{pmatrix}\alpha_1\\\ldots\\\alpha_n\end{pmatrix}$. Le **vecteur conjugué** (transposé et complexe conjugué) est représenté par $\vec \psi ^ \dagger = \vec \psi ^ {T, *}$ ou $\bra{\psi}$ (appelé "**bra**") en notation de Dirac. A noter que c'est un vecteur lignes, donc $\bra{\psi} = (\alpha_1^*, \ldots, \alpha_n^*)$

Le produit scalaire de $H$ est défini entre un vecteur ligne ($\vec \phi ^ \dagger = (\beta_1^*, \beta_2^*, \ldots, \beta_n^*)$) et un vecteur colonne ($\vec \psi = (\alpha_1, \alpha_2, \ldots, \alpha_n)$):
$$
\vec \phi ^ \dagger \cdot \vec \psi = \beta_1^* \alpha_1 + \ldots + \beta_n^* \alpha_n
$$
En notation de Dirac, on note (appelé "**bracket**"):
$$
\vec \phi ^ \dagger \cdot \vec \psi = \bracket{\phi}{\psi} \in \C
$$

On note que le produit scalaire garde les même propriétés:

- positivité : $\bracket{\psi}{\psi} \ge 0$ et égal 0 ssi $\ket{\psi} = 0$
- linéarité: $\bra{\phi}(\alpha \ket{\psi_1}+ \beta \ket{\psi_2})$
- symmétrie: $\bracket{\phi}{\psi} = \bracket{\psi}{\phi}^*$



Avec $n=1$ (donc $H= \C^2$ (vecteurs à 2 composantes complexes)), on a donc $\begin{pmatrix}\alpha\\\beta\end{pmatrix} = \ket\psi$. Si $|\alpha|^2 + |\beta|^2= \bracket{\psi}{\psi}=1$, on appelle alors le vecteur un **bit quantique**

On définit la **base computationnelle** $\ket{0} = \begin{pmatrix}1\\0\end{pmatrix}$ et $\ket{1} = \begin{pmatrix}0\\1\end{pmatrix}$, ainsi que la **base de Hadamard** avec $\ket{+} = \frac{1}{\sqrt{2}}\begin{pmatrix}1\\1\end{pmatrix}$ et $\ket{-} = \frac{1}{\sqrt{2}}\begin{pmatrix}1\\-1\end{pmatrix}$

En prenant $n=2$ (donc $H=\C^4$). Les vecteurs de norme 1 représentent une **paire de bits quantiques**. La base computationnelle de deux bits quantiques comprend les vecteurs $\ket{00} = \begin{pmatrix}1\\0\\0\\0\end{pmatrix}$, $\ket{01} = \begin{pmatrix}0\\1\\0\\0\end{pmatrix}$, $\ket{10} = \begin{pmatrix}0\\0\\1\\0\end{pmatrix}$, et $\ket{11} = \begin{pmatrix}0\\0\\0\\1\end{pmatrix}$

De manière générale, la base orthonormée computationnelle pour $n$ bits quantiques vaut $\{\ket{b_1 b_2\ldots b_n}:b_i \in \{0, 1\}\}$



Dans les espaces $H_1$ pour $n_1$ bits quantiques et $H_2$ pour $n_2$ bits quantiques, on construit un espace plus grand pour $n_1+n_2$ bits quantiques $H_1 \otimes H_2$. On note que $\dim(H_1)=2^{n_1}, \dim(H_2)=2^{n_2} \implies \dim(H_1 \otimes H_2)=2^{n_1+n_2}$. On construit la base de $H_1 \otimes H_2$ avec les bases de $H_1$ et $H_2$
$$
base(H_1)= \{\ket{\phi_1}, \ldots, \ket{\phi_{2^{n_1}}} \} \\
base(H_2)= \{\ket{\psi_1}, \ldots, \ket{\psi_{2^{n_2}}} \}
$$
On obtient donc
$$
base(H_1 \otimes H_2)= 
\bigg\{
	\ket{\phi_i} \otimes \ket{\psi_j}, 
	\begin{cases}
		i \in \{1, \ldots, 2^{n_1}\} \\
    j \in \{1, \ldots, 2^{n_2}\}
	\end{cases}
\bigg\}
$$
où $\otimes$ est le **produit tensoriel**. On note aussi $\ket{\phi} \otimes \ket{\psi} \equiv \ket{\phi,\psi}$

Le produit scalaire de $H_1 \otimes H_2$ est défini de la manière suivante
$$
\big(\bra{\phi'} \otimes \bra{\psi'} \big) 
\cdot 
\big(\ket{\phi} \otimes \ket{\psi} \big)
=
\bracket{\phi'}{\phi} \bracket{\psi'}{\psi} 
\in \C
$$
Le produit tensoriel est distributif par rapport à la somme, i.e.
$$
\ket \phi \otimes ( \alpha \ket \psi + \beta \ket{\psi'})
=
\alpha \ket \phi \otimes \ket \psi + \beta \ket \phi \otimes \ket{\psi'} \\

(\alpha^* \bra \psi + \beta^* \bra{\psi'}) \otimes \bra \phi
=
\alpha^* \bra \psi \otimes \phi + \beta^* \bra{\psi^*} \otimes \bra \phi
$$
 Le conjugé d'un produit tensoriel vaut
$$
(\ket \phi \otimes \ket \psi)^ \dagger = \bra \phi \otimes \bra \psi
$$
