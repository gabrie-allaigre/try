# try

Comment gérer la propagationn des exceptions avec Java 8 et les lambdas.

Le code provient de http://blog.zenika.com/2014/02/19/repenser-la-propagation-des-exceptions-avec-java-8/

# Usage

```xml
<dependency>
    <groupId>com.talanlabs</groupId>
    <artifactId>try</artifactId>
    <version>1.0.0</version>
</dependency>
```

# Exemples

Dans les exemples suivant `myList` est une liste d'entier.

``` java
List<Integer> myList = Arrays.asList(1, 2, 3);
```

- Appel de la fonction sur toutes les valeurs de liste et après il vérifie si il y a des erreurs

``` java
Map<Type, List<Try<Double>>> result = myList.stream().
    map(Try.of(this::doSomething)).
    map(trry -> trry.map(i -> Math.PI * i)).
    collect(Try.groupingBySuccess()); 
if(result.containsKey(Type.FAILURE)) {
    // do something with failure
} 
if(result.containsKey(Type.SUCCESS)) {
    // do something with success
}
```

- Appel de la fonction sur toutes les valeurs de la liste, si une valeur génère une exception, il s'arrête

``` java
Try<List<Integer>> result = myList.stream().
    map(Try.lazyOf(this::doSomething)).
    collect(Try.collect());
if (result.isSuccess()) {
    // process success
} else {
    // process failure
}
```