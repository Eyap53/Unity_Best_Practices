### [Consult this site in english](https://jaayap.github.io/Unity_Best_Practices/)

Ce guide a pour but de rassembler des informations sur les bonnes pratiques de d�veloppement (versionning, tests, architecture, ...) et de montrer comment les appliquer dans un projet Unity.  
  
La mise en place des bonnes pratiques a pour but de g�n�rer des projets plus facilement maintenable et adaptable.  
  
Si vous n�avez jamais utilis� les bonnes pratiques de d�veloppement, il faudra les apprendre et les pratiquer pour les ma�triser.

# __Plan :__

- [Versionning : Git & Unity](Versionning.md/#versionning--git--unity)
  - [Cr�er un .gitignore](Versionning.md/#cr�er-un-gitignore)
  - [Enregistrer des fichiers en YAML](Versionning.md/#enregistrer-des-fichiers-en-yaml)
  - [Merger avec l'outil Smart Merge](Versionning.md/#merger-avec-loutil-smart-merge)
  - [G�rer les fichiers volumineux (Versionning.md/images, objets 3D, ...)](#g�rer-les-fichiers-volumineux)
        
- [Tests unitaires & TDD](#tests-unitaires--tdd)
  - [Tour d'horizon sur les tests](#tour-dhorizon-sur-les-tests)
  - [Tests Unitaires (TU)](#tests-unitaires-tu)
      - [Pourquoi �crire un TU ?](#pourquoi-�crire-un-test-unitaire-)
      - [Qu'est-ce qu'un bon TU ?](#quest-ce-quun-bon-test-unitaire-)
      - [Comment �crire un TU ?](#comment-�crire-un-test-unitaire-)
      - [Comment mettre en pratique Les TU avec Unity ?](#comment-mettre-en-pratique-les-tu-avec-unity-)
        - [Unity Test Runner](#unity-test-runner)
        - [Les scripts de tests](#les-scripts-de-tests)   
  - [d�veloppement pilot� par les tests (TDD)](#d�veloppement-pilot�-par-les-tests-tdd)

- [Gestion des d�pendances **(en cours)**](#gestion-des-d�pendances)
  - [Les d�pendances dans les TU  **(en cours)**](#les-d�pendances-dans-les-tests-unitaires) 
  - [TU avec d�pendances dans Unity **(en cours)**](#tu-avec-d�pendances-dans-unity)
  - [Injection de d�pendances  **(prochainement)**](#injection-de-d�pendances)
  
- [Propret� du code avec Clean Code **(en cours)**](#propret�-du-code-avec-clean-code)
  - [Par o� commencer ?](#par-o�-commencer-)
    - [Les noms](#les-noms)
    - [Les commentaires](#les-commentaires)
    - [La loi de D�m�ter](#la-loi-de-d�m�ter)
  - [Principes et m�thodes](#principes-et-m�thodes)
  
- [Concevoir un code mieux structur� **(prochainement)**](#concevoir-un-code-mieux-structur�)
  - [Un code facilement �volutif gr�ce � la clean architecture **(prochainement)**](#la-clean-architecture)
  - [ECS architecture **(prochainement)**](#ecs-architecture)

- [A propos **(prochainement)**](#a-propos)
- [Licence](#licence)




# A propos
# Licence

Licence MIT