---
tags:
  - web-security
  - frontend
  - cookies
  - xss
  - csrf
  - cors
  - jwt
  - sql-injection
  - csp
  - https
  - authentication
source: Frontend Masters
instructor: Steve Kinney
course: Web Security v2
---

# Web Security — Base de connaissances

Cours "Web Security v2" — Steve Kinney / Frontend Masters
Source : https://github.com/stevekinney/web-security

## Philosophie generale

- Le web est fondamentalement bizarre et le probleme de securite, c'est que ce qui fait sa force (ouverture, interconnexion) est aussi sa faiblesse.
- La securite, c'est toujours une affaire de trade-offs : entre securite, UX, et complexite d'infrastructure.
- "Je n'ai pas besoin d'apprendre la securite, mon framework gere ca" → recette pour creer des failles accidentellement.
- Bien faire la securite, c'est justement quand personne ne le remarque.

---

## 1. Les Cookies

### Qu'est-ce qu'un cookie ?

Petits morceaux de donnees stockes cote client pour tracker et identifier les utilisateurs. Utilises pour :

- Gerer les sessions (utilisateur authentifie)
- Stocker le contenu d'un panier
- Tracker les utilisateurs (usage plus sombre)

HTTP est stateless → les cookies implementent les sessions.

### Historique

| Date | Evenement |
|------|-----------|
| 1994 | Implemente par Netscape, sans spec |
| 1997 | Premiere tentative de standardisation |
| 2000 | "Cookie2", deuxieme tentative |
| 2011 | Standardise via RFC 6265 |

### Mecanique HTTP

```
# Le serveur set un cookie
Set-Cookie: username=bobbytables;

# Le navigateur le renvoie dans les requetes suivantes
Cookie: username=bobbytables;
```

### Attributs importants

#### Expiration

```
Set-Cookie: username=bobbytables; Expires=Thu, 29 Feb 2024 17:45:00 GMT;
Set-Cookie: username=bobbytables; Max-Age=2592000;
```

Sans expiration → le cookie dure le temps de la session. Pour "supprimer" un cookie → mettre sa date d'expiration dans le passe.

#### Scoping

```
# Limiter a un chemin (peu fiable pour la securite)
Set-Cookie: username=bobbytables; Path=/profile;

# Limiter a un domaine et ses sous-domaines
Set-Cookie: username=bobbytables; Domain=.frontendmasters.com;
```

#### Attributs de securite

```
Set-Cookie: session=abc; HttpOnly; Secure; SameSite=Lax;
```

| Attribut | Role |
|----------|------|
| `HttpOnly` | Empeche les scripts JS d'acceder au cookie (protection XSS) |
| `Secure` | Transmis uniquement en HTTPS |
| `SameSite` | Controle l'envoi cross-origin |

### SameSite en detail

```
SameSite=None    # Toujours envoyer le cookie (meme cross-site)
SameSite=Lax     # Cookie envoye avec les navigations top-level + methodes sures (GET/HEAD) — DEFAUT depuis 2020
SameSite=Strict  # Cookie uniquement si la requete vient du meme site
```

**Site ≠ Origin** — important a comprendre :

- **Origin** = Protocole + Host + Port
- **Site** = TLD+1 (ex: `example.com` et `login.example.com` sont le meme site)
- Exception : `github.io` et domaines similaires

**Lax vs Strict : le trade-off**
`Strict` est le plus securise, mais si un utilisateur suit un lien depuis un email, il sera considere deconnecte. `Lax` offre un bon equilibre : il permet les navigations top-level cross-site (liens) mais bloque les requetes AJAX/iFrame.

`SameSite=Lax` est le defaut depuis :

- Safari 12.1 (mars 2019)
- Chrome 80 (janvier 2020)
- Firefox 79 (juin 2020)
- Edge 86 (octobre 2020)

---

## 2. Same Origin Policy (SOP)

Mecanisme de securite du navigateur qui empeche les documents/scripts d'une origine d'interagir avec les ressources d'une autre origine.

**Origin = Protocole + Host + Port**
Les trois doivent etre identiques pour que deux ressources aient la meme origine.

### Moyens de contourner la SOP (legitimement)

