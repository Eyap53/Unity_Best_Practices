# Concevoir un code mieux structur�

Il existe plusieurs architecture logicielle permettant d'avoir une structure de code adaptable et maintenable.
La mise en place d'une bonne architecture doit permettre de : 
  - S�parer les responsabilit�s
  - Rendre le code facilement testable
  - Rendre le code facilement modifiable et maintenable dans le temps 
  - S�parer les parties facilement testable (fonctions m�tier ? stable)  et les parties difficilement testable (infrastructure ? volatile)
  
> �Architecture is not about tools and building materials, architecture is about usage.� 
> Robert C. Martin (Oncle Bob)

Une bonne architecture doit supporter :
  - Les cas d�utilisation (=l�intention) et le fonctionnement du syst�me
  - La maintenance du syst�me
  - Le d�veloppement du syst�me
  - Et le d�ploiement du syst�me
   
Elle est centr� sur les cas d�utilisation, les architectes logiciel peuvent donc d�crire les structures qui les supportent en toute s�curit� sans devenir d�pendant des frameworks, des outils ou de l�environnement.   
Le choix d�un framework (bases de donn�es, serveur web, �) ne doit pas impacter l�architecture.   
Oncle bob dit ***�les framework sont des produits commerciaux, tout est fait pour que ce soit simple � utiliser mais tout n'est pas une bonne id�e.�***

## La Clean Architecture

## ECS Architecture
