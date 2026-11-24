# Mise en ligne pas à pas : KONATÉ & FRÈRE Gestion (GitHub + Firebase)

Prévoyez 30 à 40 minutes. Sur téléphone, passez le navigateur en « version ordinateur » pour GitHub et Firebase.

Fichiers à avoir sous la main (8 fichiers) :
`index.html`, `firebase-config.js`, `manifest.json`, `sw.js`, `logo.png`, `icon-192.png`, `icon-512.png`, `firestore.rules`.
Les fichiers `LISEZMOI.md` et `GUIDE-DEUX-UTILISATEURS.md` sont seulement des guides : il n'est pas obligatoire de les envoyer.

Cochez chaque étape avant de passer à la suivante.

---

## PARTIE 1 : Créer le projet Firebase

1. Ouvrez console.firebase.google.com et connectez-vous avec votre compte Google.
2. Touchez **Créer un projet** (ou **Ajouter un projet**).
3. Nom du projet : `konate-et-frere`. Touchez **Continuer**.
4. Google Analytics : vous pouvez le **désactiver**. Touchez **Créer le projet**, attendez, puis **Continuer**.
5. Sur la page d'accueil du projet, touchez l'icône web **`</>`** (« Ajouter une application web »).
6. Surnom de l'application : `gestion`. **Ne cochez pas** « Firebase Hosting ». Touchez **Enregistrer l'application**.
7. Une zone de code s'affiche avec un bloc `firebaseConfig = { ... }`. **Laissez cette page ouverte** : vous allez recopier 6 valeurs à la partie 4. Vous pouvez aussi copier ce bloc dans une note du téléphone.
8. Touchez **Accéder à la console**.

---

## PARTIE 2 : Activer la connexion (Authentication)

1. Menu de gauche : **Build** (ou **Créer**), puis **Authentication**, puis **Commencer**.
2. Dans la liste des moyens de connexion, choisissez **E-mail/Mot de passe**.
3. Activez **le premier interrupteur** (E-mail/Mot de passe). Laissez le deuxième (lien par e-mail) désactivé. Touchez **Enregistrer**.
4. Onglet **Users** (Utilisateurs), puis **Ajouter un utilisateur**.
5. Saisissez l'e-mail et un mot de passe (au moins 6 caractères) pour le premier gérant, puis **Ajouter un utilisateur**.
6. Recommencez pour chaque autre gérant. **Notez les e-mails et mots de passe.**

Il n'y a pas de bouton d'inscription dans l'application : seuls les comptes que vous créez ici peuvent se connecter.

---

## PARTIE 3 : Créer la base de données (Firestore)

1. Menu de gauche : **Build**, puis **Firestore Database** (ou « Databases & Storage », puis Firestore).
2. Touchez **Créer une base de données**.
3. Édition : **Standard**. Laissez le nom `(default)`.
4. Emplacement : choisissez un emplacement proche, par exemple `europe-west` ou `africa-south1` s'il est proposé. **Il ne pourra plus être changé.** Touchez **Suivant**.
5. Règles de sécurité : choisissez **Démarrer en mode production**, puis **Créer**.
6. Attendez la fin de la création, puis ouvrez l'onglet **Règles**.
7. Effacez tout le texte, puis collez ceci en remplaçant les deux e-mails par ceux de vos gérants (les mêmes qu'à la partie 2, écrits exactement pareil, entre apostrophes, séparés par une virgule) :

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null
        && request.auth.token.email in ['gerant1@gmail.com','gerant2@gmail.com'];
    }
  }
}
```

8. Touchez **Publier**. Vérifiez qu'une date de publication s'affiche.

---

## PARTIE 4 : Relier l'application à Firebase (avant d'envoyer sur GitHub)

Le plus simple est de modifier `firebase-config.js` **avant** de l'envoyer.

1. Ouvrez le fichier `firebase-config.js` dans un éditeur de texte (Bloc-notes, ou l'application de texte de votre téléphone).
2. Remplacez chaque valeur `VOTRE...` par celle de votre bloc `firebaseConfig` (partie 1, étape 7). Gardez **les guillemets et les virgules**. Exemple :

```
window.FB_CONFIG = {
  apiKey: "AIzaSy................",
  authDomain: "konate-et-frere.firebaseapp.com",
  projectId: "konate-et-frere",
  storageBucket: "konate-et-frere.firebasestorage.app",
  messagingSenderId: "123456789012",
  appId: "1:123456789012:web:abcdef123456"
};
```

3. Enregistrez le fichier. Gardez bien le **même nom** : `firebase-config.js`.

Si vous avez perdu le bloc : Firebase, roue dentée à côté de « Vue d'ensemble du projet », **Paramètres du projet**, puis en bas **Vos applications**.

---

## PARTIE 5 : GitHub (héberger l'application)

1. Ouvrez github.com et connectez-vous. Notez votre **nom d'utilisateur**.
2. **+** (en haut à droite), puis **New repository**.
3. Nom du dépôt : `gestion-konate` (minuscules, sans espace). Choisissez **Public**. Touchez **Create repository**.
4. Sur la page du dépôt vide, touchez **uploading an existing file**.
5. Sélectionnez **les fichiers un par un** (et non un dossier) : `index.html`, `firebase-config.js`, `manifest.json`, `sw.js`, `logo.png`, `icon-192.png`, `icon-512.png`.
6. Attendez que tous les fichiers soient chargés, puis touchez **Commit changes**.

**Contrôle important (c'est la cause de l'erreur 404) :** revenez sur la page principale du dépôt. La liste doit montrer **directement** `index.html` et les autres fichiers. Si vous voyez seulement un dossier, les fichiers sont trop bas : refaites l'envoi sans le dossier. Le nom doit être exactement `index.html`, en minuscules (pas `index (1).html`).

7. Allez dans **Settings**, puis **Pages**.
8. **Source** : « Deploy from a branch ». **Branch** : `main`, dossier `/ (root)`. Touchez **Save**.
9. Attendez 2 minutes puis rechargez la page : un bandeau « Your site is live at… » apparaît avec votre lien : `https://VOTRE-NOM.github.io/gestion-konate/`.

