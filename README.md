# Initial Setup

### How to import the framework into the project

First open the project that you want to add AvneraSDHM framework

Then select the file .xcodeproj and go to the target that you want

Search the Embedded Binaries section

Press the + icon

Press add Other… button

Search the framework, after that please press open.

Add the following line in all classes that you need to use the framework: 
```objective-c
*#import <AvneraSDHM/AvneraSDHM.h>*
```
### Plist setup

Add the following key in the info.plist file in your project:
```Objective-c
*Key: Supported external accessory protocols*

*Type: Array*
```
