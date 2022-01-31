#  Programmation BA2 - Formulaire

## StringBuilder - StringJoiner

**StringBuilder([String str])** : Constructs a sb with str as a starting string

```java
sb.append(E e);								// adds element in sb
sb.charAt(int index);					// returns char at pos index
sb.deleteCharAt(int index);		// removes char at pos index
sb.length();									// returns length of sb
sb.reverse();									// reverses the sequence
```

/!\ Use `sb.toString()` when returning the value



**StringJoiner([CharSequence delimiter, CharSequence prefix, CharSequence suffix])** : Constructs a sj

```java
sj.add(CharSequence el);			// adds element in sj
sj.length();									// returns length of sj
sj.merge(StringJoiner oSj);		// appends oSj's content to sj
```

/!\ Use `sj.toString()` when returning the value

## Complexité listes

|    |      ArrayList      |  LinkedList |
|----------|:-------------:|:------:|
| .add()<br />.remove() |  O(n) | O(1) |
| .get()<br />.set() |    O(1)   |   O(n) |

## Vues

```java
// sublists
List<E> subL = list.subList(start, end); // (included, excluded)

// unmodifiable lists
List<E> ul = Collections.unmodifiableList(list);
```

## Iterator

```java
Iterator<E> it = list.iterator();

while (it.hasNext())
  system.out.println(it.next() + " ");
```

## Comparator

```java
class myComp implements Comparator<E> {
  @Override
  public int compare(E o1, E o2) {
    return o2.compareTo(o1);
  }
}

// Sort by length
Comparator<String> cLength = (s1, s2) -> s2.length() - s1.length();

Collections.sort(words, Comparator.comparing(String::toString));
```

**Sorting**

```java
// Sort by length, then alphabetically
Collections.sort(words, Comparator.comparing(String::length)
                   								.thenComparing(String::toString));

// Sort by decreasing length
Collections.sort(words, Comparator.comparing(String::length).reversed());
```

## Patrons

- **Patron Builder** : Aide à la création d'un objet (souvent immuable)

  Example : StringBuilder

- **Patron Decorator** : Altère le comportement d'un objet. Le décorateur a un type différent

  Example : Vues (sublist, unmodifiableList, …), Filtering Streams (BufferedInputStream, ...)

- **Patron Composite** : Regroupe plusieurs objets du même type

  Example : SequenceInputStream

- **Patron Adapter** : Adapte un objet en un autre. L'adapteur a le même type que l'objet de base

  Example : Array into list (`Arrays.asList(...)`)

- **Patron Observer** : Permet à des observers d'observer des subjects. Les observers s'updatent lorsque l'un de leurs sujets observés se met à jour. Incompatible avec l'immuabilité

  Example : Tableur excel (les cellules se modifient si celles qu'elles observent changent)

- **Patron MVC** : Découpage d'un programme en 3 parties : Model - View - Controler

  Example : Projet BA2 Javass

## Functional Interfaces

- The functional interface **Consumer** represents a function of *one* argument returning nothing

  ```java
  void accept(T x);
  ```

- The functional interface **Function** represents a function of *one* argument

  ```java
  R apply(T x);
  ```

- The functional interface **BiConsumer** represents a **Consumer** with *two* arguments

  ```java
  void accept(T t, U u);
  ```

- The functional interface **Predicate** represents a function of *one* argument returning a *boolean*

  ```java
  boolean test(T x);
  ```


