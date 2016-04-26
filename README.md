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

(plusieurs racines)

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

Hypothèse simplificatrice :
* Utilisation d'un DODAG et non d'un DAG

Un DODAG représente une entité fédérée. Dans le cas d'une fédération, il y a plusieurs entités donc plusieurs DAGs.

Une structure en DODAG permet de positionner les noeuds (collaborateurs) en fonction d'un indice de confiance, de leurs droits sur le(s) document(s) et de tout autre paramètre liés de près ou de loin au système de fédération.

Seul le père est en mesure d'envoyer ou non le message à ses fils en fonction des droits que possèdent ces derniers, ainsi la maîtrise des messages est assurée et seuls les noeuds en droit de voir les messages y auront accès. De la même façon, toute modification de rang qui affecterait la position du noeud dans le réseau doit se faire par un noeud parent (qui a par conséquent un rang supérieur)

Le DODAG est constitué d'une ou plusieurs racines, ces racines constituent les passerelles vers les autres entités fédérées. Ces racines ont le rang le plus faible (r = 0), elles ont donc le niveau de privilège le plus élevé. Ce qui peut être logique, car ce sont ces noeuds qui vont communiquer avec les autres entités. En fonction de la politique mise en place et du message, les racines décideront de transmettre ou non l'information aux autres entités. Les racines réceptionneront ainsi les messages provenant des autres entités et les transmettrons aux noeuds fils (si ils sont en droit de le recevoir), et ainsi de suite, les noeuds (autorisés) recevront le message.

## Méchanisemes et cas d'utilisation

### Cas d'utilisation

Trois Ministères cherchent à collaborer sur un document officiel ...

* Il y aura trois DAGs distincts représentant chacun un ministère
* Le noeud racine peut-être un robot en permanence connecté (ça peut aussi être un collaborateur, initiateur du projet par exemple ou aillant un niveau de provollège assez élevé)
* Le rang d'un noeud (collaborateur) peut se calculer suivant différentes métriques :
  * La confiance
  * La fiabilité
  * Le taux de disponibilité
  * Des métriques réseau :
    * ping
    * ...
  * Les privilèges accordés
  * Un rôle (hiérarchie)
  * ...

### Construction du réseau

Étapes de construction d'un réseau en DODAG :
1. Désignation d'une racine (plusieurs si c'est un DAG)
2. Envoi d'un message de contrôle vers tous les autres noeuds permettant de fournir les éléments
3. Calcul du rang sur chaque noeud ...
4. ... une fois le calcul terminé les noeuds remontent le rang à la racine
5. La racine indique au noeud de rang le plus élevé de contacter les noeuds de rang immédiatement inférieur
6. (possibilité d'établir les liens père-fils en fonction des affinités : réseau, rôle)
7. Propagation des liens :
  * À la réception de la demande de la part d'un fils, le père envoie (si ce n'est pas déjà fait ) une demande aux noeuds de rang directement inférieur. Et ainsi de suite ...   

### Résillience, reconstruction et mises à jour des rangs
TODO...

### Communication entre plusieurs entités de fédération  
TODO...

### Architecure en couche

À terme, l'idée est de pouvoir intégrer une librairie de fédération au-dessus de celle en charge du transport. La topologie du réseau est celle de la fédération sont indépendantes (mais communiquent entre elles).

Architecture en couches :
```
\               \
 \               \
  \  Application  \
   \_______________\

  \               \
   \               \
    \    Lib fédé   \
     \_______________\

   \               \
    \               \
     \  Transport    \
      \_______________\
```
