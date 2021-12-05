# **Dead man’s switch - Inherit bitcoin safely**

Have you ever wondered how to inherit Bitcoins without depositing your seed? The danger that the complete seed words, despite trusting the heirs, end up in the wrong hands or that the relationship with the heirs suddenly breaks down is real. Therefore, in this article I will show you how to set up a setup where you don't have to trust anyone and still be able to bequeath your Bitcoins to your heirs in case of death.

**What is a dead man's switch?**

A dead man's switch is a mechanism designed to be activated when the human operator becomes incapacitated, e.g., by death, loss of consciousness, or when physically removed from control. Originally, the term was applied to switches on vehicles or machines, but it is now used for other intangible applications as well. And it is this mechanism that we will apply here to Bitcoin.

**How does a dead man's switch work with Bitcoin?**

First, an empty wallet is created and its seed words are written down. Important: This wallet will always remain empty, unless the switch is activated. We call this wallet the recovery wallet in our example here. After that, we need to create a Bitcoin transaction that states which coins (UTXO) will be sent to this new recovery wallet. This Bitcoin transaction is validly signed, but timelocked. This is also where all the magic happens: this timelock makes the Bitcoin transaction valid only at a later and self-determined time. If it is broadcast today, it will be rejected by the full nodes and the miners because it is invalid. However, if time X comes at which the transaction would be valid, it can be sent without changing anything. The Timelock can be set in 2 different ways: Block height or the UNIX Time. If you choose the block height, you can expect an average of 144 blocks per day. Now you can pass on the recovery wallet with the already signed Bitcoin transaction and the self-defined timelock to your heirs. 

**What are the scenarios now?**

Let's take as an example that a timelock of 764800 was set. Then the transaction would be valid only in one year. (State of the block height when writing the article plus one year: 712240 + (144 * 365)). If one is still alive in one year, the valid signed transaction is invalidated by sending the UTXOs, which are input in the transaction, to a new address of oneself and simply extending the whole setup for one year. If you are now no longer able to do what was described before, the transaction that my heirs own becomes valid and can be sent. 

**Advantages**

- Trust is not necessary at any time
- The signed transaction is also a kind of backup in case the own seeds, for example the hardware wallet, gets lost.

- Easy handling for the heirs: they only have to copy the transaction (text) and broadcast it at time X via a block explorer.

- This backup strategy can be used with any wallet. (Also with multisig wallets)

**Disadvantages** 

- The UTXO must not change. However, if this is the case, you have to renew this signed transaction for the heirs every time after a transaction. Thus, this mechanism is only suitable for HODLR wallets.

#### **Instruction with Sparrow** 

**Creating a recovery wallet with seeds**.

First, we need to create a new empty Bitcoin wallet. This is where the Bitcoins will be sent in case of death. The heirs must also have these seed words to access the Bitcoins they receive. This can be done with any bitcoin wallet software. I use the Sparrow Wallet for this in the example:

