![image alt text](image_0.png)

# Avnera Corporation

SDHMI iOS Framework

SDHMI iOS 

Framework Documentation

Version 1.0

July 18, 2016

# Table of Contents

[[TOC]]

# Initial Setup

### How to import the framework into the project

First open the project that you want to add AvneraSDHM framework

Then select the file .xcodeproj and go to the target that you want

Search the Embedded Binaries section

Press the + icon

Press add Other… button

Search the framework, after that please press open.

Add the following line in all classes that you need to use the framework: 
```
*#import <AvneraSDHM/AvneraSDHM.h>*
```
### Plist setup

Add the following key in the info.plist file in your project:
```
*Key: Supported external accessory protocols*
*Type: Array*
```
Inside of this array add the following item:
```
*Value: com.avnera.sdhm this is the EA Protocol Name in the board*
*Type: String*
```
### Recommendations

It is recommend to create an instance of the class AvneraService as a service.  A singleton class can be used for this.

### Class setup

The class that is going to be used as a service needs to include the delegate AvneraServiceDelegate.  Following is an example of how to include the delegate:
```Swift
class HomeViewController: UIViewController, AvneraServiceDelegate {
}
```

Also you must create a property of type AvneraService and set the delegate.
```Swift
import UIKit
class HomeViewController: UIViewController, AvneraServiceDelegate {
    let avneraService = AvneraService()
    
    override func viewDidLoad(){
        super.viewDidLoad()
        self.avneraService.delegate = self
    }
}
```

# Methods:

### Automatic Connection

After you initialize the AvneraService property you will start receiving callbacks through the delegate interface.  In order to have the application load automatically when the device is connected, implement the following method:

<table>
  <tr>
    <td>override func viewDidLoad(){
    super.viewDidLoad()
    self.avneraService.delegate = self
    self.avneraService.startConnectionAutomaticallyWhenDeviceIsConnectedAndTheApplicationIsLaunched()
}</td>
  </tr>
</table>


**Possible Errors Returned:**

* CONNECTION_PROCESS_FAILED_IS_ALREADY_CONNECTED:  The device is currently connected

### Connection Status

To determine if the device is already connected, implement the following method:

<table>
  <tr>
    <td>func isDeviceConnected(){
    if self.avneraService.isAvneraDeviceConnected(){
        print("Device is already connected")
    }else{
        print("Device is not connected")
    }
}</td>
  </tr>
</table>


### Set Noise Cancellation State

To set the Noise Cancellation state implement the following method:  

<table>
  <tr>
    <td>func setNoiseCancellationState(){
    avneraService.avneraModule.setNoiseCancelationState(true)
}</td>
  </tr>
</table>


Note that methods that operate on the device will not work unless the device is connected.

**Possible Errors Returned:**

* DEVICE_NOT_CONNECTED:  The device is currently not connected and the function cannot be executed

* NO_ROW_DATA_ERROR:  The package sent to the board is nil

### Request Noise Cancelation State

This method will return true if noise cancelling is enabled, false otherwise.

If you want to receive a noise cancelling state callback, you will need to call this method: requestNoiseCancelationState().  Note that the device needs to be connected to the iOS device for the callback to work.

<table>
  <tr>
    <td>func requestNoiseCancelationState(){
    avneraService.avneraModule.requestNoiseCancelationState()
}</td>
  </tr>
</table>


And the callback is the following:

<table>
  <tr>
    <td>func isANoiceCancelationEnable(state: Bool) {
    if state {
        //Noise Cancellation is enabled
    }else{
        //Noise Cancellation is disabled
    }
}</td>
  </tr>
</table>


**Possible Errors Returned:**

* DEVICE_NOT_CONNECTED:  The device is currently not connected and the function cannot be executed

* NO_ROW_DATA_ERROR:  The package sent to the board is nil

* CHALLENGE_LENGHT_NOT_16_BYTES_AS_EXPECTED:  The package doesn’t have the correct size

* NO_ROW_DATA_ERROR:  The package sent to the board is nil

### Request Max Values For Left And Right Sides on Ambient Listener

To get the max values for left and right side on ambient listener, call the following method:

<table>
  <tr>
    <td>func requestMaxValuesForLeftAndRightSidesOnAmbientListener() {
    avneraService.avneraModule.requestMaxValuesForLeftAndRightSidesOnAmbientListener()
}</td>
  </tr>
</table>


The values will be returned by the following callback:

<table>
  <tr>
    <td>func getMaxValueForAmbientListening(value: Int){
}</td>
  </tr>
</table>


**Possible Errors Returned:**

* DEVICE_NOT_CONNECTED:  The device is currently not connected and the function cannot be executed

* NO_ROW_DATA_ERROR:  The package sent to the board is nil

