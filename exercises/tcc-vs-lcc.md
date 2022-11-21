# TCC *vs* LCC

Explain under which circumstances *Tight Class Cohesion* (TCC) and *Loose Class Cohesion* (LCC) metrics produce the same value for a given Java class. Build an example of such as class and include the code below or find one example in an open-source project from Github and include the link to the class below. Could LCC be lower than TCC for any given class? Explain.

## Answer

Le TTC et le LCC sont des moyens d'évaluer la cohésion d'une classe : c'est à dire savoir comment les méthodes d'une classes sont liés entre elles.

Lorsque les valeurs de TCC et de LCC sont égales cela signifie que les liens entre les méthodes de la classes sont directes (peut importes si les méthodes sont connectées).

```java
class Humain()

    private int taille;
    private int poids;

    public int getTaille(){
        return taille
    }

    public int getPoids(){
        return poids
    }

    // La méthode calcul_imc lie les méthodes getTaille et getPoids directement par appel
    public float calcul_imc(){
        float imc= getPoids()/getTaille();
        return imc;
    }

```

