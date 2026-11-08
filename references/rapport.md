# Contrat des livrables

Produire audit-cybersecurite.md, audit-cybersecurite.json et audit-cybersecurite.html. Adapter les noms pour éviter d'écraser un autre audit. En cas de limitation de création de fichiers, fournir le contenu disponible et nommer le livrable manquant.

## Rapport humain

1. Synthèse décisionnelle : risques principaux, limites d'assurance, prochaines actions.
2. Périmètre : cibles, exclusions, environnement, rôles, date et fuseau, nature des autorisations actives.
3. Méthode : références/versions/URL et date de consultation, outils réellement exécutés et versions, erreurs rencontrées.
4. Couverture : tableau contrôle, cible, méthode, statut, preuve et limitation. Calculer une couverture uniquement avec un dénominateur défini ; ne pas l'appeler score de sécurité.
5. Constats : identifiant stable, titre, actif, emplacement, statut, gravité, confiance, référence OWASP/CWE vérifiée, prérequis, preuve expurgée, reproduction minimale autorisée, impact, correction, effort et test de validation.
6. Plan : P0 immédiat pour exposition critique confirmée ; P1 court terme pour risques élevés ; P2 planifié ; P3 durcissement. Ajuster au métier et ne pas confondre priorité et CVSS.
7. Non testé : raison, conséquence et action nécessaire pour vérifier.
8. Retest : date, correction observée, résultat et risque résiduel.

## Structure JSON

Utiliser un objet comportant :

- schema_version : "1.0"
- audit : date ISO 8601 avec fuseau, périmètre, exclusions, modes, autorisation, références et outils.
- coverage : tableau d'objets {id, control, target, method, status, evidence_refs, limitation}.
- findings : tableau d'objets {id, title, target, location, status, severity, confidence, taxonomy, prerequisites, evidence, reproduction, impact, remediation, validation, priority, effort, retest}.
- limitations : tableau de chaînes ou objets explicites.
- action_plan : tableau {finding_id, priority, action, validation}.

Valeurs status : confirmed, suspected, passed_sample, not_tested, not_applicable.
Valeurs severity : critical, high, medium, low, informational ; valeur null si non évaluée.
Valeurs confidence : high, medium, low.
Utiliser null ou un tableau vide pour les informations absentes ; ne pas inventer d'URL, preuve, score ni propriétaire d'action. Pour taxonomy, stocker référentiel, version, identifiant et source vérifiés. Pour evidence, stocker observation, date et référence expurgée ; aucune donnée d'authentification.

## Contrôle qualité final

Valider la syntaxe JSON et la cohérence des identifiants et statuts entre les trois rapports. Échapper dans HTML URL, code, titres et extraits ; aucun script distant ni ressource obligatoire externe. Vérifier que tous les liens locaux de preuve existent, que les secrets sont masqués, que les contrôles non effectués restent visibles et que les correctifs correspondent aux versions réellement utilisées.