* CHALLENGE_LENGHT_NOT_16_BYTES_AS_EXPECTED:  The package doesn’t have the correct size

* NO_ROW_DATA_ERROR:  The package sent to the board is nil

### Request Left Value For Ambient Listener

To get the left value for ambient listener, call the following method:

<table>
  <tr>
    <td>func requestLeftValueForAmbientListening() {
    avneraService.avneraModule.requestLeftValueForAmbientListening()
}</td>
  </tr>
</table>


The value will be returned on the following callback:

<table>
  <tr>
    <td>func getLeftValueInAmbientListening(value: Int){
}</td>
  </tr>
</table>


**Possible Errors Returned:**

* DEVICE_NOT_CONNECTED:  The device is currently not connected and the function cannot be executed

* NO_ROW_DATA_ERROR:  The package sent to the board is nil

* CHALLENGE_LENGHT_NOT_16_BYTES_AS_EXPECTED:  The package doesn’t have the correct size

* NO_ROW_DATA_ERROR:  The package sent to the board is nil

### Request Right Value For Ambient Listener

To get the right value for ambient listener, call the following method:

<table>
  <tr>
    <td>func requestRightValueForAmbientListening(){
    avneraService.avneraModule.requestRightValueForAmbientListening()
}</td>
  </tr>
</table>


The value will be returned on the following callback:

<table>
  <tr>
    <td>func getRightValueInAmbientListening(value: Int){
}</td>
  </tr>
</table>


### Possible Errors Returned:

* DEVICE_NOT_CONNECTED:  The device is currently not connected and the function cannot be executed

* NO_ROW_DATA_ERROR:  The package sent to the board is nil

* CHALLENGE_LENGHT_NOT_16_BYTES_AS_EXPECTED:  The package doesn’t have the correct size

* NO_ROW_DATA_ERROR:  The package sent to the board is nil

### Set Left Value In Ambient Listening

To set the left value in ambient listening, call the following method with a value between 0 and Max Ambient Listening value for left side (in the example method, 4 is the value being used):

<table>
  <tr>
    <td>func setLeftValueInAmbientListening(){
    avneraService.avneraModule.setLeftValueInAmbientListening(4)
}</td>
  </tr>
</table>


**Possible Errors Returned:**

* DEVICE_NOT_CONNECTED:  The device is currently not connected and the function cannot be executed

* NO_ROW_DATA_ERROR:  The package sent to the board is nil

* CHALLENGE_LENGHT_NOT_16_BYTES_AS_EXPECTED:  The package doesn’t have the correct size

* NO_ROW_DATA_ERROR:  The package sent to the board is nil

### Set Right Value In Ambient Listening

To set the right value in ambient listening, call the following method with a value between 0 and Max Ambient Listening value for right side (in the example method, 4 is the value being used):

<table>
  <tr>
    <td>func setRightValueInAmbientListening(){
    avneraService.avneraModule.setRightValueInAmbientListening(4) 
}</td>
  </tr>
</table>


**Possible Errors Returned:**

* DEVICE_NOT_CONNECTED:  The device is currently not connected and the function cannot be executed

* NO_ROW_DATA_ERROR:  The package sent to the board is nil

* CHALLENGE_LENGHT_NOT_16_BYTES_AS_EXPECTED:  The package doesn’t have the correct size

* NO_ROW_DATA_ERROR:  The package sent to the board is nil

### Request Graphic Equalizer Basic Information

To get the current graphic equalizer basic information, call the following method.  The results are returned asynchronously via a callback.

<table>
  <tr>
    <td>func requesGraphicEqualizerBasicInformation(){
    avneraService.avneraModule.requestGraphicEqualizerBasicInformation() 
}</td>
  </tr>
</table>


This method returns the information using the following callback function:

<table>
  <tr>
    <td>Func getInformationForEqualizerGraphicWithNumberOfBands(numberOfBands:
    Int, minimumValue: Int, maximunValue: Int) {
}</td>
  </tr>
</table>


The information returned includes the number of Bands, minimum and Maximum Value for the defined number of bands.

**Possible Errors Returned:**

* DEVICE_NOT_CONNECTED:  The device is currently not connected and the function cannot be executed

* NO_ROW_DATA_ERROR:  The package sent to the board is nil

* CHALLENGE_LENGHT_NOT_16_BYTES_AS_EXPECTED:  The package doesn’t have the correct size

* UP_TO_10_BANDS_ARE_UNSUPPORTED_ERROR: This problems happens when the board return more of 10 bands

* TARGET_HAS_NO_BANDS: The board doesn’t have bands

* WRONG_NUMBER_OF_BANDS: The device returns  an wrong number of bands 

### Request the Equalizer Bands Information

