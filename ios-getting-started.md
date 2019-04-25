## Required

Before you can build and use the Proximity SDK on iOS, the following must be true:

- You have an `auth token`. This token should be generated by your server using a shared secret provided to you by Interac.
- The deployment target for your app is `iOS 9.0` or newer.

## Add the Proximity SDK to your native project

### Option 1: Cocoapods

If you don't have Cocoapods installed on your local environment please refer to the [Getting Started](https://guides.cocoapods.org/using/getting-started.html#getting-started) page on the Cocoapods website.

To install using [Cocoapods](https://cocoapods.org), add the following to your `Podfile`

__Latest__

```
source 'https://ext-01.ghent.interac.ca/UnifiedSDK/IOSProximitySDK' 

target :YourTargetName do
	pod 'InteracProximitySDK', '~>0.1.1'
end
```

Then in the terminal run the command:

```
pod install
```

### Option 2: Carthage

If you don't have Carthage installed on your local environment please refer to the [Installing Carthage](https://github.com/Carthage/Carthage#installing-carthage) section in the Carthage Github repository.

To check that Carthage is installed correctly, open the terminal and run the following command:
```
carthage version
```

If all has gone to plan, you’ll see the version number of Carthage that was installed.

To install using [Carthage](https://github.com/Carthage/Carthage), add the following to your `Cartfile`:

__Use the Version Compatible To:__
```
binary 'https://fi.interac.ca/carthage/YOUR_CARTHAGE_KEY_GOES_HERE/proximitySDK.json" ~> 1.2.6
```

Using the above Carthage will download a ProximitySDK version compatible with 1.2.6. Compatibility is determined according to Semantic Versioning. This means that any version greater than or equal to 1.2.6, but less than 1.3, will be considered compatible with 1.2.6.

__Use a Specific Version:__
```
binary 'https://ext-01.ghent.interac.ca/UnifiedSDK/IOSProximitySDK/proximitySDK.json" == 1.2.6
```

__Use the Latest Version:__
```
binary 'https://ext-01.ghent.interac.ca/UnifiedSDK/IOSProximitySDK/proximitySDK.json" >= 1.2.6
```

Then in the terminal run the command:

```
carthage update --platform iOS
```

When you run `carthage update`, Carthage creates a couple of files and directories for you in your project directory.
Open the `Carthage` folder and then the `Build` folder.

In Xcode, open the `General` tab for your app target in Xcode.
Drag `InteracProximitySDK.framework` from the `Build` folder into the `Linked Frameworks and Libraries` section.

Next, switch over to Build Phases and add a new Run Script build phase by clicking the + in the top left of the editor. Add the following command:

```
/usr/local/bin/carthage copy-frameworks
```

Click the + under Input Files and add an entry for each framework:

```
$(SRCROOT)/Carthage/Build/iOS/InteracProximitySDK.framework
```

### Option 3 Manual installation

#### Download

- Download the last version of the SDK from [Interac Proximity SDK repository release section](https://ext-01.ghent.interac.ca/UnifiedSDK/IOSProximitySDK/releases)
- Unzip the content of the downloaded file.

#### Add the framework

- Open the `General` tab for your app target in Xcode.
- Drag the newly downloaded `InteracProximitySDK.framework` into the `Embedded Binaries` section.
- Select `Copy Items if Needed` and click `Finish` in the dialog that appears.

### Step 2 - Setup the SDK
Right after a user logged in, call: `setAPIKey()` on `InteracProximitySDK` and pass the Interac Proximity API Key.

__SWIFT__
```
InteracProximitySDK.setAPIKey("<Interac Proximity API Key>")
```

__OBJECTIVE-C__
```
[InteracProximitySDK setAPIKey:@"<Interac Proximity API Key>"];
```

### Step 3 - Create and show the SDK

Now that your SDK is instantiated, you need to configure it before you can launch it. The SDK provides some objects you need to create and pass to the launch method.

- `INPTheme`: This object will define the customization of the SDK to fit your brand:

	- primaryColor: UIColor
	- primaryContrastColor: UIColor
	- secondaryColor: UIColor
	- secondaryContrastColor: UIColor
	- logoImage: UIIMage

__SWIFT__
```
let theme = INPTheme()
theme.logoImage = UIImage(named: logoName)
theme.primaryColor = UIColor(hue: 0.671, saturation: 0.015, brightness: 0.283, alpha: 1.000)
theme.primaryContrastColor = .white
theme.secondaryColor = UIColor(hue: 0.671, saturation: 0.015, brightness: 0.283, alpha: 1.000)
theme.secondaryContrastColor = .white
```

__OBJECTIVE-C__
```
INPTheme *theme = [[INPTheme alloc] init];
theme.logoImage = [UIImage imageNamed:@"bankLogo"];
theme.primaryColor = [UIColor colorWithRed:0.000 green:0.239 blue:0.172 alpha:1.000];
theme.primaryContrastColor = [UIColor whiteColor];
theme.secondaryColor = [UIColor colorWithRed:0.328 green:0.727 blue:0.281 alpha:1.000];
theme.secondaryContrastColor = [UIColor whiteColor];
```

- `INPAccount`: This object represents a bank account:
	- identifier: String
	- accountNumber: String
	- name: String
	- balance: NSDecimalNumber
	- limit: NSDecimalNumber
	- fee: NSDecimalNumber

__SWIFT__
```
let chequingAccount = INPAccount(
            identifier: "24157547854147",
            accountNumber: "47854214785",
            name: "Chequing Account",
            balance: NSDecimalNumber(string: "300"),
            limit: NSDecimalNumber(string: "500"),
            fee: NSDecimalNumber(string: "0"))
```

__OBJECTIVE-C__
```
INPAccount *chequingAccount = [[INPAccount alloc] initWithIdentifier:@"24157547854147"
     accountNumber:@"47854214785"
					   name:@"Chequing Account"
          balance:[[NSDecimalNumber alloc] initWithString:@"300"]
				           limit:[[NSDecimalNumber alloc] initWithString:@"500"]
							       fee:[[NSDecimalNumber alloc] initWithString:@"0"]];
```

- `INPCustomerInfo`: This object contains all the customer's information:
	- firstName: String
	- lastName: String
	- interacUUID: String
	- listOfAccounts: [INPAccount]

__SWIFT__
```
let accounts = [chequingAccount]
let customerInfo = INPCustomerInfo(interacUUID: "487b19f7-4d93-4796-9691-dd84808d9b9c",
																		 firstName: "John",
																		  lastName: "Applebee",
														    listOfAccounts: accounts)
```

__OBJECTIVE-C__
```
NSArray *accounts = @[chequingAccount];
INPCustomerInfo *customerInfo = [[INPCustomerInfo alloc] initWithInteracUUID:@"487b19f7-4d93-4796-9691-dd84808d9b9c"
		      firstName:@"John"
				   lastName:@"Applebee"
	   listOfAccounts:accounts];
```

Once all the required objects have been created you can call the launch method and pass the previously created theme and customer info objects.

__SWIFT__
```
InteracProximitySDK.sharedInstance().presentModallyOverViewController(
            self,
            theme: theme,
            customerInfo: customerInfo,
            delegate: self)
```

__OBJECTIVE-C__
```
[[InteracProximitySDK sharedInstance] presentModallyOverViewController:self
                           theme:theme
                    customerInfo:customerInfo
			                  delegate:self];
```	
### Step 4 - Listen to the SDK callbacks

The SDK is using delegation to call back the app once a request or send transfer has been initiated.

One class should conform to the protocol `InteracProximitySDKDelegate` and set itself as the `delegate` of the `InteracProximitySDK`

Implement the delegate method: `interacProximitySDK:didInitiateTransferToUserId:nickname:amount:accountId:completionHandler:`

In the completion handler, return `true` or `false` to indicate whether the transfer was successful or not.

## Add the SDK to your React Native project
## Add the SDK to your Flutter project