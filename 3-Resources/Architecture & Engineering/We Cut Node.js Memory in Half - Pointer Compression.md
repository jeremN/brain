---
url: "https://blog.platformatic.dev/we-cut-nodejs-memory-in-half"
category: "Architecture & Engineering"
tags:
  - Node.js
  - V8
  - memory-optimization
  - pointer-compression
  - Docker
  - Kubernetes
  - performance
date_saved: "2026-03-24"
---

# We Cut Node.js Memory in Half - Pointer Compression

**Source:** [blog.platformatic.dev](https://blog.platformatic.dev/we-cut-nodejs-memory-in-half)
**Category:** Architecture & Engineering

## Summary

Article de Matteo Collina (Platformatic) expliquant comment réduire la consommation mémoire de Node.js de 50% en activant la pointer compression de V8 — une feature disponible dans Chrome depuis 2020, désormais utilisable en Node.js via une image Docker `node-caged`. Aucun changement de code requis : un simple swap d'image Docker suffit. Un heap de 1 Go compressé contient autant de données logiques qu'un heap de 2 Go non compressé, et le GC scanne moitié moins d'octets.

## Why It's Interesting

Optimisation massive (50% de RAM) avec zéro modification de code — juste un changement de ligne dans le Dockerfile. Sur Kubernetes, ça permet de doubler la densité de pods ou diviser par deux le nombre de nœuds. Potentiellement des centaines de milliers de dollars d'économies en infra cloud.

## Key Takeaways

- La pointer compression de V8 réduit la taille des pointeurs de 8 à 4 octets, divisant la mémoire heap par deux
- Overhead moyen de ~2.5% en latency médiane (~1ms sur 40ms) — trade-off négligeable
- Les p99 et max latency sont en réalité meilleurs car le GC a moins de travail (heap plus petit)
- Les pauses GC sont plus courtes et moins fréquentes grâce à la compaction réduite
- Activable via l'image Docker `node-caged` sans aucun changement de code applicatif
- Impact direct en Kubernetes : pods à 1 Go au lieu de 2 Go = densité doublée

## Tags
#Node.js, #V8, #memory-optimization, #pointer-compression, #Docker, #Kubernetes, #performance
