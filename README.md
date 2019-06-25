
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
- SS2L *
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

- No-SQL
- Architecture
- Principe
- Technologies
- Différences entre No-SQL et le modèle relationnel
- CAP
- Scalabilité horizontale
- Base / Acide



## NoSQL  : Not Only SQL
- Non relationnelle 
	- Résous les problème d'ORM impedance mismatch (encapsulation / inheritance / polymorphism...)
	- Bonne compatibilité avec les serveurs cluster (données répartis) en regroupant sous forme de noeuds les tables qui coïncident 
- Horizontally scalable
	- Ajouter des petits serveurs (nodes) dans des cluster pour séparer l'information sont meilleur pour discribuer l'information plutôt que des BDD standart
	- Grande disponibilité
	- De la replication dans les cluster (donc données dipsonibles partout)
	- Pas de downtime
- Schema-less
	- Comporte un très bon schéma de la base de données qui est facilement ajustable
- Open Source
	- Aucun coût
	- Pas pour l'éternité ?


 ## Data Models (or Categories of NoSQL)
- **Key Value** (Big has table of key & values )
	- Tout repose sur clé/valeur
	- Très facile à implémenter
	 -   **Redis** (_VMWare_) : Vodafone, Trip Advisor, Nokia, Samsung, Docker
	-   **Memcached** (_Danga_) : LiveJournal, Wikipédia, Flickr, Wordpress
	-   **Azure Cosmos DB** (_Microsoft_) : Real Madrid, Orange tribes, MSN, LG, Schneider Electric
	-   **SimpleDB** (_Amazon_)
	
- **Document**;  Clé/Docuemts 
	- Manipulation complexe sur chaque attribut tout en passant à l'échelle dans un contexte distribué
	- enregistré sous forme de JSON ou XML
	-   [**MongoDB**](https://chewbii.com/mongodb/) (_MongoDB_) : ADP, Adobe, Bosch, Cisco, eBay, Electronic Arts, Expedia, Foursquare
	-   [**CouchBase**](http://b3d.bdpedia.fr/couchbase.html) (_Apache, Hadoop_) : AOL, AT&T, Comcast, Disney, PayPal, Ryanair
	-   [**DynamoDB**](https://chewbii.com/dynamodb/) (Amazon) : BMW, Dropcam, Duolingo, Supercell, Zynga
	-   [**Cassandra**](http://chewbii.com/cassandra/) (_Facebook -> Apache_) : NY Times, eBay, Sky, Pearson Education

- **Colum** : Chaque block de stockaque contient des données d'une seule colonne.
	- On peut adresser les requêtes sur une ou plusieurs colonnes sans avoir  à traiter les informations inutiles (les autres colonnes) 
	-   **BigTable** (_Google_)
	-   **HBase** (Apache, Hadoop)
	-   **Spark SQL** (_Apache_)
	-   [**Elasticsearch**](https://chewbii.com/elasticsearch/) (_elastic_) -> moteur de recherche

- **Graph** : 
	- Database réseau qui utilise des ponts et des noeuds pour réprésenter et enregistrer les données. 
	- Langage propre (pas de SQL).
	-  Permet d'éviter les coûts de jointures et de trouver des patterns facilement entre les noeuds.(Neoj4)
	-   [**Neo4j**](https://chewbii.com/neo4j/) : eBay, Cisco, UBS, HP, TomTom, The National Geographic Society
	-   **OrientDB** (_Apache_) : Comcast, Warner Music Group, Cisco, Sky, United Nations, VErisign
	-   **FlockDB** (_Twitter_) : Twitter


/!\ noSQL avec l'intégrité des données
Dans une SGBDR : ACID
- Atomocité :  Une transaction s'effectue entièrement ou pas du tout
- Cohérence : Le contenu d'une BDD doit être cohérent au début et à la fin d'une transaction
- Isolation : Les modifications de la transaction doivent apparaître uniquement après validation
- Durabilité : Une fois la transaction validée, l'état de la BDD est paremanent (pas de panne) 

Ces propriétés ne sont pas valables pour un noSQL :
- La synchronisation pour une transaction entre cinq serveurs (lecture / écriture) = Latences. Donc on en veut pas
- Réplication des données = latence

On caractérise donc un noSQL par BASE :
- Basically Available : Quelque soit la charge de données, le système garanti l'accéssibilité
- Soft State : La base peut changer lors de MAJ ou A/S de serveurs, pas besoin de cohérence à tout instant
- Eventually Consistent : A terme, la BDD sera dans un état cohérent

En 2000 : Théorème du CAP caractériste une BDD :
- Consistency (Cohérence) : Une donnée n'a qu'un seul état visible qqs le nombre de réplicas
- Availability (Disponibilité) : Tant que le système tourne (distribué ou non) : la donnée doit-être disponible
- Partition tolerance (Distribution) : QQS le nombre de serveurs, toute requête doit fournir un résultat correct 
- On ne peut avoir que 2/3 qualités dans une BDD :
		- CA (traditionnel, Available and Consistent)
		- AP
		- CP
![](https://cdn-images-1.medium.com/max/800/1*RZ7mW4T2OSuULJu-uh2vlg.png)
## Techniques utilisés dans le noSQL 
- Arbres (Btree) : Rendre le parcours comme des index
- Hachage : Aller à la bonne rangée (structurer)
- Distrubution :Répartir pour soulager le serveur
- Elasticité : Capacité du système à s'adapter automatiquement en fonction du nombre de serveurs & quantité de données. 
- Sharding : Distributions de chunks (morceaux de fichiers) sur un ensemble de serveurs. 
	- HDFDS (distribution) : Répartition définie par un serveur central nommé "namenode". Réplication des chunks. Traitements sur les "datanode". Elasticité se fait toute seule avec
	- Clustered index (basé sur le BTREE) : serveur central agit comme un routeur, il connaît l'architecture, et fait de l'élasticité en modifiant les bonnes bornes de chaque noeud. Permet de garanir les pannes. Les noeuds s'occupent des traitements mais gère aussi la réplication des données.
	- Consistent Hashing (tables de hachage) : Chaque noeud est à la fois client et serveur. Ils se partagent les chunks comme sous forme d'un gateau.  Ainsi on gère l'élasticité en fonction de la place que prend un serveur sur le gâteau.
	- 
	
