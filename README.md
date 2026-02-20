# 🎬 Movie Graph Database --- Neo4j Project

## 📌 Overview

This project demonstrates the modeling of a **movie recommendation
graph** using the Neo4j graph database and the Cypher Query Language.

The dataset includes:

-   30 real movies across 3 genres\
-   10 fictional users\
-   Real actors and directors\
-   TV series (to illustrate domain heterogeneity)\
-   User ratings\
-   Rich semantic relationships

The goal is to showcase core concepts of graph data modeling, querying,
and recommendation scenarios.

------------------------------------------------------------------------

## 🧠 Graph Model

### Node Labels

  Label      Description
  ---------- ----------------
  User       Platform users
  Movie      Movies
  Series     TV series
  Genre      Film genres
  Actor      Actors
  Director   Directors

### Relationship Types

  Relationship   Source → Target    Properties
  -------------- ------------------ ------------
  WATCHED        User → Movie       rating
  ACTED_IN       Actor → Movie      ---
  DIRECTED       Director → Movie   ---
  IN_GENRE       Movie → Genre      ---

------------------------------------------------------------------------

## 🏗️ Data Creation --- Cypher CREATE

### Create Genres

``` cypher
CREATE
(:Genre {name:"Action"}),
(:Genre {name:"Drama"}),
(:Genre {name:"Sci-Fi"});
```

### Create Users

``` cypher
CREATE
(:User {name:"Ana"}),
(:User {name:"Bruno"}),
(:User {name:"Carla"}),
(:User {name:"Diego"}),
(:User {name:"Eduarda"}),
(:User {name:"Felipe"}),
(:User {name:"Gabriela"}),
(:User {name:"Henrique"}),
(:User {name:"Isabela"}),
(:User {name:"João"});
```

------------------------------------------------------------------------

## 🔗 Relationship Creation --- MATCH + CREATE

> In Cypher, variables do not persist between separate queries.\
> Therefore, MATCH is required to retrieve existing nodes before
> creating relationships.

### Movies → Genres

``` cypher
MATCH (g:Genre {name:"Sci-Fi"}), (m:Movie {title:"Interstellar"})
CREATE (m)-[:IN_GENRE]->(g);
```

### Actors → Movies

``` cypher
MATCH (a:Actor {name:"Christian Bale"}),
      (m:Movie {title:"The Dark Knight"})
CREATE (a)-[:ACTED_IN]->(m);
```

### Directors → Movies

``` cypher
MATCH (d:Director {name:"Christopher Nolan"}),
      (m:Movie {title:"Interstellar"})
CREATE (d)-[:DIRECTED]->(m);
```

### Users → Movies (with rating)

``` cypher
MATCH (u:User {name:"Ana"}),
      (m:Movie {title:"The Dark Knight"})
CREATE (u)-[:WATCHED {rating:5}]->(m);
```

------------------------------------------------------------------------

## 🔍 Querying the Graph --- MATCH Examples

### Visualize the entire graph

``` cypher
MATCH (n)
OPTIONAL MATCH (n)-[r]->(m)
RETURN n, r, m;
```

### Movies watched by a specific user

``` cypher
MATCH (u:User {name:"Ana"})-[w:WATCHED]->(m:Movie)
RETURN m.title, w.rating;
```

### Highest-rated movies

``` cypher
MATCH (:User)-[w:WATCHED]->(m:Movie)
RETURN m.title, avg(w.rating) AS avgRating
ORDER BY avgRating DESC;
```

------------------------------------------------------------------------

## 📚 Key Learnings

1.  Graph-oriented data modeling enables natural representation of
    highly connected domains.\
2.  MATCH retrieves existing data while CREATE generates new data.\
3.  Relationship directionality preserves semantics.\
4.  Relationships can store properties such as ratings.\
5.  Graph databases support recommendation systems and preference
    analysis.

------------------------------------------------------------------------

## 📌 Conclusion

Graph databases allow intuitive modeling of complex, interconnected
systems and are especially powerful for recommendation engines and
network analysis.
