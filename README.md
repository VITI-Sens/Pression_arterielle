# Tension

Petite appli web pour noter ses prises de tension artérielle : **systolique / diastolique / pouls**, avec date, heure et note facultative. L'historique est conservé dans une base **Supabase**.

- HTML + JS, sans étape de build ; hébergeable sur GitHub Pages
- Connexion par email + mot de passe (Supabase Auth)
- Chaque utilisateur ne voit que ses propres mesures (Row Level Security)
- Moyenne sur 7 jours
- Graphique tension (systolique/diastolique) + pouls, sur 7 j / 30 j / 90 j / tout, avec infobulle au toucher
  (pointillés : repères 135/85 mmHg, seuil de référence pour l'automesure à domicile)
- Export / import CSV (séparateur `;`, s'ouvre directement dans Excel)
- Reprise automatique des mesures saisies avec l'ancienne version (stockées sur l'appareil)

## 1. Supabase

1. Créer un projet sur [supabase.com](https://supabase.com) (région Europe conseillée).
2. **SQL Editor** → coller le contenu de `supabase/schema.sql` → *Run*.
3. **Authentication → URL Configuration** : mettre `https://<ton-compte>.github.io/tension/` dans *Site URL* (sert au lien de confirmation d'email).
4. **Project Settings → API** : copier l'URL du projet et la clé *publishable* (ou *anon*) dans `config.js`.

> La clé publishable/anon est conçue pour être publique : c'est la RLS qui protège les données.
> Ne jamais mettre la clé `service_role` / *secret* dans ce dépôt.

5. Une fois ton compte créé depuis l'appli : **Authentication → Sign In / Providers → désactiver « Allow new users to sign up »** pour que personne d'autre ne puisse créer de compte.

## 2. GitHub

```bash
git remote add origin https://github.com/<ton-compte>/tension.git
git push -u origin main
```

Puis **Settings → Pages → Deploy from a branch → `main` / `(root)`**.
L'appli est servie à `https://<ton-compte>.github.io/tension/`. Sur le téléphone : « Ajouter à l'écran d'accueil ».

> L'appli utilise des modules ES : l'ouvrir en double-cliquant sur `index.html` (`file://`) ne fonctionne pas.
> En local : `python3 -m http.server` puis http://localhost:8000.

## Structure

```
index.html            appli
config.js             URL + clé publique Supabase
manifest.json         installation sur l'écran d'accueil
supabase/schema.sql   table `mesures` + règles RLS
```
