---
title: Extensions Firefox
layout: article
---

![](/images/articles/firefox_banner.png)
# Pourquoi Firefox ?

- Les bloqueurs de pubs sont limités sur Chrome et Chromium (uBlock Origin n'est proposé qu'en version "lite" et laisse passer des pubs)
- Par rapport à Chrome, Firefox est plus respectueux de la vie privée[^1]. 
- Google Chrome et autres navigateurs dérivés (Chromium : Opera, Brave…) représentent une part de marché gigantesque ; l'idée est d'utiliser une alternative pour (on l'espère) empêcher Google d'imposer ses standards partout. C'est un peu un acte militant.
- On peut utiliser Firefox sur mobile (Android) avec les mêmes extensions et synchroniser ses onglets, historique, mots de passe *(mais il vaut mieux utiliser un gestionnaire de mots de passe!)* sur différents appareils.
# Moteur de recherche

- J'utilise Duckduckgo en priorité (par défaut), et je garde Google pour des recherches spécifiques : chercher un restaurant, voir les avis sur un médecin… pour avoir accès rapidement aux commentaires, Google Maps, etc.

# Épingler une extension à la barre d'outils

Je recommande d'épingler les extensions suivantes pour pouvoir les désactiver/réactiver rapidement : uBlock Origin, i still don't care about cookies, gestionnaire de mots de passe (si vous en utilisez un).
Pour ce faire, cliquez sur l'icône "Extensions" (pièce de puzzle), puis cliquez sur l'engrenage à côté de l'extension et cochez "Épingler à la barre d'outils".

![](/images/articles/pin_extension.png)

# Essentiel

- [uBlock Origin - bloqueur de pubs](https://addons.mozilla.org/en-US/firefox/addon/ublock-origin/) (épingler à la barre d'outils)
	Bloqueur de pubs essentiel - version complète (sur Chrome, il n'y a qu'une version "lite" qui laisse passer des pubs…).

- [i still don't care about cookies](https://addons.mozilla.org/en-US/firefox/addon/istilldontcareaboutcookies/) - pour ne plus avoir d'avertissements sur les cookies. L'add-on tente automatiquement d'accepter uniquement les cookies essentiels, mais s'il n'y arrive pas, il accepte tout.
	Note : Parfois, l'extension peut causer des problèmes de navigation sur le site. Si vous ne pouvez pas scroller ou cliquer sur les liens d'un site, essayez de désactiver l'extension sur le site en question.
	
	![](/images/articles/disable_dontcareaboutcookies.png)

- [Cookie Autodelete](https://addons.mozilla.org/en-US/firefox/addon/cookie-autodelete/) : pour supprimer automatiquement les cookies inutiles (dont ceux acceptés par i still don't care about cookies)

# Pratique

- [To Google Translate](https://addons.mozilla.org/en-US/firefox/addon/to-google-translate/) : sélectionner du texte, clic droit -> traduire sur Google Translate
- Gestionnaire de mots de passe : Bitwarden, LastPass…
# Confidentialité

- [Privacy Badger](https://addons.mozilla.org/en-US/firefox/addon/privacy-badger17/) : pour bloquer les trackers

- [Facebook Container](https://addons.mozilla.org/en-US/firefox/addon/facebook-container/) : pour éviter d'être traqué par Facebook/meta (addon natif Firefox) 

- [ClearURLs](https://addons.mozilla.org/en-US/firefox/addon/clearurls/) : retire automatiquement les éléments liés au tracking dans les liens

# Spécifiques à des sites

## YouTube

- [Sponsorblock](https://addons.mozilla.org/en-US/firefox/addon/sponsorblock/) : pour passer automatiquement les segments chiants des vidéos youtube : sponso, "laissez un pouce bleu si vous avez aimé cette vidéo" etc. 
hyper pratique, y'a même une option "remplacer les titres clickbait par des titres plus précis" 

- [Enhancer for YouTube](https://addons.mozilla.org/en-US/firefox/addon/enhancer-for-youtube/) : outils et contrôles supplémentaires sur YouTube

## Amazon

- [Keepa](https://addons.mozilla.org/en-US/firefox/addon/keepa/) : pour ajouter un graph sur Amazon qui représente l'évolution du prix d'un article 
## Twitch

- [FrankerFaceZ](https://addons.mozilla.org/en-US/firefox/addon/frankerfacez/)

- [BetterTTV](https://addons.mozilla.org/en-US/firefox/addon/betterttv/)

## Reddit

- [Reddit Enhancement Suite]([https://addons.mozilla.org/en-US/firefox/addon/reddit-enhancement-suite/](https://addons.mozilla.org/en-US/firefox/addon/reddit-enhancement-suite/)) : pour une meilleure navigation sur l'ancienne version de reddit 

## Bandcamp

- [Bandcamp Volume Control](https://addons.mozilla.org/en-US/firefox/addon/bandcamp-player-volume-control/) : ajoute un slider pour contrôler le volume sur bandcamp et éviter de se péter les oreilles

- [Bandcamp+](https://addons.mozilla.org/en-US/firefox/addon/bandcamp/) : je suis tombé.e là-dessus mais j'ai pas testé

## Last.fm

- [Web Scrobbler](https://addons.mozilla.org/en-US/firefox/addon/web-scrobbler/)

[^1]: Il existe aussi Waterfox, un *fork* de Firefox orienté confidentialité, sur PC (Linux, Windows, macOS) et Android.
