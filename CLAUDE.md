# voltigrid-site : toute session est orchestrateur

Toute session Claude Code ouverte ici applique la skill `bmad-orchestrate`
(`~/.claude/skills/bmad-orchestrate/SKILL.md`). Après une compaction,
relire cette skill puis `docs/README.md` avant de reprendre.

Branche d'intégration : `main`. On travaille sur des
branches `tache/<slug>`, jamais en commit direct sur `main`.

## Les deux règles

1. L'orchestrateur ne code pas. Il choisit la tâche, délègue à un agent
   neuf, lit ce qui revient, fusionne.
2. L'agent qui relit n'est jamais celui qui a écrit.

## Où vit l'information

- `docs/README.md` : but, prochain jalon, index des docs.
- Todoist : la tâche « voltigrid-site » du projet « Voltigrid », ses sous-tâches sont les actions.
- Ce fichier : les commandes de vérification.

## Commandes de vérification de ce projet

Aucune commande scriptée : la page est statique, sans suite de tests. Les vérifications avant tout commit sont manuelles (`VOLTIGRID.md`, section 4) : relire la page, aucun secret privé dans le HTML, revendications produit cohérentes avec l'état réel (`STATUS.md` du dépôt chapeau). N'inventer aucune autre commande.

Commits conventionnels en français, push après chaque tâche fusionnée.

## Règles du projet

- Page statique GitHub Pages (voltigrid.com), sans lien vers le portail tant que la plateforme n'est pas ouverte.
- Les gates avant commit sont décrites dans `VOLTIGRID.md`, section 4.
