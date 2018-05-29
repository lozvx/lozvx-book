# JSR-330 Annotation

Spring component model elements vs. JSR-330 variant

| Spring | javax.inject.\* | javax.inject restrictions / comments |
| :--- | :--- | :--- |
| @Autowired | @Inject | `@Inject` has no 'required' attribute; can be used with Java 8’s `Optional` instead. |
| @Component | @Named / @ManagedBean | JSR-330 does not provide a composable model, just a way to identify named components. |
| @Scope\("singleton"\) | @Singleton | The JSR-330 default scope is like Spring’s `prototype`. However, in order to keep it consistent with Spring’s general defaults, a JSR-330 bean declared in the Spring container is a `singleton` by default. In order to use a scope other than `singleton`, you should use Spring’s `@Scope` annotation. `javax.inject` also provides a [@Scope](http://download.oracle.com/javaee/6/api/javax/inject/Scope.html) annotation. Nevertheless, this one is only intended to be used for creating your own annotations. |
| @Qualifier | @Qualifier / @Named | `javax.inject.Qualifier` is just a meta-annotation for building custom qualifiers. Concrete String qualifiers \(like Spring’s `@Qualifier` with a value\) can be associated through `javax.inject.Named`. |
| @Value | - | no equivalent |
| @Required | - | no equivalent |
| @Lazy | - | no equivalent |
| ObjectFactory | Provider | `javax.inject.Provider` is a direct alternative to Spring’s `ObjectFactory`, just with a shorter `get()` method name. It can also be used in combination with Spring’s `@Autowired` or with non-annotated constructors and setter methods. |

@Inject 等同于Spring中的@Autowired

```java
import javax.inject.Inject;

public class SimpleMovieLister {

    private MovieFinder movieFinder;

    @Inject
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    public void listMovies() {
        this.movieFinder.findMovies(...);
        ...
    }
}
```

@Name

```java
import javax.inject.Inject;
import javax.inject.Named;

public class SimpleMovieLister {

    private MovieFinder movieFinder;

    @Inject
    public void setMovieFinder(@Named("main") MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // ...
}
```

@javax.inject.Named 和@javax.annotation.ManagedBean等同于Spring中的@Conponent

```text
import javax.inject.Inject;
import javax.inject.Named;

@Named("movieListener")  // @ManagedBean("movieListener") could be used as well
public class SimpleMovieLister {

    private MovieFinder movieFinder;

    @Inject
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // ...
}
```

```text
import javax.inject.Inject;
import javax.inject.Named;

@Named
public class SimpleMovieLister {

    private MovieFinder movieFinder;

    @Inject
    public void setMovieFinder(MovieFinder movieFinder) {
        this.movieFinder = movieFinder;
    }

    // ...
}
```

