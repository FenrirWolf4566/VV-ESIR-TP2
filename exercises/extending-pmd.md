# Extending PMD

Use XPath to define a new rule for PMD to prevent complex code. The rule should detect the use of three or more nested `if` statements in Java programs so it can detect patterns like the following:

```Java
if (...) {
    ...
    if (...) {
        ...
        if (...) {
            ....
        }
    }

}
```
Notice that the nested `if`s may not be direct children of the outer <QA`if`s. They may be written, for example, inside a `for` loop or any other statement.
Write below the XML definition of your rule.

You can find more information on extending PMD in the following link: https://pmd.github.io/latest/pmd_userdocs_extending_writing_rules_intro.html, as well as help for using `pmd-designer` [here](https://github.com/selabs-ur1/VV-TP2/blob/master/exercises/designer-help.md).

Use your rule with different projects and describe you findings below. See the [instructions](../sujet.md) for suggestions on the projects to use.

## Answer

En ouvrant PMD Designer il a fallut mettre une code simple à tester. Celui-ci contenent trois conditions "if" imbriquée correspondait : 
```java
class ClasseTest {
    
    public int ifSimple(int entier) { 
        if(entier==4){
            entier=entier+1;
            if(entier==5){
                entier=entier+1;
                if(entier==6){
                    return 6;
                }
            }
        } 
    }  
}
```

Il fallait ensuite trouver la l'expression à mettre dans la partie XPath. 
* La solution la plus simple à d'abord été : `//IfStatement//IfStatement//IfStatement`
* Puis afin d'éviter les warning pour une même ligne : `/IfStatement[count(ancestor::IfStatement)>=2]`.
* Enfin pour garder les conditions `else(if)` : `/IfStatement[count(ancestor::BlockStatement/Statement/IfStatement)>=3]`

En important la règle nous obtenons quelques chose de ce type (il à fallut rajouter une balise ruleset ainsi que préciser le type "java") :
````xml
<ruleset name = "RecurenceStatement">
   <rule name="ifStatement"
         language="java"
         message="More than 3 if statement detected !"
         class="net.sourceforge.pmd.lang.rule.XPathRule">
      <description>

      </description>
      <priority>3</priority>
      <properties>
         <property name="version" value="2.0"/>
         <property name="xpath">
            <value>
   <![CDATA[
   /IfStatement[count(ancestor::BlockStatement/Statement/IfStatement)>=3]

   ]]>
            </value>
         </property>
      </properties>
   </rule>
</ruleset>
```

Finalement pour vérifier notre règle, il a fallut la lancer depuis pmd :
`.\pmd.bat -d ..\commons-cli-master\src\main\java -f java -R ..\ifStatement.xml`