# Applying the SOLID Principles in Java

A refactoring exercise. A small program that computes the total area of a collection of shapes is redesigned step by step until it follows the five **SOLID** object-oriented design principles.

Developed by André Baptista ([@andrefb26](https://github.com/andrefb26)) for the **Software Design and Testing Laboratory** course (L.EIC014) of the Bachelor in Informatics and Computing Engineering at FEUP (University of Porto), 2025/26. It is lab assignment "SOLID Principles" (week 8).

## The problem

The program has to:

- compute the sum of the areas of many kinds of shapes (squares, rectangles, circles, ellipses, triangles) and of houses;
- compute the total area of a city made of houses;
- print each result in several formats (plain text and XML);
- support objects that have no area at all, such as lines.

## How each principle is applied

| Principle | In this code |
|---|---|
| **S**ingle Responsibility | Summing areas (`AreaAggregator`) is separate from formatting the result (`AreaStringOutputter`, `AreaXMLOutputter`). Each class has one reason to change. |
| **O**pen/Closed | `AreaAggregator` only knows the `HasArea` interface, so adding a new shape means adding a new class. No existing code changes and there are no `instanceof` chains. |
| **L**iskov Substitution | `Square` does not inherit from `Rectangle`, which avoids the classic broken square/rectangle hierarchy. `Line` has no area, so it is only a `BasicShape` and is never forced to implement an area method that would have to throw. |
| **I**nterface Segregation | Shape behaviour is split into small interfaces: `BasicShape`, `Drawable` and `HasArea`. `Shape` combines them, while `House` only implements `HasArea` because it doesn't need to be drawable. |
| **D**ependency Inversion | The outputters depend on the `SumProvider` abstraction, not on a concrete class. Both `AreaAggregator` (shapes) and `City` (houses) implement it, so the same outputters format either one. |

## Class overview

```
            HasArea        Drawable      BasicShape
               ▲   ▲          ▲              ▲   ▲
               │    \         │              │   └── Line
               │     └──── Shape ────────────┘
             House           ▲
                 Square, Rectangle, Circle, Ellipse, Triangle

            SumProvider ◄── AreaAggregator  (sum of HasArea objects)
                 ▲     ◄── City            (sum of its houses)
                 │
   AreaStringOutputter, AreaXMLOutputter
```

## Running

Requirements: JDK 17+ (Gradle downloads the rest).

```bash
./gradlew build
java -cp build/classes/java/main Application
```

Example output:

```
Sum of areas: 369.9557428756428
<area>369.9557428756428</area>
Sum of areas: 200.0
<area>200.0</area>
```

## Technologies

Java · Gradle · JUnit 5