The following function retrieves the frequency setting for each band defined by the Equalizer:

<table>
  <tr>
    <td>func getEqualizerBandsInformation(equalizerBandsInformationArray:
    NSMutableArray!){
}</td>
  </tr>
</table>


The array passed to the function will contain an EqualizerBandModel object for the number of bands currently defined by the Equalizer upon return from the function call.  An EqualizerBandModel object is defined as follows:

<table>
  <tr>
    <td>@interface EqualizerBandModel : NSObject
@property (nonatomic, strong) NSString* unitOfMeasure;
@property (nonatomic) float frequencyValue;
@end
</td>
  </tr>
</table>


Where the unitOfMeasure is either Hertz (Hz) or Kilohertz (kHz) and the defined frequency value for that band.  Note that the array index starts a 0 and goes up to the number of defined bands minus 1.  So if there are ten bands defined for the equalizer, the array index would be from 0 to 9.

**Possible Errors Returned:**

* DEVICE_NOT_CONNECTED:  The device is currently not connected and the function cannot be executed

* NO_ROW_DATA_ERROR:  The package sent to the board is nil

* CHALLENGE_LENGHT_NOT_16_BYTES_AS_EXPECTED:  The package doesn’t have the correct size

* NO_ROW_DATA_ERROR:  The package sent to the board is nil

### Request Actual Values For Graphic Equalizer

To get the current settings for each band in the graphic equalizer, you need to call the following function.  Note that before calling this method, you must call getEqualizerBandsInformation to ensure the library has the appropriate information to process this request.  Failure to do so will result in the error TARGET_HAS_NO_BANDS being generated.

<table>
  <tr>
    <td>func requestActualValuesForGraphicInEqualizer(){
    avneraService.avneraModule.requestActualValuesForGraphicInEqualizer()
}</td>
  </tr>
</table>


After calling the above function, two callbacks are generated.  The first tells you the current preset for the equalizer with this callback:

<table>
  <tr>
    <td>func graphicNumberSelectedInEqualizer(option: Int){
}</td>
  </tr>
</table>


The option returned will be either 0, 1, 2 or 3 with the following being a default definition for each value:

* 0: Music

* 1: Movie

* 2: Voice

* 3: User

The second callback will return an array with the current settings for each band:

<table>
  <tr>
    <td>func getActualValuesFromDeviceForEqualizerSection(valuesArray: NSMutableArray!){
}</td>
  </tr>
</table>


The returned array will contain a value between and including the minimum and maximum setting for each band.  For example, if the minimum and maximum for a band are -10 and 10, then the value in the array for a band could be -10 to 10.  Note that the array starts at 0 and goes up to the number of bands minus 1.  The frequency setting for each band is returned by the function getEqualizerBandsInformation().

**Possible Errors Returned:**

* DEVICE_NOT_CONNECTED:  The device is currently not connected and the function cannot be executed

* NO_ROW_DATA_ERROR:  The package sent to the board is nil

* CHALLENGE_LENGHT_NOT_16_BYTES_AS_EXPECTED:  The package doesn’t have the correct size

* NO_ROW_DATA_ERROR:  The package sent to the board is nil

* TARGET_HAS_NO_BANDS:  Failed to call getEqualizerBandsInformation() before calling requestActualValuesForGraphicInEqualizer()

### Set values in the equalizer bands

These methods allows a user to change the values for each band in the equalizer.  

The following method will set the an equalizer band at the value specified.  Note that you can also indicate a preset value as part of the call.  The following information is required as part of the call:

* **currentOptionSelectedInEqualizer:**  The value  could be 0,1,2 or 3 which represent the preset number that the setting will be saved under

* **bandIndex: **This value represents the index of the band it starts at 0 and goes up to the number of predefined bands

* **Value:**  The value that is going to be set for the band identified in bandIndex.  Note that the value must be in the range of the minimum to maximum band values as returned by getInformationForEqualizerGraphicWithNumberOfBands().

<table>
  <tr>
    <td>func setValueInEqualizerGraphicWithOptionSelected(currentOptionSelectedInEqualizer currentOptionSelectedInEqualizer: UInt32, bandIndex: UInt32, value: Int32){
              avneraService.avneraModule.setValueInEqualizerGraphicWithOptionSelected
              (currentOptionSelectedInEqualizer, bandIndex:bandIndex, value:value)
    }
</td>
  </tr>
  <tr>
    <td></td>
  </tr>
</table>


**Possible Errors Returned:**

* DEVICE_NOT_CONNECTED:  The device is currently not connected and the function cannot be executed

* NO_ROW_DATA_ERROR:  The package sent to the board is nil

* CHALLENGE_LENGHT_NOT_16_BYTES_AS_EXPECTED:  The package doesn’t have the correct size

