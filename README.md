# proto-federation
A federation architecture based on DAG topology (inspired by RPL protocol)

## Contexte
Prototype développé dans le cadre du stage de dernière année (stage ingénieur) en vue de la diplomation (Diplôme d'ingénieur TELECOM Nancy)

Stage effectué en partenariat avec Linagora

Inspiration : protocole RPL

Le protocole RPL est un protocole de routage utilisé dans les LLNs (Low Power and Lossy Networks). Ces réseaux de capteurs ont les caractéristiques suivantes :
* Limitation en terme de puissance
* Limitation en terme de mémoire
* Limitation en terme d'énergie

RPL a été implémenté de façon à respecter ces contraintes

La topologie utilisée est un DODAG (Destination Oriented Directed Acyclic Graph)
Le DDOAG hérite de certaines caractéristiques du DAG (Directed Acyclic Graph)

DAG :

```
r=0    O      1
      /   \  /  \
r=1  2       4    5
      \   /
r=2     3
```

DODAG :

```
r=0       O     
        /   \  
r=2   2      4    
      \   /   \
r=3     3      5
```

Différentes opérations possibles :
* One-to-many
* Many-to-one

## Motivations

Un DAG représente une entité fédérée. Dans le cas d'une fédération, il y a plusieurs entités donc plusieurs DAGs.

Une structure en DAG permet de positionner les noeuds (collaborateurs) en fonction d'un indice de confiance, de leurs droits sur le(s) document(s) et de tout autre paramètre liés de près ou de loin au système de fédération.

Seul le père est en mesure d'envoyer ou non le message à ses fils en fonction des droits que possèdent ces derniers, ainsi la maîtrise des messages est assurée et seuls les noeuds en droit de voir les messages y auront accès. De la même façon, toute modification de rang qui affecterait la position du noeud dans le réseau doit se faire par un noeud parent (qui a par conséquent un rang supérieur)

Le DAG est constitué d'une ou plusieurs racines, ces racines constituent les passerelles vers les autres entités fédérées. Ces racines ont le rang le plus faible (r = 0), elles ont donc le niveau de privilège le plus élevé. Ce qui peut être logique, car ce sont ces noeuds qui vont communiquer avec les autres entités. En fonction de la politique mise en place et du message, les racines décideront transmettre ou non l'information aux autres entités.
