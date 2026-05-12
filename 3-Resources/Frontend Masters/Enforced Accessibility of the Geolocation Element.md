---
url: https://frontendmasters.com/blog/the-enforced-accessibility-of-the-geolocation-element/
category: "Frontend Masters"
tags:
  - accessibility
  - html
  - geolocation
  - security
  - css
date_saved: "2026-04-07"
---

# The Enforced Accessibility of the Geolocation Element

**Source:** [https://frontendmasters.com/blog/the-enforced-accessibility-of-the-geolocation-element/](https://frontendmasters.com/blog/the-enforced-accessibility-of-the-geolocation-element/)
**Category:** Frontend Masters

## Summary
Analyse du nouvel élément HTML `<geolocation>` et de ses contraintes d'accessibilité imposées par le navigateur. Le bouton a un design renforcé qui enforce des exigences a11y strictes — mais la motivation réelle est la sécurité (empêcher le "tricking" des utilisateurs).

## Why It's Interesting
Cas d'étude fascinant où sécurité et accessibilité convergent : le navigateur impose des contraintes CSS (contraste 3:1 minimum, taille de police minimum) et désactive le bouton si elles ne sont pas respectées.

## Key Takeaways
- **Contraintes CSS** : ratio de contraste ≥ 3:1, font-size ≥ small — sinon le bouton est désactivé
- **Shadow DOM** : contenu dans un user-agent Shadow Root, pas d'accès via `::part`
- **Certains CSS autorisés mais cassent le bouton**, d'autres sont plafonnés
- **Motivation = sécurité** : empêcher le phishing de géolocalisation

## Tags

#accessibility, #html, #geolocation, #security, #css
