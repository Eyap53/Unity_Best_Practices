# Versionning : Git & Unity

Si vous utiliser *git* pour versionner vos projets *Unity*, vous avez du remarquez que parfois les fichiers tels que les sc�nes et les *prefabs* subissent des r�gressions lors des merges. Deux personnes modifiant ainsi le m�me fichier en m�me temps provoquent un conflit que git, par d�faut n�arrive pas � r�soudre. Vous retrouvez alors votre sc�ne enti�rement vide ou votre *prefab* tout cass�. 
Voici la solution que j�ai choisi d�adopter.

## Cr�er un .gitignore

Il est important d�ajouter un fichier **.gitignore** � votre d�p�t. Il permettra � git d�ignorer certains fichiers g�n�r�s par *Unity* et par votre IDE (dans l�exemple Visual Studio, mais vous pouvez modifier le *gitignore* pour l�adapter aux outils que vous utilisez). Dans certains projets, il est pertinent d�ajouter des dossiers de votre projet afin de ne pas les enregistrer dans le d�p�t en ligne, un *asset* de l�*asset store* par exemple (peut peser lourd dans l�archive, ce qui ralenti consid�rablement l��quipe lorsqu�elle doit mettre � jour le d�p�t ou le r�cup�rer).

[voir d�pot .gitignore Unity](https://github.com/github/gitignore/blob/master/Unity.gitignore)

## Enregistrer des fichiers en YAML

![Image of Unity Project Settings](https://s3.amazonaws.com/gamasutra/UnityVersionControlSettings.png)

Passer le mode d�enregistrement des assets en �Force Text� (voir image ci-dessus). Vos fichiers s�enregistrent maintenant au format YAML.
Cela permet � Git de mieux g�rer les conflits, mais cela peut s'av�rer insuffisant.

## Merger avec l�outil Smart Merge

Depuis la version 5.0 de *Unity*, un **UnityYAMLMerge tool** est fourni avec son installation. Pour l�utiliser, il suffit de copier/ coller les lignes suivantes dans le fichier **.git/config** :

```
[merge]
tool = unityyamlmerge
[mergetool "unityyamlmerge"]
cmd = 'C:\\Program Files\\Unity\\Editor\\Data\\Tools\\UnityYAMLMerge.exe' merge -p "$BASE" "$REMOTE" "$LOCAL" "$MERGED"
trustExitCode = false
keepTempories = true
keepBackup = false
```

Pensez � modifier le lien "*C:\\ ...*" selon l'emplacement de votre installation Unity, Si vous travaillez en �quipe, il est pr�f�rable d'utilisez le m�me lien.  
:warning: **Attention : n'oublier pas de mettre des doubles '\\\\' et non des simples '\\' ou '/'.**  
*[Ce merge tool peut �tre utilis� avec d�autres outils (Perforce, SVN, Mercurial, et SourceTree) , je vous conseille de vous r�f�rez � la documentation](https://docs.unity3d.com/Manual/SmartMerge.html)*  
  
Cet outil est tr�s performant et permet de travailler � plusieurs sans se soucier de tout perdre (fonctionne avec les sc�nes et les prefabs).

### Comment �a marche ?

Continuez � utiliser *git* comme � votre habitude et si lors d�un *merge*, vous avez un conflit, effectuez la commande *�git mergetool�* pour appeller le *SmartMerge* qui s�occupera, dans la plupart des cas, de r�gler les conflits. Un exemple est donn� ci-dessous, deux branches sont cr��es, *�Master�* et *�Another�*, Chacune modifie la sc�ne *�sceneTest�*. Ensuite chacune est *commit* sur le d�p�t et on essaye de �fusionner� les branches.  
A ce moment l�, git nous dit qu�il y a un conflit et lorsque l�on ouvre la sc�ne dans *Unity*, on s'aper�oit qu'elle est vide. On ex�cute alors *git mergetool* et on retrouve une sc�ne qui poss�de tous nos objets.  

![Schema explicatif du Smart Merge](https://raw.githubusercontent.com/jaayap/Unity_Best_Practices/master/Img/schemaSmartMergeV2.PNG?token=AHjeDgISOj3_CsXFl6gK4QxKd5kcyGPBks5baXFawA%3D%3D)

Le *SmartMerge* poss�de ses limites. Il ne peut pas r�soudre seul le conflit lorsque vous toucher au m�me objet d'une sc�ne. Dans ce cas, il vous demandera de choisir la bonne version (la sc�ne actuelle, l�ancienne sc�ne ou celle de votre coll�gue ?)

## G�rer les fichiers volumineux

  Versionner des fichiers volumineux (image, objets 3D) peut s'av�rer douloureux, *GitHub* vous emp�che de pousser des fichiers de plus de 100 Mo et un d�p�t *Git* contient toutes les versions de chaque fichier.  
  Les r�visions multiples de fichiers volumineux augmentent les temps de clonage et de r�cup�ration pour les autres utilisateurs du d�p�t.  
  
  *Git* demande � chaque utilisateur d'avoir autant d'espace libre sur un disque dur que d'espace consomm� � tout moment. Par exemple, si une archive est de 1 Go, Git n�cessite de 1 Go d'espace libre suppl�mentaire pour �tre disponible.  
  
  Il est conseill� de garder les fichiers suivant dans le d�p�t : fichiers de code, assets qui ont besoin d��tre versionn� (graphiques), les fichiers de configuration (volumineux ou non) et de ne pas versionner les fichiers suivants : Bases de donn�es, fichiers temporaires (journaux, log, �).   
  
  Si le d�p�t que vous utilisez est compatible avec [Git LFS](https://git-lfs.github.com/), je vous recommande de l�utiliser. ([Git LFS](https://git-lfs.github.com/) est compatible avec GitHub / Bitbucket / Gitlab depuis la version 8). [Git LFS est une extension de Git et est open source](https://github.com/git-lfs/git-lfs?utm_source=gitlfs_site&utm_medium=repo_link&utm_campaign=gitlfs).  

Cependant, cette solution fonctionne **seulement** pour les fichiers **inf�rieurs � 2Go**.

### Comment �a marche ?

![Image Explicative de Git LFS](https://raw.githubusercontent.com/jaayap/Unity_Best_Practices/master/Img/image9.png?token=AHjeDpsdDG7nmpaEzQkxpTVyQp2cHdTmks5bWG9nwA%3D%3D)

[Git LFS](https://git-lfs.github.com/), va *tracker* les fichiers que vous voulez, soit avec leurs noms, soit avec leurs extensions, soit avec leurs emplacements. Ensuite, ces fichiers seront automatiquement stock�s sur un serveur (*Large File Storage*) et seulement le lien de votre fichier sera gard�. Attention, ce syst�me ne versionne pas les fichiers track�s. Mais avez-vous vraiment besoin d�un *versionning* de vos *assets* 3D ou de vos �l�ments UI ? G�n�ralement d�j� versionn�s en amont par les Designers.

**Pour installer git LFS, lancer la commande :**
```sh
git lfs install
```
Git LFS  devrait s�installer automatiquement, si c�est le cas, la commande devrait retourner ce message :
```sh
>Updated git hooks.
>Git LFS initialized.
```
**Utilisez la commande suivante pour tracker les fichiers avec les extensions voulu (remplacer le *.extension par *.png, *.fbx, ...)**

```sh
git lfs track "*.extension" 
>Tracking "*.extension"
```

**Assurez vous que votre projet poss�de un *.gitattributes***

```sh
git add .gitattributes
```
et voil� le tour est jou�, � pr�sent vous pouvez ajouter vos fichiers et *commit* comme vous le faisiez avant, la diff�rence est que *Git* va envoyer les fichiers track�s sur [Git LFS](https://git-lfs.github.com/).  

Si vous utilisez github, la notification ***�stored with Git LFS�*** devrait appara�tre dans l�interface web lorsque vous ouvrez votre fichier.

![Image of Github Stored with Git LFS](https://raw.githubusercontent.com/jaayap/Unity_Best_Practices/master/Img/image7.png?token=AHjeDnlpMeQpTc27ikncn_j53g50GMEOks5bWG4gwA%3D%3D)

Pour comprendre le fonctionnement de [Git LFS](https://git-lfs.github.com/), je vous conseille de regarder la vid�o propos� par bitbucket : 

[![video Btbucket Git LFS](http://img.youtube.com/vi/9gaTargV5BY/0.jpg)](http://www.youtube.com/watch?v=9gaTargV5BY "Git LFS explain")


D�autres solutions existes avec *git* : *Git Annex*, *Git Fat* , *Git Media* , *Git Bigstore*, *Git Sizer*, � Ces derni�res sont moins document�es, plus difficiles � mettre en place et � prendre en main et semblent moins adapt�es � un projet *Unity*.

