# AAR-EquToString
Listing 57 AAR-EquToString/EquToString.java Page 93

Cont. from [53.54.55.56.DJW-InherAnimal](https://github.com/Java-PJATK/53.54.55.56.DJW-InherAnimal)

## 10.1 Importance of equal and hashCode methods

Class `Object` defines — among others (in particular

```
public String toString()
```

that we already know) two other important methods:

* `public boolean equals(Object ob)` — which is used to check if two objects are, according to some criterion, equal: for two references `ob1` and `ob2`, the expression `ob1.equals(ob2)` compares the objects and yields `true` or `false`.

But according to what criterion? In class Object there is no data to be compared, so the default implementation just compares addresses of the two objects. 

Very often, this is _not_ what we want. 

When we have two objects of class `Person` and these persons have identical names, dates of birth, passport numbers etc., we rather want to consider the two objects as representing exactly the same person, in other words we want to consider these two objects equal. To get this behavior, we thus have to redefine (override) `equals` in our class.

The `equals` method should implement an equivalence relation on non-null object references. This means that for any non-null references `a`, `b` and `c`  `a.equals(a)` should always be `true` (reflexivity), `a.equals(b)` should have the same value (`true` or `false`) as b.equals(a) (symmetry), and if `a.equals(b)` and `b.equals(c)` are `true` then `a.equals(c)` must also be `true` (transitivity).

Reflexivity and symmetry are usually obvious, but transitivity — not always.

Suppose, we consider two two-element sets equal if they have at least one common element. Then A = {a, b} is equal to B = {b, c} and B is equal to C = {c, d}, but A and C have no common element and are not equal.

* `public int hashCode()` — which is used to calculate the so called **hash code** of an object.

This is necessary if we want to put objects of our class in collections implemented as hash tables (e.g., `HashSet` or keys in a `HashMap`). The implementation of hashCode method from the Object class uses just the address of the object to calculate its hash code.

However, very often this is not desirable as it would lead to situations when two objects that are considered equal (according to `equals`) have different hash codes: as a consequence the collections of such objects would be invalidated. 

Therefore, we have to override also this method remembering to do it consistently: whenever two objects are equal according to `equal`, their hash codes should be exactly the same.

The following example illustrates `toString` and `equals` methods:

## Listing 57 AAR-EquToString/EquToString.java

```java
public class EquToString {
    public static void main(String[] args) {
        PersonGood johny = new PersonGood("John",1980);
        PersonGood john  = new PersonGood("John",1980);
        PersonBad  billy = new PersonBad("Bill",1980);
        PersonBad  bill  = new PersonBad("Bill",1980);

        if (johny.equals(john))
            System.out.println("johny == john");
        else
            System.out.println("johny != john");

        if (billy.equals(bill))
            System.out.println("billy == bill");
        else
            System.out.println("billy != bill");

        System.out.println("johny: " + johny);
        System.out.println("billy: " + billy);
    }
}

class PersonBad {
    private String name;
    private    int byear;
    PersonBad(String n, int y) {
        name  = n;
        byear = y;
    }
    public String getName() { return name;  }
    public    int getYear() { return byear; }
}

class PersonGood {
    private String name;
    private    int byear;
    PersonGood(String n, int y) {
        name  = n;
        byear = y;
    }

    public String getName() { return name;  }
    public    int getYer()  { return byear; }

    @Override
    public String toString() {
        return name + "(" + byear + ")";
    }

    @Override
    public boolean equals(Object ob) {
        if (ob == null || getClass() != ob.getClass()) return false;
        PersonGood p = (PersonGood)ob;
        return name.equals(p.name) && byear == p.byear;
    }
}
```

The program prints

```
    johny == john
    billy != bill
    johny: John(1980)
    billy: PersonBad@659e0bfd
```

As we can see, with `toString` and `equals` methods _not_ overriden (as in class `PersonBad`), the program works but uses the versions of these methods from class `Object`.

To get the expected results, we have to override these methods, as we did in class `PersonGood`.

Next [Listing 58 HUM-HashEquals/Person.java](https://github.com/Java-PJATK/58.59.HUM-HashEquals)
