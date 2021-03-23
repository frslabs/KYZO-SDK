# KYZO-SDK

KYZO SDK can be used to integrate by native applications to request documents from Kyzo. This will open up Kyzo if installed and allow the user to share by consent and PIN and data shared will come to the SDK. 


# Table Of Content
- [Requirements](#requirements)
- [Usage example](#Usage-example)
- [SDK Result](#SDK-Result)
- [Help](#help)


## Minimum Requirements

- iOS 11.0+
- Xcode 12.4


## Usage example

```swift
import Forus

@objc func invokeForus(){

     let forus = ForusController(delegate:self)
        forus.modalPresentationStyle = .fullScreen
        forus.licenceKey = "LICENCE_KEY"
        forus.livenessMode = LivenessMode.smile.rawValue (smile) / LivenessMode.eyeBlink.rawValue (eyeblink)
        forus.setCamera = Utility.front.rawValue (default) / Utility.back.rawValue 
        forus.isTimeOnFaceNeeded = true / false (Boolean)
        forus.timestampColor = UIcolor.yellow (Color)
        forus.timestampFontSize = 30.0 (CGfloat)
        forus.timeFormat = "yyyy-MM-dd HH:mm:ss" (default) (String)
        present(forus, animated: false, completion: nil)
        
}
```
#### Handling the result

```swift

class ViewController: UIViewController, ForusControllerDelegate {

    func forusControllerSuccess(_ scanner: ForusController, didFinishScanningWithResults results: forusResult) {
         print("Forus Result: ", results)
      
    }
    
    func forusControllerDidCancel(_ scanner: ForusController) {
        
    }
    
    func forusControllerFailed(_ scanner: ForusController, didFailWithError error: Int) {
        print("Forus Error Code: ", error)
    }
    
    }
``` 

## SDK Result

```swift

    let faceImage = results.faceResult   // Face image UIImage format 
    let isFaceDetected = results.isFaceDetected   // Boolean value for face detection
    let isSmileDetected = results.isSmileDetected   // Boolean value for smile detection on face
    let isEyeBlinkDetected = results.isEyeBlinkDetected   // Boolean value for eye blink detectin on face
    let timeStamp = results.timeStamp    // Timestamp, Int64 format 
     
```     

  
## Help

For any queries/feedback , contact us at `support@frslabs.com` 

