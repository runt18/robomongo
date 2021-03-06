## Instructions to package Robomongo.app

Current as at 23 March, 2013

### Prerequisites
 - Qt Creator **4.8.x** : `http://qt-project.org/downloads` (source is not yet compatible with Qt 5.x)
 - Github checkout of: `https://github.com/paralect/robomongo`
 - (optional) Github checkout of: `https://github.com/paralect/robomongo-deps`
 - Qt tools must be included in your path (installed by default in: `/Developer/Tools/Qt`)

#### Build the binary

 - change to your `robomongo/build` directory

 - build appropriate **target** (debug|release): `./build.sh debug`
 
NOTE: the Github repo currently includes prebuilt libraries for debug.  If you want to build dependencies for other targets you will need to build matching libaries using the  `robomongo-deps` repository.

#### Bundle linked frameworks/libs into Robomongo.app

 - change to the output directory for the **target** binary that has been built
 
 - eg for `debug` target: `cd ../target/debug/app/out`
 
 - if you're in the right directory, there should be a `robomongo.app` file there
 
 - you can test this works with `./run.sh`
 
 - (optional) rename `robomongo.app` to `Robomongo.app`
 
 - run `macdeployqt Robomongo.app` to copy the required libaries into the .app bundle (if any of these fail due to incorrect paths, you may have to copy them in manually)
 
 - test the app deploy was successful by launching `robomongo.app` without the run.sh script
 
 - you can also check the required libaries using `otool`:
    
	   $ otool -L Robomongo.app/Contents/MacOS/robomongo
	   Robomongo.app/Contents/MacOS/robomongo:
	   @executable_path/../Frameworks/libqjson.0.dylib (compatibility version 0.0.0, current version 0.7.1)
	   @executable_path/../Frameworks/libqscintilla2.8.dylib (compatibility version 8.0.0, current version 8.0.2)
	   @executable_path/../Frameworks/QtGui.framework/Versions/4/QtGui (compatibility version 4.8.0, current version 4.8.4)
	   @executable_path/../Frameworks/QtCore.framework/Versions/4/QtCore (compatibility version 4.8.0, current version 4.8.4)
	   /usr/lib/libstdc++.6.dylib (compatibility version 7.0.0, current version 56.0.0)
	   /usr/lib/libgcc_s.1.dylib (compatibility version 1.0.0, current version 1669.0.0)
	   /usr/lib/libSystem.B.dylib (compatibility version 1.0.0, current version 169.3.0)
	   
#### Copy the app icon and set the version info

 - copy the database.icns resource from `robomongo/install/osx/Contents/Resources` into `.../Robomongo.app/Content/Resources/`
 - edit the details in the `Info.plist` file generated by `macdeployqt`:
 	- Set **CFBundleIconFile** to the icon file to use: `database.icns`
 	- Set **CFBundleGetInfoString** to something appropriate, eg: `Robomongo 0.6.5 beta`
 
 - example Info.plist
 		<?xml version="1.0" encoding="UTF-8"?>
		<!DOCTYPE plist SYSTEM "file://localhost/System/Library/DTDs/PropertyList.dtd">
		<plist version="0.9">
		<dict>
				<key>NSPrincipalClass</key>
				<string>NSApplication</string>
				<key>CFBundleIconFile</key>
				<string>database.icns</string>
				<key>CFBundlePackageType</key>
				<string>APPL</string>
				<key>CFBundleGetInfoString</key>
				<string>Robomongo 0.6.5 beta</string>
				<key>CFBundleSignature</key>
				<string>????</string>
				<key>CFBundleExecutable</key>
				<string>robomongo</string>
				<key>CFBundleIdentifier</key>
				<string>org.robomongo</string>
		</dict>
		</plist>

#### Testing
 - Zip Robomongo.app and give it a consistent name including the version, eg: `Robomongo-0.6.5-beta.zip`
 - Copy the zip file to another OS X machine (ideally one without Qt installed) and confirm the app runs correctly (i.e. all frameworks/libaries have been properly bundled)
 
#### Publish!
 - OK, we're done :)