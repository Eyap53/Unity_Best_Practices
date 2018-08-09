# Propret� du code avec Clean Code

> � Il existe deux fa�ons de concevoir un logiciel : une fa�on est de le rendre si simple qu'il n'a visiblement aucun d�faut, et une autre est de le rendre si complexe qu'il n'y a pas de d�faut visible. La premi�re fa�on est de loin la plus difficile. � 
> Hoare

**Qu�est-ce qu�un code propre ?** 

![Image intro](https://raw.githubusercontent.com/jaayap/Unity_Best_Practices/master/Img/wtfm.jpg)  

- Un code propre doit �tre agr�able � lire.
- Un code propre doit pr�ter attention aux d�tails et doit �tre focalis�.
- Chaque fonction, chaque classe, chaque objet doit avoir une attitude simple qui reste enti�rement ind�pendante, et non pollu�e par les d�tails non n�cessaires.
- Un code non test� n�est pas un code propre, peu importe s�il est lisible et accessible.
- Ne contient pas de duplication.
- �vident, simple et convaincant: quand on lit un code propre, on ne doit pas d�penser trop d�effort pour le comprendre.

La propret� du code d�finit la **qualit� logicielle**.

**Pourquoi avoir un code simple et propre est si important ?**

Le code est le premier **environnement de travail** du d�veloppeur. Il est donc plus agr�able pour le d�veloppeur de travailler avec une bonne compr�hension du syst�me et de pouvoir le faire �voluer sans tout r��crire. En effet un code propre permet de gagner du temps lorsqu�il faut faire �voluer le projet.  De plus, la qualit� interne du code (structure) **influe** directement sur la qualit� externe (fonctionnement) du projet. Avoir un code propre sert � **maintenir la maintenabilit�**.

Un code propre permet donc de **faire �voluer** et de **maintenir** un projet.

## Par o� commencer ? 

### Les noms

Lorsque nous d�veloppons, bien trop souvent nous oublions de r�fl�chir aux noms que nous donnons aux variables, aux m�thodes et parfois m�me aux classes.  Ce qui peut entra�ner des incompr�hensions du syst�me par d�autres d�veloppeurs et une relecture du code plus longue. Un mauvais nommage peut �galement impacter la maintenabilit� d�une application.

Lorsque vous programmer : 

- Utilisez des noms qui r�v�lent l�intention (= noms descriptifs), pour vous aider posez vous la question �� quoi va servir cette m�thode/classe/variable ? �
- Essayer de choisir qu�un seul mot par concept. Par exemple, si une classe sert � instancier un objet ne m�langez pas, �create�, �instantiate�, generate�, � Mais choisissez un seul mot qui  correspond � ce que vous faites ou le mot utilis� par le m�tier.
- Choisissez des mots facile � prononcer.
- Evitez de placer des valeurs sans les nommer (magic number)

### Les commentaires

Dans un code, les commentaires sont importants mais parfois utilis�s de mani�re abusives (�crire un commentaire pour expliquer tout le fonctionnement d�un algorithme plut�t que de le r��crire de mani�re simple et compr�hensible). 

Les bons commentaires sont des commentaires facultatifs, si possible, la compr�hension du code ne doit pas �tre d�pendante des commentaires. Ils doivent servir de compl�ment d�information, d�avertissement sur les cons�quences ou � expliquer une intention.

### La loi de D�m�ter

La loi de D�m�ter (en anglais Law of Demeter ou LoD) peut s��noncer de la mani�re suivante :
�Chaque unit� doit avoir une connaissance limit�e des autres unit�s, chaque unit� doit voir que les unit�s �troitement li�es � l�unit� actuelle.�

Cela signifie qu�un objet devrait faire le moins d�hypoth�se possible � propos des autres structures que lui m�me. 

En pratique, cela veut dire qu�une m�thode peut invoquer les m�thodes :
- de l�objet lui-m�me
- des param�tres de la m�thode
- des objets cr��s / instanci�es dans la m�thode
- des propri�t�s et champs de l�objet 

L'avantage de suivre la r�gle de D�m�ter est que le logiciel est plus maintenable et adaptable. Puisque les objets sont moins d�pendants de la structure interne des autres objets, ceux-ci peuvent �tre chang�s sans changer le code de leurs appelants.

**Exemple**

**Code qui ne respecte pas la loi de D�m�ter**

```cs
public class VideoService {
    public IVideoRepository videoRepository;
    
    public void Play(string videoId) {
        �
        var video = this.videoRepository.GetVideo(videoId);
        var format = video.GetFormat();
        var codec = format.GetCodec();
        codec.DecodeVideo(video.GetContent());
    }
}
```
**Refacto de ce code pour qu'il respecte la loi de D�m�ter**

```cs
public class VideoService {
    public IVideoRepository videoRepository;
    
    public void Play(string videoId) {
        �
        var video = this.videoRepository.GetVideo(videoId);
        var codec = video.GetCodec();
        DecodeVideo(video, codec);
    }

    private void DecodeVideo(Video video, Codec codec) {
        codec.DecodeVideo(video.GetContent());
    }
}

public class Video {

    public void GetCodec() {
        return this.GetFormat().GetCodec();
    }
}
```


## Principes et m�thodes

Depuis les ann�es 1990/2000, plusieurs m�thodes et principes ont �t� d�finis pour apporter une ligne directrice dans le d�veloppement de logiciel plus fiable et plus robuste. Parmis eux, beaucoup proviennent des pratiques eXtreme Programming et Clean Code. Ces pratiques sont populaires aujourd�hui car elles r�pondent aux questions soulev� par l�agilit�.

Ainsi, les r�gles d�un design simple sont d�finis par *Kent Beck* en 1990 dans son livre *�Extreme Programming Explained�*, ces r�gles sont les suivantes : 

1. les tests passent (signifie que le code fonctionne correctement et sous entend que le code doit �tre couvert par les tests)
2. L�intention du code est claire
3. **DRY** principle (Don�t Repeat Yourself) :      
       Il n�y a pas de duplication de code
4. Le code a le moins d��l�ments possible :
    - **KISS : Keep it Simple & Stupid**             
                Principe de la responsabilit� unique (un �l�ment poss�de qu'une seule raison d'�tre modifi�)
    - **YAGNI  : You ain�t gonna need it**            
                Essayer d'anticiper les probl�mes futurs, n'est pas une bonnes id�e. Il est pr�f�rable de s'occuper du pr�sent et de ne pas faire d'hypoth�se sur ce que l'application pourrait utiliser.


Quelques ann�e plus tard, *Robert Cecil Martin alias Oncle Bob* met en avant plusieurs grands principes : Les principes *SOLID* et les principes composants. 

**Les principes SOLID sont d�finis comme suit :**

- **<mark>S</mark>RP (Single Responsibility Principle)**      
Ne faire qu�une seule chose mais la faire bien. Une classe doit avoir une et une seule raison de changer. 

- **<mark>O</mark>CP (Open-Closed Principle)**      
Une classe doit �tre ouverte � l'extension mais ferm�e � la modification (plugins)

- **<mark>L</mark>SP (Liskov Substitution Principle)**     
Toutes impl�mentation d�une interface doit pouvoir se substituer � une autre

- **<mark>I</mark>SP (Interface Segregation Principle)**    
Une classe ne doit impl�menter une interface que si elle � r�ellement le besoin de 
remplir son contrat 

- **<mark>D</mark>IP (Dependency Inversion Principle)**     
Le code ne doit pas interagir directement avec l'ext�rieur mais doit passer par des 
abstractions (et vice versa).


**Les trois principes composants**

Les composants sont les unit�s du d�ploiement. Ils sont les plus petits morceaux qui peuvent �tre d�ploy�s comme une partie du syst�me (fichier .jar / fichier .gem/ .DLL). 

Les trois principes sont les suivants : 

- **REP (Reuse/Release Equivalence Principle)**    
Cr�er un code qui peut �tre r�utilis� (plugin, code g�n�rique avec le moins de d�pendance possible)
�the granule of reuse is the granule of release� 
"Le germe de la r�utilisation est le germe de la lib�ration"

- **CCP (Common Closure Principle)**     
Un composant (une classe, une m�thode, ...)  ne doit pas avoir plusieurs raisons de changer

- **CRP (Common Reuse Principle)**        
�Don�t force users of a component to depend on things they don�t need.� 
�Ne forcez pas les utilisateurs d�un composant � d�pendre d�une chose dont ils n�ont pas besoin� 
