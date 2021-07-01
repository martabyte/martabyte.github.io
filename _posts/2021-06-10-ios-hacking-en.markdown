---
layout: post
title:  "iOS Hacking - A Beginner's Guide to Hacking iOS Apps [2021 Edition]"
date:   2021-06-10 17:15:00 +0200
categories: ios hacking
---

*h3ll0 fr13nds!* My first post will be about iOS Hacking, a topic I'm currently learning, so this will be a kind of gathering of all information I have found in my research. It must be noted that I won't be using any MacOS tools, since the computer used for this task will be a **Linux** host, specifically a Debian-based distribution: Kali Linux. I will also be using '**checkra1n**' for the device jailbreaking, but there are other alternatives such as 'unc0ver' or 'Taurine', so just choose the one that fits best to your device or your liking. For background, my device is an iPhone 6s updated to the **latest iOS version** as of April 2021 (14.5).

This post is purely educational and intended for Ethical Hacking purposes only. If you find any mistakes or think I should improve something in this post, please let me know! :)

This post will be divided into different sections:

- [iOS Device Jailbreaking](#ios-device-jailbreaking)
  - [Checkra1n](#checkra1n)
- [Initial Recommended Hacking Setup (Using Linux as the Host Computer's OS)](#initial-recommended-hacking-setup-using-linux-as-the-host-computers-os)
  - [Cydia Impactor](#cydia-impactor)
  - [OpenSSH](#openssh)
  - [Filza](#filza)
  - [AppSync Unified](#appsync-unified)
    - [**Practical Example: Downloading an Unsigned App into the Device and Installing It**](#practical-example-downloading-an-unsigned-app-into-the-device-and-installing-it)
  - [Cycript](#cycript)
  - [Frida](#frida)
  - [SSL Kill Switch 2](#ssl-kill-switch-2)
  - [Liberty Lite](#liberty-lite)
  - [Darwin CC Tools](#darwin-cc-tools)
  - [BigBoss Recommended Tools](#bigboss-recommended-tools)
- [Static Analysis 101](#static-analysis-101)
  - [Initial App Binary Recon](#initial-app-binary-recon)
    - [**iOS App Binary**](#ios-app-binary)
  - [App Decryption](#app-decryption)
    - [**Sensitive Information in the App Filesystem**](#sensitive-information-in-the-app-filesystem)
    - [**Plist Files**](#plist-files)
    - [**Filesystem Checksums**](#filesystem-checksums)
    - [**Further Analysis**](#further-analysis)
  - [Reverse Engineering](#reverse-engineering)
- [Dynamic Analysis 101](#dynamic-analysis-101)
  - [Objective-C Quick Start](#objective-c-quick-start)
    - [**The main concepts of Obj-C**](#the-main-concepts-of-obj-c)
  - [Cycript](#cycript-1)
    - [**Class Enumeration**](#class-enumeration)
    - [**Exploring the Application**](#exploring-the-application)
    - [**Method Tampering Example**](#method-tampering-example)
  - [Frida / Objection](#frida--objection)
    - [**Exploring the Application**](#exploring-the-application-1)
    - [**Tampering with the Application**](#tampering-with-the-application)
    - [**Disable Certificate Pinning**](#disable-certificate-pinning)
    - [**Bypass Jailbreak Detection**](#bypass-jailbreak-detection)
    - [**Binary Information**](#binary-information)
    - [**Keychain Dump**](#keychain-dump)
    - [**Cookies**](#cookies)
  - [Further App Filesystem Analysis](#further-app-filesystem-analysis)
    - [**Screenshots**](#screenshots)
    - [**Pasteboard**](#pasteboard)
    - [**Snapshots**](#snapshots)
    - [**Keyboard Cache**](#keyboard-cache)
  - [Analyzing the App Communications](#analyzing-the-app-communications)
    - [**Certificate Pinning**](#certificate-pinning)
    - [**Traffic Analysis**](#traffic-analysis)

<br>

- - - -

<br>

## iOS Device Jailbreaking ##
All mobile devices have restrictions set by the manufacturer that prevent end users to install certain components or applications, accessing low-level resources, tweaking configurations... Jailbreaking is a way to escalate privileges to remove these restrictions and be able to perform any desired operation on an iOS device. Its Android equivalent is called 'Rooting'.

**Warning!** Apple warns users that Jailbreaking their devices causes them to void the warranty of the product, so continue at your own risk and don't perform this on your main Apple device.

To perform the Jailbreak of the device from a Linux host, I found out that a set of libraries were needed for the device to be detected correctly. Keep in mind that depending on your Linux installation these may not be needed, but these are the ones that worked for me:

```
sudo apt install ifuse usbmuxd libimobiledevice libimobiledevice-utils libplist-utils ideviceinstaller python3-imobiledevice python3-plist python3-libplist (and/or libplist3)
```


### Checkra1n ###
One of the programs that allow the Jailbreaking of iOS devices is 'checkra1n'. This method of jailbreaking requires a host computer and a iOS device connected to it. In this case, the host computer will be a Linux host, being the recommended distribution a Debian-based one. The steps to download it are listed in the website, giving options of both using the repo or downloading the app binary. I followed the repo method:

```
echo 'deb https://assets.checkra.in/debian /' | sudo tee /etc/apt/sources.list.d/checkra1n.list
sudo apt-key adv --fetch-keys https://assets.checkra.in/debian/archive.key
sudo apt-get update
sudo apt-get install checkra1n
```

Once downloaded, make sure to launch it with **admin privileges** otherwise, it won't detect your device once it's in recovery mode [Don't be like me and spend a whole day wondering what was wrong just to find out I am simply stupid]. To launch the gui version:

```
sudo checkra1n --gui &
```

If your device's version is unsupported/untested, for instance, it's uploaded to the latest iOS version, the jailbreaking can still be performed, as this method exploits a vulnerability in the BootROM ('checkm8'), therefore it's unpatchable through software updates and so the jailbreaking will most likely also work. In order for checkra1n to jailbreak an unsupported/untested version just go to 'Options' and select 'Allow untested iOS/iPAD/tvOS versions'. If your device is a A11 device on OS 14.0 and above, it is required to remove the passcode and to enable “Skip A11 BPR check” in the options. Keep in mind, A7 devices do not work on the Linux version for now. More and updated information about specific devices and iOS version compatibility can be found in [Checkra1n's Official Website](https://checkra.in).

Now the jailbreaking can be done just by following the steps checkra1n provides. Keep in mind that checkra1n performs a semi-tethered jailbreak, which means that if the device is rebooted, the device will be completely usable for normal iOS operations and apps, but the jailbreak process needs to be performed again from the computer to access its root functions. It will keep all its previous jailbroken apps and configurations once jailbroken again.


- - - -

<br>

## Initial Recommended Hacking Setup (Using Linux as the Host Computer's OS) ##
Once the device is correctly jailbroken, the 'checkra1n' app should appear on the device. If your device is not already connected to wifi, connect it and download 'Cydia' from the 'checkra1n' app.

### Cydia Impactor ###
Cydia is an AppStore for jailbroken devices which contains advanced applications for the device. From Cydia, a great amount of interesting apps can be downloaded. The ones I'm going to talk about are the ones I have needed so far, and keep in mind there may be alternatives in case one of these is not working for you.

### OpenSSH ###
OpenSSH will enable SSH for the jailbroken device. The default password is 'alpine' in all iOS devices, so the recommended step is to change the SSH password right away. An additional step I recommend for further security is to disable Password Authentication, allowing SSH Key Authentication only.

This reddit post by u/4z0k covers how to secure iOS devices' SSH connections (although I still have to try it out so look for other additional resources as well): [Secure and customize your SSH installation and port! - Reddit](https://www.reddit.com/r/jailbreak/comments/f7logb/tutorial_secure_and_customize_your_ssh/)


### Filza ###
Filza will allow you to navigate through all files in the device. This will allow us, among other things, to navigate to downloaded IPAs and install them with the next app on the list.


### AppSync Unified ###
AppSync Unified is needed to correctly install downloaded ad-hoc signed, fakesigned or unsigned IPAs. To download it through Cydia, follow the following steps:

1. Go to the 'Sources' tab > 'Edit' > 'Add'
2. Add the latest AppSync Unified repo: 'http://cydia.akemi.ai' (as of April 2021)
3. Once the repo is added it will appear as 'Karen Repo' and the app should appear in the 'Search' tab


#### **Practical Example: Downloading an Unsigned App into the Device and Installing It** ####
1. Copy the file into the device with 'scp' to the desired folder, for instance: `scp <file> root@<iDevice-IP>:/User/Downloads` 
2. Open Filza and navigate to the file
3. Click on the IPA file and select 'Install'

Great! Now you have your unsigned app correctly installed onto the device :)


### Cycript ###
To be able to perform dynamic analysis by hooking into apps and being able to inject JavaScript code. It is accessed through SSH. In depth information can be found in the [Dynamic Analysis](#dynamic-analysis-101) section.


### Frida ###
Frida is another app that allows you to perform dynamic analysis by hooking into apps and being able to inject JavaScript code. It can be accessed both by connecting the device to the host computer or by SSH. This is my current personal favorite due to its extended capabilities when using its extension: **'Objection'**. In depth information can be found in the [Dynamic Analysis](#dynamic-analysis-101) section.

To download it through Cydia, follow the steps below:

1. Go to the 'Sources' tab > 'Edit' > 'Add'
2. Add the latest Frida repo: 'http://build.frida.re' (as of April 2021)
3. Once the repo is added, the app should appear in the 'Search' tab


### SSL Kill Switch 2 ###
A frequent protection against the hijacking of app communications is Certificate Pinning. There are many ways to bypass this control, a couple of which we will see in the [Dynamic Analysis](#dynamic-analysis-101) section, but the easiest way to do it is by downloading and activating this app through Cydia.  


### Liberty Lite ###
Another common protection against app hacking is through the implementation of jailbreak controls. Depending on the type and quality of the controls used there are many different ways to try to bypass them, one of the easiest ones being downloading and activating this app through Cydia. Other ways to bypass it can also be found in the [Dynamic Analysis](#dynamic-analysis-101) section.


### Darwin CC Tools ###
This package will install a series of useful command line tools, such as 'otool' or 'nm'.


### BigBoss Recommended Tools ###
This package will install a series of useful command line tools, such as 'top', 'whois' or 'cURL'. If there is a conflict with the installation due to its dependency to 'gdb', just add 'http://cydia.radare.org' to the repo sources and install 'gdb' from there.


- - - -

<br>

## Static Analysis 101 ##
Static analysis of apps mainly focuses on the evaluation of the executable file itself and its configurations before executing it to try to find misconfigurations or possible vulnerable functions. In iOS, apps are compiled to execute natively on the device, therefore, decompilation/reversing in high-level Objective-C it's not possible. But it is possible to reverse it to machine-level assembly code, although it will require further assembly and reversing skills to start to understand the code.

<br>
### Initial App Binary Recon ###

#### **iOS App Binary** ####
An iOS app binary should have some further security measures. The app binary is found inside the app directory, which is usually found in '/private/var/containers/Bundle/Application/'.

* **PIE (Position Independent Executable)**: When enabled, the application loads into a random memory address everytime it launches, making it harder to predict its initial memory address.
  ```
  otool -hv <app-binary> | grep PIE   # It should include the PIE flag
  ```

* **Stack Canaries**: To validate the integrity of the stack, a 'canary' value is placed on the stack before calling a function and is validated again once the function ends.
  ```
  otool -I -v <app-binary> | grep stack_chk   # It should include the symbols: stack_chk_guard and stack_chk_fail
  ```

* **ARC (Automatic Reference Counting)**: To prevent common memory corruption flaws
  ```
  otool -I -v <app-binary> | grep objc_release   # It should include the _objc_release symbol 
  ```

* **Encrypted Binary**: The binary should be encrypted
  ```
  otool -arch all -Vl <app-binary> | grep -A5 LC_ENCRYPT   # The cryptid should be 1
  ```


### App Decryption ###
To perform reverse engineering over the app, we'll need access to an unencrypted copy of the binary. If the binary is encrypted, there are a few ways to decrypt it, but I personally only have used '[frida-ios-dump](https://github.com/AloneMonkey/frida-ios-dump)'. The steps to perform this, and another 2 ways for decrypting the binary, can be found here: [Decrypt iOS Applications: 3 Methods](https://fadeevab.com/decrypt-ios-applications-3-methods/). If Frida is the method of choice, make sure Frida is installed on the device.

Once the decrypted ipa is obtained, change its extension to 'zip' and unzip it to analyze its internal files.


#### **Sensitive Information in the App Filesystem** ####
A good way to identify hardcoded sensitive information is to look for usually sensitive values in the files of the app filesystem, such as IPs, email addresses... This can easily be done with 'strings', and/or 'grep' and a little bit of Regex.

```
strings <filename>

grep -Eo '<regex-expression>' -r *
```

Example Regex Expressions:
```
IPv4: [0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3} or ^\d{1,3}[.]\d{1,3}[.]\d{1,3}[.]\d{1,3}$

Email Addresses: [a-zA-Z0-9_.+-]+@[a-zA-Z0-9-]+\.[a-zA-Z0-9-.]+$ or ^[\w\.=-]+@[\w\.-]+\.[\w]{2,3}$

Visa Card Numbers: \b([4]\d{3}[\s]\d{4}[\s]\d{4}[\s]\d{4}|[4]\d{3}[-]\d{4}[-]\d{4}[-]\d{4}|[4]\d{3}[.]\d{4}[.]\d{4}[.]\d{4}|[4]\d{3}\d{4}\d{4}\d{4})\b

IBAN: [a-zA-Z]{2}[0-9]{2}[a-zA-Z0-9]{4}[0-9]{7}([a-zA-Z0-9]?){0,16}

Base64: (?:[A-Za-z0-9+\/]{4})*(?:[A-Za-z0-9+\/]{2}==|[A-Za-z0-9+\/]{3}=)?$

URLs: (?i)\b((?:[a-z][\w-]+:(?:\/{1,3}|[a-z0-9%])|www\d{0,3}[.]|[a-z0-9.\-]+[.][a-z]{2,4}\/)(?:[^\s()]+|\(([^\s()]+|(\([^\s()]+\)))*\))+(?:\(([^\s()]+|(\([^\s()]+\)))*\)|[^\s`!()\[\]{};:'".,?«»“”‘’]))
```

Also, files with potential sensitive information can be found in the filesystem, specially those with extensions such as: plist, sql, xml, localstorage, db... These can be easily found with a simple 'find' command:

```
find . -iname "*<extension>"
```

#### **Plist Files** ####
iOS stores app configuration data in 'plist' files. These files can either be in binary or XML format, but we'll need it to be in XML format for us to be able to easily read it, therefore if the 'plist' file is in binary format, duplicate the file and run `plutil -convert xml1 <duplicated-file>.plist`. Now, misconfigurations can be analyzed in the 'plist' file:

* **Identify URL Handlers**: If the application uses custom URLs, the server may not correctly parse their content and therefore, leading to further attacks.
* **Identify API Keys or Hardcoded Credentials**
* **Identify Internal Paths**: The developers may have left some internal paths that could provide information about their computers
* **Identify Misconfigured App Transport Security Settings**: If the application allows loading arbitrary URLs in the application (App Transport Security Settings set to YES), it could potentially cause malicious behaviour in the application.


#### **Filesystem Checksums** ####
An iOS app should validate the changes on the contents of the filesystem. Try changing some values in files you may find, for instance, a 'sqlite' db file:
```
sqlite3 <file>.sqlite
> (perform the needed SQL recon)
> update <table> set <field>='<value>' WHERE <condition>
```

Save the file and relaunch the app, if the newly set value is correctly displayed, the app does not validate the filesystem's checksum.


#### **Further Analysis** ####
* **Examine Logging (ASL) Messages**: The contents of Apple System Log (ASL) can be retrieved by any application without any further permissions, therefore, there can't be any sensitive information about the application or its users on the logs.

* **Password Storing**: The following is a quick guide from worst to best ways to handle application passwords.
  * Saving the password as plaintext in 'NSUserDefaults' object or any other file storage
  * Storing plaintext passwords in Keychain
  * Storing encrypted passwords in Keychain
  * Using passwords for initial authentication and then issuing device-specific keys instead of saving the password
  * Using short-lived authentication tokens that limit accessibility so there's no need for storing passwords

  <br/>

  The iOS Keychain provides a secure way to store sensitive data, which is protected using the following class structure:
  * **kSecAttrAccessibleWhenUnlocked**: The file is stored encrypted and cannot be read while the device is booting or locked.
  * **kSecAttrAccessibleAfterFirstUnlock**: The file remains accessible until the next restart. Recommended for items accessed by background apps.
  * **kSecAttrAccessibleAlways**: The data can be read anytime, even if the device is locked. It is not recommended under any circumstance.
  * **kSecAttrAccesibleWhenPasscodeSetThisDeviceOnly**: The data can be accessed only when the device is unlocked.

<br>
### Reverse Engineering ###
Reverse Engineering is a whole world by itself. I don't have much experience with it so I'll just go over the few things I know, therefore I recommend doing more research on your own. Reverse Engineering iOS apps is a complex and time-consuming task due to the way apps are compiled, which makes them a bit more secure against attacks like app cloning.

First of all, it's recommended to find out if the app is written in Swift or in Objective-C. Apps written in Swift will have both of these binaries:
```
nm <app> | grep '_OBJC_CLASS_$__' 
otool -L <app> | grep libswift   # Don't think that because it says OBJC it's an Objective-C binary, it's not
```

If these queries do not provide any results, the app is written in Objective-C.

The objective of reverse engineering the code is to understand the behaviour of the app without executing it. The tool I mainly use is **Ghidra**, since it's a free reverse engineering tool available for Linux and relatively easy to understand if you're new to reverse engineering. To import the file I select the 'Batch' option and the ipa file. From this point on, I'll try to find either hardcoded credentials, or try to understand how the functions work, or try to find, for instance, the jailbreak detection function to try to understand how to avoid detection...


- - - -

<br>

## Dynamic Analysis 101 ##
Static analysis is important, but it is a bit more complex and in my opinion requires a quite some more coding and reversing experience than the average beginner penetration tester. In an static analysis, the application may be obfuscated or hard to follow, and it may require some server interaction or loading of dynamic code, so this is when dynamic analysis comes in handy. Dynamic analysis consists of monitoring the application's behaviour when it is being executed (real-time). This includes inspecting local storage and the interaction with the platform, and even modifying the behaviour of the application at runtime, to, for instance, force the app to use HTTP instead of HTTPS or disable jailbreak detection.

As the application cannot be decompiled, manipulating the source code and recompiling it is not an option, but it is possible to manipulate the app by accessing the reflective runtime properties of Obj-C. This will allow us to access internal variables and objects at runtime, to be able to dynamically change the app's behaviour.

<br>
### Objective-C Quick Start ###
Objective-C used to be the primary language used for iOS development, although now Swift is becoming more popular due to its simplicity and improvements compared to Obj-C. I personally haven't dug into the world of Swift yet, so in this post I will focus on Obj-C, but I'll probably update it once I learn more about the dynamic analysis of Swift applications.  

#### **The main concepts of Obj-C** ####
* **Object**: Main element which can combine groups of data (variables) and functionalities in the form of methods that take action on the data.
* **Class**: Central feature which is an instantiated object that holds variables and methods.
* **Method**: Section of code that is called from the object class (aka function).
* **Message**: Used to instruct a method to perform an action.
* **Singleton**: A singleton class is a special class where only one instance of the class exists. Normally used to share data between multiple classes in the running process.
* **Delegate**: Object interface used by two independent objects for communication and interaction.

For further information, checkout this Objective-C quick guide from Tutorials Point: [Objective-C Tutorial - Quick Guide](https://www.tutorialspoint.com/objective_c/objective_c_quick_guide.htm)

<br>
### Cycript ###
Cycript is a runtime manipulation tool that uses JavaScript syntax to access Obj-C object data in an iOS app. It uses a Cydia library, Cydia Substrate, which allows to write a program that attaches to a running program and is able to manipulate its behaviour in memory, therefore, those changes will be lost once the app is terminated. If the application is written in Swift, Cycript in general can only modify the methods marked with the '@objc' header.

Cycript will be launched from the iOS device using an SSH session. To launch Cycript the name or PID of the app needs to be identified: `ps aux | grep <App-Name>`. Then, launch Cycript attaching to that process: `cycript -p <PID or App Name>`.

#### **Class Enumeration** ####
Cycript has a couple of functions that enumerate all classes. These methods provide a lot of output, so it's recommended to dump the output to a file and parse it from there.

```
class-dump

ObjectiveC.classes
[ObjectiveC.classes allKeys]

#To write it into a file
var classes = [[ObjectiveC.classes allKeys] componentsJoinedByString:@"\n"]
[classes writeToFile:"<output-path>/classesoutput.txt" atomically:NO encoding:4 error:NULL]
```

#### **Exploring the Application** ####
* `UIApp.keyWindow`: Current open window interface
* `UIApp.keyWindow.rootViewController`: Provides access to the current window
* `printMethods(<object>)`: To explore the object

#### **Method Tampering Example** ####
```
<object>.prototype.<function> = function() { return <true/false/...>; } 
```

<br>
### Frida / Objection ###
Frida is a dynamic code instrumentation and reverse engineering toolkit that allows to inject snippets of code or even full libraries into apps.

In order to install Frida and Objection in the host computer: `pip3 install frida-tools` and/or `pip3 install objection`. Frida will also need to be installed in the iOS device from Cydia as mentioned in the [Initial Recommended Setup](#initial-recommended-hacking-setup-using-linux-as-the-host-computers-os) section. By default both Frida and Objection will connect to the iOS device via USB, but it can be configured to connect over a network connection.

The Frida Server will run from the iOS device and will inject the code into the application.

```
frida-ls-devices   # To list all available connections to frida-servers
frida-ps -U   # To list of all running processes on the target device

frida -U <process-name>   # To attach to a process
frida -U -l <script>.js -n <process-name> --no-pause   # To inject a script into a process and attach to it

frida-ps -Uai   # To list of all installed applications on the target device

frida-discover -U <process-name>   # To discover internal functions
frida-trace -m "<function>" -U -f <process-name>   # To trace function calls

frida-kill -U <PID>   # To kill a process
```

Objection is a toolkit built on top of Frida, extending Frida's capabilities and facilitating some actions such as bypassing SSL Certificate Pinning or Jailbreak Detection.

```
objection -g <app-name> explore   # To attach to an app process - Objection restarts the app and injects into its process

[usb] # env   # To locate all directories related to the app
[usb] # pwd print / ls / !cat <file>   # To print the current directory path, to list the current directory and to execute the cat command into the iOS device's system
[usb] # ios plist cat <info-plist>.plist   # To print the contents of the Info.plist file
```

Note: If there's an error where Objection or Frida can't access the Frida Server, open an SSH session to the device and run: ` frida-server & `, to open Frida Server as a background task.

#### **Exploring the Application** ####
```
[usb] # ios hooking list classes   # To list all classes in the app
[usb] # ios hooking search classes <keyword>   # To search all classes containing a certain keyword
[usb] # ios hooking list class_methods <class>   # To list all methods of a class
[usb] # ios hooking watch class/method <class/method> (--dump-args --dump-return --dump-backtrace)   # To get a notification whenever the method is triggered by the application

[usb] # memory dump all <output-file>   # To dump all app memory into a file

[usb] # import <frida-script>.js   # It will import a Frida script
```

#### **Tampering with the Application** ####
```
[usb] # ios hooking set return_value <method> <true/false>   # To set the return value of a method to a certain Boolean value

# If an 'invalid query' error appears, try to input the method following the recommended format, for instance: "*[<class> <method>]"
```


#### **Disable Certificate Pinning** ####
Another way to disable Certificate Pinning on the device is using Objection:

```
[usb] # ios sslpinning disable --quiet   # The quiet option is because this hook can generate a lot of noise
```


#### **Bypass Jailbreak Detection** ####
```
[usb] # ios jailbreak disable   # To bypass jailbreak detection
[usb] # ios jailbreak simulate   # To simulate a jailbroken environment
```


#### **Binary Information** ####
The app binary information can be also inspected from Objection and checked against the values mentioned in the [iOS App Binary](#ios-app-binary) section:

```
[usb] # ios info binary   # It will show the app binary info
```

#### **Keychain Dump** ####
The contents of the device's Keychain can be analyzed from Objection to look for sensitive information.

```
[usb] # ios keychain dump   # It will dump the contents of the keychain
```

#### **Cookies** ####
The app cookies should also be checked to make sure it has the correct flags set ('Secure', 'HTTPOnly', 'Path' pointing to the correct path...):

```
[usb] # ios cookies get   # It will dump the application cookies
```

<br>
### Further App Filesystem Analysis ###

#### **Screenshots** ####
Check if the app allows screenshots. It might be a common feature, but it can lead to sensitive data leakage.


#### **Pasteboard** ####
If the app is able to save values to the shared clipboard, it is vulnerable to 'Side Channel Data Leakage', as the information in the clipboard can be accessed through other apps. It can be checked through different ways: through Objection, manually looking into the contents of the Pasteboard in the system... The app should implement its own private clipboard.

From Objection:
```
[usb] # ios pasteboard monitor   # It will notify you whenever a new value is set to the pasteboard 
```

Manually:
* Look at the content of the shared clipboard '/private/var/mobile/Library/Caches/com.apple.Pasteboard/' (or 'com.apple.UIKit.pboard') and see if it changes with values copied from the app 


#### **Snapshots** ####
Application Snapshots are 'screenshots' the app performs when it is sent to the background, and they are used, for instance, when looking through the open apps in the device. As with screenshots, these may contain sensitive information that can lead to sensitive data leakage. These snapshots can be found in: '< app-directory >/Library/SplashBoard/Snapshots/'.


#### **Keyboard Cache** ####
iOS keeps whatever users type to provide features such as auto-correct or text completion, so this records potential sensitive information as plaintext in the system. Sensitive fields must be marked as secure so they're not cached (UITextAutocorrectionType). The cached text can be found at: '/var/mobile/Library/Keyboard/dynamic-text.dat'.

<br>
### Analyzing the App Communications ###
In order to analyze the communications between the app and its server, a proxy can be configured to observe and tamper with these communications. The recommended proxy is Burp Suite, which is an integrated platform for performing security testing of web applications by PortSwigger.

To configure Burp as the proxy for the device, follow the following steps:
1. Open 'Burp Suite' on your computer > Inside the 'Proxy' tab > Go to the 'Options' tab
2. In 'Proxy Listeners' > Edit the current active proxy listener and select 'All Interfaces' in 'Bind to Address' (here you can also select the desired proxy port, I will leave the default one: 8080)
4. In your iOS device, go to 'Settings' > 'Wi-Fi' > Select the current connection
5. In the 'HTTP Proxy' section > 'Configure Proxy' > 'Manual' > Enter your computer's IP, Burp's Proxy port (in this example, 8080) and leave the Authentication option as off
6. In order for the device to trust Burp to navigate through HTTPS websites, download Burp's CA certificate from: 'http://burp' (or 'http://< computer-IP >:8080') and install it: Go to 'Settings' > 'General' > 'Profile' > Select 'PortSwigger CA' and follow the instructions
7. Now you should see the device's communications through Burp Suite


#### **Certificate Pinning** ####
Certificate Pinning should be used by mobile applications to validate a specific server certificate and do not accept any other certificates when trying to access the app server. This is achieved by generating self-signed or privately signed certificates and validating certain parameters when the certificate is issued to the server. Certificate Pinning prevents the addition of a new CA trust on the device to intercept TLS transactions, such as Burp's, but it can be bypassed on a jailbroken device.

One of the problems with Certificate Pinning is that when the certificate expires, it requires some coordination between the app and the server for updating it, where usually there's an overlapping period of time when more than one certificate is valid. Another problem is the local storage of the CA file: the worst way to store it is in the app's sandbox 'Documents' directory, a better way would be to add it as an app resource next to the executable, and a even better way would be to integrate it as an 'NSString' object inside the app, which is the hardest way an attacker could manipulate it.

If the app to analyze does not have Certificate Pinning, its communications will be seen through the proxy automatically. If after following the previous proxy configuration steps, the communication between the app and its server cannot be intercepted, it's probable the app implements Certificate Pinning to its communications.

One of the ways it can be bypassed is by activating the previously installed 'SSL Kill Switch 2' app through the device's 'Settings' app. It manipulates the Certificate handling routines at low-level, overriding any app delegate method, including the Certificate Pinning routines.


#### **Traffic Analysis** ####
From this point onward the analysis of the communications is the same as in a normal web applications, keeping in mind limitations that may be present in a mobile phone environment.

<br>

I hope this guide helped you in your introduction to iOS application ethical hacking, and again, any feedback is welcome if you find any mistakes or sections that could be improved.

***@martabyte***