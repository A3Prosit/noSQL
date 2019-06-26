
# Aller No-SQL
## Roles :
- Animateur : Fantou
- Secrétaire : Max (qui ne sait pas lire)
- Gestionnaire : Maz
- Scribe : Maquecime
- Contexte : On est chez Clarisys

## Mots Clé :
- Service QA *
- Théorème CAP *
- POC
- Système redondant *
- Fault tolerant *
- Transaction *
- Scalabilité verticale *
- SS2L : ss2i open source
- Procédure stockée
- Base relationnelle
- Jointure multiple
- Schéma de la base
- Sprint-démo
- Du de le esport *
- Données hétérogènes

## Contexte :
### Quoi :
- Refonte de la BDD

### Comment :
- En changeant de technologie de base de données

### Pourquoi : 
Pour augmenter les performances et la redondance

- Qui : Laurent Custet
- Où : Clarisys
- Quand : après le stage de Gilly

## Contraintes :
- Conforme au CAP

## Problématique :
- Comment agrandir la taille de votre pé augmenter les performances d’une BDD avec le NoSQL ?
- (Comment passer d’une BDD relationnelle à une base No-SQL ?)

## Généralisation :
- Optimisation
- MC 

## Hypothèses :
- No-SQL = Not Only SQL
- Théorème CAP = Cohérence Adaptativité Pas de Panne
- Théorème CAP = Cohérence Disponibilité Tolérance
- Dans certains contextes le No-SQL est plus performant que le No-SQL
- No-SQL est une architecture et pas un langage
- Le stockage s’effectue de manière différente
- Le No-SQL = json en général
- Le No-SQL se divise en plusieurs parties, comme le GraphQL

## Plan d’action :

## No-SQL

il n'y a pas vraiment de définition standard de ce qu'est une base NoSQL et des centaines d'offres dies "NoSQL" sont dispo, en général on utilise le terme pour dire "non-relationnel". Cependant en général:
elles sont scalable hrizontalement, hautemnt disponible, schema-less, et généralement open source

**non-relationnel**
principe clé du NoSQL, une bdd qui ne suit pas le model relationnel

un problème important que on utilise un ORM est le fait que les objets sont "flattened" en base et répartits sur plusieurs tables. On perds donc les infos tels que l'héritage, l'encapsulation, le polymorphisme. On appelle ce pb ORM impedance mismatch. On doit donc trouver des workaround et cela mène en général à des complications. 
Les BDD Objet sont des bdd relationnelles qui tentent de résoudre ce pb

Un autre pb du model relationel survient lorsqu'on distribue la bdd sur un cluster de server, on a beaucoup de sauts sur le réseau pour récup juste un objet ce qui pose problème pour la scalabilité si on prends un exemple de site web avec des milliers d'users et des millions de requêtes par jour.

Là ou NoSQL est différent... dépends du model de donnée de notre base NoSQL (key-value, document, column, graph)
En général on regroupe les données en aggregat  qui tendent à être récup depuis la bdd en même temps. Par exemple si on a unclient qui possède plusieurs adresses, plutôt que de séprer les données dans plusieurs tables et les joindre après la requêt, on stock le tout au même endroit (même aggregat) dans une node. Il est donc plus facile de recup les infos sans avoir à sauter un peu partout et cette façon de faire est plus flexible si on ajoute un autre type d'info au client par la suite.
Un des inconvénient de cette façon cepenadant et que lorsqu'on veut recup des infos entre agrégats, par exemple une liste des toutes les adresses de tous les clients cela prendra autant de temps qu'un système distribué et donne une requête plus difficile


**scalabilité horizontale:**

la scalage verticale (plus de ressources de serv) c'est cher, risqué et limité (tu veux des yottaflops? dommage on en a pas). Du coup on préfère le scalage horizontal en rajoutant plein de petits serv (des nodes) en cluster. 
Les bases NoSQL sont mieux adaptées que les bdd relationnelles pour la distribution

La scalabilité horizontale devient très intéressante pour les start ups, on commence petit et on peut assez simplement devenir un géant

**forte disponibilité**

data tjrs dispo si on a un pb (hardware, réseau). Coment ? 2 raisons:
- le clustering
- la réplication

la plupart des clusters de bdd nosql sont masterless. Si on en perds un celane créé pas de downtime. 
Pour la perte de donnée: la plupart des du temps (dépends du modèle) les données sont répliquées sur plusieurs nodes, 3 est un bon chiffre , si un plante on en a un deuxième et même si lui il plante on en a tjrs un 3eme. Le failover est auto, pas de démarche à faire lors d'un pb. LA réplication permet également de faire des MAJ sans downtime.

