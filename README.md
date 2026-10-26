# 🌍 Sankofa Translate

Application mobile de **traduction et d'apprentissage des langues africaines**, développée avec React Native et Expo. Elle fonctionne sur **Android** et **iOS** à partir d'une seule base de code.

> « Sankofa » : concept akan qui signifie *retourner chercher ce qui a été oublié*. L'application aide à préserver et transmettre le patrimoine linguistique africain.

---

## ✨ Fonctionnalités

- **Traduction** entre le français, l'anglais et 11 langues africaines : swahili, lingala, yoruba, hausa, zoulou, ewondo, duala, ghomala, fulfulde, bulu, gbaya
  - plusieurs moteurs (Google, Groq / Llama 3.3 70B, dictionnaire interne) et cache local
  - détection de la langue, indicateur de fiabilité, signalement des erreurs
- **Quiz progressifs** : 3 niveaux par langue, 10 questions par niveau (240 questions, 8 langues), 70 % requis pour débloquer le niveau suivant
- **11 jeux éducatifs** : Quiz, Flashcards, Orthographe, Paires, Texte à trous, Course de vitesse, Mots mélangés, Quiz culturel, Chasse aux mots, Constructeur de phrases, Écoute
- **Espace social** : publications, likes, commentaires (partagés entre appareils via PostgreSQL)
- **Messagerie** avec écran de discussion
- **Notifications** de rappel quotidiennes ou hebdomadaires
- **Interface bilingue FR / EN**, animations, retours haptiques, thème vert / or / terracotta

## 🧱 Stack technique

| Couche | Technologies |
|---|---|
| Mobile | React Native 0.81, Expo SDK 54, Expo Router, React 19, TypeScript, Reanimated |
| Stockage local | AsyncStorage |
| API | Node.js 24, Express 5 |
| Base de données | PostgreSQL, Drizzle ORM |
| Validation / API | Zod, OpenAPI, Orval (génération du client) |
| IA / traduction | Groq API, Google Translate |
| Build | EAS Build, esbuild |
| Monorepo | pnpm workspaces |

## 📁 Structure du projet

```
.
├── artifacts/
│   └── sankofa-translate/     # Application Expo
│       ├── app/               # Écrans (Expo Router) : tabs, login, chat, settings…
│       ├── components/        # Composants UI partagés
│       ├── context/           # État global (AppContext, I18nContext)
│       ├── services/          # Traduction, auth, posts, messages, notifications
│       ├── theme/quiz.ts      # Questions des quiz
│       └── constants/colors.ts
├── features/games/            # Module de jeux (registre + 11 jeux)
├── lib/
│   ├── api-spec/              # Spécification OpenAPI + config Orval
│   ├── api-client-react/      # Client API généré
│   ├── api-zod/               # Schémas Zod générés
│   └── db/                    # Schéma Drizzle + connexion PostgreSQL
├── android/  ios/             # Projets natifs
└── scripts/
```

## ⚡ Installation

### Prérequis

- Node.js ≥ 20 (24 recommandé)
- [pnpm](https://pnpm.io) (le projet refuse npm et yarn)
- Une base PostgreSQL
- Android Studio (Android) ou Xcode (iOS, Mac uniquement)
- Un compte [Expo / EAS](https://expo.dev) pour construire l'APK / IPA

### Étapes

```bash
# 1. Installer les dépendances
pnpm install

# 2. Générer le client API et les schémas Zod
pnpm --filter @workspace/api-spec run codegen

# 3. Créer les tables de la base de données
pnpm --filter @workspace/db run push

# 4. Lancer le serveur API (port 3000)
pnpm --filter @workspace/api-server run dev

# 5. Lancer l'application
cd artifacts/sankofa-translate
pnpm run dev
```

Pour un build natif en local :

```bash
pnpm run android   # ou : pnpm run ios
```

## 🔐 Configuration

Crée un fichier `.env` (jamais commité) avec tes propres valeurs :

```env
DATABASE_URL=postgresql://utilisateur:motdepasse@localhost:5432/sankofa
EXPO_PUBLIC_DOMAIN=ton-domaine-ou-ip
GROQ_API_KEY=ta_cle_groq
```

Clé Groq gratuite : <https://console.groq.com>

> ⚠️ **Ne mets jamais une clé API en dur dans le code source.** Si une clé a déjà été commitée ou partagée, révoque-la et génères-en une nouvelle.

## 🔌 API

| Méthode | Route | Description |
|---|---|---|
| GET | `/api/healthz` | Vérification de l'état du serveur |
| GET / POST | `/api/posts` | Lister / créer des publications |
| PATCH / DELETE | `/api/posts/:id` | Modifier (likes) / supprimer |
| GET / POST | `/api/posts/:postId/comments` | Commentaires d'une publication |
| DELETE | `/api/comments/:id` | Supprimer un commentaire |
| POST | `/api/users` | Inscription (nom + e-mail) ou connexion (e-mail) |
| GET / PATCH | `/api/users/:id` | Lire / mettre à jour un utilisateur |
| GET | `/api/messages/:userId/:friendId` | Messages d'une conversation |
| POST | `/api/messages` | Envoyer un message |
| PATCH | `/api/messages/:id/read` | Marquer comme lu |

## 📦 Générer l'APK / IPA

```bash
npm install -g eas-cli
eas login
eas build --platform android   # APK / AAB
eas build --platform ios       # IPA
```

La configuration des builds se trouve dans `eas.json`.

## 🎮 Ajouter un nouveau jeu

1. Crée le composant dans `features/games/games/`.
2. Exporte-le dans `features/games/games/index.ts`.
3. Ajoute sa configuration (id, titre, description, icône, catégorie) dans `features/games/GameRegistry.ts`.

## 🗺️ Pistes d'évolution

- Traduction vocale et traduction d'image
- Mode hors ligne complet
- Dictionnaire collaboratif validé par des locuteurs natifs
- Classements, défis entre amis et badges
- Messagerie en temps réel
- Davantage de langues nationales camerounaises

## 👤 Auteur

**Rush**, étudiant à l'Institut Catholique Saint-Jérôme de Douala.

## 📄 Licence

MIT
