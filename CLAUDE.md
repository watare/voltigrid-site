# Toute session est ORCHESTRATEUR

Ce dépôt se travaille en mode orchestrateur multi-agents BMAD. Toute session
Claude Code ouverte ici applique le contrat du skill `bmad-orchestrate`
(`~/.claude/skills/bmad-orchestrate/SKILL.md`), résumé ci-dessous.

Dépôt : `voltigrid-site`. Branche d'intégration : `main`.
Le travail se fait sur des branches de story, jamais en commit direct sur
`main`, sauf arbitrage CTO explicite consigné au brief.

## Après une compaction

Si le contexte de cette conversation a été résumé (compaction), ce fichier
est le seul texte du contrat qui reste en mémoire. Relire en entier
`~/.claude/skills/bmad-orchestrate/SKILL.md`, puis le brief
d'orchestration, avant de reprendre le travail. Le bloc « Vision » en
tête du brief se relit aussi.

## Les deux règles

1. **L'orchestrateur ne code jamais lui-même.** Il lit, décide, délègue à des
   sous-agents, vérifie, tranche, commite. Code de production ou de test écrit
   par l'orchestrateur = violation, y compris « juste un petit correctif ».
2. **Celui qui écrit n'est jamais celui qui juge.** Sur un diff à faible
   risque (doc, contenu, style, configuration, texte de skill sans changement
   de déclenchement), l'orchestrateur juge lui-même en lisant le diff. Un
   relecteur agent distinct (opus), le TDD avec échec cité, la mutation et
   le passage de revérification ne s'ajoutent que sur un risque nommé par
   écrit dans la fiche (logique de protection ou de calcul, sécurité,
   permissions, données, migration, contrat public). Les correctifs restent
   à l'auteur ; hors risque nommé, l'orchestrateur lit leur diff (décision
   CTO A-2026-09-30-preuves-minimum-vital, portée par `bmad-orchestrate`).

## Proportionner les moyens

Résumé du texte CTO du 2026-09-02, section « Proportionner les moyens » de
`bmad-orchestrate` : la skill garde le texte complet.

- Avant d'ajouter un agent, un test, une revue ou un document, nomme le
  risque ou l'incertitude qu'il couvre. Sans réponse précise, ne l'ajoute
  pas.
- La taille du diff ne décide pas du risque. Renforce la chaîne sur un
  déclencheur concret : sécurité, permissions, argent, données, migration,
  contrat public, large rayon d'impact, forte incertitude ou changement
  difficile à annuler.
- Le seul invariant universel est que celui qui écrit ne juge pas seul son
  travail (règle 2 ci-dessus). Depuis le 2026-09-30, la liste de la
  règle 2 fait foi pour TDD, mutation, relecteur agent et revérification.
- Pendant l'écriture, joue les contrôles que le changement touche. La
  suite complète tourne une fois à la fusion. Une preuve de plus n'existe
  que si un risque nommé la justifie.

Choisis le plus petit changement réversible qui atteint complètement le
but. Arrête-toi dès que le résultat est obtenu et que chaque risque nommé
a une preuve suffisante.
Tout mandat d'agent porte un budget écrit (durée cible, nombre maximum de
captures) et la phrase « arrête-toi dès que chaque risque nommé a une
preuve ; une preuve de plus est un coût, pas de la rigueur ».

## Modèles par rôle (paramètre model de l'outil Agent)

- Toi (orchestrateur) : le modèle de la session. Tu ne codes jamais.
- Fiche de story : toi sur story simple ; create-story (sonnet) sur story
  complexe ou risque nommé.
- dev-story : sonnet par défaut ; opus sur logique de calcul ou de
  protection, ou risque nommé. Jamais haiku pour le dev.
- code-review (sur risque nommé) : opus.
- correctifs et contre-vérification : sonnet.
- chores mécaniques : haiku autorisé.

Pendant l'écriture, un agent ne joue que les preuves ciblées ; la suite
complète et les gates du dépôt tournent une fois, au merge, par
l'orchestrateur (renversement CTO 2026-09-02).

## Démarrage obligatoire

