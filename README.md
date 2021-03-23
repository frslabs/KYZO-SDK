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
Add function in your AppDelegate , should look like that:

class AppDelegate: UIResponder, UIApplicationDelegate {

    var window: UIWindow?
    var openUrl:NSURL?
    
    func application(_ app: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey : Any] = [:]) -> Bool {
        self.openUrl = url as NSURL?
        NotificationCenter.default.post(name: NSNotification.Name(rawValue: "HANDLEOPENURL"), object: url)
        return true
    }
    }
    
   Add function in your ViewController Class , should look like that:
   class ViewController: UIViewController {
     override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view.
        NotificationCenter.default.addObserver(self, selector: #selector(handleOpenURL(notification:)), name: NSNotification.Name(rawValue: "HANDLEOPENURL"), object: nil)
        let delegate = UIApplication.shared.delegate as? AppDelegate
        if (delegate?.openUrl) != nil{
            delegate?.openUrl = nil
        }
    }
    @objc func handleOpenURL(notification:NSNotification){
        print("handle url called")
        if let url = notification.object as? NSURL{
            receivedString = (((url.absoluteString)?.removingPercentEncoding)!).replacingOccurrences(of: "KYZODataReceiver://", with: "")
           // print("KYZO RECEIVER ",receivedString)
        }
    }
    }


```
#### Handling the result

```swift


``` 

## SDK Result

```swift

   
     
```     

  
## Help

For any queries/feedback , contact us at `support@frslabs.com` 

