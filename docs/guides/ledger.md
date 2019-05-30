# Ledger Hardware Wallet Guide
NERVA has support for the Ledger Nano S hardware wallet, but is not currently included in the Ledger Live application, so some configuration is required to 'sideload' the NERVA app onto your Ledger device. You will need to have Python installed on your computer, and to [download the NERVA Ledger files](https://getnerva.org/#downloads).

You should read and understand this document and ensure you are confident in the process before continuing. No responsibility will be taken for loss of funds or any unexpected behaviour of your device. If you aren't comfortable with this process, it's recommended to use a paper wallet to store your NERVA funds.

### Supported environment

The Ledger app is ONLY tested with the Ledger Nano S with Firmware 1.5.5. No other configurations will work.  
The app is specifically compiled against the 1.5.5 firmware. If you require it for a different unsupported firmware please download the source code from [BitBucket](https://bitbucket.org/nerva-project/ledger-app-nerva)

### Before update

Before update, make sure you have a means of wallet recovery. Ensure you have a backup of your seed and that you can restore from that seed.  
Before installing the new version, run `ledger uninstall` in this package to uninstall the old version.
 
### Installation

Before proceeding, please ensure you have python installed and your ledger is visible within the ledger live app.

There is an installation script for Linux/Mac and Windows which contain the following functions

- Setup: `ledger setup`  
This option will set up the ledger loader so you can install the app

- Install: `ledger install`
This option will install the nerva app on your ledger

- Uninstall: `ledger uninstall`
This option will uninstall the ledger app.

- Seed: `ledger seed`
This option will send the seed for your ledger wallet to the device so that it can be viewed under the *Show Seed* option within the app  
It is advised to backup the seed for your wallet, as it will allow you to restore the wallet independent of an ledger device in the future

### Using the ledger with the Nerva CLI

Generate a hardware wallet  
`./nerva-wallet-cli --generate-from-device <wallet-path>`

Once done, you can load the wallet in future with  
`./nerva-wallet-cli --wallet-file <wallet-path> --hw-device "Ledger"`
