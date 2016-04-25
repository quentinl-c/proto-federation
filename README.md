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

## Motivations
