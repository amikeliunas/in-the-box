

# 1 - Prerequisites #
  * To build and run In-The-Box, you need to use a mac with Xcode installed (In-The-Box has only been tested with Xcode 4).
  * To be able to convert class files to dex files (the input format of the Dalvik VM), you need to have the Android SDK with the "platform tools" installed.



# 2 - Checking out the project #
To check out In-The-Box in Xcode 4, go to Window > Organizer. Go to the Repositories tab, click on the "+" at the bottom-left corner and select "Add Repository".

![http://in-the-box.googlecode.com/svn/wiki/addrepo.png](http://in-the-box.googlecode.com/svn/wiki/addrepo.png)

Under "Location", enter "https://in-the-box.googlecode.com/svn/", or replace "https" by "http" for a read-only checkout. "Subversion" should be automatically selected as type. Enter a name to identify the repository and click "Next".

![http://in-the-box.googlecode.com/svn/wiki/repoaddr.png](http://in-the-box.googlecode.com/svn/wiki/repoaddr.png)

As Trunk, Branches and Tags, enter the following and click "Add" :

![http://in-the-box.googlecode.com/svn/wiki/repodirs.png](http://in-the-box.googlecode.com/svn/wiki/repodirs.png)

If you want an access that allows you to commit (with "https"), select your newly added repository, enter your username and your [generated googlecode.com password](http://code.google.com/hosting/settings).

![http://in-the-box.googlecode.com/svn/wiki/repousername.png](http://in-the-box.googlecode.com/svn/wiki/repousername.png)

FInally, select "Trunk" and click "Checkout". Choose where you want to put the checked out project.



# 3 - Building and running In-The-Box #
Open the checked out project in the Finder and double-click to open "InTheBox.xcodeproj".
Select an "InTheBoxSim" target and click run. If you want to run it on a device, make sure you have valid provisioning profiles installed (for more info, check the [iOS provisioning portal](https://developer.apple.com/ios/my/overview/index.action)).

![http://in-the-box.googlecode.com/svn/wiki/runtarget.png](http://in-the-box.googlecode.com/svn/wiki/runtarget.png)



# 4 - Running your own java code #
If you want to run your own java code, you need to write one or more java classes, and one must have a main method. These classes must be compiled as ".class" files using javac. Then to feed it to Dalvik, you must convert it to Dex files. To do this, use "dx", inside the "Platform Tools" of the Android SDK.
Usage example :
` android-sdk-mac_x86/platform-tools/dx --dex --output=hello.jar Hello.class `
(This will put the dex files inside a jar archive).

The resulting jar must be added to the Xcode project. To do so, right-click on a folder inside your project and select "Add Files to 'In-the-box'" (for example you can add it inside "InTheBoxSim/test"). We also want it to be copied inside the Application Bundle. To do so, click on the root element of your project, select the target "InTheBoxSim" and go to the tab named "Build Phases". Expand the "Copy Bundle Resources" section and add your jar (either by drag & dropping or by clicking "+").

![http://in-the-box.googlecode.com/svn/wiki/bundleres.png](http://in-the-box.googlecode.com/svn/wiki/bundleres.png)

Next you need to modify In-The-Box's main file so that it uses your dex files. "main.m" can be found in "Supporting Files". In this file you have to modify the classpath of the jar containing your dex files and the name of the class containing your main method. This can be done easily by modifying the constants ` "MAIN_JAVA_CLASS_NAME" ` and ` "JAR_NAME". ` The jar will be searched inside the Application Bundle, where it should be copied if you followed the previous step.

![http://in-the-box.googlecode.com/svn/wiki/main.m.png](http://in-the-box.googlecode.com/svn/wiki/main.m.png)