[Sparrow Downloadpage](https://www.sparrowwallet.com/download/)

1. Open Sparrow and click on: File -> New Wallet click

<img src="./images/2021-12-02 17_11_40-Sparrow.png" style="zoom: 67%;" />

2. Enter wallet name

<img src="./images/2021-12-02 17_12_07-Sparrow.png" style="zoom:67%;" />

3. Select New or Imported Software Wallet

<img src="./images/2021-12-02 17_12_21-Sparrow - Recovery Wallet.png" style="zoom:67%;" />

4. Enter 24 (or 12) Words -> Generate New

<img src="./images/2021-12-02 17_13_04-Sparrow - Recovery Wallet.png" style="zoom:67%;" />

*Note: This seed in the screenshot is only used as an example. Please do not use this one!*

**Now write off this seed and make it available to the heirs.**

5. Create Keystore -> m/84'/0'/0' -> Import Keystore -> Apply

<img src="./images/2021-12-02 17_20_11-Sparrow - Recovery Wallet.png" style="zoom:67%;" />

Now we need to generate a receive address from this wallet to create the Timelock transaction afterwards. To do this, click Receive and copy the address:

*bc1qts6l0rdq22l24fqm79mm9fmnk0zx9udlmps6nr*

<img src="./images/2021-12-02 17_22_23-Sparrow - Recovery Wallet.png" style="zoom:67%;" />

6. Connect Main Wallet with Sparrow

Now comes the Timelock transaction. To do this you need to connect your hardware wallet, multisig wallet or whatever you have to Sparrow and import it. If you are using Bitbox02, here is a good blog post from Stadicus on how it works with Sparrow and Bitbox: 

[Peek into Bitcoin internals using Sparrow Wallet with your BitBox02 (shiftcrypto.ch)](https://shiftcrypto.ch/blog/peek-into-bitcoin-internals-using-sparrow-wallet-with-your-bitbox02/)

Once you have access to your Main Wallet, click Send:

<img src="./images/2021-12-02 17_30_20-Sparrow - Main Wallet.png" style="zoom:67%;" />

Now enter the previously generated address from the recovery wallet for the recipient. You can enter your own text as a label. What you write here is not relevant. For the amount, you can now select either everything (Max button) or only individual UTXOs. 

**Important: If these UTXOs change in the future or new ones are added, you have to renew the Timelock transaction.**

Then click on: Create Transaction and right into the tab: Detail

<img src="./images/2021-12-02 17_34_33-Sparrow - An meine Erben.png" style="zoom:67%;" />

Under the item: Absolute Locktime you can now enter either the block height or a date from which the transaction should be valid.
If the entries are correct, click on "Finalize Transaction for Signing".



<img src="./images/2021-12-02 17_36_30-Sparrow - An meine Erben.png" style="zoom:67%;" />

Now you need to sign the transaction. This can be done with Bitbox, Ledger or whatever main wallet you use. In our example, the main wallet is also a software wallet, so you can simply click the sign button.

**Important: Some hardware wallets (e.g. BitBox) also show the Timelock block height on the display for verification. You should also check this with the recipient's address and the amount before you sign!**



**Export and save signed Timelock transaction**.

To do this, we first click on: View Final Transaction. Now you can copy the hex code (shown in color below) and attach it to the seed with the recovery wallet. In our example it is the following code: 

<img src="./images/2021-12-02 17_42_43-.png" style="zoom:67%;" />

The transaction can also be saved as a file: File -> Save Transaction (Ctrl + S)

<img src="./images/2021-12-02 17_42_55-Sparrow - An meine Erben.png" style="zoom:67%;" />

The signed transaction can now also be verified at any time by copy and paste here on this website:

[Bitcoin Wallet by Coinb.in](https://www.coinb.in/#verify)

<img src="./images/2021-12-02 17_44_58-Bitcoin Wallet by Coinb.in und 3 weitere Seiten - Persönlich – Microsoft​ Edge.png" style="zoom:67%;" />

#### **Conclusion**

It is certainly also always important that you include the contact details of a trusted person together with the signed Timelock transaction and the seed words for the recovery wallet. Basically, the heirs don't have to do anything more than broadcast this hex text (the raw Bitcoin transaction). Again, I have linked some websites below with what this can be done. But a trusted person who already knows what to do is certainly helpful.

In the example setup, I copied all the information to a USB stick. Attached is a README file with the following text:

--------------------------------------------------------------------------------------------------------------------------------

*This file describes how to deal with an incident and someone gets my bitcoins.* 
*I solved this as follows.*

*On this USB stick is a file with an already signed transaction from my cold storage setup. However, this transaction is only valid from a certain block height. This block height can be seen in the filename of the file.*

*The transaction is set up to send all my UTXOs to a new BTC Wallet. This new BTC Wallet is located incl. Seed on this USB stick. So the transaction just has to be broadcasted to the network.*

*For this I have listed some website links here.*
*https://btc.bitaps.com/broadcast*

*https://btc.network/broadcast*

*https://www.blockchain.com/btc/pushtx*

*https://blockchair.com/broadcast*

*https://hashraw.com/#broadcast*

*So all you have to do is copy the text in the TX file and paste it on one of these websites. After that, the Bitcoins will go to the wallet, which is located on this USB stick. You can decide for yourself what to do with them afterwards.* 

-------------------------------------------------------------------

If you have any questions, just contact me.


Twitter: [@cercatrova_21](https://twitter.com/cercatrova_21)

Telegram: @cercatrova21

E-Mail: [cercatrova21@protonmail.com](mailto:cercatrova21@protonmail.com)

If you were able to take any added value from this article, I would be very happy to receive a Lightning Donation:

<img src="./images/QR_lightning.png" style="zoom:67%;" />

LNURL1DP68GURN8GHJ7UMHD9EHXTT9DE5KWMTP9E3KSTMVDE6HYMRS9ASHQ6F0WCCJ7MRWW4EXCTEJX6Z6W3