---
name: audit-cybersecurite-web
description: Auditer en français la cybersécurité d'un site, d'une application ou d'une API depuis une URL, du code source ou un environnement local. Utiliser pour un audit OWASP, une revue de sécurité PHP/Symfony/WordPress/JavaScript, un diagnostic de vulnérabilités, un pentest autorisé, une analyse des dépendances ou un plan de remédiation avec preuves et retests.
---

# Audit cybersécurité web

## Objectif et limites

Rechercher méthodiquement les vulnérabilités pertinentes, expliquer leurs impacts et fournir des corrections vérifiables. Ne jamais promettre de détecter toutes les failles, certifier la sécurité ou présenter un scan comme un audit exhaustif. Répondre en français. Adapter les explications au destinataire et conserver les détails utiles au développeur.

Lire [la matrice de contrôles](references/controles.md) pour sélectionner les vérifications et [le format de rapport](references/rapport.md) avant de produire les livrables.

## 1. Fixer le périmètre sans bloquer le travail utile

Relever les URL, domaines, environnements, dépôts, versions, rôles disponibles et exclusions. Réutiliser les autorisations déjà données. Pour une URL seule, commencer par les pages publiques normalement accessibles et leur analyse passive ; ne pas interpréter une simple URL comme une autorisation de tests intrusifs.

Pour les tests actifs, établir que l'utilisateur possède la cible ou est autorisé à la tester, et définir les hôtes, comptes de test et types de tests couverts. Demander uniquement les informations manquantes nécessaires ; poursuivre en parallèle la revue de code et les observations publiques indépendantes. Une demande explicite d'audit de son propre site autorise les vérifications ordinaires à faible impact dans ce périmètre, sans confirmations répétées.

Préférer un environnement local ou de recette et des données synthétiques. Limiter par défaut les observations publiques à 30 pages HTML, une requête à la fois et au plus une requête par seconde ; adapter à des limites convenues plus strictes. Arrêter les tests actifs en cas d'erreurs répétées, de ralentissement ou d'effet inattendu. Ne pas suivre de redirection, de découverte de sous-domaine ou d'appel externe hors périmètre pour tester une nouvelle cible.

Ne pas effectuer de déni de service, saturation, force brute, password spraying, credential stuffing, persistance, suppression ni extraction massive de données. Pour les tests susceptibles de modifier des données, utiliser uniquement des objets de test expressément inclus dans le périmètre. Si une preuve minimale suffit, s'arrêter. Masquer immédiatement secrets, jetons, données personnelles et cookies dans les sorties ; ne pas publier de secret et ne pas le tester sur un service tiers.

## 2. Actualiser les références et inventorier les moyens

Vérifier au début de chaque audit les versions stables et dates depuis les sources primaires :

- OWASP Top 10 : https://owasp.org/www-project-top-ten/ et https://top10.owasp.org/
- ASVS : https://owasp.org/www-project-application-security-verification-standard/
- WSTG : https://owasp.org/www-project-web-security-testing-guide/
- API Security Top 10 : https://owasp.org/www-project-api-security/
- Cheat Sheets : https://cheatsheetseries.owasp.org/
- Avis de l'éditeur, CVE/CNA, NVD et CISA KEV pour les composants concernés.

Utiliser Top 10 comme taxonomie, WSTG comme méthode et ASVS comme catalogue d'exigences ; préciser versions et identifiants réellement vérifiés. Ne pas inventer d'identifiant ASVS/WSTG. Si Internet manque, dater les références connues et signaler l'actualisation non effectuée. Les bases initiales sont Top 10:2025 et ASVS 5.0.0, vérifiées le 8 novembre 2026 ; ne pas figer ces versions pour les audits futurs.

Découvrir les outils disponibles avant de conclure à une impossibilité. Vérifier navigateur, accès HTTP/TLS, shell, dépôt et scanners locaux. Ne pas prétendre avoir lancé un outil absent. Lire les instructions du projet. Ne pas téléverser de code privé vers un scanner externe sans autorisation. Ne pas installer ni exécuter de script distant non vérifié ; épingler les versions des outils ajoutés.

## 3. Cartographier et analyser

