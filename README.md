Mediachad
Une collection d'histoires, de sons et d'univers.

Mediachad est une application web personnelle pour suivre et noter ce que tu regardes, lis, joues et écoutes — films, séries/anime, manga, musique, jeux. Pensée comme un mix entre Letterboxd et Pinterest, dans une ambiance sombre et chaleureuse.

Toutes les données sont stockées dans le localStorage du navigateur — aucun compte, aucun serveur.

Fonctionnalités
Bibliothèque masonry style Pinterest avec différents formats selon le type (film, musique, manga, jeu, anime).
Rail "Ajoutés récemment" en haut de la page pour retrouver vite tes dernières entrées.
Filtres par catégorie, statut, favoris, recherche en direct, et tri (récents / mieux notés / titre).
Page détail cinématique avec hero plein écran, notes en format journal, score stylisé.
Formulaire d'ajout / édition avec aperçu de la couverture, sélecteur de note interactif, validation.
Stats : total, note moyenne, répartition par catégorie, coups de cœur, ajouts récents.
Sauvegarde / restauration : export et import de toute ta bibliothèque en JSON.
Mode admin : seul l'admin peut ajouter, modifier, supprimer ou favoriser. Les visiteurs voient mais ne touchent à rien.
Interface 100 % en français, dark mode, glassmorphism, accents ambre.
Connexion admin
Page : /login (ou clique sur l'icône cadenas en haut à droite)
Mot de passe par défaut : mediachad-admin-2026
Une fois connecté, le bouton "Ajouter" apparaît dans la barre du haut, ainsi que les boutons d'édition / suppression et les cœurs favoris.
Pour te déconnecter, clique sur "Déconnexion" dans le menu en haut à droite.
Le mot de passe est vérifié côté navigateur (hash SHA-256 stocké dans le code source). C'est suffisant pour empêcher les visiteurs de modifier ta bibliothèque, mais ce n'est pas un système de sécurité fort. Pour changer le mot de passe, génère un nouveau hash SHA-256 et remplace la constante ADMIN_PASSWORD_HASH dans artifacts/media-tracker/src/lib/auth.ts.

Lancer en local
Prérequis : Node 20+ et pnpm 10+.

pnpm install
pnpm --filter @workspace/media-tracker run dev

Puis ouvre l'URL affichée dans le terminal.

Construire pour la production
pnpm --filter @workspace/media-tracker run build

Le résultat est dans artifacts/media-tracker/dist/.

Déploiement
Netlify (le plus simple)
Va sur app.netlify.com/drop.
Glisse-dépose le dossier artifacts/media-tracker/dist/ (ou le zip fourni).
Le fichier _redirects inclus gère les routes SPA au refresh.
Ou connecte le repo : Netlify lit artifacts/media-tracker/netlify.toml et fait tout tout seul.

GitHub Pages (automatique via GitHub Actions)
Un workflow est déjà prêt dans .github/workflows/deploy.yml.

Push le projet sur GitHub.
Dans Settings → Pages, choisis Source : GitHub Actions.
Push sur main (ou lance le workflow depuis l'onglet Actions).
Le site sera disponible à https://<ton-pseudo>.github.io/<nom-du-repo>/.

Vercel / Cloudflare Pages
Build : pnpm --filter @workspace/media-tracker run build
Dossier de sortie : artifacts/media-tracker/dist
Structure du projet
artifacts/media-tracker/
├── public/
│   └── _redirects              # Fallback SPA pour Netlify
├── src/
│   ├── assets/seed/            # Couvertures des entrées de démo
│   ├── components/             # MediaCard, Layout, MediaForm, RatingDisplay, etc.
│   ├── hooks/
│   │   ├── use-auth.tsx        # Contexte d'authentification admin
│   │   └── use-media-library.ts # Lecture/écriture localStorage + Zod
│   ├── lib/
│   │   ├── auth.ts             # Hash SHA-256 + vérification mot de passe
│   │   ├── types.ts            # Schémas Zod
│   │   └── utils.ts
│   ├── pages/
│   │   ├── Library.tsx
│   │   ├── Detail.tsx
│   │   ├── AddOrEdit.tsx
│   │   ├── Stats.tsx
│   │   ├── Settings.tsx
│   │   └── Login.tsx
│   ├── App.tsx
│   ├── main.tsx
│   └── index.css
├── index.html
├── netlify.toml
├── package.json
└── vite.config.ts

Stack
React 19 + Vite 7
React Router v7
Tailwind CSS v4
Framer Motion pour les animations
Zod pour la validation des données
localStorage pour la persistance (clé : reel.media.v1)
SHA-256 (Web Crypto API) pour le hash du mot de passe admin
Sauvegarde
Tes données vivent uniquement dans le navigateur. Pour ne rien perdre :

Exporter : Paramètres → Exporter une sauvegarde (télécharge un fichier JSON).
Importer : Paramètres → Importer une sauvegarde (remplace la bibliothèque actuelle).
Vider le localStorage du navigateur effacera tout.
Personnaliser
Changer le mot de passe admin : remplace ADMIN_PASSWORD_HASH dans src/lib/auth.ts par le SHA-256 de ton nouveau mot de passe.
Changer les couleurs / typo : src/index.css (variables CSS et imports Google Fonts).
Ajouter une catégorie : édite MediaType dans src/lib/types.ts puis TypeBadge.tsx et le formulaire.
