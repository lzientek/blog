---
layout: post
title:  "Améliorer les performances de github actions"
date:   2020-11-09 18:15:09 +0100
author: Lucas ZIENTEK
categories: ops
tags: [dev, devops, jekyll, ci]
---

Suite à mon premier article sur [le deploiement d'un blog jekyll](https://lzientek.fr/dev/ops/2020/11/06/creation-blog.html) mais le deploiement était un peu long. Donc deuxième article!

## Comment j'ai diviser par 5 le temps de déploiement de mon blog

Nous allons voir dans cet article, quels sont les moyens faciles d'accélérer le déploiement sur github actions. Ici nous sommes dans le cas d'un site jekyll donc __CI__ orienté ruby mais le concept s'adapte à la plupart des technos. 

### L'utilisation du cache

Le cache est le meilleur moyen d'améliorer les performances de votre CI/CD sans pour autant modifier fondamentalement le code. Faut-il encore choisir correctement ce qu'il faut mettre en cache. Petite liste non exaustive:

* Les dépendences externes téléchargées ou installées (par apt, apk, etc...)
* Les installations de gestionnaire de packages (npm, bundle, nuggets, etc...)
* Tous fichiers provenant de source externe

#### Mise en place du cache pour ruby

![Screenshot github actions](/assets/img/dependencies_slow.png)  
Comme on peut le voir ci-dessus, le plus long est la partie installation avec `bundle install`.

On va donc ajouter du cache sur l'installation des bundle ruby, parce que cela est long et est rarement mis à jour. Dans github actions ça se présente sous cette forme:


```yaml
 - uses: actions/cache@v2
      with:
        path: vendor/bundle
        key: ${{ "{{runner.os" }}}}-gems-${{ "{{hashFiles('**/Gemfile.lock')" }}}}
        restore-keys: |
          ${{ "{{runner.os" }}}}-gems-
    - name: Install dependencies
      run: |
        bundle config path vendor/bundle
        bundle install --jobs 4 --retry 3
```
Avant le lancement le lancement de l'`install` on déclare le bloc de cache de github actions. On lui donne le path à mettre en cache et on fait un hash depuis le fichier `Gemfile.lock` pour lui dire de ne pas utiliser le cache si ce fichier change.

> Pour ruby/bundle on doit lui configurer le path pour l'installation des dépendances j'ai mis ici vendor/bundle.

Petit bonus de bundle on peut lui dire de faire plusieurs call en parallèle pour accélérer un peu l'installation avec le paramètre --jobs.

Après deux lancements la durée totale n'est plus que de 1min8s.

![Screenshot github actions](/assets/img/dependencies_fast.png)  

