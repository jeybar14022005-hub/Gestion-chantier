# Gestion Chantiers & Facturation

Application web complète pour la gestion de chantiers et la facturation, destinée aux entreprises du secteur du BTP. Le système permet de suivre les projets, gérer les équipes, traiter les dépenses et émettre les factures de manière centralisée.

Ce projet a été développé dans le cadre d'un travail académique pour démontrer la maîtrise des technologies web modernes et des bonnes pratiques de développement logiciel.

## Fonctionnalités principales

### Gestion des chantiers
- Création, modification et suppression de chantiers
- Suivi de l'avancement par statut (planifié, en cours, terminé, annulé)
- Attribution des techniciens aux projets
- Suivi budgétaire en temps réel
- Historique complet des modifications

### Module de facturation
- Génération et édition de factures
- Calcul automatique de la TVA
- Processus de validation (brouillon, envoyée, payée)
- Export PDF des factures
- Détail des lignes de facturation

### Notes de frais
- Saisie des dépenses par les techniciens
- Classification par catégorie (transport, matériel, repas, hébergement, autre)
- Validation par les administrateurs
- Gestion des justificatifs

### Gestion des équipes
- Fiches détaillées des techniciens
- Configuration des taux horaires
- Activation/désactivation des comptes
- Suivi de l'historique d'activité

### Tableau de bord
- Synthèse de l'activité globale
- Statistiques en temps réel
- Indicateurs de performance
- Vue du chiffre d'affaires

## Stack technique

- Frontend: React 18 avec TypeScript et Vite
- Styles: TailwindCSS
- Backend: Supabase (PostgreSQL, Authentification, Stockage)
- Routage: React Router v6
- Icônes: Lucide React
- Gestion des dates: date-fns

## Prérequis

- Node.js version 18 ou supérieure avec npm
- Un compte Supabase (version gratuite disponible)
- Git pour le contrôle de version

## Installation

### 1. Récupérer le code source

```bash
git clone <votre-repo-url>
cd gestion-chantiers
```

### 2. Installation des dépendances

```bash
npm install
```

### 3. Configuration de la base de données

