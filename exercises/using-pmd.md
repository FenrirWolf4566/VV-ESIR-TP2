# Using PMD

Pick a Java project from Github (see the [instructions](../sujet.md) for suggestions). Run PMD on its source code using any ruleset. Describe below an issue found by PMD that you think should be solved (true positive) and include below the changes you would add to the source code. Describe below an issue found by PMD that is not worth solving (false positive). Explain why you would not solve this issue.

## Answer

Projet sélectionné : [apache/commons-cli](https://github.com/apache/commons-cli)

* Faux positif
  
En lançant la commande avec "performance.xml" comme ruleset, nous obtenons cette spécification : 
```src\main\java\org\apache\commons\cli\DefaultParser.java:606:  SimplifyBooleanReturns: Avoid unnecessary if..then..else statements when returning booleans```
A la ligne 606 du fichier Java indiqué nous retrouvons les conditions "if" :
```java
private boolean isLongOption(final String token) {
        if (token == null || !token.startsWith("-") || token.length() == 1) {
            return false;
        }

        final int pos = token.indexOf("=");
        final String t = pos == -1 ? token : token.substring(0, pos);

        if (!getMatchingLongOptions(t).isEmpty()) {
            // long or partial long options (--L, -L, --L=V, -L=V, --l, --l=V)
            return true;
        }
        if (getLongPrefix(token) != null && !token.startsWith("--")) {
            // -LV
            return true;
        }

        return false;
    }
```
Sauf qu'ici le premier "if" à le rôle d'assert afin de vérifier l'attribut "token"
Et les deuxièmes conditions vérifient chacunes une instruction bien particulière que les devéloppeur on prit soin de commenter. Ainsi nous aurions tout intérer à ne pas fusioner ces deux conditions.
Il s'agit donc d'un faux positif : le problème ne mérite pas d'être résolu.


* Vrai positif
  
Sur le meme fichier et avec la même ruleset nous avons également cette erreur : 
```src\main\java\org\apache\commons\cli\DefaultParser.java:599:  UseIndexOfChar: String.indexOf(char) is faster than String.indexOf(String).```
Ici nous l'utilisation de `String.indexOf(String)` doit être remplacer par `String.indexOf(char)` par soucis de rapidité.
Si nous nous souçions de la rapidité de notre programme, ce problème peut devenir un vrai positif.
