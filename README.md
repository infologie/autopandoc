# autopandoc ![pandoc](https://github.com/infologie/autopandoc/workflows/autopandoc/badge.svg)

Modèle de dépôt GitHub pour automatiser la fabrication de documents avec [Pandoc](https://pandoc.org/) dans le cadre d’une écriture académique collaborative en Markdown.

## Principe

Il existe de nombreux outils pour écrire à plusieurs en Markdown, comme par exemple [StackEdit](https://stackedit.io/) ou [HackMD](https://hackmd.io/).

En revanche, il existe peu de solutions pour utiliser collaborativement Pandoc. Le logiciel [Stylo](https://stylo.huma-num.fr/) constitue une piste.

Je propose ici une méthode qui repose sur les fonctionnalités d'automatisation de GitHub : au lieu de faire installer et exécuter Pandoc par chaque personne, on délègue cette tâche à GitHub via un [*workflow*](https://docs.github.com/en/actions/configuring-and-managing-workflows/configuring-a-workflow).

Un *workflow* GitHub est un fichier YAML (ici `autopandoc.yml`) placé sur un chemin invisible (`.github/workflows`). Les instructions sont exprimées par des champs constitués de paires `clé: valeur`.

Le [déclenchement](https://docs.github.com/en/actions/configuring-and-managing-workflows/configuring-a-workflow#triggering-a-workflow-with-events) du *workflow* est ici réglé sur `on: push`. Ceci signifie que la conversion se lance automatiquement à chaque *push* sur le dépôt, quel que soit le contributeur.

Le champ `args` liste les options de conversion pour Pandoc. Dans une commande Pandoc, les options doivent se suivre sur une seule et même ligne. Ici, pour plus de lisibilité, elles sont écrites ligne par ligne, précédées de l'indicateur `>-` qui gère l'espacement au moment du traitement (cf. [YAML multi-ligne](https://yaml-multiline.info/)).

## Éléments requis

Le fichier à convertir (`.md`) doit être rédigé en [Pandoc Markdown](https://pandoc.org/MANUAL.html#pandocs-markdown).

Le dépôt doit contenir une feuille de style (ou modèle) pour chaque format d’export voulu : `.css` pour une version web, `.docx` pour une version Microsoft Word, etc.

Pour que Pandoc traite automatiquement les citations, le dépôt doit également contenir un fichier contenant les références au format [BibTeX](http://www.bibtex.org/) (ici `references.bib`) et une feuille de style bibliographique au format [CSL](https://citationstyles.org/) (ici `iso690-author-date-fr-no-abstract.csl`). Vous pouvez visiter le [Zotero Style Repository](https://www.zotero.org/styles) pour télécharger des styles au format CSL.

## Crédits

- `stylesheet.css` : [Typesafe.css](https://uglyduck.ca/typesafe-css/)