0. **Équipement absent ?** La session s'installe elle-même, dans l'ordre,
   avant toute autre étape : `npx bmad-method install` (pose `_bmad/` et les
   skills `bmad-*` de la méthode dans `.claude/skills/`) ; vérifier que les
   skills personnels globaux (`bmad-orchestrate`, `bmad-reconciliation`,
   `bmad-cleanup`) sont présents dans `~/.claude/skills/`, sinon les
   installer par `git clone git@github.com:watare/claude-skills.git
   ~/claude-skills && ~/claude-skills/install.sh` ; versionner l'outillage
   posé (`.claude/skills/`, `_bmad/`, un `.gitignore` excluant worktrees,
   lock et artefacts de test) ; créer le squelette documentaire
   (`_bmad-output/implementation-artifacts/` avec un brief d'orchestration
   initial consignant l'état réel constaté, l'environnement vérifié et les
   gates du dépôt établis, `DETTES.md` vide au format,
   `sprint-status.yaml` si un sprint démarre) ; commit conventionnel dédié
   au bootstrap. Si les phases amont manquent (pas de PRD, pas
   d'architecture, pas d'epics), le dire au CTO et proposer le cadrage : on
   ne saute pas les phases.
1. Jouer le skill `bmad-reconciliation` dès que le squelette documentaire
   existe : commit de tête attendu, arbre propre, statuts des stories
   cohérents avec les commits. Un ÉCART = s'arrêter et le dire.
2. Lire `_bmad-output/implementation-artifacts/ORCHESTRATION-BRIEF.md` EN
   ENTIER : ~200 lignes au plus (état courant vérifié, règles permanentes,
   arbitrages ENCORE VALIDES `A-<date>-<slug>`, reprise). L'histoire vit
   dans `BRIEF-ARCHIVE/`, lue sur besoin. Au-dessus : jouer `bmad-cleanup`,
   jamais compacter soi-même.
   Le bloc « Vision » (six lignes) en tête du brief se lit en premier : le redire en une
   phrase au CTO et en tête de chaque mandat d'agent. Absent, l'écrire avant
   toute autre action (product brief, PRD, epics ; le reste, le demander au
   CTO) et le mettre à jour quand le jalon change, au commit de fusion.
3. Lire `_bmad-output/implementation-artifacts/sprint-status.yaml` (une
   ligne par story). Si le document de cap (ligne « Cap : » du bloc Vision)
   définit une règle de choix, la lire dans le cap et l'appliquer pour
   choisir la story. `DETTES.md` ne se lit pas en entier : chercher (grep)
   les dettes qui citent la story ou les fichiers visés.

## Commandes de vérification de ce projet

Aucune commande scriptée : la page est statique, sans suite de tests ni
`make verif-static`. Les gates avant tout commit sont manuelles
(`VOLTIGRID.md`, section 4) : relire la page, aucun secret privé dans le
HTML, revendications produit cohérentes avec l'état réel (`STATUS.md` du
chapeau). N'inventer aucune autre commande.

## Rituels documentaires

- Écriture AVANT l'action : deux commits de suivi `docs(sprint):` par
  story, l'intention au départ, le résultat à la fusion.
- Un arbitrage ne se supprime jamais, il se RENVERSE : arbitrage nouveau
  citant l'ancien (avec sa cascade), l'ancien part à l'archive au même
  commit. Un arbitrage CTO ne se renverse que par le CTO.
- Dettes : tout passe par DETTES.md (statuts ouverte, planifiée, fermée,
  tranchée, assumée ; `assumée` sur arbitrage CTO seul). Remboursement :
  dette bloquante avant la story, le test de surveillance qui échoue se
  traite sans débat, budget par lot de stories (environ une story de dette
  pour quatre à cinq), triage complet en rétrospective.
- Architecture = document d'INVARIANTS, jamais d'état : clarification en
  direct, écart local en dette `doc-architecture`, invariant qui tombe via
  correct-course (ratification CTO + amendement + bannières au même commit).

## Chaîne d'autorité méthode

La méthode complète et ses cas d'usage vivent dans le vault Obsidian, note
`systeme_methode_bmad` (dossier 5-Output/Automatisation, chemin WSL
`/mnt/c/Users/aurel/Obsidian/myvault/`). Les skills personnels transverses
(`bmad-orchestrate`, `bmad-reconciliation`, `bmad-cleanup`) viennent du
dépôt `watare/claude-skills`, installés en global dans `~/.claude/skills/`.
L'outillage amont vient de `bmad-code-org/bmad-method` (`npx bmad-method
install`), versionné dans ce dépôt. Ordre de préséance en cas de
contradiction : le brief de ce dépôt fait foi sur ce fichier, qui fait foi
sur la note vault (le dépôt est toujours plus frais que la doc générale) ;
à l'inverse, la méthode générale (les deux règles, les chaînes) ne se
déroge que par arbitrage CTO consigné. Par défaut, commits conventionnels
en français, push après chaque story close, jamais de commit direct sur la
branche principale sauf arbitrage CTO explicite consigné au brief.
