# ReplayKitDemo
Demo app for recording screen using Apple's ReplayKit framework. We use Xcode 11.0 with Swift 4.0 for this demo.

To start recording use the following method:

```

func startRecording() {
    if RPScreenRecorder.shared().isAvailable {
        RPScreenRecorder.shared().startRecording(handler: { (error) in
            if error == nil {
            } else {
                print(error ?? "")
            }
        })
    }
}

```

To stop recording use the following code:

```

func stopRecording() {
    
    RPScreenRecorder.shared().stopRecording { (previewController, error) in
        if error == nil {
            
            let alertController = UIAlertController(title: "Recording", message: "Do you wish to discard or view your gameplay recording?", preferredStyle: .alert)
            
            let discardAction = UIAlertAction(title: "Discard", style: .default) { (action: UIAlertAction) in
                RPScreenRecorder.shared().discardRecording(handler: { () -> Void in
                    // Executed once recording has successfully been discarded
                })
            }
            
            let viewAction = UIAlertAction(title: "View", style: .default, handler: { (action) in
                previewController?.previewControllerDelegate = self
                self.present(previewController!, animated: true, completion: nil)
            })
            
            alertController.addAction(discardAction)
            alertController.addAction(viewAction)
            
            self.present(alertController, animated: true, completion: nil)
            
        } else {
            print(error ?? "")
        }
    }
    
}

```

Delegate callback methods to dismiss preview controller launched by ReplayKit:

```
extension GameViewController: RPPreviewViewControllerDelegate {
    public func previewControllerDidFinish(_ previewController: RPPreviewViewController) {
        previewController.dismiss(animated: true, completion: nil)
    }
    
    
    /* @abstract Called when the view controller is finished and returns a set of activity types that the user has completed on the recording. The built in activity types are listed in UIActivity.h. */
    public func previewController(_ previewController: RPPreviewViewController, didFinishWithActivityTypes activityTypes: Set<String>) {
        
    }
}

```
