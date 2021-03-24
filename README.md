# KYZO-SDK

KYZO SDK can be used to integrate by native applications to request documents from Kyzo. This will open up Kyzo if installed and allow the user to share by consent and PIN and data shared will come to the SDK. 


# Table Of Content
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage example](#Usage-example)
- [SDK Result](#SDK-Result)
- [Help](#help)


## Minimum Requirements

- iOS 11.0+
- Xcode 12.4

## Installation

URL Schemes: 
This is used to communicate between SDK and KYZO App.

Register the SDK in Info.plist with the some custom name (Eg: DemoSDK) like below:

```
<key>CFBundleURLTypes</key>
<array>
   <dict>
     <key>CFBundleURLSchemes</key> 
     <array>
	<string>DemoSDK</string>
     </array>
   </dict>
</array>
```

Register the KYZO in Info.plist with the some custom name (Eg: KYZOApp) like below:

```
<key>CFBundleURLTypes</key>
<array>
   <dict>
     <key>CFBundleURLSchemes</key> 
     <array>
	<string>KYZOApp</string>
     </array>
   </dict>
</array>
```

## Usage example
Calling the function in Demo SDK to invoke KYZO App to select the Documents for share
```swift
let url = ("KYZOApp://")
UIApplication.shared.open(NSURL(string: url)! as URL, options: [:], completionHandler: nil)
```
- This opens up the KYZO App and asks to select documents to share.
- Once user selects and share with enter PIN sucess in KYZO.
- Response is sent to Demo SDK.

#### Handling the result
- This is used KYZO to share the data from KYZO to Demo SDK with JSON String
```swift
let url1 = ("DemoSDK://"+kyzoSDKString).   // DemoSDK has requested the Documents so that property identifier["DemoSDK"] should be used here.
let url2 = url1.addingPercentEncoding(withAllowedCharacters: (NSCharacterSet.urlQueryAllowed))
UIApplication.shared.open(NSURL(string: url2!)! as URL, options: [:], completionHandler: nil)
``` 
- Add this in Appdelegate of Demo SDK
```swift
var openUrl:NSURL?
    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        self.openUrl = url as NSURL?
        NotificationCenter.default.post(name: NSNotification.Name(rawValue: "HANDLEOPENURL"), object: url)
        return true
    }
```
## SDK Result

```swift

   
     
```     

  
## Help

For any queries/feedback , contact us at `support@frslabs.com` 