La disponibilité est un trade-off pour laconsistance (CAP) (je suppose que c'est le pb classique de "si on copie, en attendant de faire la copie certains serv ont les mauvaises données" )


**Schema-less**

ou no schema, flexible schema, late-bound schema, dynamic schema (oui tout ça) 

un schéma est ce qui éfini les données qu'on peut mette dans la base: colonnes et type de données 
Dans un bddr le schéma est créé à l'avance, cela permet de garder les données structurées et organiséees mais peut nous empécher de renter les donées dot on a besoin, on doit s'assurer que les données qu'on rentre réspectent le schéma ou le changer (ajouter des nouvelles tables ou colonnes) ce qui peut poser pb. 
Avec un gros volume de données cela pose d'énormes pb mais on peut en avoir à plus petite échelle

en NoSQL on peut stocker les données peut importe le type de données, les clés ou les colonnes. on peut utiliser un schéa pour structurer et roga les données mais il n'est pas requis. On peut stocker les données même si elles ne respectent pas le schéma et on peut appliquer le schéma après ques les données aient déjà étées stockées. Les migrations vers NoSQL sont donc simples et avec le clustering et haute dispo on peut ajuster le schéma sans downtime. Le schéma a cependant un rôle important: il aide à trouver les données dont on a besoin car on sait comment elles sont orga, on recommande donc quad même d'avoir untemplate ( aka predictive template) dans une bdd schema-less


**Les trade offs**

plus on réplique plus il faut faire de lecture écriture : permet de lire plus rapidement (requête la node la plus proche) mais plus lent à écrire

CAP:  on peut avoir que 2 des 3, CA, CP ou AP
Consistency : tt les dupli ont les même valeurs
Availability : tjrs accessible
Partition  tolerance: fonctionnement en cas de panne

CA: mysql, oracle
AP: couchbase, cassandra, amazon DynamoDB, neo4j
CP:
mongoDB, apache HBase, google big table


**différents data models**

pas de chiffre universellement reconnu mais en général on est d'accord pour en donner 4:
- clé-valeurs
	ce que beaucoup d'orm essayent d'émuler. Grosse hashmap de clés et valeurs où les valeurs sont schema-less (peuvent être des strings, images, docs, etc) les clés sont associées et agréguées dans des buckets. Modèles le plus simple à implémenter. En général ce sont des modèles AP, ex: Riak, DynamoDB
- document
	stock des docs JSON ou XML les tags à l'intérieur du doc sont comme les colones d'une bddr. Puisqu'on est schema-less on peut ajouter des tags au besoin. Ainsi, on peut avoir des docs aussi complexes que besoin pour représenter l'info. Ex: mongoDB et CouchDB. Note: ressemblent aux key-value mais au lieu d'utiliser des clés on utilise des tags et ils sont aggéger dans le doc JSON ou XML au lieu d'un bucket
- column
	plutôt que de grouper en ligne on groupe en colonne et on les place de façon contigüe et on peut chercher et indexer plus facilement. On peut avoir une colonne qui map d'autres colonnes qu'on appelle une super-colonne et on peut faire des famille de colonnes. Ex: Cassandra, HBase, Bigtable
- graph
	très spécialisées. On stock la data dans les nodes avec des propriétés, ce qui est similaire à la POO. On stock les données en relations (relationships) aussi appellées edges et leur props. Les relations organisent les nodes et ont une direction. 


des fois les gens ajoutent un 5eme : hydric cache store ou séparent volatile et non-volatile key-value
ou groupent cl-valeur et colonnes. On peut catégoriser une techno NoSQL dans plusieurs catégories selon à qui on demande mais rien n'est strictement défini









- Architecture
- Principe
- Technologies
- Différences entre No-SQL et le modèle relationnel
- CAP
- Scalabilité horizontale
- Base / Acide

Les transactions doivent êtres :
-   **A**tomic: Everything in a transaction succeeds or the entire transaction is rolled back.
-   **C**onsistent: A transaction cannot leave the database in an inconsistent state.
-   **I**solated: Transactions cannot interfere with each other.
-   **D**urable: Completed transactions persist, even when servers restart etc.

qualités indispensables mais incompatibles avec la dispo et les perfs

l'alternative est base:

-   **B**asic  **A**vailability
-   **S**oft-state
-   **E**ventual consistency

souple, il peut donner des réponses approx et sera consistent à un point

on peut évidement être entre les deux

