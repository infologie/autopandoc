# autopandoc ![pandoc](https://github.com/infologie/autopandoc/workflows/autopandoc/badge.svg)

Modèle de dépôt GitHub pour automatiser la fabrication de documents avec [Pandoc](https://pandoc.org/) dans le cadre d’une écriture académique collaborative en Markdown.

## Principe

Il existe de nombreux outils pour écrire à plusieurs en Markdown, comme par exemple [StackEdit](https://stackedit.io/) ou [HackMD](https://hackmd.io/).

En revanche, il existe peu de solutions pour utiliser collaborativement Pandoc. Le logiciel [Stylo](https://stylo.huma-num.fr/) constitue une piste.

Je propose ici une méthode qui repose sur les fonctionnalités d'automatisation de GitHub : au lieu de faire installer et exécuter Pandoc par chaque personne, on délègue cette tâche à GitHub via un [*workflow*](https://docs.github.com/en/actions/configuring-and-managing-workflows/configuring-a-workflow).

L'écriture peut alors se faire de manière asynchrone (chacun dans sa copie locale du dépôt) ou bien synchrone, via un outil comme HackMD. Pour la deuxième option, l'extension de navigateur [HackMD-it](https://hackmd.io/s/hackmd-it) facilite beaucoup l'intégration de GitHub et HackMD.

## Fonctionnement

### Fichiers requis

Le fichier à convertir (`.md`) doit être rédigé en [Pandoc Markdown](https://pandoc.org/MANUAL.html#pandocs-markdown).

Le dépôt doit contenir une feuille de style (ou modèle) pour chaque format d’export voulu : `.css` pour une version web, `.docx` pour une version Microsoft Word, etc.

Pour que Pandoc traite automatiquement les citations, le dépôt doit également contenir un fichier contenant les références au format [BibTeX](http://www.bibtex.org/) (ici `references.bib`) et une feuille de style bibliographique au format [CSL](https://citationstyles.org/) (ici `iso690-author-date-fr-no-abstract.csl`). Vous pouvez visiter le [Zotero Style Repository](https://www.zotero.org/styles) pour télécharger des styles au format CSL.

### Paramétrer la conversion

Le *workflow* GitHub est un fichier YAML qui contient des instructions, exprimées par des champs constitués de paires `clé: valeur`. Ici le fichier s'intitule `autopandoc.yml` et se situe dans `.github/workflows`. Ce dernier est un dossier caché : il est visible dans l'interface web de GitHub mais pas forcément sur votre machine. Sous macOS, dans le Finder, appuyez sur <kbd>Fn</kbd>+<kbd>Maj</kbd>+<kbd>Cmd</kbd>+<kbd>;</kbd> pour afficher les dossiers et fichiers cachés ; sous Windows, utilisez [l'option équivalente dans le Panneau de configuration](https://support.microsoft.com/fr-fr/help/14201/windows-show-hidden-files).

Le champ `args` liste les options de conversion pour Pandoc. Dans une commande Pandoc, les options doivent se suivre sur une seule et même ligne. Ici, pour plus de lisibilité, elles sont écrites ligne par ligne, précédées de l'indicateur `>-` qui gère l'espacement au moment du traitement (cf. [YAML multi-ligne](https://yaml-multiline.info/)).

Il est tout à fait possible de prévoir plusieurs conversions dans un seul *workflow*. L'exemple fourni dans ce dépôt génère à la fois un export `.html` et un export `.docx`.

### Déclencher la conversion

Le [déclenchement](https://docs.github.com/en/actions/configuring-and-managing-workflows/configuring-a-workflow#triggering-a-workflow-with-events) du *workflow* est ici réglé sur `on: push`. Ceci signifie que la conversion se lance automatiquement à chaque *push* sur le dépôt, quel que soit le contributeur.

Pour configurer un déclenchement manuel, remplacer `on: push` par `on: workflow_dispatch`. Pour lancer la conversion, il faut alors :

- se rendre dans l'onglet **Actions** du dépôt ;
- cliquer sur **autopandoc** ;
- cliquer sur le menu déroulant **Run workflow** ;
- cliquer sur le bouton vert **Run workflow**.

### Récupérer les exports

Chaque exécution du *workflow* est appelée un *run* (= exécution). Pour récupérer les artefacts produits durant un *run* (ici les exports Pandoc), il faut se rendre sur sa page de résultats :

- se rendre dans l'onglet **Actions** du dépôt ;
- dans la liste des résultats au centre de la page, cliquer sur le plus récent *run* nommé **autopandoc** (au sommet de la liste) ;
- sous **Artifacts**, cliquer sur **output** pour télécharger les artefacts sous forme d'archive compressée.

On peut également recevoir une notification avec un lien direct vers cette page. Par défaut, lorsqu'un *run* réussit, GitHub reste silencieux : il n'envoie d'email ou de notification web que lorsqu'un *run* échoue. On peut changer ce paramètre dans son profil GitHub : cliquez sur **Settings**, **Notifications**, puis décochez la case **Send notifications for failed workflows only** sous **GitHub Actions**.

## Réutilisation, maintenance, crédits

Ce dépôt est configuré comme *template* GitHub. Ceci permet de le réutiliser tel quel après un *fork* en cliquant sur **Use this template**. Ceci crée un nouveau dépôt dont on peut choisir le nom, sans l'historique de *commits* du dépôt d'origine, mais avec tous ses contenus. Il n'y a plus alors qu'à modifier `article.md`, remplacer les modèles par d'autres, personnaliser le *workflow*, etc.

Je partage ce dépôt sans garantie de maintenance et sans promesse de support technique.

**Crédits :**

- `stylesheet.css` : [Typesafe.css](https://uglyduck.ca/typesafe-css/), Bradley Taunt ;
- `template.docx` basé sur [pandoc-templates](https://github.com/jgm/pandoc-templates).