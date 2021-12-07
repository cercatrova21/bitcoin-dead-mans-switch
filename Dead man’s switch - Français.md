# **Dead man’s switch - Léguer le bitcoin en toute sécurité**

T'es-tu déjà demandé comment on peut léguer des bitcoins sans y déposer son seed ? Le risque que l'intégralité des mots de la graine tombe entre de mauvaises mains, malgré la confiance accordée aux héritiers, ou que la relation avec les héritiers soit soudainement rompue, est réel. C'est pourquoi je te montre dans cet article comment mettre en place une configuration dans laquelle on ne doit faire confiance à personne et où l'on peut quand même léguer ses bitcoins à ses survivants en cas de décès.

**Qu'est-ce qu'un dead man's switch?**

Un dead man's switch est un mécanisme conçu pour être activé lorsque l'opérateur humain est incapable d'agir, par exemple en raison d'un décès, d'une perte de conscience ou lorsqu'il a été physiquement privé de contrôle. À l'origine, ce terme s'appliquait aux interrupteurs des véhicules ou des machines, mais il est désormais également utilisé pour d'autres applications immatérielles. Et c'est précisément ce mécanisme que nous allons appliquer ici au bitcoin.

**Comment fonctionne un dead man's switch sur Bitcoin?**

Pour cela, il faut d'abord créer un portefeuille vide et écrire ses mots de départ. Important : ce portefeuille restera toujours vide, sauf si le switch est activé. Dans notre exemple, nous appelons ce portefeuille le portefeuille de récupération. Ensuite, nous devons créer une transaction Bitcoin qui indique quels coins (UTXO) seront envoyés dans ce nouveau portefeuille de récupération. Cette transaction Bitcoin est signée de manière valide, mais munie d'un verrou temporel. C'est là que la magie opère : grâce à ce timelock, la transaction Bitcoin ne sera valable qu'à une date ultérieure que l'utilisateur aura lui-même fixée. Si elle est broadcastée aujourd'hui, elle sera refusée par les nœuds complets et les mineurs, car elle n'est pas valable. Mais si la transaction arrive à un moment X où elle serait valable, elle peut être envoyée sans aucune modification. Le timelock peut être défini de deux manières différentes : La hauteur de bloc ou le temps UNIX. Si l'on opte pour la hauteur de bloc, on peut compter en moyenne sur 144 blocs par jour. On peut maintenant transmettre le portefeuille de récupération à ses héritiers avec la transaction Bitcoin déjà signée et le Timelock que l'on a fixé soi-même. 

**Quels sont les scénarios possibles?**

