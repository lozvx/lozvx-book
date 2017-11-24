Immutable Collections:

Advantages:

* Safe for use by untrusted libraries.

* Thread-safe: can be used by many threads with no risk of race conditions.

* Doesn't need to support mutation, and can make time and space savings with that assumption. All immutable collection implementations are more memory-efficient than their mutable siblings. \(
  [analysis](https://github.com/DimitrisAndreou/memory-measurer/blob/master/ElementCostInDataStructures.txt)
  \)
* Can be used as a constant, with the expectation that it will remain fixed.

| Interface | JDK or Guava? | Immutable Version |
| :--- | :--- | :--- |
| `Collection` | JDK | [`ImmutableCollection`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/ImmutableCollection.html) |
| `List` | JDK | [`ImmutableList`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/ImmutableList.html) |
| `Set` | JDK | [`ImmutableSet`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/ImmutableSet.html) |
| `SortedSet`/`NavigableSet` | JDK | [`ImmutableSortedSet`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/ImmutableSortedSet.html) |
| `Map` | JDK | [`ImmutableMap`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/ImmutableMap.html) |
| `SortedMap` | JDK | [`ImmutableSortedMap`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/ImmutableSortedMap.html) |
| [`Multiset`](https://github.com/google/guava/wiki/NewCollectionTypesExplained#Multiset) | Guava | [`ImmutableMultiset`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/ImmutableMultiset.html) |
| `SortedMultiset` | Guava | [`ImmutableSortedMultiset`](http://google.github.io/guava/releases/12.0/api/docs/com/google/common/collect/ImmutableSortedMultiset.html) |
| [`Multimap`](https://github.com/google/guava/wiki/NewCollectionTypesExplained#Multimap) | Guava | [`ImmutableMultimap`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/ImmutableMultimap.html) |
| `ListMultimap` | Guava | [`ImmutableListMultimap`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/ImmutableListMultimap.html) |
| `SetMultimap` | Guava | [`ImmutableSetMultimap`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/ImmutableSetMultimap.html) |
| [`BiMap`](https://github.com/google/guava/wiki/NewCollectionTypesExplained#BiMap) | Guava | [`ImmutableBiMap`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/ImmutableBiMap.html) |
| [`ClassToInstanceMap`](https://github.com/google/guava/wiki/NewCollectionTypesExplained#ClassToInstanceMap) | Guava | [`ImmutableClassToInstanceMap`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/ImmutableClassToInstanceMap.html) |
| [`Table`](https://github.com/google/guava/wiki/NewCollectionTypesExplained#Table) | Guava | [`ImmutableTable`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/ImmutableTable.html) |

---

new Colection:

* Multiset
* Multimap
* BiMap
* Table
* ClassToInstanceMap
* RangeSet
* RangeMap

---

Collection Utils:

| Interface | JDK or Guava? | Corresponding Guava utility class |
| :--- | :--- | :--- |
| `Collection` | JDK | [`Collections2`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/Collections2.html) |
| `List` | JDK | [`Lists`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/Lists.html) |
| `Set` | JDK | [`Sets`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/Sets.html) |
| `SortedSet` | JDK | [`Sets`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/Sets.html) |
| `Map` | JDK | [`Maps`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/Maps.html) |
| `SortedMap` | JDK | [`Maps`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/Maps.html) |
| `Queue` | JDK | [`Queues`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/Queues.html) |
| [`Multiset`](https://github.com/google/guava/wiki/NewCollectionTypesExplained#Multiset) | Guava | [`Multisets`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/Multisets.html) |
| [`Multimap`](https://github.com/google/guava/wiki/NewCollectionTypesExplained#Multimap) | Guava | [`Multimaps`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/Multimaps.html) |
| [`BiMap`](https://github.com/google/guava/wiki/NewCollectionTypesExplained#BiMap) | Guava | [`Maps`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/Maps.html) |
| [`Table`](https://github.com/google/guava/wiki/NewCollectionTypesExplained#Table) | Guava | [`Tables`](http://google.github.io/guava/releases/snapshot/api/docs/com/google/common/collect/Tables.html) |



