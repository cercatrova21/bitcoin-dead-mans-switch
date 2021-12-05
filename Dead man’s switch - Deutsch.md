# **Dead man’s switch - Bitcoin sicher vererben**

Hast Du Dich auch schon gefragt, wie man Bitcoins vererben kann ohne dabei seinen Seed zu hinterlegen? Die Gefahr, dass die kompletten Seed Wörter, trotz Vertrauen zu den Erben, in falsche Hände kommen oder, dass die Beziehung zu den Erben plötzlich bricht, ist real. Deshalb zeige ich Dir in diesem Artikel, wie man ein Setup einrichtet, bei dem man niemandem vertrauen muss und trotzdem im Falle des Todes seine Bitcoins den Hinterbliebenen vererben kann.

**Was ist ein dead man’s switch?**

Ein dead man’s switch ist ein Mechanismus, der so konzipiert ist, dass er aktiviert wird, wenn der menschliche Bediener handlungsunfähig wird, z.B. durch Tod, Bewusstseinsverlust oder wenn er körperlich der Kontrolle entzogen wurde. Ursprünglich wurde der Begriff auf Schalter an Fahrzeugen oder Maschinen angewandt, doch inzwischen wird er auch für andere immaterielle Anwendungen verwendet. Und genau diesen Mechanismus werden wir hier bei Bitcoin anwenden.

**Wie funktioniert ein dead man’s switch bei Bitcoin?**

Hierbei wird zuerst eine leere Wallet eingerichtet und dessen Seed Wörter aufgeschrieben. Wichtig: Diese Wallet wird immer leer bleiben, ausser der Switch wird aktiviert. Diese Wallet nennen wir in unserem Beispiel hier die Recovery Wallet. Danach müssen wir eine Bitcoin Transaktion erstellen, die besagt welche Coins (UTXO) in diese neue Recovery Wallet gesendet werden. Diese Bitcoin Transaktion wird gültig signiert, jedoch mit einem Timelock versehen. Hier passiert auch bereits die ganze Magie: Durch diesen Timelock wird die Bitcoin Transaktion erst zu einem späteren und selbst festgelegten Zeitpunkt gültig. Sollte diese heute gebroadcastet werden, wird diese durch die Full Nodes und die Miner abgelehnt, da sie ungültig ist. Kommt aber Zeitpunkt X, bei dem die Transaktion gültig wäre ist diese ohne etwas zu verändern versendbar. Der Timelock kann auf 2 verschiedene Arten gesetzt werden: Blockhöhe oder die UNIX Time. Sollte man sich für die Blockhöhe entscheiden, kann man pro Tag im Durchschnitt mit 144 Blöcken rechnen. Nun kann man die Recovery Wallet mit der bereits signierten Bitcoin Transaktion und dem selbst festgelegtem Timelock an seine Erben weitergeben. 

**Welche Szenarien gibt es nun?**

Nehmen wir mal als Beispiel, dass ein Timelock von 764800 gesetzt wurde. Dann wäre die Transaktion erst in einem Jahr gültig. (Stand der Blockhöhe beim Schreiben des Artikels plus ein Jahr: 712240 + (144 * 365)) Sollte man in einem Jahr noch leben, wird die gültig signierte Transaktion ungültig gemacht, in dem man die UTXOs, die sich als Input in der Transaktion befinden, an eine neue Adresse von sich selber sendet und das ganze Setup einfach um ein Jahr verlängert. Sollte man nun nicht mehr in der Lage sein, das vorher beschriebene auszuführen, wird die Transaktion, die meine Erben besitzen, gültig und kann versendet werden. 

**Vorteile**

- Vertrauen ist zu keiner Zeit notwendig

- Die signierte Transaktion ist auch eine Art Backup, falls die eigenen Seeds, beispielsweise der Hardware Wallet, verliert.

- Einfache Handhabung für die Erben: Sie müssen die Transaktion (Text) lediglich kopieren und zum Zeitpunkt X über einen Blockexplorer Broadcasten.

- Diese Backup Strategie kann mit jeder Wallet angewendet werden. (Auch mit Multisig Wallets)

**Nachteile** 

- Die UTXO dürfen sich nicht verändern. Sollte dies jedoch der Fall sein, muss man jedes mal nach einer Transaktion diese signierte Transaktion für die Erben erneuern. Somit ist dieser Mechanismus nur für HODLR Wallets geeignet.

