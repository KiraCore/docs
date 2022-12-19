# [⏎](README.md#Roadmap) KIP_56
> SAIFU Login & Transaction Signing

We have to enable logging into the account using a QR code, that is by scanning or inputting a public key as raw text.

![picture 1](https://i.imgur.com/4E0g5mr.png)  

The instance where a **valid** public key is detected by the QR code scanner or by the textbox the UI should redirect user to his account balance page. 

Cache regarding all user preferences should be stored in an encrypted form similarly to software signer option, but this time using empty string as password. We will replace the empty password with the pin in the future. 

User should further be able to sign transactions using SAIFU by UI generating a QR code with unsigned transaction. A SAIFU dart library should be imported enabling for a data frame generation.

Following properties should be passed to the QR popup generation library.

* `props` - dictionary containing properties
  * Key: `version` - Value: `v0.0.1`
  * Key: `request` - Value: `sign`
  * Key: `pubkey` - Value: KIRA public key of the account that should sign the data
* `data` - Base 64 encoded data to be signed

After user closes the popup the frontend application should display a QR scanning popup that will await a signed message from the SAIFU wallet.

Once popup is closed transaction should be sent or error message displayed if something went wrong or user canceled the action.

