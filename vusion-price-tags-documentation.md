# Vusion Price Tags Documentation

## Vusion Manager
Vusion Manager is used to manage the stores and their access points and price tags  
https://manager-us.vusion.io/

### Setup new store
1. Send an email requesting a new store be added with the branch name and branch number
2. Once the store is setup send a list of items
    * Run the PriceTagUpdate script
3. Get the access point installed in the store
4. [Setup new access point](#setup-new-access-point) for the store in Vusion Manager
5. [Setup Vusion Link](#setup-vusion-app) on phones that will be used to link price tags to items

### Setup new access point
1. Go to https://manager-us.vusion.io/chains/uap_idealsupply/stores
2. Click on the store you want to add the new access point to
3. Click the `Infrastructure` tab
4. Click the `Add` button
5. Enter the access points Transmitter ID
    * The Transmitter ID can be found on the bottom of the access point of on a piece of paper in the access points box
6. Click the `Save` button
7. Wait for the status of the new access point to say `OK`

## Vusion Link App
The Vusion Link App is used to link a price tag to an item  
[Android Play Store](https://play.google.com/store/apps/details?id=com.ses.link.linkvcore)  
[Apple App Store](https://apps.apple.com/ca/app/vusion-link/id1542333267)

### Setup Vusion Link
1. Download the `VUSION Link` app from the app store
2. Open the app and click the `USA` button
3. Click the `SCAN QR CODE` button and scan the stores QR code

### How to Get QR Code
#### Vusion Manager
1. Go to https://manager-us.vusion.io/chains/uap_idealsupply/stores
2. Click on the store you want
3. Click the `Phone with a gear icon` in the top right hand of the screen

#### Vusion Link App
Only if you already have Vusion Link connected to the store
1. Click the `Gear icon` in the top right hand of the screen
2. Click the `Advanced` button
3. Click the `QR code icon`

### How to Logout
1. Click the `Gear icon` in the top right hand of the screen
2. Click the `Log out` button

### Link Price Tag to an Item
1. Open the Vusion Link App and make sure it is connected to a store and the correct one
2. Click the `Camera icon` beside the `Label Id` text field
    * Or turn of the barcode scanner and type in the field manually
3. Scan the barcode on the bottom or back of the price tag 
    * Or type in the ID on the back of the price tag
4. Click the `Camera icon` beside the `Item Id` text field
    * Or turn of the barcode scanner and type in the field manually
5. Scan the items UPC code or the items line code and part number
    * Or type in the UPC code or the items line code and part number

### Turn Off Barcode Scanner
- If you are trying to link price tags to items but the barcode is not scanning, it does not have a barcode, or you just want to use a keyboard you can click the toggle beside the `Gear icon` in the top right hand of the screen to switch from the barcode scanner to the keyboard


## Vusion Studio
Vusion Studio is used to edit how the data is displayed on the price tags  
Login with the uap_idealsupply@studio.com account  
https://studio.ses-imagotag.com/

### Upload Studio File
1. Go to the Ideal Supply Vusion Studio project
2. Click the `Generate Settings` button
3. Go to https://manager-us.vusion.io/chains/uap_idealsupply/configuration/settings
4. Click the `Plus icon` button under the `Manual Deploy` tab
5. Click on `Select or Drop an .SMF or .ZIP file here` and select the settings file the you downloaded from Vusion Studio
6. Select the new file in the drop down under the `Manual Deploy` tab
7. Select the checkbox's beside all the stores you want to update
8. Click the `Save` button


### TODO
- How to use


## Vusion API Portal
The Vusion API Portal shows all the endpoints used for the Vusion Manager  
Login with the support@idealsupply.com account  
https://api-portal-us.vusion.io/


## Vusion Contacts
Vusion Contacts as of Augest 9, 2023

### Edisan Jayaseelan
> Solution Project Manager - North America  
Phone: +1 (617) 319-2172  
Email: edisan.jayaseelan@ses-imagotag.com

### William Arfi
> Business Developer - Canada  
Email: william.arfi@ses-imagotag.com