Prenons l'exemple d'un timelock de 764800. Dans ce cas, la transaction ne serait valable que dans un an. (Niveau du bloc lors de la rédaction de l'article plus un an : 712240 + (144 * 365)). Si l'on est encore en vie dans un an, la transaction valablement signée est invalidée en envoyant les UTXOs qui se trouvent en entrée de la transaction à une nouvelle adresse de soi-même et en prolongeant simplement l'ensemble du setup d'un an. Si l'on n'est plus en mesure d'effectuer ce qui a été décrit précédemment, la transaction que possèdent mes héritiers devient valable et peut être envoyée. 

**Avantages**

- La confiance n'est à aucun moment nécessaire

- La transaction signée est également une sorte de sauvegarde en cas de perte de ses propres graines, par exemple du portefeuille matériel.

- Simplicité d'utilisation pour les héritiers : il leur suffit de copier la transaction (texte) et de la diffuser au moment X via un explorateur de blocs.

- Cette stratégie de sauvegarde peut être appliquée avec n'importe quel portefeuille. (Même avec les portefeuilles multi-sigles).

**Inconvénients** 

- Les UTXO ne doivent pas changer. Mais si c'est le cas, il faut à chaque fois renouveler cette transaction signée pour les héritiers après une transaction. Ce mécanisme ne convient donc qu'aux portefeuilles HODLR.

#### **Instruction avec Sparrow** 

**Création d'un portefeuille de récupération avec des graines**.

Tout d'abord, nous devons créer un nouveau portefeuille Bitcoin vide. C'est là que seront envoyés les bitcoins en cas de décès. Les héritiers doivent également posséder ces mots de passe pour pouvoir accéder aux bitcoins reçus. Cela peut se faire avec n'importe quel logiciel de portefeuille Bitcoin. J'utilise à cet effet le portefeuille Sparrow dans l'exemple :

[Page de téléchargement de Sparrow](https://www.sparrowwallet.com/download/)

1. ouvrir Sparrow et cliquer en haut sur : File -> New Wallet

<img src="./images/2021-12-02 17_11_40-Sparrow.png" style="zoom : 67% ;" />

2. entrer le nom du portefeuille

<img src="./images/2021-12-02 17_12_07-Sparrow.png" style="zoom:67% ;" />

3. sélectionner New or Imported Software Wallet

<img src="./images/2021-12-02 17_12_21-Sparrow - Recovery Wallet.png" style="zoom:67% ;" />

4. entrer 24 (ou 12) mots -> Générer nouveau

<img src="./images/2021-12-02 17_13_04-Sparrow - Recovery Wallet.png" style="zoom:67% ;" />

*Note : Cette graine dans la capture d'écran sert uniquement d'exemple. Ne l'utilisez pas !*

**Copier cette graine et la mettre à disposition des héritiers.**

5. créer un keystore -> m/84'/0'/0' -> importer un keystore -> appliquer

<img src="./images/2021-12-02 17_20_11-Sparrow - Recovery Wallet.png" style="zoom:67% ;" />

Nous devons maintenant générer une adresse de réception de ce portefeuille pour pouvoir ensuite créer la transaction Timelock. Pour ce faire, cliquez sur Receive et copiez l'adresse :

*bc1qts6l0rdq22l24fqm79mm9fmnk0zx9udlmps6nr*

<img src="./images/2021-12-02 17_22_23-Sparrow - Recovery Wallet.png" style="zoom:67% ;" />

6. connecter le Main Wallet à Sparrow

Vient ensuite la transaction Timelock. Pour cela, tu dois connecter ton hardware wallet, ton multisig wallet ou tout ce que tu as à Sparrow et l'importer. Si tu utilises la Bitbox02, voici un bon blog post de Stadicus sur la façon dont cela fonctionne avec Sparrow et Bitbox : 

[Peek into Bitcoin internals using Sparrow Wallet with your BitBox02 (shiftcrypto.ch)](https://shiftcrypto.ch/blog/peek-into-bitcoin-internals-using-sparrow-wallet-with-your-bitbox02/)

Une fois que tu as accès à ton main wallet, clique sur Send :

<img src="./images/2021-12-02 17_30_20-Sparrow - Main Wallet.png" style="zoom:67% ;" />

Maintenant, tu inscris chez le destinataire l'adresse générée précédemment du portefeuille de récupération. Tu peux saisir ton propre texte comme étiquette. Ce que tu écris ici n'est pas pertinent. Pour le montant, tu peux maintenant choisir soit tout (bouton Max), soit seulement certains UTXO. 

**Important : si ces UTXOs changent à l'avenir ou si de nouveaux UTXOs sont ajoutés, tu dois renouveler la transaction Timelock.**

Ensuite, tu cliques sur : Create Transaction et à droite dans l'onglet : Detail

<img src="./images/2021-12-02 17_34_33-Sparrow - An meine Erben.png" style="zoom:67% ;" />

Sous le point : Absolute Locktime, tu peux maintenant saisir soit la hauteur du bloc, soit une date à partir de laquelle la transaction doit être valable.
Si les données sont correctes, clique sur "Finalize Transaction for Signing".



<img src="./images/2021-12-02 17_36_30-Sparrow - An meine Erben.png" style="zoom:67% ;" />

Maintenant, tu dois signer la transaction. Cela peut être fait avec la Bitbox, le Ledger ou tout ce que tu utilises pour un Main Wallet. Dans notre exemple, le Main Wallet est également un Software Wallet et tu peux donc simplement cliquer sur le bouton Sign.

**Important : Pour certains hardware wallets (BitBox par ex.), la hauteur du bloc Timelock s'affiche également à l'écran pour la vérification. Tu devrais également le vérifier avec l'adresse du destinataire et le montant avant de signer !**



**Exporter et sauvegarder la transaction Timelock signée**.

Pour cela, nous cliquons d'abord sur : View Final Transaction. Tu peux maintenant copier le code hexadécimal (représenté en couleur ci-dessous) et le joindre au seed avec le portefeuille de récupération. Dans notre exemple, il s'agit du code suivant : 

<img src="./images/2021-12-02 17_42_43-.png" style="zoom:67% ;" />

La transaction peut également être sauvegardée sous forme de fichier : File -> Save Transaction (Ctrl + S)

<img src="./images/2021-12-02 17_42_55-Sparrow - An meine Erben.png" style="zoom:67% ;" />

La transaction signée peut maintenant aussi être vérifiée à tout moment par copier-coller ici sur ce site :

[Bitcoin Wallet by Coinb.in](https://www.coinb.in/#verify)

<img src="./images/2021-12-02 17_44_58-Bitcoin Wallet by Coinb.in und 3 weitere Seiten - Persönlich – Microsoft​ Edge.png" style="zoom:67%;" />

#### **Conclusion**

Il est certainement toujours important que tu joignes les coordonnées d'une personne de confiance avec la transaction Timelock signée et les mots Seed pour le Recovery Wallet. En principe, les héritiers n'ont rien d'autre à faire que de diffuser ce texte hexadécimal (la transaction brute en bitcoin). Là aussi, j'ai mis en lien ci-dessous quelques sites web qui permettent de le faire. Mais une personne de confiance, qui sait déjà ce qu'il faut faire, est certainement utile.

Dans l'exemple de configuration, j'ai copié toutes les informations sur une clé USB. Vous trouverez ci-joint un fichier README avec le texte suivant :

--------------------------------------------------------------------------------------------------------------------------------

*Dans ce fichier, il est décrit comment réagir en cas d'incident et comment quelqu'un peut obtenir mes bitcoins.*
*J'ai résolu cela de la manière suivante :*

*Sur cette clé USB se trouve un fichier avec une transaction déjà signée de mon installation de stockage à froid.* *Mais cette transaction n'est valable qu'à partir d'une certaine hauteur de bloc. Cette hauteur de bloc est indiquée dans le nom du fichier.*

*La transaction est structurée de manière à envoyer tous mes UTXO vers un nouveau portefeuille BTC. Ce nouveau portefeuille BTC* *se trouve sur cette clé USB, y compris le seed. Il suffit donc de broadcaster la transaction sur le réseau*.

*Pour cela, j'ai listé ici quelques liens de sites web:*.
*https://btc.bitaps.com/broadcast*

*https://btc.network/broadcast*

*https://www.blockchain.com/btc/pushtx*

*https://blockchair.com/broadcast*

*https://hashraw.com/#broadcast*

*Ainsi, il suffit de copier le texte dans le fichier TX et de le coller sur l'un de ces sites web. Ensuite, les bitcoins arrivent sur le portefeuille qui se trouve sur cette clé USB. C'est à vous de décider ce que vous voulez en faire ensuite.*

-------------------------------------------------------------------

Si tu as des questions, n'hésite pas à me contacter.


Twitter : [@cercatrova_21](https://twitter.com/cercatrova_21)

Telegram : @cercatrova21

Courrier électronique : [cercatrova21@protonmail.com](mailto:cercatrova21@protonmail.com)

Si tu as pu tirer une valeur ajoutée de cet article, je serais très heureux de recevoir un Lightning Donation :

<img src="./images/QR_lightning.png" style="zoom:67% ;" />

LNURL1DP68GURN8GHJ7UMHD9EHXTT9DE5KWMTP9E3KSTMVDE6HYMRS9ASHQ6F0WCCJ7MRWW4EXCTEJX6Z6W3