# Environnement Hadoop et Spark sous Docker

Monter un cluster Hadoop de trois nœuds sur une seule machine, en quelques commandes, pour expérimenter sans matériel ni installation système.

L'image est publiée sur Docker Hub : **`zenitsu93/spark-hadoop:v1`**.

## Mise en route

**1. Récupérer l'image**

```sh
docker pull zenitsu93/spark-hadoop:v1
```

**2. Créer le réseau**

Les trois conteneurs doivent se voir entre eux : un réseau Docker dédié leur donne une résolution de noms interne, indispensable à Hadoop qui identifie ses nœuds par leur nom d'hôte.

```sh
docker network create --driver=bridge hadoop
```

**3. Démarrer les conteneurs**

Un nœud maître et deux nœuds de travail, attachés à ce réseau.

## Pourquoi un cluster local

Hadoop et Spark sont conçus pour le calcul réparti, et leur comportement diffère de celui d'une exécution locale : partitionnement des données, transfert entre nœuds, tolérance aux pannes. Ces mécanismes ne se manifestent que sur plusieurs nœuds.

Trois conteneurs sur un portable ne reproduisent évidemment pas les performances d'un vrai cluster — la latence réseau y est quasi nulle et le disque est partagé. Mais ils reproduisent la **logique** : c'est suffisant pour écrire un traitement MapReduce ou un travail Spark et vérifier qu'il se répartit correctement.

## Ce dépôt

Il ne contient que ces instructions : le contenu réel est dans l'image Docker publiée, avec Hadoop et Spark déjà configurés.

Les traitements exécutés sur cet environnement utilisent MrJob, qui permet d'écrire un travail MapReduce en Python et de le soumettre au cluster sans passer par Java.
