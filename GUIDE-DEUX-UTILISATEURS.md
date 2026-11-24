# Utiliser l'application à deux (ou plus) : KONATÉ & FRÈRE

Oui, plusieurs personnes peuvent utiliser l'application en même temps, chacune sur son téléphone ou son ordinateur. Les données passent par Firebase : ce que l'un enregistre apparaît chez l'autre en quelques secondes. Sans internet, chacun continue à travailler ; la synchronisation se fait au retour de la connexion.

Cela fonctionne seulement après la mise en ligne (GitHub Pages + Firebase, voir `LISEZMOI.md`). Ouvert comme simple fichier, chaque appareil garde ses propres données.

---

## Partie A : Ajouter une personne (une seule fois par personne)

1. console.firebase.google.com, votre projet, **Authentication**, onglet **Users**, **Ajouter un utilisateur** : e-mail et mot de passe (au moins 6 caractères). Notez-les.
2. **Firestore Database**, onglet **Règles** : ajoutez son e-mail dans la liste, entre apostrophes, séparé par une virgule :

```
request.auth.token.email in ['gerant1@gmail.com','gerant2@gmail.com','nouveau@gmail.com']
```

3. Touchez **Publier**. Vérifiez la date de publication.

Sans l'étape 2, la personne peut se connecter mais ne voit aucune donnée.

---

## Partie B : Installer sur l'appareil de la 2e personne

**Téléphone (Android / Chrome)**
1. Ouvrez le lien `https://VOTRE-NOM.github.io/gestion-konate/`.
2. Connectez-vous avec son e-mail et son mot de passe. En haut doit s'afficher « ● Connecté ».
3. Menu **⋮** de Chrome, puis **Ajouter à l'écran d'accueil**.

**Ordinateur**
1. Ouvrez le même lien dans Chrome ou Edge et connectez-vous.
2. Vous pouvez cliquer sur l'icône d'installation à droite de la barre d'adresse (facultatif).

**Code PIN :** il est propre à chaque appareil. Chacun le règle dans Paramètres, sur son appareil.

---

## Partie C : Test de vérification (5 minutes, à deux)

1. Appareil 1 : ajoutez un produit « Test ». Appareil 2 : il doit apparaître dans le Stock en quelques secondes.
2. Appareil 2 : faites une vente de 1 article « Test ». Appareil 1 : le stock a baissé et la vente est dans l'Historique.
3. Les deux en même temps : chacun vend 1 article « Test ». Le stock doit baisser de 2 au total.
4. Appareil 1 : mode avion, puis une vente. Rallumez internet : la vente arrive sur l'appareil 2.
5. Annulez la vente de test et supprimez le produit « Test ».

---

## Partie D : Règles de bon usage à deux

- **Stock et règlements sont protégés :** deux ventes du même article en même temps diminuent chacune le stock, et deux règlements sur la même facture sont tous les deux enregistrés.
- **Évitez de modifier le même reçu en même temps** pour autre chose qu'un règlement (annulation, retour de marchandise) : dans ce cas rare, la dernière modification enregistrée l'emporte.
- **Après une longue période hors ligne**, ouvrez l'application avec internet quelques minutes pour laisser la synchronisation se terminer.
- **Premier lancement sur un appareil qui contient déjà des données :** l'application demande si elle doit les envoyer au cloud. Répondez **OK** pour les conserver.
- **Départ d'une personne :** retirez son e-mail de la liste des règles (publier), puis supprimez son compte dans Authentication > Users. Elle perd l'accès tout de suite.
- **Changer un mot de passe :** Authentication > Users > les trois points à droite du compte > **Réinitialiser le mot de passe**.

---

## Si ça ne marche pas

- **« E-mail ou mot de passe incorrect » :** vérifiez le compte dans Authentication > Users.
- **Connecté mais aucune donnée :** l'e-mail est absent ou mal écrit dans les règles, ou les règles ne sont pas publiées.
- **« Mode local » en haut :** `firebase-config.js` contient encore `VOTRE...`.
- **Les données de l'autre n'apparaissent pas :** vérifiez que les deux appareils affichent « ● Connecté » et rechargez la page.