- CORS (Cross-Origin Resource Sharing)
- JSONP (obsolete)
- Proxies
- PostMessage API
- WebSockets
- `document.domain`

---

## 3. Vulnerabilites liees aux sessions

### Session Hijacking

Utiliser la valeur d'un cookie pour faire croire au serveur qu'on est quelqu'un d'autre.

### Privilege Escalation

L'attaquant obtient d'abord un acces limite, identifie des failles, les exploite pour obtenir des privileges superieurs.

Etapes :

1. **Initial Access** : acces limite a l'application
2. **Discovering Weaknesses** : identification des mauvaises configurations
3. **Exploiting Vulnerabilities** : exploitation pour escalader les privileges
4. **Gaining Control** : acces a des donnees sensibles / actions non autorisees

### Man-in-the-Middle (MitM)

Attaquant qui intercepte (et potentiellement modifie) la communication entre deux parties.

Fix simple : utiliser HTTPS.

---

## 4. Injection Attacks

Classe de vulnerabilites ou l'attaquant envoie des inputs malicieux pour "sortir" du contexte prevu et causer des dommages.

### Command Injection

Executer des commandes arbitraires sur l'OS via une application vulnerable.

Exemple vulnerable :

```javascript
import { exec } from 'child_process';
app.get('/files', (req, res) => {
  const command = `ls ${req.body}`;
  exec(command, (error, stdout, stderr) => { ... });
});
// Input malicieux : test.txt; rm -rf /
```

Remediation :

1. Ne pas faire ca du tout
2. Utiliser des modules Node built-in (ex: `fs`)
3. Utiliser `execFile` si vraiment necessaire
4. Sanitizer, allowlister, principe du moindre privilege

### File Upload Vulnerabilities

Mauvaise gestion des fichiers uploades → vecteur d'attaque.

### Remote Code Execution (RCE)

L'attaquant peut executer du code arbitraire sur le serveur.

Mecanisme :

1. **Injection** : input non sanitise
2. **Processing** : execute via `eval`, commandes shell, deserialisation non securisee
3. **Execution** : code malicieux s'execute dans l'environnement serveur

Prevention :

- Valider et sanitiser tous les inputs
- Ne pas utiliser `eval()`, `Function()`, `exec()`
- Utiliser des libraries securisees (DOMPurify, VM2 pour code non-trusted)
- Principe du moindre privilege (ne pas tourner en root)
- Maintenir Node.js et ses dependances a jour

---

## 5. CSRF — Cross-Site Request Forgery

### Definition

Vulnerabilite qui permet a un attaquant de faire effectuer des requetes non autorisees au nom de l'utilisateur. L'attaquant exploite la session authentifiee de l'utilisateur sans lui voler son token.

### Cas reels

| Cible | Annee | Detail |
|-------|-------|--------|
| Netflix | 2006 | GET endpoint pour ajouter un DVD, exploitable via `<img src="...">` |
| New York Times | 2008 | Formulaire "Email This" vulnerable |
| ING Direct | 2008 | Ouverture de comptes et transferts de fonds |
| YouTube | 2008 | Quasi toutes les actions utilisateur |
| Twitter | 2010 | URL pour tweeter exploitable |
| TikTok | 2020 | Reset de mot de passe → prise de compte en 1 clic |

### Les 3 ingredients d'une attaque CSRF

