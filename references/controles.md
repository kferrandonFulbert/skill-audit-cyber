# Matrice de contrôles

Sélectionner les contrôles selon la surface réelle ; reporter chacun dans la couverture, même lorsqu'il n'est pas testable. Faire correspondre aux catégories de l'édition OWASP vérifiée. Inclure les exigences métier et API au-delà du Top 10.

| Domaine | Vérifications | Preuves et précautions |
|---|---|---|
| Contrôle d'accès | Autorisation côté serveur, IDOR/BOLA, BFLA, isolation locataires, restrictions champs, élévation de rôle, méthodes alternatives | Comparer des comptes et objets synthétiques autorisés ; ne pas explorer les données de vrais utilisateurs |
| Authentification | Énumération de comptes, MFA, réinitialisation, validation email, politique mots de passe, limitation des tentatives | Lire code/configuration ; ne pas provoquer de verrouillage ni mener de brute force |
| Sessions et jetons | Rotation après connexion, fixation, expiration, révocation, cookies Secure/HttpOnly/SameSite, JWT, OAuth/OIDC | Ne jamais inclure de valeur de jeton dans le rapport ; examiner aussi audience, émetteur et validation serveur |
| Injection | SQL/NoSQL, commandes, LDAP, templates, XSS stockée/réfléchie/DOM, injections d'en-têtes | Suivre les flux du code ; en recette autorisée employer des marqueurs inoffensifs, sans lecture destructive ni exfiltration |
| Requêtes et fichiers | SSRF, traversal, inclusion de fichiers, upload, archives, XXE, désérialisation | Examiner les restrictions d'URL, chemins, types, stockage et parseurs ; ne pas interroger métadonnées cloud ou réseaux internes en production |
| Navigateur | CSP, framing, types MIME, CORS, cache de contenu sensible, CSRF, stockage local, postMessage | Évaluer contexte, effets et contrôles compensatoires ; éviter les conclusions automatiques sur en-tête seul |
| Transport et cryptographie | TLS, redirection HTTPS, certificat, HSTS adapté, hachage mots de passe, aléas, chiffrement, gestion des clés | Distinguer vérification réelle et hypothèse ; ne pas affaiblir la configuration pour tester |
| Configuration | Debug, erreurs verbeuses, méthodes HTTP, permissions, pages admin, fichiers de configuration exposés | Se limiter aux chemins connus pertinents ; ne pas aspirer un dépôt ou un fichier secret exposé |
| Dépendances et chaîne logicielle | Versions verrouillées, avis de sécurité, composants inutiles, intégrité artefacts, CI/CD, permissions et secrets | Vérifier version et applicabilité ; ne pas confondre paquet ancien et CVE exploitable |
| Intégrité | Signatures webhooks, rejeu, mises à jour, sérialisation et données de confiance | Utiliser des événements de test ; vérifier authentification, intégrité et idempotence |
| Conception et logique métier | Machine à états, prix calculés serveur, quotas, concurrence, abus, règles de validation | Revoir scénarios et code ; réserver essais de concurrence et transactions à la recette autorisée |
| Journalisation et alertes | Événements sensibles, protection des logs, absence de secrets, alertes, conservation, réponse incident | Requiert accès aux configurations/journaux ; ne pas déduire l'absence d'alertes depuis une URL |
| Conditions exceptionnelles | Erreurs, échecs partiels, fail-open, rollback, limites de ressources et délais | Préférer tests unitaires et recette ; ne pas provoquer d'épuisement de ressources |
| API | Schémas, autorisations objet/propriété/fonction, inventaire, versions obsolètes, consommation d'API tierces | Comparer contrat et code ; vérifier validation des données entrantes et sortantes |

## Sources de travail

Utiliser les pages officielles indiquées dans SKILL.md et la documentation de sécurité du framework identifié. Vérifier les identifiants précis dans l'édition consultée avant de les citer. Le Top 10 est un regroupement de risques, pas une liste exhaustive de tests.

## Tests impossibles à distance

Signaler explicitement les limites sur : permissions serveur, sauvegardes/restauration, secrets hors navigateur, segmentation réseau, configuration de base de données, journalisation interne, chaîne CI/CD et exigences métier. Proposer un accès ou une vérification locale ciblée plutôt qu'une conclusion positive.
