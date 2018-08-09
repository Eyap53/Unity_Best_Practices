 Gestion des d�pendances

## Les d�pendances dans les tests unitaires 

Dans la pratique, un TU ne doit jamais s�appuyer sur **une d�pendance ext�rieure** (service web, base de donn�es, librairie, �). En effet, cela peut entra�ner un **biais** et peut rendre les TU plus difficiles � maintenir dans le temps.  
Une d�pendance ext�rieure aura tendance � **rallonger le temps d'ex�cution**.

Pour �tre s�r de tester un seul comportement, sans aucune d�pendance, il est possible de cr�er **des bouchons (mock ou stub)** pour remplacer une classe.
Cela permet d��viter un null exception en renseignant un objet de substitution, ou encore de v�rifier qu�une fonction est bien appel�e sans ex�cuter son code.

**Quelle est la diff�rence entre Stub et Mock ?**

**Un Stub** est un objet que vous pouvez contr�ler, qui sert de substitut **� une d�pendance ext�rieure** (exemple : un faux service web qui renvoie toujours la m�me information)  
  
**Un Mock** est un objet de substitut **dans le syst�me**, qui d�cide si un test unitaire passe ou �choue (exemple : une base de donn�es qui attend une certaine requ�te bien pr�cise)

## TU avec d�pendances dans Unity

Pour cr�er des mocks ou des substituts avec *Unity*, vous pouvez installer **NSubstitute**, pour cela il suffit de placer le fichier **NSubstitute.dll** dans votre projet *Unity*. Vous pouvez t�l�charger ce fichier [ici](https://github.com/jaayap/Unity_Best_Practices/tree/master/NSubstitute/dll). 

**Exemple :**
 On veut s�assurer que la m�thode *InitializeObject()* du *GameManager.cs* est appel�e avec le bon param�tre lors du *Start()* du script *ObjectBehaviour.cs*

```cs
[Test]
public void WhenGameStartObjectInitialize()
{
    //Arrange
    ObjectBehaviour object = new GameObject().AddComponent<ObjectBehaviour>();
    GameManager gameManagerMock = Substitute.For<GameManager>();
    object .GameManager = gameManagerMock;

    //Act
    object.Start();

    //Assert
    gameManagerMock.Received().InitializeObject(object);
}
```

## Injection de d�pendances 

D de SOLID (voir partie suivante)
 => Zenject