1. **Action pertinente** pour l'attaquant (changement d'email, transfert d'argent...)
2. **Session cookie** utilisee pour l'authentification
3. **Aucun parametre imprevisible** (l'attaquant doit pouvoir deviner la requete)

> Important : une attaque CSRF ne requiert pas necessairement de JavaScript, et l'attaquant n'a pas besoin d'acces a votre site.

### Comment ca marche

1. L'utilisateur se connecte et recoit un cookie de session
2. Il visite un site malicieux en restant authentifie
3. Le site malicieux envoie une requete vers l'application cible
4. L'application traite la requete comme si elle venait de l'utilisateur

```javascript
// Exemple d'attaque CSRF sans JS
function createAndSubmitForm() {
  const form = document.createElement('form');
  form.method = 'POST';
  form.action = 'https://example.com/delete-account';
  form.style.display = 'none';
  document.body.appendChild(form);
  form.submit();
}
createAndSubmitForm();
```

### Techniques de protection

#### 1. CSRF Tokens

Token aleatoire genere par le serveur. Si absent ou invalide → requete rejetee.

```html
<!-- Dans un formulaire -->
<input type="hidden" name="csrf" value="3964ccc5b64f54696134..." required />

<!-- Pour les apps AJAX, dans un meta tag -->
<meta name="csrf-token" content="3964ccc5b64f546961343c57cf">
```

Bonnes pratiques :

- Ne jamais mettre le token CSRF dans une requete GET (visible dans les logs)
- Per-request tokens > per-session tokens (plus securise)
- Le token dans le body/header est preferable a l'URL

#### 2. SameSite Cookies

Utiliser `SameSite=Lax` ou `Strict` pour limiter les cookies aux requetes same-site.

#### 3. Double-Signed Cookie Pattern

Alternative aux CSRF tokens : un second cookie validant la session.

#### 4. Validation basee sur le Referer

Verifier que la requete vient de votre domaine. Peu fiable seul (peut etre omis).

#### 5. UX comme mecanisme de securite

- Re-authentification pour les actions sensibles
- 2FA pour les actions importantes
- CAPTCHA (si vous detestez vos utilisateurs)

> Note : CORS n'est PAS un mecanisme de protection CSRF. Les POST depuis des formulaires ne sont pas couverts par CORS.

---

## 6. CORS — Cross-Origin Resource Sharing

Mecanisme de securite des navigateurs pour permettre (ou restreindre) les requetes cross-origin de maniere controlee.

Objectif : contourner la Same Origin Policy de facon securisee.
PSA : CORS ≠ protection CSRF.

### Requetes "simples" (pas de preflight)

Une requete est "simple" si :

- `Content-Type` est `application/x-www-form-urlencoded`, `multipart/form-data` ou `text/plain`
- Methode est `GET`, `POST` ou `HEAD`

### Preflight (requetes complexes)

Pour les methodes non-simples (PUT, DELETE, headers custom), le navigateur envoie d'abord une requete OPTIONS.

### Headers CORS importants

| Header | Description |
|--------|-------------|
| `Access-Control-Allow-Origin` | Domaines autorises a acceder aux ressources |
| `Access-Control-Allow-Methods` | Methodes autorisees |
| `Access-Control-Allow-Headers` | Headers HTTP autorises dans la vraie requete |
| `Access-Control-Allow-Credentials` | Si `true`, cookies et headers d'auth inclus dans les requetes cross-origin |

### Anti-patterns CORS

- Ne pas remplacer les methodes soumises a CORS (PUT, DELETE) par des GET/POST "pour simplifier"
- Ne pas utiliser des wildcards si vous avez besoin de `credentials` — `Access-Control-Allow-Origin: *` et `credentials: true` sont incompatibles
- Ne pas essayer de supporter un tableau d'origines via regex bancal

---

## 7. Request Security Headers (sec-)

Tout header commencant par `sec-` ne peut pas etre modifie par JavaScript → fiable cote serveur.

| Header | Valeurs possibles |
|--------|-------------------|
| `sec-fetch-site` | `cross-site`, `same-site`, `same-origin`, `none` |
| `sec-fetch-dest` | `empty`, `image`, `worker`, `document`, `iframe`... |
| `sec-fetch-user` | `?1` (booleen, seulement quand vrai) |
| `sec-fetch-mode` | `cors`, `navigate`, `no-cors`, `same-origin`, `websocket` |

**`sec-fetch-site`** :

- `cross-site` : initiateur et serveur ont une origine ET un site differents
- `same-site` : meme site, possiblement origines differentes
- `same-origin` : meme origine exacte
- `none` : l'utilisateur a tape l'URL / ouvert un marque-page

---

## 8. XSS — Cross-Site Scripting

### Definition

Type d'injection ou des scripts malicieux sont injectes dans des sites web de confiance. Le script s'execute dans le contexte de la session de la victime.

### 3 types de XSS

| Type | Description |
|------|-------------|
| **Stored** | Le payload malicieux est stocke en base de donnees |
| **Reflected** | Le payload est glisse dans l'URL ou les query params |
| **DOM-based** | Le payload est injecte dans le DOM (ex: champ de saisie qui modifie la page) |

### XSS vs CSRF

- **CSRF** : l'attaquant fait faire des actions a la victime depuis un autre site
- **XSS** : l'attaquant execute du code sur le site lui-meme ("the call is coming from inside the house")

### Cas reels notables

| Incident | Annee | Impact |
|----------|-------|--------|
| Samy Worm (MySpace) | 2005 | 1M+ profils infectes en 20h, auto-propagation via CSS+JS |
| Twitter onmouseover worm | 2009 | Worm via l'evenement `onmouseover` sur des liens |
| TweetDeck Worm | 2014 | Propagation ultra-rapide |
| eBay | 2015-2016 | Parametre `redirectTo` non valide |
| McDonald's | 2017 | Faille Angular sandbox + decryptage de MDP cote client |
| British Airways | 2018 | 500k clients affectes via librairie Feedify compromise, 250k CB volees |
| Fortnite | 2019 | Page legacy vulnerable, toutes les donnees utilisateurs exposees |
| CIA | 2011 | Oui, la CIA |
| GitLab | 2024 | CVE-2024-4835, plugin VS Code, exfiltration d'infos sensibles |

### Comment ca marche

1. **Injection** : script malicieux via champ utilisateur (commentaires, etc.)
2. **Execution** : le script s'execute dans le contexte de la session victime
3. **Data theft** : vol de cookies, tokens de session, manipulation du DOM

### Safe Sinks vs Unsafe Sinks

Un safe sink est un endroit ou mettre des donnees sans risque d'execution de code.

**Safe sinks (surs)** :

```javascript
element.textContent = userInput      // ✅ Safe
element.insertAdjacentText(...)      // ✅ Safe
element.className = userInput        // ✅ Safe
element.setAttribute(name, val)      // ✅ Safe
element.value = userInput            // ✅ Safe
document.createTextNode(userInput)   // ✅ Safe
document.createElement(tag)         // ✅ Safe
```

**Unsafe sinks (dangereux)** :

```javascript
element.innerHTML = userInput        // ❌ DANGER
document.write(userInput)           // ❌ DANGER
eval(userInput)                     // ❌ DANGER
```

### Protection contre XSS

1. **Validation et sanitisation des inputs** — toujours cote serveur ET client
2. **Output encoding** — encoder les donnees avant de les afficher (HTML, JS, URL contexts)
3. **Safe methods** — eviter `innerHTML`, `document.write`
4. **Frameworks** — React, Svelte, Vue auto-escape le contenu par defaut
5. **DOMPurify** — library pour sanitiser le HTML si vous devez en afficher
6. **Content Security Policy (CSP)** — deuxieme couche de defense

---

## 9. Content Security Policy (CSP)

Feature de securite qui permet de controler quelles ressources le navigateur est autorise a charger. CSP = deuxieme couche de defense, pas un substitut a la sanitisation.

### Fonctionnement

- **Allowlist de domaines** : specifier quels domaines peuvent charger des ressources
- **Directives** : controler le type de ressources (`script-src`, `style-src`, `img-src`...)
- **Report violations** : configurer une URI pour reporter les violations

### Implementation

```
# Via HTTP header
Content-Security-Policy: script-src 'self' https://trusted.cdn.com

# Via meta tag HTML
<meta http-equiv="Content-Security-Policy" content="script-src 'self' https://trusted.cdn.com">
```

### Strict CSP

```
block-all-mixed-content;
default-src 'none';
script-src 'nonce-rAnd0m' 'strict-dynamic';
base-uri 'self';
```

| Directive | Signification |
|-----------|---------------|
| `default-src 'none'` | Aucune source autorisee par defaut |
| `script-src 'nonce-xyz'` | Scripts autorises uniquement avec le nonce correspondant |
| `'strict-dynamic'` | Les scripts autorises peuvent charger d'autres scripts dynamiquement |
| `base-uri 'self'` | Restreint l'element `<base>` pour eviter la manipulation des sources |
| `block-all-mixed-content` | Interdit le chargement de ressources HTTP sur une page HTTPS |

### Nonce

Un nonce = "number used once" = token a usage unique.

```html
<script nonce="rAnd0m"></script>
<style nonce="rAnd0m"></style>
```

Avantages : headers legers, pas besoin de mise a jour si le fichier change.
Inconvenient : impossible de cacher les pages HTML (le nonce change a chaque requete).

### Hash

Alternative au nonce — le hash du fichier JS/CSS est dans le header.

```html
Content-Security-Policy: default-src 'none'; script-src 'sha256-/JqT3...'
<script src="jquery.min.js" integrity="sha256-/JqT3..." crossorigin="anonymous"></script>
```

Avantage : pas d'infrastructure speciale.
Inconvenient : si le fichier change (ou si le CDN le modifie), ca casse.

### Applications legacy

Utiliser `Content-Security-Policy-Report-Only` pour logger les violations sans les bloquer → permet de prioriser le travail.

> PSA : Ne pas utiliser `X-Content-Security-Policy` ou `X-WebKit-CSP` (obsoletes).

---

## 10. Clickjacking

### Definition

Technique ou un attaquant trompe un utilisateur en lui faisant cliquer sur quelque chose de different de ce qu'il voit.

### Comment ca marche

1. L'attaquant embed la page cible dans un `<iframe>` sur son site malicieux
2. L'iframe est rendu invisible (opacity: 0) ou positionne derriere du contenu
3. L'attaquant place des boutons trompeurs par-dessus
4. L'utilisateur croit cliquer sur le bouton visible mais clique en realite sur l'iframe

### Protection

#### X-Frame-Options

```html
<meta http-equiv="X-Frame-Options" content="DENY">
<meta http-equiv="X-Frame-Options" content="SAMEORIGIN">
```

```javascript
// Express
app.use((req, res, next) => {
  res.setHeader('X-Frame-Options', 'DENY');
  next();
});
```

| Valeur | Effet |
|--------|-------|
| `DENY` | Interdit tout embedding dans un iframe |
| `SAMEORIGIN` | Autorise uniquement les iframes du meme site |

#### CSP frame-ancestors

```html
<meta http-equiv="Content-Security-Policy" content="frame-ancestors 'self' https://trustedorigin.com">
```

---

## 11. postMessage

API pour la communication cross-origin securisee entre fenetres/iframes.

### Vulnerabilites

Deux modes de defaillance :

1. Accepter les messages de n'importe quelle origine : `targetOrigin = '*'`
2. Faire confiance au payload sans validation

### Pattern securise

```javascript
window.addEventListener('message', (event) => {
  // Verifier l'origine TOUJOURS
  if (event.origin !== 'https://place-that-i-trust.com') {
    return; // Ignorer les messages non-trusted
  }
  const data = sanitize(event.data); // Toujours sanitiser
  // Traitement...
});
```

---

## 12. Data Encryption

### Pieges classiques

- Pas de chiffrement : infos sensibles (MDP, CB) stockees en clair
- Mauvais algorithmes : algos obsoletes ou casses
- Cles mal gerees : hardcodees dans le code source
- Pas de HTTPS : donnees non chiffrees en transit

### Bonnes pratiques

- Chiffrer avec des algorithmes forts (AES-256)
- Utiliser TLS (HTTPS) pour les donnees en transit
- Stocker les cles de facon securisee (pas en variables d'env, pas dans le code)
- Ne pas "roll your own crypto"

> **PSA : Ne pas chiffrer les mots de passe**
> → Les hacher et saler (bcrypt, argon2). Le chiffrement est reversible, le hash non.

---

## 13. JSON Web Tokens (JWT)

Prononce "jot". Moyen compact et URL-safe de representer des claims entre deux parties.

### Anatomie

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.        <- Header (base64)
eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6...       <- Payload (base64)
SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c  <- Signature
```

| Partie | Contenu |
|--------|---------|
| Header | Type du token + algorithme de signature (HS256, RS256...) |
| Payload | Claims (donnees) : user ID, roles, permissions... |
| Signature | Garantit que le token n'a pas ete altere |

### Types de claims

- **Registered** : standardises, non obligatoires (ex: `iss` = issuer)
- **Public** : definis par l'utilisateur, enregistres avec IANA
- **Private** : custom claims pour des usages specifiques

### JWT vs Session ID

| | JWT | Session ID |
|-|-----|------------|
| Stockage | Donnees dans le token | Donnees cote serveur |
| Stateless | ✅ Oui | ❌ Non (stateful) |
| Scalabilite | ✅ Facile (cloud, load balancing) | ⚠️ Necessite session partagee |
| Securite | ⚠️ Volable si stocke non-securise, XSS | ✅ Donnees sur le serveur, mais sensible au hijacking |
| Revocation | ⚠️ Besoin d'une denylist | ✅ Facile |
| Expiration | Auto-geree par le token | Geree par le serveur |

### Vulnerabilite algorithmique

L'attaquant peut modifier le header JWT pour specifier `alg: 'none'` et supprimer la signature, bypassant la verification.
→ Toujours valider l'algorithme cote serveur.

### Stockage des JWT

| Lieu | Avantages | Inconvenients |
|------|-----------|---------------|
| Local Storage | Simple, persistant | Vulnerable XSS, pas d'HttpOnly |
| Session Storage | Isolation par onglet | Efface a la fermeture, XSS aussi |
| Cookie | HttpOnly, Secure, scoping | Taille limitee (4KB), vulnerable CSRF si mal configure |
| En memoire | Safe XSS si script non compromis | Perdu au rechargement |

### Best Practices JWT

1. Stocker dans des cookies `HttpOnly + Secure + SameSite=Lax/Strict`
2. Limiter la duree de vie : 15 min a 1h
3. Utiliser des refresh tokens pour maintenir la session
4. Ne pas stocker d'infos sensibles dans le payload (decodable facilement)
5. Definir le scope et l'audience du token
6. Logger et monitorer l'usage des tokens
7. Audits de securite reguliers

---

## 14. SQL Injection

### Principe

L'attaquant injecte des commandes SQL dans un champ de saisie pour manipuler la base de donnees.

### Protection

- **Prepared statements / parameterized queries** — la solution principale
- ORM avec requetes parametrees
- Valider et sanitiser les inputs
- Principe du moindre privilege sur les comptes DB
- Ne jamais construire des requetes SQL par concatenation de strings

---

## 15. Strict-Transport-Security (HSTS)

Header qui force tout le trafic a passer par HTTPS.

```
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
```

### Preload HSTS

Soumettre son domaine a la liste preload des navigateurs → le navigateur sait en avance que votre site est HTTPS-only, sans avoir besoin d'une premiere requete HTTP.

> ⚠️ Warning officiel Chrome : "Inclusion in the preload list cannot really be undone. You can request to be removed, but it will take months." → N'activer que si vous etes sur de supporter HTTPS a long terme.

---

## Resume des bonnes pratiques

### Cookies

- `HttpOnly` + `Secure` + `SameSite=Lax` (minimum)
- `SameSite=Strict` pour les actions sensibles

### Authentification

- Hacher les mots de passe (bcrypt/argon2), ne pas chiffrer
- JWT dans des cookies `HttpOnly`, duree courte + refresh tokens
- 2FA pour les actions critiques

### XSS

- Sanitiser les inputs utilisateur (DOMPurify)
- Utiliser les safe sinks (`textContent` > `innerHTML`)
- CSP comme deuxieme couche de defense

### CSRF

- CSRF tokens dans les formulaires
- `SameSite=Lax/Strict` sur les cookies
- Politique CORS stricte

### Infrastructure

- HTTPS partout (HSTS)
- Mettre a jour les dependances regulierement
- Principe du moindre privilege
- Ne pas tourner en root

### Frameworks

React, Svelte, Vue protegent par defaut contre XSS via l'auto-escape. Mais comprendre les mecanismes reste essentiel pour ne pas creer de failles accidentellement (`innerHTML`, `dangerouslySetInnerHTML`, etc.).

---

*Genere a partir du cours Frontend Masters "Web Security v2" de Steve Kinney*
*Repo : https://github.com/stevekinney/web-security*

## Why It's Interesting
Essential knowledge for building secure web applications.

## Key Takeaways
- Common web vulnerabilities (XSS, CSRF, etc.)
- Defense mechanisms and best practices
- Authentication and authorization
- Secure coding patterns

## Tags
