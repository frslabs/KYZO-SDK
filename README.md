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

```swift
This is used in KYZO App with the.


```
#### Handling the result

```swift


``` 

## SDK Result

```swift

   
     
```     

  
## Help

For any queries/feedback , contact us at `support@frslabs.com` 

