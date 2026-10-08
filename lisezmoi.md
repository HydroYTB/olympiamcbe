# Site Olympia : guide d'installation

Le site se compose de `index.html` et du dossier `assets/` (logo et icônes tirés du pack de ressources OlympiaV1). Tous les textes (actus, vote, wiki, règlement, staff, grades) se modifient dans le bloc **CONFIGURATION** en haut de `index.html`.

## 1. Mettre le site en ligne

N'importe quel hébergement de site statique fonctionne :

- **Netlify** (le plus simple) : app.netlify.com → « Add new site » → « Deploy manually » → glisse le dossier `olympia-site`.
- **Vercel**, **GitHub Pages** ou ton hébergeur actuel fonctionnent aussi : envoie `index.html` et le dossier `assets/`.

Ajoute ensuite ton domaine (ex. `olympiambe.com`) dans les réglages de l'hébergeur.

## 2. Configurer Tebex

Ton core (CoreOlympia) a déjà son propre connecteur Tebex (`/tebex`). Il lit la file Tebex et exécute les commandes en console, **même quand le joueur est hors ligne**. La boutique fonctionne avec les **Points Boutique** : les joueurs achètent des PB sur le site, puis les dépensent en jeu avec `/boutique` (grades, clés, Battle Pass).

### a) Activer la boutique
Elle est actuellement **désactivée** côté Tebex. Dans creator.tebex.io, termine la configuration des paiements et la vérification du compte.

### b) Relier le serveur (si ce n'est pas déjà fait)
Dans Tebex → Game Servers, récupère la clé secrète du serveur, puis tape en console : `/tebex secret <clé>`. Vérifie avec `/tebex info`.
La clé secrète reste **uniquement** sur le serveur : ne la mets jamais dans le site et ne la partage pas.

### c) Créer les packs de PB
Packages → crée une catégorie, par exemple **Points Boutique**, puis un package par pack. Pour chaque pack, ajoute cette commande (à adapter au montant) :

```
pb give "{username}" 1000 {transaction}
```

Les guillemets autour de `{username}` sont importants : beaucoup de Gamertags Xbox contiennent des espaces. `{transaction}` sert d'identifiant d'achat pour l'historique (`/pb log`). Les montants et les prix en euros sont à fixer par toi. Repères : un grade coûte de 1 000 PB (Centurion) à 8 000 PB (Olympien) en jeu.

Conseillé : dans l'onglet des commandes « en cas de rétrofacturation (chargeback) », ajoute `pb take "{username}" 1000 {transaction}` pour retirer les PB si un paiement est annulé.

Ajoute une image à chaque pack si tu veux (sinon le site affiche la gemme PB). Tout ce que tu crées dans Tebex apparaît automatiquement sur le site. Tu peux supprimer la catégorie vide « Packages ».

### d) Règles Minecraft
Le règlement commercial de Mojang (EULA) interdit de vendre des avantages qui donnent un avantage de jeu (kits de combat, etc.). Tebex peut suspendre une boutique qui ne le respecte pas. Pense à vérifier ce point pour les kits des grades.

## 3. Fonctionnement du paiement

1. Le joueur ajoute des packs au panier et saisit son **pseudo Xbox**.
2. Le site crée un panier Tebex et redirige vers la page de paiement sécurisée de Tebex.
3. Après le paiement, Tebex renvoie le joueur sur la page « Merci ». Le core exécute `pb give` à la vérification suivante (environ 2 minutes au plus).

Le nombre de joueurs en ligne est récupéré via le service public mcsrvstat.us (il faut que le serveur soit joignable depuis Internet).

## 4. À compléter dans la configuration

| Clé | À faire |
|---|---|
| `vote.sites[0].url` | Mets le lien de la **page Olympia** sur minecraftpocket-servers.com (pour l'instant c'est l'accueil du site). |
| `staff` | Ajoute les membres du staff et leurs rôles (N2Z5323 est marqué « Staff » par défaut). |
| `news` | Ajoute tes actualités (les plus récentes en premier). |
| `tickets` | Lien direct vers le salon ticket si tu en as un. |

Les grades, clés et prix en PB affichés sur la page Boutique viennent de `boutique.yml` et des grades PurePerms. Mets `pbShop` à jour si tu changes les prix en jeu.
