# DOJO Réécrire les bases - Java

## Principe

Dans ce dojo vous allez implémenter des fonctions de bases du langage vous-même. 
Pour cela le corps de fonctions à compléter vous sont fournis vous n'aurez qu'à écrire le code dedans pour répondre aux spécifications. 
Une batterie de tests unitaires vous permettra de vérifier que votre code fonctionne.

## Prérequis - Outils

1. [JDK 17](https://adoptium.net/) minimum
2. [Maven](https://maven.apache.org/download.cgi)
3. Un IDE : [Intellij](https://www.jetbrains.com/fr-fr/idea/download/) ou [Eclipse](https://www.eclipse.org/downloads/packages/), voire [VSCode](https://code.visualstudio.com/) 

## Tester son code

Pour lancer les tests, lancez la commande :

``` bash
mvn test
```

## Règles

Le but est d'implémenter ces fonctions, donc évidement utiliser la version native pour résoudre le problème est interdit. 
Il faut jouer le jeu sinon cela ne sert à rien. La plupart des fonctions peuvent être résolues juste avec une boucle for classique. 
Essayez de vous y cantonner.

## À vous de jouer !

### 1. filter

``` java
public static <T> List<T> filter(List<T> originalLst, Predicate<T> predicate)
```

`filter` va, comme son nom l'indique filtrer les éléments d'un tableau selon une condition et retourner un nouveau tableau avec uniquement les éléments qui remplissent la condition `predicate`.


### 2. map

``` java
public static <T,D> List<D> map(List<T> originalLst, Function<T,D> function)
```

`map` est une projection. On a en entrée une liste de `T` originalLst, et une fonction `function` qui a en entrée un élément de type `T` et en sortie un élément de type `R`. 
On applique cette fonction à l'ensemble des éléments de la liste et on obtient une nouvelle liste de `R`.

### 3. flatMap

``` java
public static <T,D> List<D> flatMap(List<List<T>> originalLst, Function<List<T>, Stream<? extends D>> function)
```

`flatMap`, tout comme map, elle permet d'appliquer une fonction de mappage, mais produit un flux de nouvelles valeurs. Elle peut être utilisée là où nous devons aplatir ou transformer.
On a une liste de liste `originalLst` en entrée et une fonction `function` qui est appliquée à chaque élément et la fonction renvoie le nouveau flux `Stream<D>`.

### 4. Matchers

Les matchers permettent de déterminer si un ou plusieurs éléments de la liste répondent à un prédicat

#### a. anyMatch

``` java
public static <T> Boolean anyMatch(List<T> originalLst, Predicate<T> predicate)
```

`anyMatch` retourne `true` dans le cas où au moins un élément de la liste satisfait le prédicat

#### b. allMatch

``` java
public static <T> Boolean allMatch(List<T> originalLst, Predicate<T> predicate)
```

`allMatch` retourne `true` dans le cas où tous les éléments de la liste satisfont le prédicat

#### c. noneMatch

``` java
public static <T> Boolean noneMatch(List<T> originalLst, Predicate<T> predicate)
```

`noneMatch` retourne `true` dans le cas où aucun élément de la liste ne satisfait le prédicat

### 5. count

``` java
public static <T> Long count(List<T> originalLst)
```

`count` retourne le nombre d'éléments contenus dans la liste

### 6. distinct

``` java
public static <T> List<T> distinct(List<T> originalLst)
```

`distinct` retourne une liste contenant les éléments uniques de la liste originale (aka, une liste dédoublonnée). L'unicité doit se faire en fonction de la méthode `Object#equals`

### 7. findAny
``` java
public static <T> Optional<T> findAny(List<T> originalLst)
```

`findAny` retourne un `Optional` contenant n'importe quel élément de la liste originale, ou un `Optional` vide si la liste originale est vide

Le comportement décrit est (tout comme celui de la méthode `java.util.stream.Stream#findAny`) volontairement non-déterministe. Une implémentation de cette méthode est libre d'adopter (ou pas) un comportement déterministe.

### 8. findFirst
``` java
public static <T> Optional<T> findFirst(List<T> originalLst)
```

`findFirst` retourne un `Optional` contenant le premier élément de la liste originale, ou un `Optional` vide si la liste originale est vide

### 9. min/max

``` java
public static Optional<T> T min(List<T> originalLst, Comparator<? super T> comparator)
public static Optional<T> T max(List<T> originalLst, Comparator<? super T> comparator)
```

`min` et `max` retournent un `Optional` contenant respectivement le minimum et le maximum de la liste originale d'après le comparateur fourni, ou un `Optional` si la liste originale est vide

### 10. reduce

``` java
public static <T> Optional<T> reduce(List<T> orignalLst, BinaryOperator<T> accumulator)
```

`reduce` retourne un `Optional` contenant le résultat de l'application d'une fonction d'accumulation associative, ou un `Optional` vide si aucune réduction n'est applicable

Pour une liste d'entier contenant les éléments de 1 à 5 (inclusifs) si la fonction d'accumulation est la somme de deux entiers, alors la réduction retournera la somme des entiers de 1 à 5 (15).

Pour donner un autre exemple, si la fonction d'accumulation est le produit de deux entiers, alors la réduction retournera 5! (factorielle de 5 = 120).

### 11. Etape finale

Lorsque l'on utilise les Streams on va forcément être amené à enchainer plusieurs fonctions, on va donc tenter dans le dernier exercice de mélanger tout ça avec un cas concret.
On va fournir une liste de personnes ayant voyagé dans différentes villes autour du monde, le Stream que l'on va vouloir reproduire doit :
- filtrer sur les personnes ayant plus de 30 ans, mais moins de 40
- récupérer la liste des villes visitées
- réduire à une liste globale et non plus par personne
- récupérer le pays associé à la ville
- enlever les doublons
- trier pas ordre alphabétique
- sortir le résultat sous la forme d'une liste

La méthode à implémenter a la signature suivante :

``` java
public static <P,C,S> List<S> fatality(List<P> originalLst,
          Predicate<P> filterTopLevel,
          Function<P, List<C>> mapTopLevel,
          Function<List<C>, Stream<? extends C>> flatMap,
          Function<C, S> mapSecondLevel,
          Comparator<S> comparator)
```

C'est une bonne conclusion pour la partie Stream qui se clôt avec cet exemple.

## Et Spring dans tout ça ?

Nous avons vu comment réécrire les fonctions Stream afin de démystifier un peu tout cela.
Mais quand on travaille avec Spring, les gens parlent de magie.
Dans cet exercice nous allons tenter de créer un mini framework, que l'on va appeler Summer, qui imitera le principe d'inversion de contrôle (IoC) que l'on connait tous bien.
Plusieurs versions sont mises à disposition avec des **TODO** indiquant les bouts de code à compléter.
La structure des projets est également indiquée avec les endroits à modifier. 

### 1. Posons les bases

Dans le package `fr.java.spring.ioc.version1`, vous trouverez une classe `App` qui utilise `PersonService` pour créer et rechercher une personne dans la foulée.
Il vous faudra utiliser l'injection de dépendance manuellement pour utiliser le Service et exécuter les actions.

```
version1
    |_ webapp
    |   |_ dao
    |   |   |_ PersonDAO.java
    |   |   |_ PersonDAOImpl.java
    |   |_ service
    |       |_ PersonService.java
    |       |_ PersonServiceImpl.java
    |_ App1 (à mettre à jour)
```

### 2. Ajoutons un peu de _Context_

Nous allons introduire la notion de contexte, dans le package `fr.java.spring.ioc.version2` vous trouverez une nouvelle version de la webapp mais également les prémices de notre framework :

```
version2
    |_ summer
    |   |_ context
    |       |_ ApplicationContext.java (à mettre à jour)
    |_ webapp
    |   |_ dao
    |   |   |_ PersonDAO.java (à mettre à jour)
    |   |   |_ PersonDAOImpl.java (à mettre à jour)
    |   |_ service
    |       |_ PersonService.java (à mettre à jour)
    |       |_ PersonServiceImpl.java (à mettre à jour)
    |_ App2 (à mettre à jour)
```

Il faudra utiliser, dans la classe `ApplicationContext`, de la reflexion et les annotations présentes dans le package `fr.java.spring.ioc.common.annotation` pour trouver tous les Bean candidats à être instanciés et éventuellement les dépendances vers d'autres Bean.  

#### Annotations

On va utiliser 2 annotations pour définir nos Bean :

- `Autowired` : Avec Spring il est possible d'injecter des dépendances de plusieurs manières mais il est recommandé de la faire via le constructeur.

- `Component` : Pour permettre au framework de connaitre les classes déclarées en tant que Bean, il peut y avoir tout un tas d'annotations que Spring utilise.
                Pour simplifier les explications, on va de notre côté utilisé uniquement `@Component`.

Ref: https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans-annotation-config

#### Contexte

Pour simuler le comportement de Spring, on va avoir plusieurs étapes à respecter :

1. Notre classe de contexte va se baser sur une classe principale qui va indiquer le package à partir duquel il faudra scanner les Beans candidats 
2. Récupérer tous les Beans nécessaires au programme
3. Appliquer le même pattern singleton par défaut comme Spring (d'autres scopes existent, mais on va uniquement appliquer celui-ci dans ce DOJO)
4. Vérifier qu'il n'y a qu'une implémentation possible pour un Bean donné (on ne va pas traiter les Qualifier dans cet exemple), et la récupérer
5. Déduire le constructeur à utiliser s'il y a une injection de dépendance possible
6. Et enfin un peu de récursion pour itérer sur tous les Beans de notre programme

Ref: https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#context-introduction

### 3. Proxy AOP

Le principe de Proxy permet d'utiliser un objet qui englobe l'instance du Bean avec lequel on souhaite travailler.
Cela permet d'ajouter des comportements spécifiques en fonction de l'annotation utilisée sur celui-ci.

Avec Spring, il est possible d'utiliser :
- l'implémentation dynamique Java par défaut (JDK dynamic proxies)  
![JDKProxy](assets/img/JDKProxy.png)

- les proxies CGLIB  
![CGLib](assets/img/cglib.png)

Dans cet exemple, nous allons nous concentrer sur les proxies dynamiques, vous pourrez approfondir le second type via la documentation en référence.

Ref:
- https://www.baeldung.com/spring-aop-vs-aspectj
- https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#aop-introduction-proxies

#### InvocationHandler

Nous allons créer une classe implémentant `InvocationHandler` qui a pour méthode :

```
public Object invoke(Object proxy, Method method, Object[] args)
```

Nous allons stocker le Bean proxifié dans le `ProxyInvocationHandler`, il ne faudra donc pas passer le proxy en paramètre, mais seulement la méthode à appeler et les paramètres de celle-ci.

``` java
public class ProxyInvocationHandler implements InvocationHandler {

    private final Object objectToHandle;

    public ProxyInvocationHandler(Object objectToHandle) {
        this.objectToHandle = objectToHandle;
    }

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) {
        return invokeMethod(method, args);
    }

    private Object invokeMethod(Method method, Object[] args) {
        try {
            return method.invoke(objectToHandle, args);
        } catch (IllegalAccessException | InvocationTargetException e) {
            throw new SummerException("Error occurred in proxy", e);
        }
    }
}
```

Le projet a déjà prévu une annotation `@Cacheable` que nous allons gérée via un `CacheableHandler`.
Celui-ci étends `AbstractProxyHandler` qui définit le comportement de tous les futurs handlers et propose une méthode `isSupported` pour vérifier qu'une méthode a bien l'annotation correspondante au handler instancié.

Il faudra ensuite que notre `ApplicationContext` manipule des objets de type `ProxyInvocationHandler` au travers de la méthode `createBean` et non plus l'instance réelle de nos Bean.

Cela permettra d'ajouter un comportement spécifique pour chaque annotation qui étendra `AbstractProxyHandler`. 

```
version3
    |_ summer
    |   |_ context
    |   |   |_ ApplicationContext.java (à mettre à jour)
    |   |_ handler
    |       |_ AbstractProxyHandler.java (à mettre à jour)
    |       |_ CacheableHandler.java (à mettre à jour)
    |       |_ ProxyInvocationHandler.java (à mettre à jour)
    |_ webapp
    |   |_ dao
    |   |   |_ PersonDAO.java
    |   |   |_ PersonDAOImpl.java
    |   |_ service
    |       |_ PersonService.java
    |       |_ PersonServiceImpl.java
    |_ App3
```

NB : Un piège dans lequel on peut facilement tombé est le fait d'appeler une méthode annotée à l'intérieur même de son Bean.
Cela a pour effet de ne pas repasser par le Proxy géré par le framework et de ne pas exécuter le code associé.

### 4. Conclusion

Nous avons posé les bases du framework :
- Contexte
- IoC
- AOP

Il manque énormément de chose pour couvrir tous les aspects offerts par Spring voir d'autres solutions (Quarkus, Micronaut AOT, ...).
Mais cela permet déjà d'avoir une idée des mécaniques avec lesquelles nous jouons tous les jours.
