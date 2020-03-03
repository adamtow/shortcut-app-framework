# Shortcut App Framework
By Adam Tow • [tow.com](tow.com) • [@atow](https://twitter.com/atow/)

[App Framework](https://tow.com/shortcuts/framework/) is a starting point for shortcut developers to create complex applications in Shortcuts. Study and learn from this documentation and the heavily-commented shortcut; you’ll be writing your own shortcut applications in no time!

## Table of Contents
- [Why Shortcut Applications?](#why)
- [Download App Framework](#download)
- [Application Settings](#settings)
- [Preferences](#preferences)
- [Localization](#localization)
- [Assets](#assets)
- [Application Loop](#loop)
- [vCard Menus](#menus)
- [Pseudo-Global Variables](#global-vars)
- [Pseudo-Local Variables](#local-vars)
- [Pseudo-Classes](#classes)
- [Pseudo-Functions](#functions)
- [Special Comments](#comments)
- [Miscellaneous Tips and Tricks](#misc)
- [Frequently Asked Questions](#faq)
- [Localization](#localization)
- [License](#license)

***

<span id="why"></span>
## Why Shortcut Applications?
Shortcuts are capable of doing far more than simple actions to calculate driving directions, setting reminders, or converting files from one format to another. Complicated shortcuts are possible, but there hasn’t been an easy way to create them short of building your own framework.

Shortcuts do not feature traditional programming concepts like functions, while loops, and local variables. And, most shortcuts are designed to run from the top of its list of actions to the bottom. As a result, many seasoned developers have scoffed at Shortcuts as a toy or gimmick.

>I’ll be the first to tell you, shortcuts are no gimmick, and you can actually create [application experiences that actually go beyond](http://cronios.com) what’s currently possible with apps developed with Xcode.

### Features
App Framework addresses these issues with design and programming methodologies for creating shortcuts. With App Framework, you have a starter shortcut that implements the following application features:

- [Application Settings](#settings)
- [Preferences](#preferences)
- [Localization](#localization)
- [Assets](#assets)
- [Application Loop](#loop)
- [vCard Menus](#menus)
- [Pseudo-Global Variables](#global-vars)
- [Pseudo-Local Variables](#local-vars)
- [Pseudo-Classes](#classes)
- [Pseudo-Functions](#functions)
- [Special Comments](#comments)

With App Framework, you will be able to create complex applications in Shortcuts using a fully touch-oriented and visual programming interface. 

***

<span id="download"></span>
## Download App Framework
The latest version of App Framework is available from RoutineHub.co:

- [Download App Framework Shortcut from RoutineHub.co](https://routinehub.co/shortcut/1510)

Localization files and this documentation for App Framework are available on GitHub:

- [Visit App Framework on GitHub](https://github.com/adamtow/shortcut-app-framework)

>You’re welcome to send me a pull request for new localizations and corrections to the documentation.

***

<span id="settings"></span>
## Application Settings
Application settings and defaults are stored in a dictionary called `App`. Here you’ll find things such as:

- `App Name`
- `Shortcut Name`
- `RoutineHub ID`
- `Current Version`
- `Build Number`
- `Default Preferences`
- `Default Language`
- `Device-Specific Preferences`

![App-Dictionary-in-App-Framework](https://atow.files.wordpress.com/2019/01/App-Dictionary-in-App-Framework.png?w=1280)

The `App` variable is an example of a [pseudo-global variable](#global-vars) and is available throughout your entire application lifecycle.

***

<span id="preferences"></span>
## Preferences
Preferences are stored as a JSON file in the Shortcuts directory of iCloud Drive in a folder named `App Framework` (the value of the `App.App Name` variable):

`iCloud Drive/Shortcuts/App Framework 2/config.json`

Default Preferences, found in the `App` global, are loaded the first time the shortcut is launched by the user. In this shortcut, we have six preferences:

1. **Auto Update**: Determines whether the shortcut checks for update automatically via RoutineHub on launch.
2. **Debug**: Displays alerts when moving through the application. Useful for understanding what [pseudo-function](#functions) is being called at the exact moment.
3. **Debug Level**: The level of debug. You can choose to display different information depending on the debug level.
4. **Donated**: If you want to add donation to your app, this flag checks whether or not the user has donated.
5. **Language**: The language that will be used to generate the `Localized Strings` global variable.
6. **Version**: This corresponds to the build number of the application. It’s updated whenever a new version of the shortcut is added and run by the user.

Preferences are available throughout your program’s execution via the `Preferences` global variable.

![Preferences in Code and During Program Execution](http://adamtow.github.io/shortcut-app-framework/images/2_preferences.png)

As you build your own application, you’ll add and remove items to the `Preferences` variable in the `App` global. Remember to add them to your preferences menu in the [pseudo-function `Application.settings`](#application-settings).

>When you make changes to the `Preferences` variable, remember to call **Set Variable** on `Preferences`. Otherwise, your changes will not be saved back to the `Preferences`. Follow this up with either the **Save File** action to save your changes back to iCloud Drive or use the `Preferences.save` pseudo-function to queue your save up during the next iteration of the application loop.

![Calling Set Variable and Preferences.save pseudo-function when saving preferences.](http://adamtow.github.io/shortcut-app-framework/images/2_preferences_save.png)

### Device-Specific Preferences
You may want to have separate preferences for your shortcut when it is run on the user’s iPad or iPhone. If desired, set the value of the `App.Device-Specific Preferences` boolean to `true`.

This will use the Device Name of the iOS device as the unique name for the preferences file.

![Device-Specific Preferences](http://adamtow.github.io/shortcut-app-framework/images/2_device-specific-preferences.png)

>Note: If the user has multiple iOS devices with the same name, those devices will share the same preferences file.

### Show Debugging Information
The preference `Debug` turns on additional debugging alerts when running the App Framework shortcut. It will stop execution at key moments during program execution, including the beginning and end of the Application Loop. You’ll be able to view the function that will be called next, the state of the `Current Number` and `Current Text` global variables, and more.

![Debug Preference and Alerts](http://adamtow.github.io/shortcut-app-framework/images/2_debug.png)

Use IF/EQUALS Debug True blocks when developing your application so you can inspect what is happening during complex portions of your code. You can also place a [specialty formatted comment](#comments) immediately below the IF block if you intend to remove this code prior to releasing your shortcut to the public.

![Remove Debug Code Prior to Releasing Your Shortcut](http://adamtow.github.io/shortcut-app-framework/images/2_debug_code.png)

Also consider using my shortcut [Inspector](https://tow.com/shortcuts/inspector/), which lets you **view and modify** objects such as Dictionaries, Lists, Text, Numbers, and Booleans at runtime.

![Inspector: View and Modify Objects at Runtime While Developing Your Shortcuts](https://atow.files.wordpress.com/2019/01/35d3c4e8-bff9-45c6-b5ef-8e699146b078.png?w=1280)

***

<span id="localization"></span>
## Localization
App Framework is ready to be translated into your preferred language. It comes with a default English localization and a test Emoji localization. Copy the default English localization and replace its strings with your translated version. Add your localized dictionary to the `All Localizations` global variable.

```
{
	"English": { 
		"Hello": "Hello",
		…
	},
	"Emoji":  { 
		"Hello": "👋🏻",
		…
	},
	"Français":  { 
		"Hello": "Bonjour",
		…
	},
	…
}
```

***

<span id="assets"></span>
## Assets
Image assets for menus are stored as Base64 strings within the `Assets` global variable. In general, menu icons should be resized to between 82 pixels and 132 pixels wide. Note that when displayed as a vCard menu, your image will be cropped to a circle. Be mindful of image elements that go edge-to-edge, as they may be cropped.

![Assets Dictionary in App Framework](http://adamtow.github.io/shortcut-app-framework/images/2_assets.png)

***

<span id="loop"></span>
## Application Loop
The Application Loop is where your application lives most of the time. Once initialization is complete, the shortcut enters a repeat loop and stays there until the user presses the **Cancel** button or otherwise invokes a command which runs the **Exit Shortcut** action.

At the start and end of the repeat loop, there is a comment with the following block of text:

```
✳️✳️✳️✳️◻️✳️✳️✳️
✳️◻️◻️✳️◻️✳️◻️✳️
✳️✳️✳️✳️◻️✳️✳️✳️
✳️◻️✳️◻️◻️✳️◻️◻️
✳️◻️◻️✳️◻️✳️◻️◻️
```

This comment tells you that you are starting the primary application loop.

>You may want to add this comment block to other long and complicated repeat loops.

<span id="flowchart"><span>
### Flowchart
The following diagram displays the flow of operations within the App Framework shortcut. Study this flowchart carefully so you understand how the loop functions.

![App Framework Application Flowchart](https://raw.githubusercontent.com/adamtow/shortcut-app-framework/master/flowchart.png)

Here’s what’s happening during each iteration of the application loop:

1. At the beginning of the loop, the variable `Function Name` is evaluated in a series of IF statements.
2. It looks within the appropriate [pseudo-class](#classes) via an IF/CONTAINS comparison and all class [pseudo-functions](#functions) via an IF/EQUALS comparison for a matching string. When found, it executes the code contained in the IF statement block. If there’s no match, it goes to the next IF/CONTAINS block for a match.
3. At the end of the matching function, the [`Return` object](#return) must be properly set up.
4. Program execution goes to the end of the repeat loop, where the `Return` object is evaluated and global variables are reset.
5. Program execution returns to the top of the loop, where the process repeats itself.

![Pseudo-Functions in App Framework](http://adamtow.github.io/shortcut-app-framework/images/2_pseudo-functions.png)

>Remember, at the end of the function, you must configure the [`Return` object](#return), which will be evaluated at the very end of the repeat loop.

<span id="return"><span>
### Function.stack Object
The `Function.stack` object contains information about the next function. It is normally a list that contains a list of functions to call next.

![Function.stack list in App Framework 2](http://adamtow.github.io/shortcut-app-framework/images/2_function-stack.png)

> If you enter a function that doesn't exist in your app, App Framework will skip over it. If there are no more functions in the `Function.stack` list, it will use the value of the `Default Function` in the `App` dictionary object.

### Shortcut Input
In App Framework 2, the shortcut checks the value of the `Input` pseudo-global variable. If the variable contains a dictionary that has a `fn` key, it will jump to the end of the shortcut and run whatever command is specified by the `fn` key. This allows you to use code outside of the application loop, which is useful to simulate using the Run Shortcut action on itself. 

The easiest way to evaluate if input was provided to the app is to coerce the input to a text string, surround it with `#` characters and do a text comparison.

![Evaluating the Shortcut Input](https://atow.files.wordpress.com/2019/01/Evaluating-the-Shortcut-Input.png?w=1280)

If the value is `##`, it means the app was launched without parameters. This could mean the user tapped on it from the Shortcuts home screen, launched it from the Notification Widget screen, or invoked via a Siri voice command.

If the text comparison returns false, your shortcut was launched with parameters. You may then choose to run a different function when the Application Loop begins.

***

<span id="menus"></span>
## vCard Menus (App Framework 1.0.1 Documentation Version)
Standard menus generated by the **Choose from List** or **Choose from Menu** actions are limited in what they can display. A trick many developers have been using in their shortcuts is supplying the **Choose from List** action with a list filled with Contact cards, also known as [vCards](https://en.m.wikipedia.org/wiki/VCard).

![Pretty Menus Using vCards](https://atow.files.wordpress.com/2019/01/Pretty-menus-using-vCards.png?w=1280)

As you can see below, vCards can hold a lot of information; fields not visible to the user can still be retrieved using the **Get Details of Contacts** action.

![Get Details of Contacts](https://atow.files.wordpress.com/2019/01/Get-Details-of-Contacts.png?w=1280)

Each vCard is a block of text that looks like this. The only required field is the `N` or `ORG` field. If you don’t have an image for your menu, don’t include the `PHOTO;ENCODING=b:` field. 

```
BEGIN:VCARD
VERSION:3.0
N:{{Menu Name}};
TITLE:{{Menu Type}};
ORG:{{Menu Description}};
NOTE:{{Additional Field}}
PHOTO;ENCODING=b:{{Base64 Image}}
END:VCARD
```

- **N**: The menu title.
- **TITLE**: App Framework uses this field to differentiate between types of Menu Items: Actions, Separators, and Data.
- **ORG**: The menu description or subtitle. If `N` is blank and `ORG` is filled out, it will be displayed in both the title and subtitle sections.
- **NOTE**: Not used in App Framework, you can store additional information in this field.
- **PHOTO;ENCODING=b**: A Base64 version of your menu icon. Keep the icons between 82 and 132 pixels in width/height. Image elements along the edges of the box may be cropped, since vCards icons are displayed in a cropped circle.

When transformed into a menu, a vCard looks like this: 

![From vCard text to Pretty Shortcut Menu](https://atow.files.wordpress.com/2019/01/9e72b73f-69c7-413a-b3e2-19cbc10684f7.png?w=1280)

### Creating a vCard Menu
To create a vCard menu, follow these steps:

1. Create an empty list object.
2. Use Set Variable to give your list object a name.
3. Add your vCard text items to the list using the **Add to Variable** action.
4. Run the **Combine Text** action on the list with `Spaces` as the separator when you have added all of the items.
5. Place a **Set Name** action and give the menu a name. You **must add** `.vcf` to the end of the name.
6. Add a **Get Variable** action.
7. Tap **Magic Variable**.
8. **Set Name** action that you created in Step 4.
9. Tap on the variable in the **Get Variable** action and make sure that the variable is being interpreted as a Contact. Your menu **will not** display properly if you forget this step.
10. Add the **Choose from List** action immediately after the **Get Variable** action.

>Alternatively, you can add a **Get Contacts From Input** action in place of **Get Variable** in Step 8.

![vCard Menu Creation](https://atow.files.wordpress.com/2019/01/IMG_2109.png?w=1280)

### Toolbox Pro
You can also use tools such as [Toolbox Pro](https://toolboxpro.app) to create menus in your apps instead of having to do it manually using the techniques described above.

***


<span id="global-vars"></span>
## Pseudo-Global Variables
Shortcuts do not distinguish between global and local variables. All variables are considered global when writing shortcuts. 

However, not all of the variables that you define will be accessible by every function, even though they will appear in the list of variables you can choose from in actions like **Get Variable**. This is because we are defining certain variables within a [pseudo-class](#classes) or [pseudo-function](#functions). During program execution of one function, variables defined in another function may not be available; or if they are, they may not hold the right information.

For variables that you will be using everywhere in your shortcut, App Framework considers pseudo-global variables to have the following attributes:

1. **No Prefix**: No periods in the variable name.
2. **Title Case**: Every word in a global variable is capitalized.

Here are some examples of global variables used in App Framework:

- `App`
- `All Localizations`
- `Localized Strings`
- `Debug`
- `Function.name`
- `Function.stack`

***

<span id="local-vars"></span>
## Pseudo-Local Variables
Local variables are variables that are used solely within a specific [pseudo-function](#functions). For instance, in the `Application.main` function, we create a vCard menu. We don’t use this menu variable anywhere else, so it makes sense to distinguish it from variables in other functions.

To accomplish this, we prefix the variable name with the name of the function that it belongs to. The other named components of a local variable are in lowercase or [lower camel case](https://en.m.wikipedia.org/wiki/Camel_case). For example:

- `Application.menu.choice`
- `Application.main.menuItems`

The local variables in the `Settings.open` function are named as follows:

- `Settings.open.menuItems`
- `Settings.open.menu`
- `Settings.open.choice`

As you can see in the screenshot below, it’s now easy to distinguish which variables belong to which function just by looking at their names.

![Pseudo-Global and Pseudo-Local Variables in App Framework](https://atow.files.wordpress.com/2019/01/Global-Variables-and-Local-Variables-in-App-Framework.png?w=1280)

***

<span id="classes-vars"></span>
## Pseudo-Classes (App Framework 1.0.1 Documentation Version)
Pseudo-classes combine related pseudo-functions under the same prefix. When evaluating the `Function Name` variable, the shortcut can skip unrelated functions belong to other classes.

![Pseudo-Classes in App Framework](https://atow.files.wordpress.com/2019/01/Pseudo-Classes-in-App-Framework.png?w=1280)

In App Framework, there are only two classes:

- `Program`
- `Application`

When you develop your own applications, you may have more pseudo-classes. [LaunchCuts](https://tow.com/shortcuts/launchcuts/), for instance, features the following classes:

- `Folder`
- `Shortcuts`
- `Keywords`
- `Cache`
- `Application`
- `Preferences`
- `Settings`

The start of a class is preceded by a comment with the following text block:

```
◻️🔵🔵◻️🔵◻️◻️
🔵◻️◻️◻️🔵◻️◻️
🔵◻️◻️◻️🔵◻️◻️
🔵◻️◻️◻️🔵◻️◻️
◻️🔵🔵◻️🔵🔵🔵
```

When you see this [comment](#comments), you know we are defining a new pseudo-class. It’s good practice to add another comment explaining what the class does immediately following the IF/CONTAINS statement matching the class name with the global variable `Function Name`.

![Class Definition Comments](https://atow.files.wordpress.com/2019/01/Class-Definition-Comments.png?w=1280)

We also separate pseudo-classes with [columned comments](#comments). Choose an emoji that best represents what your class does when creating the column comments.

![Columned comments separate pseudo-classes](https://atow.files.wordpress.com/2019/01/Columned-comments-separate-pseudo-classes.png?w=1280)

***
 
<span id="functions"></span>
## Pseudo-Functions
Pseudo-functions are contained within a pseudo-class. Their names are prefixed by the name of the class:

- `Application.main`
- `Application.about`
- `Application.help`
- `Application.donate`

You use functions within a class to organize your code in a logical manner. 

The start of the function is a comment with the text block:

```
⬛️⬛️⬛️◻️⬛️◻️◻️⬛️
⬛️◻️◻️◻️⬛️⬛️◻️⬛️
⬛️⬛️⬛️◻️⬛️◻️⬛️⬛️
⬛️◻️◻️◻️⬛️◻️◻️⬛️
⬛️◻️◻️◻️⬛️◻️◻️⬛️
```

This helps you identify the upcoming block of code as belonging to a function.

Be sure to add a comment explaining what the function does immediately following the IF/EQUALS statement matching the function name with the global variable `Function Name`.

### Functions
App Framework has the following functions defined:

- **Application.main**: Displays the main menu for the app.
- **Application.about**: Displays the about page.
- **Application.help**: Opens a web view to the documentation.
- **Application.donate**: Allows the user to donate to the developer.
- **Primary.run**: The primary function in App Framework. This just displays a notification to the user.
- **Settings.open**: Displays the settings page.
- **Settings.language**: Displays a menu allowing the user to change the language of the shortcut.
- **Settings.update**: Checks for updates to the shortcut on RoutineHub.
- **Settings.reset**: Resets all settings for the app.
- **Preferences.get**: Retrieves preferences.
- **Preferences.save**: Saves the value of the `Preferences` variable to disk.
- **Welcome.main**: Code that's run when the application is first installed.

***

<span id="comments"></span>
## Special Comments
Throughout this document, there have been mention of using specially formatted comments to distinguish different sections of your code. Here is a list of special comments that are used in App Framework and other apps:

### Class
This denotes the start of a pseudo-class.

```
◻️🔵🔵◻️🔵◻️◻️
🔵◻️◻️◻️🔵◻️◻️
🔵◻️◻️◻️🔵◻️◻️
🔵◻️◻️◻️🔵◻️◻️
◻️🔵🔵◻️🔵🔵🔵
```

### Debug
This denotes code that should be removed prior to release of the shortcut.

```
‼️‼️‼️	‼️‼️‼️	‼️‼️‼️
‼️‼️‼️	‼️‼️‼️	‼️‼️‼️
‼️‼️‼️	‼️‼️‼️	‼️‼️‼️
‼️‼️‼️	‼️‼️‼️	‼️‼️‼️
‼️‼️‼️	‼️‼️‼️	‼️‼️‼️
```

### External
Not used in App Framework, this comment denotes a call to another shortcut. It’s also used when calling your shortcut recursively. 

```
◻️◻️◻️◻️🔵◻️◻️
◻️◻️◻️◻️🔵🔵◻️
🔵🔵🔵🔵🔵🔵🔵
◻️◻️◻️◻️🔵🔵◻️
◻️◻️◻️◻️🔵◻️◻️
```

### Function
This denotes the start of a pseudo-function. 

```
⬛️⬛️⬛️◻️⬛️◻️◻️⬛️
⬛️◻️◻️◻️⬛️⬛️◻️⬛️
⬛️⬛️⬛️◻️⬛️◻️⬛️⬛️
⬛️◻️◻️◻️⬛️◻️◻️⬛️
⬛️◻️◻️◻️⬛️◻️◻️⬛️
```

### Repeat
This marks the beginning and end of a complex, multi-action repeat loop (like the Application Loop). 

```
✳️✳️✳️✳️◻️✳️✳️✳️
✳️◻️◻️✳️◻️✳️◻️✳️
✳️✳️✳️✳️◻️✳️✳️✳️
✳️◻️✳️◻️◻️✳️◻️◻️
✳️◻️◻️✳️◻️✳️◻️◻️
```

***

<span id="misc"></span>
## Miscellaneous Tips and Tricks
Here are some other tips and tricks that you’ll find useful when developing shortcut applications.

### Backups
There are [plenty of shortcuts](https://routinehub.co/search/?q=backup) that back up your shortcuts to iCloud Drive. Choose one of them and use it regularly. When doing development, make copies of your shortcut applications and suffix the name with:

`App Framework Backup YYYYMMDD_hhmm`

For instance:

- `App Framework Backup 20190110_2032`
- `App Framework Backup 20190109_0930`
- `App Framework Backup 20190108_1050`

Change the color of the shortcut icon so you don’t get confused with the current version you’re working on.

### iCloud Sync and the Corrupt Database Error
Make sure you have iCloud Sync turned on for your Shortcuts in Settings.

![iCloud Sync Turned on for Shortcuts](https://atow.files.wordpress.com/2019/01/iCloud-Sync-Turned-on-for-Shortcuts.png?w=1280)

#### Corrupt Database Error
During the course of developing a complicated shortcut, Shortcuts may crash and display the following alert to you. This is alert looks very scary, and it’s not always obvious which button to press:

![Shortcuts Corrupt Database Error](https://atow.files.wordpress.com/2019/01/Corrupt-Database-Error.png?w=1280)

```
Corrupt Database
The Shortcuts database cannot be read because it is corrupt.

You can email support with your corrupted database or reset your custom shortcuts. Your shortcuts will be lost, but if you use iCloud Sync they will be restored.

- Email Support
- Reset Shortcuts
- Exit
```

Your mileage may vary, but I have found success in tapping Reset Shortcuts. This sometimes makes the Shortcuts app unusable (it launches and immediately crashes). When this happens, I turn restart the iOS device off and on by following these steps:

1. Press the Volume Up button.
2. Press the Volume Down button.
3. Press and hold the Sleep/Wake button.
4. Slide to power off.
5. Wait a few moments after the screen goes completely black.
6. Press and hold the Sleep/Wake button until the Apple logo appears

After the device has rebooted, I open back up to the Shortcuts app, which may be completely empty.
 
Do not panic.

![Don’t Panic Just Yet](https://atow.files.wordpress.com/2019/01/Empty-Shortcuts-Application-after-database-corruption-crash.png?w=1280)

I repeat. **Do not panic.** 
 
After a minute, all of my shortcuts return from iCloud. The order of the shortcuts, however, may be completely hosed (so much for the **Sync Shortcut Order** preference in iOS Settings).

It may be time to panic if your shortcuts never return (make sure you are first connected to the internet because iCloud sync cannot work while offline). In this event, you will have to restore from a backup that you made earlier to iCloud Drive. 

### JavaScript
You can run JavaScript in your shortcuts using the following steps:

1. Add a **Text** action containing your script. Consider using an app like the [excellent Scriptable iOS application](https://scriptable.app) to debug your script prior to pasting it into Shortcuts.
```
const dict = { "name": "John Doe", "address": "123 Main Street" };
document.write( JSON.stringify( dict ) );
```
2. Add a **Text** action that references your script in (1).
```
<!DOCTYPE html>
<head>
<meta http-equiv="content-type" content="text/html; charset=UTF-16">
</head>
<script>
{{Script Variable from Step 1}}
</script>
</head>
<body>
</body>
</html>
```

3. Add a **Base64 Encode** action. Set Line Breaks to None.
4. Add the **URL** action with the following text: `data:text/html;base64,{{Base64 Variable From Step 3}}`.
5. Add a **Get Contents of Web Page** action.
6. Add a **Get Text From Input** action.
7. Add a **Replace Text** action to get rid of the occasional trailing space in the output. Search string is `^\s+|\s+$|\s+(?=\s)/g` and the replace string is `$1` with the Regular Expression slider activate.
8. Insert a **Get Dictionary From Input** action. What is returned is the contents of the `document.write( JSON.stringify( dict ) );` command in Step 1.

![Running JavaScript](https://atow.files.wordpress.com/2019/01/Running-JavaScript.png?w=900)
	
You can change the output format, but your JavaScript should end with a `document.write` command. For instance, if all you want to return is a 0 or 1, just end the script with `document.write(1)` or `document.write(0)`. Make sure that the return string doesn’t have trailing spaces (see Step 7).

### Restarting Shortcuts
When your shortcut gets very large, it’s just a matter of time before the Shortcuts application becomes unstable and crashes. Here are symptoms you can look out for before it crashes:

- Scrolling becomes slow and jittery.
- Fields that have editable text go white and can’t be tapped on.
- Action blocks are shown visually overlapping each other.
- Tapping on an action to add it to the shortcut’s bottom takes a long time.

When you see any of the above, consider closing your shortcuts, waiting a minute, and force-quitting the Shortcuts app. This will free up any memory used up during your development period and make it available again for the Shortcuts app. Scrolling will become smoother and the other visual defects will be resolved (until it starts getting slow again).

### Scrolling Through Your Code
Shortcut applications can get quite long, and it’s a pain to scroll through hundreds or thousands of lines of code at a time.

#### Jumping to the Top
One tip is to put the most important code at the top or bottom of your shortcut. You can jump to the top of the shortcut by tapping on your device’s status bar.

>I hope future versions of Shortcuts improves navigation in complicated shortcuts. It would be great to jump more quickly between sections of code through search or via something akin to [Kodex’s minimap](https://kodex.app/#features-href).

#### Actions at the Bottom
By tapping on an action in the Actions sidebar, the action will be added to the bottom of your shortcut. Remember to delete the action if you aren’t planning to use it. 

>I like to use the **Comment** action since it does nothing when run.

![Jump to the bottom by adding a new action.](https://atow.files.wordpress.com/2019/01/Jump-to-the-bottom-by-adding-a-new-action..png?w=1280)

#### Swiping Speed
The faster your swipe, the faster Shortcuts will scroll through your code. By separating your classes with [comments](#comments), you can see which section of code you’re in as you pass the columned comments.

#### Scrollbar Position
You can also take a look at the scrollbar position of the current code block you’re in. If the app crashes, or you have to exit and re-open your shortcut, you can swipe scroll until you reach that remembered position in the scrollbar.

![Remember your scrollbar position](https://atow.files.wordpress.com/2019/01/Remember-your-scrollbar-position.png?w=1280)

### Save File
Be sure to enable **Overwrite If File Exists** when saving the `Preferences` global back to disk. If you don’t, Shortcuts will create another preferences file but won’t change the one that your shortcut relies on.

![Remember to check Overwrite If File Exists when saving preferences.](https://atow.files.wordpress.com/2019/01/Remember-to-check-Overwrite-If-File-Exists-when-saving-preferences..png?w=1280)

### Version Track Your Shortcut
[App Framework is on GitHub](https://github.com/adamtow/shortcut-app-framework) and you’re welcome to send me a pull request for new localizations and corrections to this documentation.

>It’s a little more difficult to get the actual shortcut into git, but there are some [intriguing developments](https://routinehub.co/shortcut/1486) to watch for in the future.

Sign up for a GitHub account. Now that [free accounts can create unlimited private repositories](https://blog.github.com/2019-01-07-new-year-new-github/), there’s no excuse to version track your code.

***

<span id="faq"></span>
## Frequently Asked Questions

### Why put all this code into one shortcut? It’s too complex and takes too long to scroll through all of the code. Why not compartmentalize the functionality into smaller sets of shortcuts? 
If you plan on distributing your shortcuts to the public, I think it’s better to place all of your code into one shortcut rather than having the user download (and maintain) several dependency shortcuts. 

My hope is that the Shortcuts team at Apple addresses this in the future. Allowing shortcuts to be a bundle or container for multiple shortcuts would go a long way towards creating small re-usable shortcuts that can be used across multiple shortcut applications.

### Shortcuts are not as powerful as what I can create in Xcode. 
Of course, and I am not saying shortcut applications are. At the same time, can you efficiently and effectively program in Xcode while laying in bed or strolling in the park? You can’t lug your laptop everywhere you go like you can carry your iPhone. Can you make an app in 30 seconds in Xcode to select a group of photos from the user’s Photos Library and resize them for uploading to a website?

>App Framework provides tools and design methodologies for creating complex applications just in Shortcuts. I wrote upwards of 30% of LaunchCuts, Cronios, and Inspector when away from my desk.

The visual programming style of Shortcuts, coupled with the touch UI of iOS enables on-the-go mobile programming. Rather than trying to shoehorn decades of desktop programming tools into a phone or tablet interface, why not try to create new development tools and interfaces optimized for mobile and touch? And, where appropriate, port the best tools from desktop programming to the mobile environment.

>I’d rather have a beefed-up version of Shortcuts than a port of Xcode to iOS.

***

<span id="localization"></span>
## Localization
App Framework is available in English, but the application is fully ready to be localized. I have developed an application, Localization Helper, that will assist you in localizing App Framework into your language.

> [**Download Localization Helper from RoutineHub &raquo;**](https://routinehub.co/shortcut/1931)

When the localization file is complete, either submit a pull request on [my GitHub page](https://github.com/adamtow/).


***

<span id="license"></span>
## License
Copyright © 2018-2020 Adam Tow • tow.com • @atow

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.