#### **Anleitung mit Sparrow** 

**Erstellung einer Recovery Wallet mit Seeds**

Als erstes müssen wir eine neue leere Bitcoin Wallet erstellen. Dahin werden die Bitcoins im Falle des Ablebens hingesendet. Diese Seedwörter müssen die Erben ebenfalls besitzen, um auf die erhaltenen Bitcoins zuzugreifen. Dies kann mit jeder beliebigen Bitcoin Walletsoftware gemacht werden. Ich verwende hierfür im Beispiel die Sparrow Wallet:

[Sparrow Downloadseite](https://www.sparrowwallet.com/download/)

1. Sparrow öffnen und oben auf: File -> New Wallet klicken

<img src="./images/2021-12-02 17_11_40-Sparrow.png" style="zoom: 67%;" />

2. Wallet Name eingeben

<img src="./images/2021-12-02 17_12_07-Sparrow.png" style="zoom:67%;" />

3. New or Imported Software Wallet auswählen

<img src="./images/2021-12-02 17_12_21-Sparrow - Recovery Wallet.png" style="zoom:67%;" />

4. Enter 24 (or 12) Words -> Generate New

<img src="./images/2021-12-02 17_13_04-Sparrow - Recovery Wallet.png" style="zoom:67%;" />

*Hinweis: Dieser Seed im Screenshot dient nur als Beispiel. Diesen bitte nicht benutzen!*

**Diesen Seed nun abschreiben und den Erben zur Verfügung stellen.**

5. Create Keystore -> m/84'/0'/0' -> Import Keystore -> Apply

<img src="./images/2021-12-02 17_20_11-Sparrow - Recovery Wallet.png" style="zoom:67%;" />

Nun müssen wir eine Empfangsadresse von dieser Wallet generieren, um danach die Timelock Transaktion zu erstellen. Dazu auf Receive klicken und die Adresse kopieren:

*bc1qts6l0rdq22l24fqm79mm9fmnk0zx9udlmps6nr*

<img src="./images/2021-12-02 17_22_23-Sparrow - Recovery Wallet.png" style="zoom:67%;" />

6. Main Wallet mit Sparrow verbinden

Nun kommt die Timelock Transaktion. Dazu musst du deine Hardware Wallet, Multisig Wallet oder was auch immer du hast mit Sparrow verbinden und diese importieren. Solltest du die Bitbox02 verwenden, ist hier ein guter Blog Post von Stadicus wie es mit Sparrow und Bitbox funktioniert: 

[Peek into Bitcoin internals using Sparrow Wallet with your BitBox02 (shiftcrypto.ch)](https://shiftcrypto.ch/blog/peek-into-bitcoin-internals-using-sparrow-wallet-with-your-bitbox02/)

Sobald du Zugriff auf deine Main Wallet hast, klicke auf Send:

<img src="./images/2021-12-02 17_30_20-Sparrow - Main Wallet.png" style="zoom:67%;" />

Nun trägst du beim Empfänger die vorher generierte Adresse von der Recovery Wallet ein. Als Label kannst du einen eigenen Text eingeben. Was du hier schreibst, ist nicht relevant. Beim Betrag kannst du nun entweder alles (Max Button) oder nur einzelne UTXOs auswählen. 

**Wichtig: Ändern sich diese UTXOs in der Zukunft oder es kommen neue dazu, musst du die Timelock Transaktion erneuern.**

Anschließend klickst du auf: Create Transaction und rechts in den Reiter: Detail

<img src="./images/2021-12-02 17_34_33-Sparrow - An meine Erben.png" style="zoom:67%;" />

Unter dem Punkt: Absolute Locktime kannst du nun entweder die Blockhöhe oder ein Datum eingeben, ab wann die Transaktion gültig sein soll.
Wenn die Eingaben stimmen, klickst du auf “Finalize Transaction for Signing”.



<img src="./images/2021-12-02 17_36_30-Sparrow - An meine Erben.png" style="zoom:67%;" />

Nun musst du die Transaktion signieren. Dies kann mit der Bitbox, Ledger oder was auch immer du für eine Main Wallet benutzt, gemacht werden. In unserem Beispiel ist die Main Wallet ebenfalls eine Software Wallet und somit kann man einfach auf den Sign Button klicken.

**Wichtig: Bei diversen Hardware Wallets (BitBox z.B.) wird dir auf dem Display ebenfalls noch die Timelock Blockhöhe zur Verifizierung angezeigt. Diese solltest du ebenfalls mit der Empfängeradresse und dem Betrag überprüfen, bevor du signierst!**



**Signierte Timelock Transaktion exportieren und abspeichern**

Dafür klicken wir zuerst auf: View Final Transaction. Alle Informationen werden noch einmal dargestellt.Nun kannst du den Hex Code (unten farblich dargestellt) kopieren und zum Seed mit der Recovery Wallet beilegen. In unserem Beispiel ist es folgender Code: 

<img src="./images/2021-12-02 17_42_43-.png" style="zoom:67%;" />

Die Transaktion kann auch noch als File abgespeichert werden: File -> Save Transaction (Ctrl + S)

<img src="./images/2021-12-02 17_42_55-Sparrow - An meine Erben.png" style="zoom:67%;" />

Die signierte Transaktion kann nun auch zu jeder Zeit mittels copy und paste hier auf dieser Webseite verifiziert werden:

[Bitcoin Wallet by Coinb.in](https://www.coinb.in/#verify)

<img src="./images/2021-12-02 17_44_58-Bitcoin Wallet by Coinb.in und 3 weitere Seiten - Persönlich – Microsoft​ Edge.png" style="zoom:67%;" />

#### **Schlusswort**

Wichtig ist sicherlich auch immer, dass du die Kontaktdaten einer Vertrauensperson zusammen mit der signierten Timelock Transaktion und der Seed Wörter für die Recovery Wallet beilegst. Grundsätzlich müssen die Erben nichts weiter tun als diesen Hex Text (die Raw Bitcoin Transaktion) zu broadcasten. Auch hier habe ich unten einige Webseiten verlinkt, womit dies geht. Aber eine Vertrauensperson, die bereits schon weiss was zu tun ist, ist sicherlich hilfreich.

Im Beispiel-Setup habe ich alle Informationen auf einen USB Stick kopiert. Anbei befindet sich eine README Datei mit folgendem Text:

--------------------------------------------------------------------------------------------------------------------------------

*In dieser Datei wird beschrieben, wie bei einem Vorfall umzugehen ist und jemand an meine Bitcoins kommt.* 
*Dies habe ich wie folgt gelöst:*

*Auf diesem USB-Stick befindet sich eine Datei mit einer bereits signierten Transaktion aus meinem Cold Storage Setup.* *Jedoch ist diese Transaktion erst ab einer bestimmten Blockhöhe gültig. Diese Blockhöhe ist im Filename der Datei zu erkennen.*

*Die Transaktion ist so aufgebaut, dass sie alle meine UTXOs auf eine neue BTC Wallet versendet. Diese neue BTC Wallet* *befindet sich inkl. Seed auf diesem USB Stick. Somit muss einfach die Transaktion ins Netzwerk gebroadcastet werden.*

*Dafür habe ich hier einige Websiten Links aufgeführt:*
*https://btc.bitaps.com/broadcast*

*https://btc.network/broadcast*

*https://www.blockchain.com/btc/pushtx*

*https://blockchair.com/broadcast*

*https://hashraw.com/#broadcast*

*Somit muss man nur den Text in der TX Datei kopieren und auf einer dieser Webseiten einfügen. Danach gelangen die Bitcoins auf die Wallet, die sich auf diesem USB Stick befindet. Was danach mit diesen geschehen soll, dürft Ihr selber entscheiden.* 

-------------------------------------------------------------------

Solltest du Fragen haben, melde dich einfach bei mir.


Twitter: [@cercatrova_21](https://twitter.com/cercatrova_21)

Telegram: @cercatrova21

E-Mail: [cercatrova21@protonmail.com](mailto:cercatrova21@protonmail.com)

Falls du einen Mehrwert aus diesem Artikel entnehmen konntest, freue ich mich sehr über eine Lightning Donation:

<img src="./images/QR_lightning.png" style="zoom:67%;" />

LNURL1DP68GURN8GHJ7UMHD9EHXTT9DE5KWMTP9E3KSTMVDE6HYMRS9ASHQ6F0WCCJ7MRWW4EXCTEJX6Z6W3