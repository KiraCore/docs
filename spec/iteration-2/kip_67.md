# [⏎](README.md#Roadmap) KIP_67
> Explorer Mode

Create explorer mode where users have ability to preview changes occurring on the network live without need to log in or even provide public address.

Currently frontend has following tabs:
* Account
* Deposits
* Withdraws
* Network
* Proposals

## Landing Page

The URL should accept an RPC address as parameter enabling for access to the website and the address parameter enabling instant connection using specified url.

If user log's out or RPC is not specified then user should see a standard log-in page.

Account selection (log-in) page should be extended with a dedicated "Explorer" button so that user has option not to log-in using a key file or mnemonic if he/she does not want to.

## Account Balances

In the explorer mode account tab should enable preview of account balances of any address that would be queried. A search bar must be added enabling for the account query. The search bar should have a regex enabling for detection if user input a correct address. If the address is not correct then the border colour of the search bar should change. It must be ensured that whitespaces and new line characters are ignored before regex test occurs.

User should have ability to query address by clicking enter or by pressing a dedicated search button.

The URL should accept an address parameter enabling query without need to input any specific address. We have to further create a dedicated "share" button enabling to share a standalone URL with address and /account tab path allowing anyone to explore the address.

## Deposits

Deposits tab should only appear if user browsed/searched for particular account in the account tab or if URL contains account parameter. In that view we should only display deposit transaction to the account.

## Withdraws

Analogically to Deposits, should only be visible if URL contains account parameter.

## Network

This tab should be visible regardless if account parameter is specified or not. 

If account is specified then it should be assumed to be validator account and the validator sub-tab with expanded entry showing all details of the particular validator should be shown.

If `conskey` is specified then it should be assumed to be validator consensus key and the validator sub-tab with expanded entry showing all details of the particular validator should be shown.

Similarly URL specified `tx` hash, `btx` block tx hash and `bnr` block number should result in UI displaying relevant information in the Networking tab.

All interactions with the UI (such as expanding tab of particular validator, block or transaction) should result in changes in the URL allowing user to share the same URL publicly.

## Settings 

Settings tab should be replaced with list of log-in options allowing for signing in with a key file, mnemonic etc. in similar fashion on a landing screen after user establishes connection.






