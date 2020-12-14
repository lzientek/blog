---
layout: post
title:  "Comparatif des solutions de features flags du marché"
date:   2020-12-14 10:27:18 +0100
author: Lucas ZIENTEK
categories: dev
tags: [feature flags, dev, trunk based, CI]
---

Dans le cadre d'un comparatif que j'ai dû faire chez [Mistertemp'](https://www.welcometothejungle.com/fr/companies/mistertemp) pour trouver la solution de __feature flag__ la plus adaptée, j'ai récolté avec mes collègues énormément d'informations. Je voulais donc vous présenter quelques solutions de feature flags, leurs points forts et leurs points faibles pour que vous puissiez choisir celui qui vous convient le mieux.

## Avant de commencer
Toutes les solutions ont été testées sur un projet test en nodejs et reactjs. Il y a peut-être des subtilités si vous utilisez d'autres langages.

Les critères de comparaisons sont les suivants :
- L'existance d'une UI/dashboard d'administration des features flags.
- La maturité du projet.
- La gestion multi-environnement.
- La segmentation des utilisateurs.
- Possibilités de faire des canary release.
- Possibilité de faire du remote config.
- type de mise à jour des flags

Le tout avec une petite conclusion personnelle qui n'engage que moi.

## 1. Projets feature flags open source
J'ai volontairement choisi de séparer les projets open source des autres. Les solutions SAAS pouvant rapidement devenir onéreuse je souhaitais mettre en avant les solutions on-premise gratuites et open source.

### 1.1. Flagsmith

[Flagsmith](https://www.flagsmith.com/) (ex bullet train) est pour moi la solution open source la plus mature, par ça duré d'existence, la communauté et les features proposées.  

| Features |  |
|-----|-----|
| Dashboard | : ✅ : |
| Maturité | : ✅ : | 
| Multi-env | : ✅ :  |
| Segmentation utilisateurs | : ✅ : |
| Canary | : 🔴 (mais possible avec la segmentation) : |
| Config | : ✅ : |
| MAJ | 1 fetch par flags / __temps réel__ |

__Conclusion:__  
Flagsmith est une solution idéale si vous souhaitez heberger vous même votre outil de feature flags. Il est mature propose beaucoup de fonctionnalités souhaitées et le côté open source permet d'influer sur la road map et de contribuer. L'interface n'est pas dégueulasse et relativement facile à utiliser. Il existe énormément de SDK et permet donc d'être intégré facilement à tous vos projets. L'installation se fait rapidement sur un environnement gérant docker, mais il existe aussi une solution cloud si vous voulez pas gérer l'infra.

### 1.2. Unleash
[Unleash](https://unleash.github.io/) est une solution qui est encore a ces débuts, aucune réel structure derrière il est aujourd'hui maintenu comme un side project. Cela se ressent sur l'interface, sur les features et sur la réactivité de la communauté. Il a malgré tout le mérite d'exister pour être une alternative à flagsmith.

| Features |  |
|-----|-----|
| Dashboard | : 🔶 : |
| Maturité | : 🔶 : | 
| Multi-env | : 🔴 :  |
| Segmentation utilisateurs | : ✅ : |
| Canary | : ✅ : |
| Config | : 🔶 : |
| MAJ | Fetch périodique (pas de temps réel mais un serveur moins solicité) |

__Conclusion:__  
Unleash est une solution en devenir, elle a l'avantage d'être légère. Il manque à mon sens quelques points essentiels, le dashboard n'a pas d'authentification par défaut, il n'existe pas de gestion des environnements pas pratique pour activer ou non des features toggles uniquement en dev, pre-prod ou prod. Il reste par contre un point fort a unleash c'est les différentes solutions de rollout/activation des features flags.

## 2. Solutions Feature toggles SAAS

Les solutions saas se ressemblent toutes beaucoup, je vais essayer de ne pas trop rentrer dans le pricing, mais cela peut vite devenir un facteur important en fonction de votre volume d'utilisateurs.

### 2.1. Split.io

| Features |  |
|-----|-----|
| Dashboard | : ✅ : |
| Maturité | : ✅ : | 
| Multi-env | : ✅ :  |
| Segmentation utilisateurs | : ✅ : |
| Canary | : 🔶  Pourcentage de traffic facilement alouable : |
| Config | : ✅  De la config peut-etre associé a chaque feature flag : |
| MAJ | Event server méttant a jour les frontend et api. __Temps réel__. |
| [Pricing](https://www.split.io/pricing/) | Pricing sur demande mais il existe une version gratuite avec seulement quelques feature flags et des fonctionnalités limitées |

__Conclusion:__  
Solution tout à fait fonctionnel, l'interface d'administration est un peu lourde mais permet pas mal de conditionnel pour activer ou non un feature flag. Il manque juste la partie canary release. Les SDK sont faciles d'utilisation et à configurer.  
__Bonne solution__


### 2.2. Optimizely

| Features |  |
|-----|-----|
| Dashboard | : ✅ : |
| Maturité | : ✅ : | 
| Multi-env | : ✅ :  |
| Segmentation utilisateurs | : ✅ : |
| Canary | : 🔴 : |
| Config | : 🔴 : |
| MAJ | Fetch périodique pour récupérer toutes les feature flags (toutes les 5minutes) |
| [Pricing](https://www.optimizely.com/plans/) | Pricing sur demande, mais il existe une version gratuite avec seulement __un__ feature flag. |

__Conclusion:__  
Solution pas forcément adapté aux features flags mais plutot à de l'A/B testing. On vois bien sur ce produit qu'on est très orienté marketing.  
__Solution bof bof.__ 

### 2.3. Launchdarkly

| Features |  |
|-----|-----|
| Dashboard | : ✅ : |
| Maturité | : ✅ : | 
| Multi-env | : ✅ :  |
| Segmentation utilisateurs | : ✅ : |
| Canary | : 🔴 : |
| Config | : ✅  De la config peut-etre associé a chaque feature flag : |
| MAJ | Event server méttant a jour les frontend et api. __Temps réel__. |
| [Pricing](https://launchdarkly.com/pricing/) | Pricing basé sur le nombre d'utilisateurs uniques par mois (MAUS). (Commence à 75$/mois pour 1k utilisateurs) |

__Conclusion:__  
Solution fonctionnel, l'interface d'administration est un peu mieux que split.io, mais on peut vite s'y perdre. Il manque la partie canary release ici aussi. Les SDK sont faciles d'utilisation et à configurer et celui de react exécute un rerender avec les nouveaux flag (gros effet waouh du vrai temps réel).  
__Bonne solution.__

### 2.4. Configcat

| Features |  |
|-----|-----|
| Dashboard | : ✅ : |
| Maturité | : ✅ : | 
| Multi-env | : ✅ :  |
| Segmentation utilisateurs | : ✅ : |
| Canary | : 🔴 : |
| Config | : ✅ : |
| MAJ | Fetch périodique pour récupérer toutes les features flags de 1 secondes à plusieurs heures. |
| [Pricing](https://configcat.com/pricing/) | Pricing très granulaires, de gratuit à 2500$/mois. Ils ont même un [calculateur](https://configcat.com/calculator/) pour trouver le pricing le plus adapté.  |

__Conclusion:__  
Je n'ai pas pu trop toucher a configcat, mais leur dashboard a plusieurs niveau de découpage avec plusieurs produits, configs et environnements. On peut facilement changer plusieurs feature-flags d'un coup avec la notion de config. Une interface facile d'utilisation et intuitive. Petit bonus ils plantent des arbres 🌳.  
__Bonne solution__

### 2.5. Rollout

| Features |  |
|-----|-----|
| Dashboard | : ✅ : |
| Maturité | : ✅ : | 
| Multi-env | : ✅ :  |
| Segmentation utilisateurs | : ✅ : |
| Canary | : ✅ : |
| Config | : ✅ : |
| MAJ | : Non testé : |
| [Pricing](https://rollout.io/pricing/) | Pricing basé sur le nombre d'utilisateurs uniques par mois (MAUS) et le nombre d'utilisateurs de la plateforme. (Commence gratuitement pour 2k MAUS) |

__Conclusion:__  
Rollout de CloudBees est un produit bien fini qui coche toutes nos cases en terme de fonctionnalités. Je n'ai pas pus vérifier les sdk et du coup je ne veux pas m'avancer dessus.  __Très Bonne solution__ (sur le papier).

## Mais que choisir
Maintenant c'est un peu à vous de travailler. A mon avis,  [Flagsmith](#11-flagsmith) pour l'open source ou en on-premise est ce que je recommanderai. Dans le SaaS je m'orienterai plutot sur [Rollout](#25-rollout) ou [LaunchDarkly](#23-launchdarkly). Mais dites-moi en commentaire ou sur [tweeter](https://twitter.com/lucas42_) si vous voulez que j'aborde un autre projet et quelle solution de feature flags vous utilisez.