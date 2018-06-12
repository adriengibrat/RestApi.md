title: The rules of REST API
author:
  name: I'm Adrien, in charge of Sharing REST API group
  email: a.gibrat@oodrive.com
theme: ./theme
controls: false
output: index.html

--

# REST API @ ![@](theme/img/logo.svg)

## Rules & Feedback

<script type="presenter/text">
Le groupe API REST a pour but de produire et maintenir les API utilisée par les produits de partage Oodrive, il a commencé lors de leur refonte il y a 4 ans.

Architectes, dev serveur et clients (mobile, lourd et web) se réunissent chaque semaine 30m et maintiennent la documentation dans un repo git dédié.

Le rôle de responsable tourne régulièrement.
</script>

--

## <small>1st rule</small> You document REST API

<script type="presenter/text">
Une API doit être documentée...
</script>

--

> Contract between backend and clients

- MUST be human readable
- SHOULD be easy to write
- MAY use [json-schema](http://json-schema.org/implementations.html)

<script type="presenter/text">
La documentation doit être accessible (HTML), facile à écrire (plus de detail après) et idéalement générer un json-schema qui permet:
- tests auto
- validation des données coté serveur
- de typer coté client
</script>

--

## <small>2nd rule</small> You DOCUMENT REST API

<script type="presenter/text">
La Documentation en premier!
</script>

--

The Waterfall mess 

```mermaid
graph LR
  b0 -.- c1
  c1 -. feedback .- b1
  b2 -.update .- c2
  subgraph Clients
    c1(feature)
    c2(deploy)
  end
  subgraph Backend
    b0(API)
    b1(other task)
    b1 -. switch .- b2(API)
  end
```

<script>
document.currentScript.parentNode.addEventListener('mermaid', event => {
  event.target.querySelector('#subGraph0 text').setAttribute('x', '-280')
  event.target.querySelector('#subGraph1 text').setAttribute('x', '-120')
})
</script>

<script type="presenter/text">
Le cycle de vie classique de création d'une API...
Problèmes, bien que l'API soit très vite disponible,
la fonctionnalité met longtemps a être dépoyée et 
c'est fustrant aussi bien pour les serveurs que les clients.
</script>

--

vs. Documentation First

```mermaid
graph LR
  a0 -. mock .- c1
  a0 -.- b1
  b1 -. plug .- c2
  subgraph API Doc
    a0(contract)
  end
  subgraph Clients
    c1(feature) -.- c2(deploy)
  end
  subgraph Backend
    b1(API)
  end
```

<script>
document.currentScript.parentNode.addEventListener('mermaid', event => {
  event.target.querySelector('#subGraph1 text').setAttribute('x', '-120')
  event.target.querySelector('.edgePath:nth-child(4)').remove()
})
</script>

<script type="presenter/text">
L'effort commun permet de se mettre d'accord et de travailler en même temps sur la fonctionnalité, le résultat attendu: livraison rapide et de qualité.
</script>

--

## <small>3rd rule</small> If backend or clients says&nbsp;"stop", fight is over

<script type="presenter/text">
Trouver un concensus entre serveur et clients ?!
</script>

--

> API expectations

1. Usable <small>for clients</small>
2. Maintainable <small>by backend</small>
3. Extendable <small>to features</small>

<small>or it's a potential dead end</small>

<script type="presenter/text">
En réalité, ce concensus n'est pas évident à obtenir:
entre les client qui veulent l'intégralité des données en une requête, svp
et le serveur qui veut éviter le code spagetti et maintenir des perfs décente...

Il faut toujours garder en tête ces 3 propriétés:
utilisable, maintenable et extensible.
</script>

--

## <small>4th rule</small> Only two kind of MR

<script type="presenter/text">
Comment maintient-on la documentaion des API ?
</script>

--

> Branch convention

to sync API & doc

- implemented<small>/descriptive_branch_name</small>
- proposal<small>/descriptive_branch_name</small>

We use [Git Octopus](https://github.com/lesfurets/git-octopus) <small>for preview</small>

<script type="presenter/text">
On traite différement les branches d'améliorations de la documentation (formulation, limitations et erreurs mal documentées, etc) que l'on revoit et merge immédiatement;
des branches qui documentent les ajouts et modifications, celles-ci sont discutée pour atteindre un concensus et sont mergée juste après l'implémentation.

La documentation reste synchronisée avec l'implémentation. On peut avoir un aperçu de la documentation à venir en mergeant toutes les branches en cours grae à Git Octopus.

Merci à Alexandre Dubreuil, des furets, de nous l'avoir présenté lors des précédents Open R&Days!
</script>

--

## <small>5th rule</small> One tooling at a time

--

> Tooling review

<font>&nbsp;&nbsp;&nbsp;&nbsp;😭</font> [apiblueprint](https://apiblueprint.org) <small>& [aglio](https://github.com/danielgtaylor/aglio) sucks</small>
<font>&nbsp;&nbsp;&nbsp;&nbsp;😔</font> [raml](https://raml.org) <small>has ecosystem issues</small>
<font>&nbsp;&nbsp;&nbsp;&nbsp;😎</font> [openAPI](https://www.openapis.org) <small>& [swagger](https://swagger.io/) wins</small>

<script type="presenter/text">
On a utilise les 3.

Historiquement apiblueprint, ça semble remplir les critères (accessible, facile à écrire et json-schema). En pratique, ce n'est pas le cas: beaucoup de bugs, et de limitations.
Analogie: ça semble etre une voiture, c'est une 'trotinette'.

A la marge, Raml, le format est bien mais les outils ne sont pas assez maintenu.
C'est une voiture, dont le modèle n'est plus produit.

Aujourd'hui, on mise sur openApi (utilisé sur les nouveaux projets, commence la migration de l'historique):
- adapté à la documentation en premier (et génération à partir du code)
- outils nombreux et maintenus
- compat json-schema
- swagger UI permet de tester l'API depuis la doc
C'est une voiture, avec la clim ;)
</script>

--

## <small>6th rule</small> No complexity, no&nbsp;contradiction

--

> KISS

- entities <small>MUST be</small> consistent
- complex <small>solutions</small> DON'T last
- <small>SHOULD use</small> simple <small>&</small> standard <small>solutions</small>

<script type="presenter/text">
On a appris de nos erreurs

content negociation avec des représentations basiques & complètes: abandonné
système d'embed pour éviter des duplications: trop complexe, utilisé sur un seul endpoint
hyperliens: pas utilisé

pagination avec header link rel=next link: Ok
</script>

--

## <small>7th rule</small> API version will go on<br>as long as they have to

--

> API versioning

- <small>client's</small> short support <small>policy</small>
- <small>long term</small> deprecation <small>notice</small>
- <small>delay & batch</small> breaking changes

We are moving from silos to [plateform](https://www.oodrive.com/products/oodrive-platform),<br>
major versions expected!

<script type="presenter/text">
C'est un chantier en cours.

Nous entrons dans un phase d'uniformisation de toutes nos API:
On commence par la représentation des utilisateurs et des fichiers.

Une politique de support claire et des méchanisme de mise à jour auto des clients
est indispensable pour pouvoir faire évoluer les API.
</script>

--

## <small>8th rule</small> If this is your first meeting,<br> you HAVE to write doc

--

## Thanks

<center>© [The Rules of Fight Club](http://www.diggingforfire.net/fightclub)</center>

<script type="presenter/text">
Daniel, Arthur ...

Questions / Suggestions @ apéro
</script>
