---
url: https://blog.revathskumar.com/2026/02/hide-your-dotenv-secrets-from-llms-using-pass.html
category: "Security"
tags:
  - secrets-management
  - dotenv
  - LLM-security
  - pass
  - bash
date_saved: "2026-03-20"
---

# Hide Dotenv Secrets from LLMs Using Pass

**Source:** [https://blog.revathskumar.com/2026/02/hide-your-dotenv-secrets-from-llms-using-pass.html](https://blog.revathskumar.com/2026/02/hide-your-dotenv-secrets-from-llms-using-pass.html)
**Category:** Security

## Summary

Technique by Revath S Kumar pour masquer les secrets des fichiers `.env` aux agents LLM en utilisant `pass` (le gestionnaire de mots de passe Unix standard). Au lieu de stocker les vraies valeurs dans `.env`, on utilise des références `pass://chemin/vers/secret` qui sont résolues au runtime par un script bash. L'agent IA ne voit jamais les vrais credentials — uniquement des pointeurs. Inspiré par le projet enveil. Extension disponible sur le repo Codeberg `pass-env`.

## Why It's Interesting

Solution légère et élégante au problème critique de fuite de secrets vers les agents IA — réutilise un outil existant (pass) plutôt que d'ajouter une dépendance supplémentaire

## Key Takeaways

- Format `.env` : `AWS_SECRET=pass://projet/dev/aws_secret` — les valeurs `pass://` sont résolues au runtime, les autres restent telles quelles
- Le script bash lit le `.env`, détecte les préfixes `pass://`, appelle `pass show` pour récupérer le vrai secret, puis exporte la variable
- Usage : `pass env -- <command>` ou `env.bash --env-file .env.prod -- <command>`
- Les agents LLM qui accèdent au dossier projet ne voient que des références, jamais les vrais secrets
- Extension pass disponible sur Codeberg : `pass-env`

## Tags

#secrets-management, #dotenv, #LLM-security, #pass, #bash
