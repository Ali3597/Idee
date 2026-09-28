# Mission : conventions frontend + fichiers pour Copilot

## Périmètre
Tu n'agis que dans `frontend/`. Tu ne crées ni ne modifies aucun fichier en dehors.

## Contexte
Projet React qui n'a pas encore démarré, prévu pour durer longtemps, développé avec GitHub Copilot dans l'IDE. Documentation en français. Le référentiel est dans `frontend/docs/conventions/` :
- `README.md` : point d'entrée. Il fixe le principe directeur (« une convention doit être vérifiable automatiquement, sinon elle n'existe pas » ; une règle temporairement humaine porte la mention « vérifiée à la relecture — fragile » et nomme un défaut observable), le contexte confirmé, et les règles non négociables sous forme de tableau (ID, règle, vérification, ce qui casse sans elle).
- `01-stack.md` … `12-developpement-sans-backend.md` : les chapitres. Rédigés pour des développeurs, ils mélangent règles, historique des décisions et justifications, ce qui les rend longs.
- `DECISIONS.md` : une entrée par décision avec un format fixe (Contexte, Options comparées, Décision retenue, Pourquoi, Ce qu'on accepte de perdre, Conséquences concrètes, Vérification prévue, Réversibilité, Comment on saura qu'on s'est trompé).
- `templates/` : gabarits déjà préparés (dont `templates/config/`), non validés. Tu ne les modifies pas.

## Objectif 1 — Des chapitres qui ne contiennent que les conventions
Réécris chaque chapitre `01` à `12` pour qu'il ne contienne que des règles, au présent, chacune avec son moyen de vérification, selon le schéma déjà fixé par le README : `automatique` (outil et règle) ou `vérifiée à la relecture — fragile` (défaut observable nommé). Pas de niveau intermédiaire, pas de préférence.

Retire l'historique et les justifications. `DECISIONS.md` porte déjà ce rôle : conserve son format, n'ajoute une décision que si une justification retirée d'un chapitre n'y figure pas encore, ajoute à chaque décision un renvoi vers le chapitre qu'elle fonde (fichier › section), et mets chaque champ sur sa propre ligne au lieu d'un paragraphe unique. Ce qui n'explique aucun choix encore en vigueur est supprimé.

Format de chaque chapitre :

    # 0X — Titre
    Périmètre : une ligne (renvoi vers le chapitre concerné pour ce qui n'est pas couvert ici)
    Décisions liées : numéros dans DECISIONS.md
    ## Règles — par sous-thème ; une règle = un identifiant stable, une phrase, sa vérification
    ## Exemples — seulement ceux qui lèvent une ambiguïté, format ✅ / ❌

Identifiants : si les chapitres en ont déjà, conserve-les ; sinon attribue-en (préfixe du chapitre + numéro, ex. `STACK-03`). Ils servent aux renvois, aux revues et aux fichiers Copilot.

`README.md` reste le point d'entrée : public visé, principe directeur, contexte confirmé, règles non négociables, ordre de lecture des chapitres, procédure pour modifier une convention (chapitre, `DECISIONS.md`, puis fichiers Copilot dérivés, listés nommément). Réduis « Statut de cette livraison documentaire » à ce qui est encore vrai, en deux lignes datées.

Contraintes :
1. Aucune règle perdue, aucune valeur concrète perdue (noms, versions, chemins, limites chiffrées).
2. Aucune règle inventée. Manque ou contradiction → tu le signales avec l'étiquette [PROPOSITION], tu ne tranches pas.
3. Noms de fichiers et numérotation inchangés. Pas de fusion.
4. Une règle vit dans un seul chapitre ; ailleurs, un renvoi par identifiant.
5. Une règle est testable : un relecteur peut répondre oui/non. Si l'original est trop vague pour être précisé, garde la règle et marque [À PRÉCISER] ; ne la supprime pas toi-même.
6. Un chapitre se lit en moins de 2 minutes.
7. Même langue, même ton, même vocabulaire que l'original.

## Objectif 2 — Fichiers pour Copilot (dans `frontend/`)
Crée les fichiers qui permettent à Copilot de respecter ces conventions sans qu'on les lui rappelle. Utilise ses mécanismes standard, uniquement ceux qui apportent quelque chose ici :
- `frontend/.github/copilot-instructions.md` — priorité absolue : c'est le seul fichier lu automatiquement par tous les IDE. Court (< 150 lignes) : contexte confirmé condensé (depuis le README), règles non négociables avec leurs identifiants, commandes de vérification (lint, typecheck, tests — `TODO` si pas encore définies dans `01`/`10`), interdits, renvoi vers `docs/conventions/` pour le détail.
- `frontend/AGENTS.md` — carte du projet pour le mode agent : stack, arborescence cible, commandes, règles essentielles par thème avec renvoi vers le chapitre source, definition of done (depuis `11-checklists.md`).
- `frontend/.github/instructions/*.instructions.md` avec `applyTo` — règles qui ne concernent qu'un type de fichier (composants, tests, styles, mocks…). Seulement si ça évite de charger la règle partout.
- `frontend/.github/prompts/*.prompt.md` — prompts réutilisables pour les tâches récurrentes du projet (nouvel écran, nouveau composant, tests d'un module, revue de code contre les conventions avec sortie : fichier, ligne, identifiant de la règle enfreinte…).

Principes : la source de vérité est `docs/conventions/` ; les fichiers Copilot en dérivent et ne les recopient pas — ils contiennent ce que le modèle doit toujours avoir en tête, plus des renvois précis (chapitre et identifiant). Instructions impératives et concrètes (« fais X / ne fais pas Y »). Ne demande pas au modèle de respecter ce que l'outillage vérifie : demande-lui de lancer la vérification et de corriger ; concentre les instructions sur les règles « fragiles ». Tout en français.

## Déroulement
1. Lis tous les fichiers. Rends dans le chat : par chapitre, ce qui est règle / historique / justification ; les doublons et contradictions ; les règles sans moyen de vérification ; les [PROPOSITION] ; la liste des fichiers Copilot que tu comptes créer, avec le rôle de chacun en une ligne. N'écris rien. Attends mon GO.
2. Réécris les chapitres un par un, dans l'ordre. Après chaque chapitre, 3 lignes : règles conservées / reformulées / justifications déplacées dans `DECISIONS.md`. Termine par `README.md` et `DECISIONS.md`. Attends mon GO.
3. Crée les fichiers Copilot.
4. Vérifie : chaque instruction Copilot renvoie à un identifiant existant, tous les liens fonctionnent, rien n'a été créé hors de `frontend/`. Résume ce qui a changé et liste les [PROPOSITION] / [À PRÉCISER] restants.

Commence par l'étape 1.
