
# Tests unitaires & TDD

## Tour d'horizon sur les tests
  
Les tests sont importants dans la r�alisation d�un logiciel car ils assurent une **qualit� logicielle minimale**. 
Beaucoup de logiciels sont con�us sans test et les cons�quences sont:  
- beaucoup d�**anomalies**
- des **bugs** compliqu�s � r�soudre et parfois lanc�s en production.
- un logiciel **qui grossit** � cause des **patchs** et des nouvelles fonctionnalit�s s�appuyant sur un code bancal. 
En bref, un code **non test�** entra�ne bien souvent une **dette technique**.

![Image de la pyramide des tests](https://raw.githubusercontent.com/jaayap/Unity_Best_Practices/master/Img/img_pyramide_des_tests.png?token=AHjeDoKKyxTJRlb2sRXe6a4YPP5RwJQTks5bYCFzwA%3D%3D)

La **pyramide des tests** pr�sente les diff�rents **types** de tests :

- Les **tests IHM** (Interface Homme-Machine) co�tent cher car il faut ex�cuter l�appli de bout en bout. Ces tests peuvent �tre automatis�s mais long � �tre ex�cut�s. Ils ne couvrent que certains sc�narios pr�cis. Quand ces tests ne sont pas automatis�s, on peut engager des testeurs.

- Les **tests d�int�gration** sont assez simples. On s�affranchit des contraintes majeures (IHM, certaines d�pendances). Ils sont �galement assez proches du code, ce qui facilite leur refactoring. Cependant, ils couvrent un spectre plus large de code et de ce fait, ont davantage de risques d��tre impact�s par une modification.

- Les **tests unitaires** sont simples � mettre en �uvre lorsqu�ils sont faits au fur et � mesure du code, ils permettent de couvrir une majeure partie du code � faible co�t. Ils peuvent �tre **automatis�s** et sont **rapides � ex�cuter**. Il est fortement recommand� de commencer par r�diger des tests unitaires, ils sont la **base** des tests et permettent de rep�rer rapidement les r�gressions ou les mauvais comportements du logiciel.

:warning: **Attention** : Il faut s�parer les tests unitaires (rapide) des tests d�int�grations (plus lent).

Pour le moment nous nous int�resserons aux **tests unitaires et au TDD (test-driven development)**.

**R�diger des tests unitaires sert � v�rifier le comportement du code, tandis qu�avec l�approche TDD, qui consiste � �crire les tests en premier, r�diger des tests unitaires sert � sp�cifier comment le code doit fonctionner**.

Pour en savoir plus sur les tests, je vous conseille [l'article sur la pyramide des tests du blog d'OCTO Technology](https://blog.octo.com/la-pyramide-des-tests-par-la-pratique-1-5) , [ou une de leurs publications, Culture Code](https://www.octo.com/fr/publications/20-culture-code).

## Tests Unitaires (TU)

Les **tests unitaires** peuvent �tre regroup�s dans un **projet de tests**.  
Ce dernier doit avoir la m�me **structure** que le projet que l�on souhaite tester. Ainsi, **Une classe de l�application = Une classe de test**.

### Pourquoi �crire un test unitaire ?

- �tre s�r de ne **pas** faire de **retour en arri�re**, de ne pas avoir cass� un code en le modifiant avant la mise en production

-  **R�duire les bugs** dans les fonctionnalit�s (d�j� impl�ment�es ou nouvelles)

- **Refacto en toute qui�tude** : ils r�duisent la peur de faire des **modifications** et d�**ajouter de nouvelles fonctionnalit�s** dans le programme ou l�application.

- Traiter **en amont** des tests d'interface et des utilisateurs certains mauvais comportements et donc de les **corriger plus t�t**.

- Avoir un **feedback rapide** sur le code que les developpeurs viennent d'ajouter ou modifier.

- Permet d�avoir une application plus **robuste**

- Avoir une documentation minimale pour les d�veloppeurs

En conclusion, plus rapidement vous saurez si un test �choue, plus vite vous pourrez corriger le probl�me et moins cher ce sera.

### Qu'est-ce qu'un bon test unitaire ?

Un bon test unitaire :
- est **enti�rement automatis�**
- retourne **toujours le m�me r�sultat** si le code n�est pas modifi�
- test qu�**un seul concept** ou **une seule logique** de l�application
- test qu�**une seule m�thode � la fois**
- porte un **nom clair et significatif**

### Comment �crire un test unitaire ? 

Diff�rentes conventions de nommage existent :
- Should_ExpectedBehavior_When_StateUnderTest
- When_StateUnderTest_Expect_ExpectedBehavior

Attention � **ne pas avoir le nom de la m�thode dans le nom du test** car difficile � maintenir. En cas de refactorisation du nom de la m�thode, le test ne veut plus rien dire.
Un test unitaire exprime une intention, c'est � dire, qu'il test une fonctionnalit�, et non son impl�mentation.

Un test se d�compose en **3 parties** : 
- **Arrange** est l��tape d�initialisation
- **Act** correspond � l�appel de la m�thode test�e
- **Assert** est l��tape de v�rification du comportement de la m�thode test�e

Lorsque l'on �crit un test unitaire, il est conseill� de commencer par r�diger l'**Assert**, qui est la r�ponse � la question : **Qu'est-ce qu'on veut tester?**

### Comment mettre en pratique les TU avec Unity ?

#### Unity Test Runner

Pour r�aliser des tests unitaires, Unity met � disposition un outil appel� le [**Unity Test Runner**](https://docs.unity3d.com/Manual/testing-editortestsrunner.html)  
Pour afficher la fen�tre *"Test Runner"*, allez dans *Windows > Test Runner*.

![Image of Test Runner](https://raw.githubusercontent.com/jaayap/Unity_Best_Practices/master/Img/UnityTestsRunner/Capture1_ouvertureOnglet.PNG)

On peut voir sur la capture d'�cran, qu'il existe **deux modes** :

- **PlayMode** :
  - permet d'executer des tests sur **plusieurs frames**
  - comportement Awake(), Start(), ... sont execut�s **automatiquement**
  - Sert davantage pour les **tests d'int�gration**
  - [UnityTest] = ex�cut� comme une **Coroutine classique**
  - Ouvre une **sc�ne** de test pour execut� les tests (:warning: pensez � bien **enregistrer** votre sc�ne avant de lancer les tests car votre sc�ne sera �cras�e au lancement des tests)

- **EditMode** :
  - Chaque test est execut� en **une frame**
  - Il faut appeler **explicitement** les m�thodes Awake() et Start(), ce qui necessite de les passer en **public**.
  - **Doit �tre plac�s dans un dossier Editor**
  - [UnityTest] est execut� dans l'editor avec **"Editor.Application.Update"**

S�lectionnez le mode qui vous int�resse puis cliquez sur le bouton *�Create PlayMode/EditMode Test Assembly Folder�*. 
*Unity* va cr�er automatiquement un dossier *Test* avec un fichier *�Tests.asmdef�de type �assembly definition�*.  
  
Une fois le dossier cr��, placez-vous � l�int�rieur puis cliquez sur *�Create Test Script in current folder�*.

![Image of Test Runner create script](https://raw.githubusercontent.com/jaayap/Unity_Best_Practices/master/Img/UnityTestsRunner/Capture2_btn_create_script.PNG)

Unity va alors cr�er un fichier *template C#* avec le code suivant :

```cs
using UnityEngine;
using UnityEngine.TestTools;
using NUnit.Framework;
using System.Collections;

public class NewTestScript {

    [Test]
    public void NewTestScriptSimplePasses() {
        // Use the Assert class to test conditions.
    }

    // A UnityTest behaves like a coroutine in PlayMode
    // and allows you to yield null to skip a frame in EditMode
    [UnityTest]
    public IEnumerator NewTestScriptWithEnumeratorPasses() {
        // Use the Assert class to test conditions.
        // yield to skip a frame
        yield return null;
    }
}
```

Ces deux tests devraient appara�tre dans le Test Runner :

![Image of Test Runner tests with test](https://raw.githubusercontent.com/jaayap/Unity_Best_Practices/master/Img/UnityTestsRunner/Capture3_tests.PNG)

Si vous cliquez sur **�Run All�**, les tests vont se lancer, vu que ces tests n�ont pas d�assertion, les tests devrait passer au **vert** directement. 

![Image of Test Runner tests green tests](https://raw.githubusercontent.com/jaayap/Unity_Best_Practices/master/Img/UnityTestsRunner/Capture4_green_tests.PNG)

Maintenant, si nous rempla�ons le premier test par celui ci :

```cs
    [Test]
    public void NewTestScriptSimplePasses() {
        // Use the Assert class to test conditions.
        Assert.AreEqual(4, 3);
    }
```

le test devrait �tre **rouge** car 3 et 4 ne sont pas �gaux :

![Image of Test Runner tests red test](https://raw.githubusercontent.com/jaayap/Unity_Best_Practices/master/Img/UnityTestsRunner/Capture5_red_tests.PNG)

Si vous cliquez sur le test en question, vous pouvez voir un message d�erreur et pourquoi le test a �chou� (dans l�exemple : Expected : 4, But was : 3) :

![Image of Test Runner tests red test explanation](https://raw.githubusercontent.com/jaayap/Unity_Best_Practices/master/Img/UnityTestsRunner/Capture5_red_tests_with_error_msg.PNG)

Aujourd'hui, *Unity* ne permet pas de lancer les tests en lignes de commande.
Les tests appelant des *MonoBehaviour* ne peuvent pas �tre �x�cut� depuis [*Visual Studio*](https://visualstudio.microsoft.com/).  
Si vous utilisez [***Rider***](https://www.jetbrains.com/dotnet/promo/unity/), vous pourrer les lancer directement dans son interface.

#### Les Scripts de tests

**Le nom de la classe de test doit avoir en pr�fixe ou en suffixe *�Test�*, selon vos pr�f�rences.**  
Veillez � ce que votre code soit harmonis�, ainsi si vous choisissez de mettre �Test� en suffixe, faites le sur toutes vos classes de tests.  

Par d�faut Unity inclus le framework de test unitaire C#  **[NUnit](https://nunit.org/)** pour les tests unitaires.
Devant chaque m�thode de test l�attribut **[Test]** doit appara�tre :

```cs
[Test]
public void Methode_Test() {
 
    ...
 
}
```

Avec le **using UnityEngine.TestTools;**, vous pouvez cr�er des **"Unity Tests"** :

```cs
[UnityTest]
public IEnumerator Methode_Unity_Test() {
 
    ...
    yield return null;
    ...
 
}
```

Un *Unity Test* est une [**Coroutine**](https://docs.unity3d.com/Manual/Coroutines.html). Dans *Unity*, les *Coroutines* sont g�n�ralement utilis� pour g�rer le rendu. Nous pouvons par exemple, **attendre une frame** (*yield return null*) ou un **nombre de secondes x** (*yield return new WaitForSecondes(x)*) pour effectuer une action.

Ces tests permettent donc de tester les comportements qui d�pendent d'*Unity*. Ils s'apparentent le plus souvent � des **tests d'int�gration**

Pour r�aliser une assertion avec un test *NUnit*, il faut utiliser la classe **Assert**, quelques exemples :

```cs
    Assert.IsNotNull(object);
    Assert.IsTrue(boolean);
    Assert.AreEqual(value1, value2)
```

**Dans le cas de *Asset.AreEqual*, la value1 correspond � la valeur attendue est la value2 est la valeur test�e. Cet ordre est important et permet notamment d'avoir des messages d'erreurs coh�rents :  *Expected "value1" but was "value2"*.**

*Unity* ajoute �galement ses **propres assertions** (avec using UnityEngine.Assertions et UnityEngine.TestTools.Utils) comme :
- *"Assert.AreApproximatelyEqual"* et *"Assert.AreNotApproximatelyEqual"* qui prennent par d�faut une tol�rance de  0.00001f qui permettent de comparer deux floats.
- *"ColorEqualityComparer"*
- *"QuaternionEqualityComparer"*
- *"Vector2EqualityComparer"* ,*"Vector3EqualityComparer"* et *"Vector4EqualityComparer"*


:warning: Les m�thode *Awake*, *Start* et *Update* doivent �tre pass� en public pour �tre appel�e avec les tests.

Si tous vos tests ont le m�me *"Arrange"* vous pouvez utilisez les attributs [SetUp] et [TearDown].
La m�thode sous l'attribut [SetUp] s'executera avant toutes les m�thodes [Tests] et la m�thode sous l'attribut [TearDown] s'executera apr�s  toutes les m�thodes [Tests].

Ainsi : 
```cs
public class MaClassTest {
  [Test]
  public void Methode_Test_0() {
    //Arrange
    GameManager game = new GameManager();
    ...
    //Act
    ...
    //Assert
    ...
  }

  [Test]
  public void Methode_Test_1() {
    //Arrange
    GameManager game = new GameManager();
    ...
    //Act
    ...
    //Assert
    ...
  }
}
```
Devient : 
```cs
public class MaClassTest {
  
  GameManager game;
  
  [SetUp]
  public void Init(){
    game = new GameManager();
  }
  
  [TearDown]
  public void Dispose() {
    Destroy(game);
  }

  [Test]
  public void Methode_Test_0() {
    //Arrange
    ...
    //Act
    ...
    //Assert
    ...
  }

  [Test]
  public void Methode_Test_1() {
    //Arrange
    ...
    //Act
    ...
    //Assert
    ...
  }

```


Source : Unite Austin 2017 - Testing for Sanity: Using Unity's Integrated TestRunner, https://www.youtube.com/watch?v=MWS4aSO7HAo




## D�veloppement pilot� par les tests (TDD)

Le **TDD ou D�veloppement pilot� par les tests** est une technique de d�veloppement qui consiste � �crire les tests **avant** d��crire le code. Cela garantit davantage la structure du code comme testable et maintenable.
En utilisant le *TDD*, les tests unitaires ne servent plus � valider un code existant mais � **sp�cifier** le futur code impl�ment�.

![schema cycle TDD](https://raw.githubusercontent.com/jaayap/Unity_Best_Practices/master/Img/RED-GREEN-REFACTO%20cycle.png?token=AHjeDtvHOiXwwhctewLgG-4a0KSUEjf7ks5bYXxlwA%3D%3D)

Comme nous pouvons le voir sur le sch�ma ci-dessus, le *TDD* poss�de **3** grandes �tapes : 
- **RED** : On commence par �crire un test et on v�rifie que ce dernier �choue (car le code n'est pas impl�ment�). Ce test sp�cifie le comportement d'une m�thode (ce qu'elle doit renvoyer, ce qu'elle doit appeler, ...).
- **GREEN** : On �crit le code minimum pour que le test passe au vert.
- **REFACTOR** : On am�liore le code sans changer son comportement.

Puis on passe � l'�criture d'un autre test, et la boucle recommence.

Dans le livre *Clean Code (p.80)*, Oncle Bob d�finit les 3 lois du *TDD* :

   1. Vous n'�tes pas autoris� � �crire du code m�tier tant que vous n'avez pas �crit un premier test unitaire qui �choue.
   2. Vous n'�tes pas autoris� � �crire plus qu'un test unitaire qui est suffisant pour �chouer et qui ne compile pas.
   3. Vous n'�tes pas autoris� � �crire plus de code que ce qui est suffisant pour passer au vert le test unitaire.
  
:warning: *Oncle Bob*, insiste sur le fait, qu'�crire des tests unitaires ou pratiquer le *TDD* n'est pas magique, et qu'on peut tr�s bien continuer d'�crire du mauvais code et d'�crire des mauvais tests.
Il pr�cise �galement que suivre les trois lois n'est pas toujours approppri�, il faut donc trouver des solutions adapt�s � notre projet.


TODO : exercice � trou (suite ou reprise du m�me exo) TU avec TDD. 