1. Créer un nouveau projet sur [supabase.com](https://supabase.com)
2. Accéder à l'éditeur SQL depuis le tableau de bord
3. Exécuter le fichier `supabase/schema.sql` pour initialiser la structure des tables
4. Pour ajouter des données de démonstration, exécuter également `supabase/seed.sql`
5. Récupérer les identifiants API depuis la section Settings > API
6. Configurer les variables d'environnement dans un fichier `.env` à la racine du projet :

```env
VITE_SUPABASE_URL=votre_supabase_url
VITE_SUPABASE_ANON_KEY=votre_supabase_anon_key
```

### 4. Configuration des comptes utilisateurs

Créer deux comptes de test dans la section Authentication > Users de Supabase :

- Compte administrateur: admin@example.com / mot de passe: admin123
- Compte technicien: tech@example.com / mot de passe: tech123

Pour attribuer les rôles, exécuter les commandes SQL suivantes dans l'éditeur :

```sql
-- Attribution du rôle administrateur
UPDATE auth.users 
SET raw_user_meta_data = jsonb_set(
  COALESCE(raw_user_meta_data, '{}'::jsonb),
  '{role}',
  '"admin"'::jsonb
)
WHERE email = 'admin@example.com';

-- Attribution du rôle technicien
UPDATE auth.users 
SET raw_user_meta_data = jsonb_set(
  COALESCE(raw_user_meta_data, '{}'::jsonb),
  '{role}',
  '"technicien"'::jsonb
)
WHERE email = 'tech@example.com';
```

### 5. Démarrage de l'application

```bash
npm run dev
```

L'application sera accessible via l'adresse http://localhost:3000

## Structure du projet

```
gestion-chantiers/
├── src/
│   ├── components/       # Composants réutilisables
│   │   └── Layout.tsx
│   ├── contexts/        # Contextes React
│   │   ├── AuthContext.tsx
│   │   └── SupabaseContext.tsx
│   ├── pages/           # Pages de l'application
│   │   ├── Dashboard.tsx
│   │   ├── Chantiers.tsx
│   │   ├── Facturation.tsx
│   │   ├── NotesFrais.tsx
│   │   ├── Techniciens.tsx
│   │   └── Login.tsx
│   ├── types/           # Types TypeScript
│   │   └── index.ts
│   ├── App.tsx
│   ├── main.tsx
│   └── index.css
├── supabase/
│   └── schema.sql       # Schéma de base de données
├── package.json
├── vite.config.ts
├── tailwind.config.js
└── tsconfig.json
```

## Sécurité

### Authentification
- Authentification gérée par Supabase Auth
- Système de rôles utilisateurs (administrateur, technicien)
- Sessions sécurisées utilisant des tokens JWT

### Contrôle d'accès
- Politiques de sécurité au niveau des lignes (RLS)
- Accès restreint basé sur l'authentification
- Protection des données sensibles

### Bonnes pratiques de sécurité
- Utilisation de variables d'environnement pour les clés API
- Validation des données côté client et serveur
- Obligation d'utiliser HTTPS en environnement de production

## Déploiement en production

### Option Vercel

1. Préparation du build

```bash
npm run build
```

2. Configuration sur Vercel
- Créer un compte sur [vercel.com](https://vercel.com)
- Importer le projet depuis votre dépôt GitHub
- Configurer les variables d'environnement dans les paramètres du projet :
  - VITE_SUPABASE_URL
  - VITE_SUPABASE_ANON_KEY

3. Lancement du déploiement

```bash
vercel --prod
```

### Option Netlify

1. Configuration du build
- Build command: `npm run build`
- Publish directory: `dist`

2. Ajouter les variables d'environnement dans les paramètres Netlify

## Structure des données

### Entité Chantiers
- Identifiant unique, nom du projet, client, adresse
- Période de réalisation (date de début et de fin)
- État d'avancement et budget alloué
- Technicien responsable du projet

### Entité Techniciens
- Identifiant, nom, prénom, adresse email
- Numéro de téléphone et spécialisation
- Taux horaire et statut d'activité

### Entité Notes de frais
- Identifiant, technicien concerné, chantier associé
- Date de la dépense, catégorie et description
- Montant, état de validation et justificatif

### Entité Factures
- Identifiant, chantier concerné, numéro de facture
- Dates d'émission et d'échéance
- Montants hors taxes, TTC et taux de TVA
- État de la facture

### Entité Journal d'activité
- Identifiant, chantier, technicien
- Date de l'activité et heures travaillées
- Description des tâches réalisées

## Tests

Les tests sont à implémenter pour assurer la qualité du code :

```bash
# Tests unitaires
npm run test

# Tests end-to-end
npm run test:e2e
```

## Pistes d'amélioration

Plusieurs fonctionnalités pourraient être ajoutées pour enrichir l'application :

- Export des données comptables aux formats CSV et Excel
- Génération de rapports PDF personnalisés
- Développement d'une version mobile avec React Native
- Système de notifications par email
- Intégration avec des logiciels comptables
- Module de gestion documentaire
- Calendrier interactif pour planifier les interventions
- Tableaux de bord avec graphiques et analyses avancées

## Contribution au projet

Pour contribuer au développement de l'application :

1. Créer un fork du dépôt
2. Créer une nouvelle branche pour votre fonctionnalité
3. Effectuer vos modifications
4. Committer vos changements avec un message descriptif
5. Pusher votre branche vers votre fork
6. Ouvrir une pull request pour proposer vos modifications

## Licence

Ce projet a été développé dans un cadre éducatif pour un étudiant en troisième année d'informatique de gestion.

## Contact

Pour toute question relative au projet, n'hésitez pas à ouvrir une issue sur le dépôt GitHub.
