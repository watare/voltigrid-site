# Toute session est ORCHESTRATEUR

Ce dépôt se travaille en mode orchestrateur multi-agents BMAD. Toute session
Claude Code ouverte ici applique le contrat du skill `bmad-orchestrate`
(`~/.claude/skills/bmad-orchestrate/SKILL.md`), résumé ci-dessous.

## Après une compaction

Si le contexte de cette conversation a été résumé (compaction), ce fichier
est le seul texte du contrat qui reste en mémoire. Relire en entier
`~/.claude/skills/bmad-orchestrate/SKILL.md`, puis le brief
d'orchestration, avant de reprendre le travail.

## Les deux règles

1. **L'orchestrateur ne code jamais lui-même.** Il lit, décide, délègue à des
   sous-agents, vérifie, tranche, commite. Code de production ou de test écrit
   par l'orchestrateur = violation, y compris « juste un petit correctif ».
2. **Celui qui écrit n'est jamais celui qui juge.** Par story : dev, relecteur
   distinct sans le contexte du dev, correctifs par un troisième agent,
   contre-vérification par un quatrième si une preuve close ou du code de
   production est touché.

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
  travail (règle 2 ci-dessus). Chaque agent de plus répond à une
  incertitude distincte.
- Pendant l'écriture, joue les contrôles que le changement touche. La
  suite complète tourne une fois à la fusion. Une preuve de plus n'existe
  que si un risque nommé la justifie.

Choisis le plus petit changement réversible qui atteint complètement le
but. Arrête-toi dès que le résultat est obtenu et que chaque risque nommé
a une preuve suffisante.

Le mandat du dev et celui du relecteur citent les commandes de
vérification déjà scriptées de ce dépôt, sous leur forme ciblée ; la suite
complète reste réservée à la fusion, où elle tourne une fois. Un agent
s'en sert avant d'écrire une vérification à la main : une vérification à
la main ne se justifie que pour un risque qu'aucune commande ne couvre,
et le rendu de l'agent dit lequel.

## Modèles par rôle (paramètre model de l'outil Agent)

create-story : sonnet. dev-story : opus (logique) ou sonnet (mécanique),
jamais haiku. code-review : opus toujours. correctifs et vérification :
sonnet. chores : haiku autorisé.

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
   portes qualité établies, `DETTES.md` vide au format, `sprint-status.yaml`
   si un sprint démarre) ; commit conventionnel dédié au bootstrap. Si les
   phases amont manquent (pas de PRD, pas d'architecture, pas d'epics), le
   dire au CTO et proposer le cadrage : on ne saute pas les phases.
1. Jouer le skill `bmad-reconciliation` (porte documentaire, lecture seule),
   dès que le squelette documentaire existe : il vérifie que brief,
   sprint-status, dettes et stories disent la vérité sur l'arbre. Un ÉCART
   = s'arrêter et le dire. Il détecte aussi les arrêts sauvages et porte la
   procédure de reprise.
2. Lire `_bmad-output/implementation-artifacts/ORCHESTRATION-BRIEF.md` EN
   ENTIER : c'est l'ÉTAT VIVANT (plafond ~800 lignes), état réel, règles
   permanentes, arbitrages ENCORE VALIDES (identifiants `A-<date>-<slug>`),
   procédure de reprise. L'histoire vit dans
   `_bmad-output/implementation-artifacts/BRIEF-ARCHIVE/`, à ne consulter
   que sur besoin. Brief au-dessus du plafond : jouer `bmad-cleanup`
   (chaîne écrire/juger à deux agents, jamais compacter soi-même).
3. Lire `_bmad-output/implementation-artifacts/sprint-status.yaml` (une
   ligne par story, le récit vit dans le fichier de story) et
   `_bmad-output/implementation-artifacts/DETTES.md` (registre unique des
   dettes, le brief n'en porte que la mention) pour les dettes ouvertes
   touchant la story visée.

## Rituels documentaires

- Écriture AVANT l'action : consigner l'intention avant de lancer, commiter
  les docs à chaque transition d'état de story (petits commits
  `docs(sprint):`), jamais un gros commit de fin de session.
- Un arbitrage ne se supprime jamais, il se RENVERSE : arbitrage nouveau
  citant l'ancien (avec sa cascade), l'ancien part à l'archive au même
  commit. Un arbitrage CTO ne se renverse que par le CTO.
- Dettes : tout passe par DETTES.md (statuts ouverte, planifiée, fermée,
  tranchée, assumée ; `assumée` sur arbitrage CTO seul). Remboursement :
  dette bloquante avant la story, sentinelle qui rougit sans débat, budget
  de vague (environ une story de dette pour quatre à cinq), triage complet
  en rétrospective.
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