* NO_ROW_DATA_ERROR:  The package sent to the board is nil

### Update firmware

The library provides the ability to upgrade the firmware on the board

The first method that should called is the following.  This will tell you if the board is in bootloader mode or not.  In order to update the firmware, the board must be put into bootloader mode

<table>
  <tr>
    <td>func requestBootloaderStatus(){
        avneraService.avneraModule.requestFirmwareImageType()
}
</td>
  </tr>
</table>


This method returns the status of the request in the following callback:

<table>
  <tr>
    <td>func getBootloaderStatusIsActivated(status: Bool) {
 }
</td>
  </tr>
</table>


If the board is in bootloader mode, the callback will return true.  If the board is not in bootloader mode, false is returned.  If the board is in bootloader mode, skip the next step.

If the board is not in bootloader mode, the next step is call the the following function to get the board to go into bootloader mode:

<table>
  <tr>
    <td>func enterInBootloaderMode(){
    avneraService.avneraModule.EnterBootloaderMode()
}</td>
  </tr>
</table>


This method returns the following callback:

<table>
  <tr>
    <td>func jumpedToBootloaderSuccessful() {
    //The device is in bootloader state
}</td>
  </tr>
</table>


If you do not receive the callback, the board failed to go into bootloader mode.  You can retry the enterInBootloaderMode() call again.  Multiple failures would indicate an issue with the board and probably requires a reboot.

**After receiving the callback, it is important to wait at least 5 seconds before to calling the next method (startUpdateFirmwareProcess) because the headsets need some time to be ready in this state**

 

If the board has been successfully put into the bootloader state, call the following function with the firmware to be used in the update.  Note that the firmware file needs to be loaded into an NSData object before calling this function and subsequently passed as a parameter when this function is called:

<table>
  <tr>
    <td>func startUpdateFirmwareProcess(firmwareFile firmwareFile: NSData){
    avneraService.avneraModule.programAppFlash(firmwareFile)
}</td>
  </tr>
</table>


After calling the startUpdateFirmwareProcess(), you will receive callbacks from the following function indicating the progress of the firmware update.  Note that the value returned is a float between 0 and 1 where 0 is the start of the process and 1 is the end:

<table>
  <tr>
    <td>func getProgressFirmwareUpdate(progress: Float) {
    //Actual progress, this value is between 0-1
}</td>
  </tr>
</table>


When the firmware update is completed, you will receive the following callback indicating that the firmware update process completed successfully:

<table>
  <tr>
    <td>func updateFirmwareProgressCompleted() {
}</td>
  </tr>
</table>


After receiving this callback, the board needs to be rebooted to complete the firmware update process.

**Possible Errors Returned:**

* DEVICE_NOT_CONNECTED:  The device is currently not connected and the function cannot be executed

* NO_ROW_DATA_ERROR:  The package sent to the board is nil

* CHALLENGE_LENGHT_NOT_16_BYTES_AS_EXPECTED:  The package doesn’t have the correct size

* ERROR_FOUND_UPDATING_THE_FIRMWARE:  Problem found in one of the steps of the firmware upgrade process

# Delegate Methods

The device communicates with the app through delegates.  You must implement the following methods in your service:

### Avnera Error With Type:

This method is called when the library detects an error with the device:

<table>
  <tr>
    <td>func avneraErrorWithType(errorType: AvneraErrorType){
    switch errorType {
    case CONNECTION_PROCESS_FAILED_UNKNOW_REASON:
        break
    case CONNECTION_PROCESS_FAILED_DEVICE_IS_ALREADY_CONNECTED:
        break
    case CONNECTION_PROCESS_FAILED_DEVICE_IS_NOT_AVAILABLE:
        break
    case WRITING_PROCESS_IN_AVNERA_FAILED:
        break
    case UP_TO_10_BANDS_ARE_UNSUPPORTED_ERROR:
        break
    case CHALLENGE_LENGTH_NOT_16_BYTES_AS_EXPECTED:
        break
    case ERROR_FOUND_UPDATING_THE_FIRMWARE:
        Break
    case DEVICE_NOT_CONNECTED:
        break
    default:
        break
    }
}</td>
  </tr>
  <tr>
    <td></td>
  </tr>
</table>


### Connection Successful

This method is called when the board has been successfully connected to the device:

<table>
  <tr>
    <td>func avneraConnectedSuccessful(accessory: EAAccessory) {
    print("The device name is \(accessory.name)")
}</td>
  </tr>
</table>


### Disconnection Successful

This method is called when the board was disconnected from the device:

<table>
  <tr>
    <td>func avneraDisconnectedSuccessful(){
    //Device was disconnected
}</td>
  </tr>
</table>


[END OF DOCUMENT]

