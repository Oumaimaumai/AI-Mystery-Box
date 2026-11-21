# AI Mystery Box — GitHub Pages + Firebase

Site pédagogique statique (GitHub Pages) avec Firebase Authentication et Firebase Realtime Database pour les buzzers. **Pas de serveur Python local.**

## 1. Créer Firebase

1. Ouvrir https://console.firebase.google.com/ et créer un projet.
2. Dans **Build > Authentication > Sign-in method**, activer **Anonymous** (étudiants) et **Email/Password** (enseignante).
3. Dans **Authentication > Users**, créer l'utilisateur enseignant (email + mot de passe). Copier son **User UID** (pas l'email).
4. Dans **Build > Realtime Database**, créer une base (choisir la région appropriée). Ne pas laisser les règles en mode test.
5. Dans **Project settings > General > Your apps**, ajouter une application **Web**, puis copier la configuration `firebaseConfig` dans `config.js` (en conservant `window.FIREBASE_CONFIG = ...`).
6. Ouvrir `database.rules.json`, remplacer **toutes** les occurrences de `REMPLACER_UID_ADMIN` par le UID de l'enseignante, puis copier le JSON dans **Realtime Database > Rules** et cliquer sur **Publish**.

**Important :** `database.rules.json` est un fichier de référence à copier dans Firebase, pas un fichier que GitHub Pages exécute. La clé Firebase Web dans `config.js` est publique par conception ; ce sont les règles Firebase qui protègent la base.

## 2. Déployer sur GitHub Pages

1. Créer un dépôt GitHub (public pour le GitHub Pages gratuit standard).
2. Déposer `index.html`, `app.js`, `config.js` à la racine du dépôt. `README.md` et `database.rules.json` peuvent aussi être déposés.
3. Ouvrir **Settings > Pages > Build and deployment > Deploy from a branch**, sélectionner **main** et **/(root)**, puis **Save**.
4. Attendre le lien `https://VOTRE_COMPTE.github.io/NOM_DU_DEPOT/`.
5. Ouvrir le lien sur le Mac, cliquer **Je suis enseignant·e**, puis se connecter avec l'email et le mot de passe créés dans Firebase. Cliquer sur **Réinitialiser la partie** pour créer la partie initiale.
6. Sur téléphone/tablette, ouvrir **le même lien**, cliquer **Je suis étudiant·e**, sélectionner une équipe et **Rejoindre**.

## 3. Déroulement

- L'enseignante choisit l'équipe active dans le menu puis clique sur une boîte.
- L'équipe dispose de 60 s pour répondre oralement. L'enseignante valide **Bonne réponse** (+200) ou **Échec/Passe** (-50) et ouvre le vol.
- Les autres équipes voient **VOLER !**. La première transaction reçue par Firebase réserve le buzzer à une équipe.
- L'enseignante valide **Vol réussi** (+150) ou **Vol échoué** (-50), puis ouvre une autre boîte.
- Chaque équipe ne peut être choisie qu'une fois comme équipe active ; les boîtes déjà ouvertes ne peuvent plus être sélectionnées.
- Les scores sont synchronisés. Le chronomètre est visuel : l'enseignante valide manuellement la fin du temps.

## 4. Limites de cette version

- Une seule partie commune (pas de codes de salle). Ne pas utiliser plusieurs classes en même temps.
- Une équipe est sélectionnée sur un téléphone ; la version de démonstration n'empêche pas quelqu'un de se déclarer dans une autre équipe. Utiliser avec une classe de confiance.
- Le buzzer est attribué selon la première transaction acceptée par Firebase, pas selon l'instant physique du toucher ; la latence réseau peut jouer.
- Les questions sont visibles dans `app.js` (normal pour un site statique). Les **réponses attendues ne sont pas dans le dépôt**.
- Ne pas utiliser pour des examens à enjeux ni avec des données sensibles.
- L'enseignante doit se connecter en premier et créer la partie initiale.

## 5. Dépannage

- « Permission denied » : vérifier le UID administrateur dans les règles, et que l'enseignante est bien connectée via Email/Password.
- « Firebase non configuré » : compléter `config.js` avec la bonne URL `databaseURL`.
- « auth/operation-not-allowed » : activer Anonymous et Email/Password dans Firebase Authentication.
- Rien ne s'affiche sur téléphone : vérifier que le déploiement GitHub Pages est terminé et ouvrir le lien HTTPS exact.