---

## PARTIE 6 : Autoriser votre lien dans Firebase

1. Firebase, **Authentication**, onglet **Settings** (Paramètres).
2. **Authorized domains** (Domaines autorisés), puis **Ajouter un domaine**.
3. Écrivez seulement `VOTRE-NOM.github.io` (sans `https://` et sans `/gestion-konate`), puis **Ajouter**.

---

## PARTIE 7 : Vérification finale

1. Ouvrez votre lien GitHub Pages. L'écran **Connexion** doit apparaître.
2. Connectez-vous. En haut doit s'afficher « ● Connecté ».
3. **Paramètres** : renseignez le nom, l'adresse, le téléphone et un code PIN si vous le souhaitez. Ajoutez un produit « Test ».
4. Firebase, **Firestore Database**, onglet **Données** : des collections `kf_produits` et `kf_reglages` doivent apparaître.
5. Faites une vente test, puis testez **WhatsApp** et **PDF / Imprimer** (choisissez « Enregistrer en PDF »).
6. Annulez la vente test, puis supprimez le produit « Test ».
7. Sur un 2e appareil, connectez-vous avec un autre compte : les mêmes données apparaissent en quelques secondes.
8. Mode avion sur un appareil, une vente, puis retour d'internet : elle arrive sur l'autre appareil.
9. Chrome, menu **⋮**, **Ajouter à l'écran d'accueil**.

---

## Mettre à jour l'application plus tard

Sur GitHub, ouvrez le dépôt, **Add file**, **Upload files**, envoyez le nouvel `index.html` (même nom), puis **Commit changes**. Attendez 2 minutes et rechargez la page deux fois.

## À savoir

- **Gratuit :** le plan gratuit (Spark) de Firebase suffit largement pour un commerce de cette taille. Aucune carte bancaire n'est demandée.
- **Ajouter un gérant :** créez son compte (partie 2), puis ajoutez son e-mail dans la liste des règles (partie 3, étape 7) et publiez.
- **Sauvegarde :** envoyez de temps en temps une sauvegarde par WhatsApp (Paramètres).

## Si ça ne marche pas

- **Erreur 404 « File not found » :** `index.html` n'est pas à la racine du dépôt, ou son nom est incorrect (voir le contrôle de la partie 5), ou Pages n'est pas réglé sur `main` et `/ (root)`.
- **« Mode local » en haut :** `firebase-config.js` contient encore `VOTRE...`, ou une faute de recopie. Refaites la partie 4.
- **« E-mail ou mot de passe incorrect » :** vérifiez le compte dans Authentication > Users.
- **Connecté mais aucune donnée, ou rien ne se synchronise :** l'e-mail est mal écrit dans les règles, ou les règles ne sont pas publiées (partie 3, étapes 7 et 8).
- **Erreur de domaine non autorisé :** refaites la partie 6.
- **Page inchangée après une modification :** attendez 2 minutes et rechargez deux fois.
