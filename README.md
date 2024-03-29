# CAPTUS iOS SDK
![version](https://img.shields.io/badge/version-v1.5.2-blue)

The Captus SDK is a set of screens to capture the front and back images of ID documents. It also allows the user to manually verify that the documents are clean and clear. This SDK is useful for IDs that cannot be processed on the mobile and needs server-side processing. 

You can find the release history at [Changelog](CHANGELOG.md)

‼ ATTENTION ‼ → BREAKING CHANGE introduced at Octus SDK `v1.5.1`. We have introduced a new license format. If you are using versions prior to `v1.5.1` and intend to update to `v1.5.1` or above please contact `support@frslabs.com` for an updated license.

# Table Of Content

- [Prerequisite](#prerequisite)
- [Requirements](#requirements)
- [Permission](#permission)
- [Installation](#installation)
- [Getting Started](#getting-started)
- [Captus Error Codes](#captus-error-codes)
- [Help](#help)

## Prerequisite

***NOTE : Encryption of CAPTUS SDK Result is under development***

You will need a valid license and Netrc credentials to use the Captus SDK, which can be obtained by contacting support@frslabs.com. 

Once you have the license , follow the below instructions for a successful integration of Captus SDK onto your iOS Application.

## Minimum Requirements

- Xcode 12.5
- iOS 13.0+
- Swift 5.0

## Permission

In Info.plist file add following code to allow your application to access iPhone's camera:
``<key>NSCameraUsageDescription</key>
<string>Allow access to camera</string>``

## Installation

### Cocoapods


You can use [CocoaPods](http://cocoapods.org/) to install `captus` by adding it to your `Podfile`:

```ruby
source 'https://gitlab.com/frslabs-public/ios/captus-ios.git'
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '12.0'
target '<Your Target Name>' do
use_frameworks!
pod 'Captus', '1.5.1'
pod 'Alamofire', '~> 4.9.1'
end
```

###### Save/Edit Netrc settings to install custom pod

You will need a valid netrc credentials to install captus from maven, which can be obtained by contacting `support@frslabs.com`. 

1. Create or edit .netrc file under current user's home directory
2. Write the below lines into that file, replace <YOUR_USERNAME> and <YOUR_PASSWORD> with your credentials which is shared through email and save the file.
```ruby
machine captus-ios.repo.frslabs.space
login <YOUR_USERNAME>
password <YOUR_PASSOWRD>
```
3. In terminal enter below command to install the pod

   pod install or pod update.

4. Connect with physical device to build and run captus, It will not build/run in simulator due to camera dependency.

To get the full benefits import `captus` wherever you import UIKit

``` swift
import UIKit
import Captus
```

## Getting Started

### Swift

1. Invoke Captus SDK

```swift
    // ...
    
    override func viewDidAppear(_ animated: Bool) {
          let ocrVC = OCRNavigationViewController(delegate: self)
          ocrVC.modalPresentationStyle = .fullScreen
          ocrVC.licenceKey = "LICENSE_KEY"
          ocrVC.baseURL = "BASE_URL"
          ocrVC.referenceID =  "SERVER_REFENENCE_ID"
          ocrVC.serverHeader = "SERVER_HEADER"
          ocrVC.idSides = 1    // 1 or 2 based on ID sides
          ocrVC.documentType = CaptusDocument.PAN.rawValue    // used while capturing for IDs
          ocrVC.isAutomatic = true      // true for capturing ID images, false for Egypt API
          if ocrVC.isOCR == true{       // if invoking SDK for Egypt API
            ocrVC.serverHeader = serverHeader
          }else{                       // if invoking for capturing IDs Images
            ocrVC.serverHeader = ""
          }
          present(ocrVC, animated: true)
      }
    // ...    
```

2. Handling the result and import delegate OCRDelegate

```swift
class YourViewController: UIViewController,OCRDelegate {

  func captusSuccessResponse(ocr: OCRNavigationViewController, didFinishOcrWithResult results: CaptusResults) {
        let amlCheck = results.amlCheckStatus
        // Will return Array of images path
        let imagePath = results.imagePath
        // Will return Array of image reference Id
        let imageReferenceId = results.imageReferenceId
    }
    func captusFailureResponse(ocr: OCRNavigationViewController, didFailWithError error: String) {
        // Error
        let error =  error
    }
  
}
```
## Captus Parameters
   Types of Documents:
 
- `ocrVC.documentType = CaptusDocument.PAN.rawValue ` ***(Required)***
  
  Sets the Document which has to be scanned. Possible values are, 
  
  | Value          | Effect                 |
  | -------------- | ---------------------- |
  | CaptusDocument.PAN   | Pan Card               |
  | CaptusDocument.PPT   | Passport               |
  | CaptusDocument.ADR   | Aadhaar Card           |
  | CaptusDocument.VID   | Voter ID               |
  | CaptusDocument.DRV   | Driving Licence        |
  
 ## Captus Error Codes

   Following error codes will be returned on the `onCaptusFailure` method of the callback

   | CODE | DESCRIPTION                  |
   | ---- | ---------------------------- |
   | 801  | Scan Time Out               |
   | 803  | Camera permission denied    |
   | 804  | Capture interrupted            |
   | 805  | Captus SDK License has expired             |
   | 806  | Captus SDK License is invalid             |
   | 808 | Transaction failed / Unable to ping |
   | 901  | Network error               |
   | 902  | Image upload failed                  |
   | 1001 | Error parsing result         |


   Sets the Captus SDK apiCredentials . Obtain the appropriate api credentials through a REST API call , for details about     the REST API, contact `support@frslabs.com`


   ## Help
   For any queries/feedback , contact us at `support@frslabs.com` 
