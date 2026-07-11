# VOLTIGRID.md — carte d'identité : voltigrid-site

## 1. Rôle dans la plateforme

Vitrine publique **voltigrid.com** : plaquette de positionnement de la practice
(digitalisation/virtualisation du contrôle-commande et de la protection de poste).
Pas un site produit : aucun lien vers le portail SaaS tant que la plateforme n'est pas
ouverte (« Remote virtual lab — available soon »). Carte complète : `~/voltigrid-main/README.md`.

## 2. Comment c'est fait

Une seule page HTML statique autoportante (`index.html`, ~860 lignes) : CSS, JS et
schémas SVG inline, zéro dépendance externe. Déployée par **GitHub Pages** au push sur
`main` (OVH = registrar du domaine uniquement ; `CNAME` fixe l'apex).

```bash
# Prévisualiser
python3 -m http.server 8000   # puis http://localhost:8000
# Déployer
git push origin main          # GitHub Pages publie automatiquement
```

## 3. Contrats

**PRODUIT :** la page publique ; soumission du formulaire de contact vers Web3Forms
(`POST https://api.web3forms.com/submit`, clé publique dans `index.html`) →
contact@voltigrid.com.

**CONSOMME :** rien des autres dépôts. Quand la chaîne portail sera ouverte (ROADMAP
R10 du chapeau), ajouter le lien vitrine → portail.

## 4. Gates avant tout commit

- Relecture manuelle de la page (statique, pas de suite de tests).
- Aucun secret privé dans le HTML (la clé Web3Forms est publique par conception).
- Cohérence des revendications produit avec l'état réel (STATUS.md du chapeau).

## 5. Sources de vérité locales

- `README.md` et `DEPLOY.md` (attention : DEPLOY.md décrit encore FormSubmit alors que
  le code utilise Web3Forms — correction tracée ROADMAP R10).
- Vue d'ensemble : `~/voltigrid-main/docs/`.

## 6. Règle de maintenance

Ce fichier est mis à jour par tout chantier qui change le rôle, les contrats ou les
gates de ce dépôt — c'est un livrable du chantier, pas de la doc morte.
