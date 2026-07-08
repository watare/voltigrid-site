# Déployer voltigrid.com sur GitHub Pages

## 1. Créer le dépôt et uploader (5 min, sans git)

1. github.com → **New repository** → nom : `voltigrid` (public) → Create.
2. **Add file → Upload files** : glisser `index.html` + `CNAME` (ce dossier) → Commit.

## 2. Activer GitHub Pages

1. Repo → **Settings → Pages**.
2. Source : **Deploy from a branch** → branche `main`, dossier `/ (root)` → Save.
3. Le champ Custom domain doit afficher `voltigrid.com` (rempli automatiquement par le fichier CNAME). Sinon, le saisir → Save.

## 3. DNS chez ton registrar (là où tu as acheté voltigrid.com)

| Type  | Nom / hôte | Valeur                    |
|-------|-----------|---------------------------|
| A     | @         | 185.199.108.153           |
| A     | @         | 185.199.109.153           |
| A     | @         | 185.199.110.153           |
| A     | @         | 185.199.111.153           |
| CNAME | www       | `TON_USER.github.io`      |

Remplacer `TON_USER` par ton nom d'utilisateur GitHub. Propagation : de quelques minutes à 24 h.

## 4. HTTPS

Settings → Pages → cocher **Enforce HTTPS** (disponible une fois le DNS vérifié par GitHub).

## 5. Activer le formulaire (une seule fois)

Le formulaire poste vers FormSubmit. Au **premier envoi réel** depuis le site en ligne, un email d'activation arrive sur `contact@voltigrid.com` → cliquer le lien. Ensuite tous les messages arrivent directement.

Astuce anti-spam optionnelle : remplacer dans l'action du formulaire `contact@voltigrid.com` par le code aléatoire que FormSubmit fournit après activation (l'email n'est alors plus lisible dans le code source).

## Vérification finale

- https://voltigrid.com charge en HTTPS, logo et schémas OK.
- Les 4 modales s'ouvrent (View diagram / Open the map).
- Envoyer un message de test via le formulaire → activer FormSubmit → recevoir le test.
