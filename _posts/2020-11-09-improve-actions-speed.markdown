---
layout: post
title:  "Améliorer les performances de github actions"
date:   2020-11-09 18:15:09 +0100
categories: ops
tags: [dev, devops, jekyll, ci]
---

Suite à mon premier article sur [le deploiement d'un blog jekyll](https://lzientek.fr/dev/ops/2020/11/06/creation-blog.html) mais le deploiement était un peu long. Donc deuxième article!

# Comment j'ai diviser par 5 le temps de déploiement de mon blog

Nous allons voir dans cet article quels sont les moyens facile d'accélérer le deploiement sur github actions. Ici nous somme dans le cas d'un site jekyll donc ci orienté ruby mais les concept s'adapte a la plupart des technos. 

## L'utilisation du cache

Le cache et le meilleur moyen d'améliorer les performances de votre CI/CD sans pour autant modifier fondamentalement le code. Faut t'il encore choisir corectement ce qu'il faut mettre en cache. Petite list non exaustive:

* Les dépendences externe téléchargé ou installé (par apt, apk, etc...)
* Les installations de gestionnaire de package (npm, bundle, nuggets, etc...)
* Tout fichier provenant de source externe

### Mise en place du cache pour ruby

![Screenshot github actions](/assets/img/dependencies_slow.png)  
Comme on peut le voir si dessus le plus long c'est la partie installation avec `bundle install`.

On va donc ajouter du cache sur l'installation des bundle ruby car c'est long et c'est rarement mis a jours. Dans github actions ça se présente sous cette forme:


```yaml
 - uses: actions/cache@v2
      with:
        path: vendor/bundle
        key: ${{ runner.os }}-gems-${{ hashFiles('**/Gemfile.lock') }}
        restore-keys: |
          ${{ runner.os }}-gems-
    - name: Install dependencies
      run: |
        bundle config path vendor/bundle
        bundle install --jobs 4 --retry 3
```
Avant le lancement le lancement de l'install on déclare le block de cache de github actions. On lui donne le path a mettre en cache et on fait un hash depuis le fichier `Gemfile.lock` pour lui dire de ne pas utiliser le cache si ce fichier change.

> Pour ruby/bundle on doit lui configurer le path pour l'installation des dépendances 

Petit bonus de bundle on peut lui dire de faire plusieur call en parallele pour accélérer un peu l'installation.


