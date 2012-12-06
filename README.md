Rillit
======

Experimental functional lenses for Scala 2.10 using Macros.

Rillit combines the use of `Dynamic` with macros to provide
functional lenses in Scala with minimal boilerplate. You can
create lenses for nested case classes in a type-safe way by
simply instantiating the lens builder and chaining whatever
fields you want to include in the lens. And that's it.

Better documentation is on it's way, but here's the gist:

```scala
case class Electronic(email: String, web: String)
case class Contact(electronic: Electronic)
case class Name(first: String, last: String)
case class Person(name: Name, age: Int, contact: Contact)

object Main {
  val aki = Person(
    name = Name("Aki", "Saarinen"),
    age  = 27,
    contact = Contact(
      electronic = Electronic(
        email = "aki@akisaarinen.fi",
        web   = "http://akisaarinen.fi"
      )
    )
  )

  def main(args: Array[String]) {
    val email = new LensBuilder[Person].contact.electronic.email.apply

    val aki2 = email.set(aki, "aki2@akisaarinen.fi")

    println("Original: %s".format(email.get(aki)))
    println("Updated:  %s".format(email.get(aki2)))
  }
}
```

When run, this will produce the following: 

```
Original: aki@akisaarinen.fi
Updated:  aki2@akisaarinen.fi
```

Requirements
============

* Scala 2.10 (tested with 2.10.0-RC5, but will probably work with older release candidates as well)
* SBT 0.12
* A bit of love for functional lenses

Caveats
=======

Macros are an *experimental* feature of 2.10, so it is probably not a good idea
to use something like this in critical production code just yet. Also, the code
is not very pretty as-is, it's just a proof-of-concept I made to convince
myself that this is even possible with `Dynamic` and macros. 

Inspiration
===========

Functional lenses has been implemented in a variety of forms for many languages.
Implementations for Scala, which were the inspiration to create rillit, include:

* Scalaz: https://github.com/scalaz/scalaz
* Shapeless: https://github.com/milessabin/shapeless
* Macrocosm: https://github.com/retronym/macrocosm

License
=======

Rillit is released under the Apache License, Version 2.0.
