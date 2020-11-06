---
layout: post
title:  "Premier post / Setup et deploiement de jekyll"
date:   2020-11-06 11:11:03 +0100
categories: dev devops jekyll
---
Nouvelle journée nouveau projet!

Déjà bienvenue sur ce nouveau blog (probablement le 5eme si on ne compte pas les skyblog)!

Le premier sujet est forcément la mise en place de ce blog. Ça va se faire en 4 parties, le choix des technos, le setup, l'hebergement et le deploiement.

## Le choix de la techno de blog (Jekyll)  

Les prérequis était rapidement trouvé:  

* Pas de bdd
* Site static (pas de bdd, ni d'api)
* Simple a customiser
* Simple a mettre en place 
* Un SEO pottable
* Redaction sur fichier mardown

Du coup le choix restraint c'est posé sur plusieurs technos les deux finalistes étant [jekyll](https://jekyllrb.com/) et [hugo](https://gohugo.io/).

J'ai choisi jekyll principallement par ~~fleme~~ ça facilité d'utilisation.

## Faire l'install de jekyll

Pour la mise en place c'est rapide 
```bash
gem install bundler jekyll
jekyll new my-awesome-blog
cd my-awesome-blog
bundle exec jekyll serve
```

Ensuite l'installation du theme pas très compliqué non plus https://jekyllrb.com/docs/themes/.

J'ai choisi un theme assez classique le [yat theme](https://github.com/jeffreytse/jekyll-theme-yat).

Un petit push sur github et me voila à rédiger cet article.

## L'hebergement

Comme vous vous douté les choix technique on etait fait de manière a mettre un minimum d'effort dans l'hebergement pas d'architecture compliqué. Le but étant d'aller a l'éssentiel et surtout pas chère (oui je suis un peu radin)! Donc pour le prix d'un éclair au café par mois le suis partie sur un hebergement ovh kimsufi. Mais un S3 aurais très bien fait l'affaire aussi ou tout autre serveur de fichier qui gere l'http.

## Le déploiement CI/CD
Étant un grand fan d'automatisation et surtout de déploiement continue (ou __continous deployment__ pour les anglophones) c'était forcément dans les premières chose a faire.

Ça faisait un moment que je voulais tester github actions, utilisant gitlab au travail la c'est l'occasion de tester quelque chose de nouveau.

