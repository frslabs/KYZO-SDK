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
- Add this in viewcontroller.swift where you wanted to show the Result.
```swift
    var receivedString = String()
    override func viewDidLoad() {
        super.viewDidLoad()
        NotificationCenter.default.addObserver(self, selector: #selector(handleOpenURL(notification:)), name: NSNotification.Name(rawValue: "HANDLEOPENURL"), object: nil)
        let delegate = UIApplication.shared.delegate as? AppDelegate
        if (delegate?.openUrl) != nil {
            delegate?.openUrl = nil
        }
    }
    @objc func handleOpenURL(notification:NSNotification){
        if let url = notification.object as? NSURL{
            receivedString = (((url.absoluteString)?.removingPercentEncoding)!).replacingOccurrences(of: "DemoSDK://", with: "")
        }
    }
```
 Add the below funtions to retrieve the documents Data.
- "dataArray" --> Where the name,address,dob,phone number,gender will reside
- "imageArray" --> Where the images of PAN,passport,Aaadhaar,Voter ID,PHOTO will reside
```swift
    var imageArray = [UIImage]()
    var dataArray = [String]()
    @objc func handleOpenURL(notification:NSNotification){
        if let url = notification.object as? NSURL{
            receivedString = (((url.absoluteString)?.removingPercentEncoding)!).replacingOccurrences(of: "DemoSDK://", with: "")
	    parseData(param: convertStringToDictionary(text: receivedString)!)
        }
    }
   func convertStringToDictionary(text: String) -> [String:AnyObject]? {
        if let data = text.data(using: .utf8) {
            do {
                let json = try JSONSerialization.jsonObject(with: data, options: []) as? [String:AnyObject]
                return json
            } catch {
                print("Something went wrong")
            }
        }
        return nil
    }
    func parseData(param:[String:AnyObject]){
        if let name = param["name"]{
            dataArray.append("name : " + (name as? String ?? ""))
        }
        if let address = param["address"]{
            dataArray.append("address : " + (address as? String ?? ""))
        }
        if let dateofbirth = param["dateofbirth"]{
            dataArray.append("dateofbirth : " + (dateofbirth as? String ?? ""))
        }
        if let gender = param["gender"]{
            dataArray.append("gender : " + (gender as? String ?? ""))
        }
        if let phonenumber = param["phonenumber"]{
            dataArray.append("phonenumber : " + (phonenumber as? String ?? ""))
        }
        
        if let panImageStr = param["panimage"]{
            imageArray.append(convertBase64ToImage(inputString: panImageStr as? String ?? ""))
        }
        if let passportImageStr = param["passportimage"]{
            imageArray.append(convertBase64ToImage(inputString: passportImageStr as? String ?? ""))
        }
        if let aadhaarFrontImageStr = param["aadhaarfrontimage"]{
            imageArray.append(convertBase64ToImage(inputString: aadhaarFrontImageStr as? String ?? ""))
        }
        if let aadhaarBackImageStr = param["aadhaarbackimage"]{
            imageArray.append(convertBase64ToImage(inputString: aadhaarBackImageStr as? String ?? ""))
        }
        if let voterFrontImageStr = param["voteridfrontimage"]{
            imageArray.append(convertBase64ToImage(inputString: voterFrontImageStr as? String ?? ""))
        }
        if let voterBackImageStr = param["voteridbackimage"]{
            imageArray.append(convertBase64ToImage(inputString: voterBackImageStr as? String ?? ""))
        }
        if let photo = param["photo"]{
            imageArray.append(convertBase64ToImage(inputString: photo as? String ?? ""))
        }
    }
    func convertBase64ToImage(inputString:String) -> UIImage{
        let dataDecoded : Data = Data(base64Encoded: inputString, options: .ignoreUnknownCharacters)!
        guard let convertedImage = UIImage(data: dataDecoded)else{return UIImage()}
        return convertedImage
    }
     
```     
## SDK Result
  The data will reside in dataArray and imageArray.
  Sample Output:
  dataArray --> ["name : John smith", "address : 355,2ndfloor,5thmain,karnataka,560071", "dateofbirth : 09/03/1993", "gender : Male", "phonenumber : 9704258166"]
  imageArray --> [<UIImage: 0x280b49ce0>, {960, 590}, <UIImage: 0x280b4aa70>, {772, 633}, <UIImage: 0x280b4dd50>, {831, 496}, <UIImage: 0x280b4dea0>, {923, 557}, <UIImage: 0x280b4abc0>, {598, 938}, <UIImage: 0x280b4dff0>, {562, 881}, <UIImage: 0x280b4e140>, {322, 407}]

## Help

For any queries/feedback , contact us at `support@frslabs.com` 

