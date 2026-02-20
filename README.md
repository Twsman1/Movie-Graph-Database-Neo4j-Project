🎬 Movie Graph Database — Neo4j Project
📌 Overview

This project demonstrates the modeling of a movie recommendation graph using the graph database Neo4j and the Cypher Query Language.

The dataset includes:

🎥 30 real movies across 3 genres

👥 10 fictional users

🎭 Real actors and 🎬 directors

📺 TV series (to illustrate domain heterogeneity)

⭐ User ratings

🔗 Rich semantic relationships

The goal is to showcase core concepts of graph data modeling, querying, and recommendation scenarios.

🧠 Graph Model
🟦 Node Labels
Label	Description
User	Platform users
Movie	Movies
Series	TV series
Genre	Film genres
Actor	Actors
Director	Directors
🔗 Relationship Types
Relationship	Source → Target	Properties
WATCHED	User → Movie	rating
ACTED_IN	Actor → Movie	—
DIRECTED	Director → Movie	—
IN_GENRE	Movie → Genre	—
🏗️ Data Creation — Cypher CREATE
🎯 Create Genres
CREATE
(:Genre {name:"Action"}),
(:Genre {name:"Drama"}),
(:Genre {name:"Sci-Fi"});
👥 Create Users
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
🎬 Create Movies (Example)
CREATE
(:Movie {title:"The Dark Knight", year:2008}),
(:Movie {title:"Interstellar", year:2014}),
(:Movie {title:"Parasite", year:2019}),
(:Movie {title:"Dune", year:2021});

(The complete dataset contains 30 movies.)

🎭 Create Actors
CREATE
(:Actor {name:"Keanu Reeves"}),
(:Actor {name:"Christian Bale"}),
(:Actor {name:"Tom Hanks"}),
(:Actor {name:"Leonardo DiCaprio"});
🎬 Create Directors
CREATE
(:Director {name:"Christopher Nolan"}),
(:Director {name:"Ridley Scott"}),
(:Director {name:"Denis Villeneuve"}),
(:Director {name:"James Cameron"});
🔗 Relationship Creation — MATCH + CREATE

⚠️ In Cypher, variables do not persist between separate queries.
Therefore, MATCH is required to retrieve existing nodes before creating relationships.

🏷️ Movies → Genres
MATCH (g:Genre {name:"Sci-Fi"}), (m:Movie {title:"Interstellar"})
CREATE (m)-[:IN_GENRE]->(g);
🎭 Actors → Movies
MATCH (a:Actor {name:"Christian Bale"}),
      (m:Movie {title:"The Dark Knight"})
CREATE (a)-[:ACTED_IN]->(m);
🎬 Directors → Movies
MATCH (d:Director {name:"Christopher Nolan"}),
      (m:Movie {title:"Interstellar"})
CREATE (d)-[:DIRECTED]->(m);
⭐ Users → Movies (with rating)
MATCH (u:User {name:"Ana"}),
      (m:Movie {title:"The Dark Knight"})
CREATE (u)-[:WATCHED {rating:5}]->(m);
🔍 Querying the Graph — MATCH Examples
👀 Visualize the entire graph
MATCH (n)
OPTIONAL MATCH (n)-[r]->(m)
RETURN n, r, m;
🎥 Movies watched by a specific user
MATCH (u:User {name:"Ana"})-[w:WATCHED]->(m:Movie)
RETURN m.title, w.rating;
⭐ Highest-rated movies
MATCH (:User)-[w:WATCHED]->(m:Movie)
RETURN m.title, avg(w.rating) AS avgRating
ORDER BY avgRating DESC;
🎭 Cast of a movie
MATCH (a:Actor)-[:ACTED_IN]->(m:Movie {title:"Dune"})
RETURN a.name;
