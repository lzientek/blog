---
layout: post
title:  "Premier post / Setup et deploiement de jekyll"
date:   2020-11-06 11:11:03 +0100
author: Lucas ZIENTEK
categories: dev ops
tags: [dev, devops, jekyll]
---
Nouvelle journée nouveau projet!

Déjà bienvenue sur ce nouveau blog (probablement le 5eme si on ne compte pas les skyblog)!

Le premier sujet est forcément la mise en place de ce blog. Ça va se faire en quattres parties, le choix des technos, le setup, l'hébergement et le déploiement.

## Le choix de la techno de blog (Jekyll)  

Les prérequis étaient rapidement trouvés:  

* Pas de bdd
* Site static (pas de bdd, ni d'api)
* Simple à customiser
* Simple à mettre en place 
* Un SEO potable
* Rédaction sur fichier markdown

Du coup le choix restreint c'est posé sur plusieurs technos les deux finalistes étant [jekyll](https://jekyllrb.com/) et [hugo](https://gohugo.io/).

J'ai choisi jekyll principalement par ~~flemme~~ sa facilité d'utilisation.

## Faire l'installation de jekyll

Pour la mise en place c'est rapide 
```bash
gem install bundler jekyll
jekyll new my-awesome-blog
cd my-awesome-blog
bundle exec jekyll serve
```

Ensuite, l'installation du thème pas très compliqué non plus https://jekyllrb.com/docs/themes/.

J'ai choisi un thème assez classique le [yat theme](https://github.com/jeffreytse/jekyll-theme-yat).

Un petit push sur github et me voila à rédiger cet article.

## L'hebergement

Comme vous vous doutez les choix techniques ont été fait de manière à mettre un minimum d'effort dans l'hébergement pas d'architecture compliquée. Le but étant d'aller à l'éssentiel et surtout pas chère (oui je suis un peu radin)! Donc, pour le prix d'un éclair au café par mois le suis partie sur un hebergement ovh kimsufi. Mais un S3 aurais très bien fait l'affaire aussi ou tout autre serveur de fichier qui gère l'http.

## Le déploiement CI/CD
Étant un grand fan d'automatisation et surtout de déploiement continue (ou __continous deployment__ pour les anglophones) c'était forcément dans les premières choses à faire.

Ça faisait un moment que je voulais tester github actions, utilisant gitlab au travail la c'est l'occasion de tester quelque chose de nouveau.

### Actions pré-construite pour ruby
À ma grande surprise j'ouvre l'onglet actions de mon projet github et là, truc quand meme super cool il me propose direct un exemple de fichier de configuration. Et pas n'importe lequel, un pour faire l'installation et le lancement des tests ruby la techno du projet.

Je me dit super cool, j'adapte le fichier de configuration pour builder le blog. Et ça ressemble à ça (et ça fonctionne):

```yaml
# .github/workflows/deploy.yml
name: Deploy

on:
  push:
    branches: [ master ]

jobs:
  build_deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - name: Set up Ruby
      uses: ruby/setup-ruby@ec106b438a1ff6ff109590de34ddc62c540232e0
      with:
        ruby-version: 2.6
    - name: Install dependencies
      run: bundle install
    - name: Build blog
      run: JEKYLL_ENV=production bundle exec jekyll build
```

### Maintenant on deploie le blog sur un FTP

Et la deuxième agréable surprise de github actions, une market place pour ajouter des fonctionnalités à actions. Dans notre cas pas besoin de s'occuper de la connexion FTP quelqu'un l'a fait pour nous (SamKirkland avec [ftp deploy](https://github.com/marketplace/actions/ftp-deploy)).
Donc on ajoute nos informations de ftp dans les secrets du projet et on ajoute un petit peu de code à la suite de notre build.

```yaml
    - name: Deploy to the server
      uses: SamKirkland/FTP-Deploy-Action@3.1.1
      with:
        ftp-server: ${{ "{{secrets.FTP_SERVER" }}}}
        ftp-username: ${{ "{{secrets.FTP_USER" }}}}
        ftp-password: ${{ "{{secrets.FTP_PASSWORD" }}}}
        local-dir: _site
```

> Attention il ne fat pas oublié de créer un fichier .git-ftp-include avec !_site/ dedans.

Et paff ~~ça fait des chocapic~~ le blog est en ligne.  
![Screenshot github actions](/assets/img/github_actions.png)


Donc en conclusion vous avez tout pour lancer votre propre blog en deploiement continue sur github. Il vous reste plus qu'a ecrire des articles.

### Pistes d'amélioration 
* Des petits problèmes de font a fixer sur le blog.
* La CD met aujourd'hui 5 minutes ça me semble beaucoup pour le peu qui est fait, je pense faire un [article dessus prochainement](https://lzientek.fr/ops/2020/11/09/improve-actions-speed.html).