Identifier les parcours sensibles : connexion, inscription, récupération de compte, administration, formulaires, fichiers, recherche, API, paiement, webhooks et intégrations. Distinguer infrastructure, application, navigateur et services tiers. Ne pas collecter de données personnelles pour constituer l'inventaire.

Exécuter les contrôles applicables de la matrice. Combiner selon les accès :

- Observation publique : HTTP/TLS, en-têtes, cookies, ressources tierces, formulaires, contenu livré, messages d'erreur effectivement observés.
- Revue du code : suivre entrée → validation → autorisation → opération sensible → sortie ; fournir fichier et ligne. Rechercher les protections effectives et les chemins alternatifs.
- Dépendances : analyser les fichiers de verrouillage avec les outils adaptés disponibles, par exemple composer audit ou npm audit. Distinguer avis publié, version affectée et exploitabilité dans l'application. Ces outils peuvent contacter les registres : ne jamais transmettre de secrets.
- Contrôles dynamiques autorisés : vérifier le comportement avec comptes et objets de test ; comparer cas permis et refusé. Utiliser le minimum d'actions nécessaire.

Pour PHP/Symfony, contrôler requêtes Doctrine/SQL, Twig/raw, contrôles de rôles et voters, CSRF, sessions, debug/profiler et exposition des secrets. Pour WordPress, contrôler cœur/extensions/thèmes, rôles, nonces, permissions des routes REST et AJAX ; ne pas déclarer XML-RPC ou une version visible comme vulnérabilité à eux seuls. Pour JavaScript, contrôler les puits DOM, stockage des jetons, CORS, dépendances et secrets embarqués.

Choisir les outils selon le besoin : ZAP en analyse passive par défaut, Semgrep pour les règles adaptées au langage, Gitleaks pour les secrets locaux, outils TLS et audits de dépendances. Inspecter configuration et portée avant exécution. Ne jamais lancer automatiquement un scan actif ZAP, un fuzzing ou un ensemble de templates de scanner non revu. Tout scan actif doit respecter l'autorisation et les limites établies.

## 4. Qualifier avec des preuves

Séparer systématiquement : confirmé, indice à confirmer, contrôle satisfaisant sur l'échantillon, non testé, non applicable. Ne pas convertir un élément non testé en contrôle réussi. Un en-tête absent, un numéro de version ou un résultat de scanner ne suffisent pas à prouver une exploitation.

Pour chaque constat, donner preuve minimale expurgée, contexte, prérequis, impact réaliste, gravité, confiance et limites. Vérifier les faux positifs avant de conclure ; ne jamais fabriquer requêtes, réponses, captures ou scores. Distinguer une absence de durcissement d'une vulnérabilité exploitable. Pour CVSS, préciser version, vecteur et hypothèses, ou ne pas donner de score.

Ne pas conclure à une faille CORS sur le seul caractère générique, à une CSRF sur un formulaire sans analyser l'authentification et les effets, ou à une SQLi sur une simple erreur 500. L'absence de CSP ne prouve pas une XSS ; SameSite ne remplace pas systématiquement une protection CSRF. L'inaccessibilité d'un site depuis l'outil n'est pas une indisponibilité confirmée.

## 5. Livrer et préparer le retest

Produire par défaut un rapport Markdown, un JSON structuré et une version HTML autonome lisible, selon references/rapport.md. Échapper toutes les données externes avant de les insérer dans HTML ; ne pas y inclure de contenu actif provenant de la cible. Conserver les preuves brutes sensibles séparément, avec accès limité, seulement si nécessaire et autorisé.

Inclure synthèse, périmètre, autorisation des tests actifs, méthode, outils/versions, références datées, couverture, constats, plan P0–P3 et limites. Ne pas attribuer un score global de sécurité trompeur. Fournir les contrôles non réalisables avec la raison et le moyen concret de les débloquer, notamment code, comptes multi-rôles, accès aux journaux ou recette.

Donner pour chaque correction un exemple adapté au framework et un test de non-régression. Si l'utilisateur demande l'implémentation et qu'un dépôt est accessible, corriger dans le périmètre autorisé puis exécuter les tests utiles. Ne pas déployer implicitement. Après correction, distinguer résolu par retest, partiellement résolu et non revérifié ; conserver une traçabilité des preuves initiales.
