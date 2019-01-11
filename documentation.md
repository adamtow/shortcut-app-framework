# Shortcut App Framework
By Adam Tow • [tow.com](tow.com) • [@atow](https://twitter.com/atow/)

[App Framework](https://tow.com/shortcuts/framework/) is a starting point for shortcut developers to create complex applications in Shortcuts. Study and learn from this documentation and the shortcut, which is heavily commented, and you’ll be writing your own shortcut applications in no time!

## Download App Framework
The latest version of App Framework is available from RoutineHub.co:

- [Download App Framework Shortcut from RoutineHub.co](https://routinehub.co/shortcut/1510)

Localization files and documentation for App Framework are also available on GitHub:

- [Visit App Framework on GitHub](https://github.com/adamtow/shortcut-app-framework)

You’re welcome to send me a pull request for new localizations and corrections to the documentation.

## Features
App Framework provides scaffolding for implementing the following application features in your shortcuts:

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

<hr />

<span id="settings"></span>
## Application Settings
Application settings and defaults are stored in a dictionary called `App`. Here you’ll find things such as:

- App Name
- Shortcut Name
- RoutineHub ID
- Current Version
- Build Number
- Default Preferences
- Default Language
- etc.

![App-Dictionary-in-App-Framework](https://atow.files.wordpress.com/2019/01/App-Dictionary-in-App-Framework.png?w=1280)

The `App` variable is an example of a [pseudo-global variable](#global-vars) and is available throughout your entire application lifecycle.

<hr />

<span id="preferences"></span>
## Preferences
Preferences are stored as a JSON file in the Shortcuts directory of iCloud Drive in a folder named `App Framework` (the value of the `App.App Name` variable):

`iCloud Drive/Shortcuts/App Framework/config.json`

Default Preferences, found in the `App` global, are loaded the first time the shortcut is launched by the user. In this framework, we have six preferences:

1. **Copy Locally**: When copying text, copy to the local clipboard or the user’s global iCloud clipboard (for Handoff).
2. **Debug**: Displays alerts when moving through the application. Useful for understanding what [pseudo-function](#functions) is being called at the time.
3. **Language**: The language that will be used to generate the Localized Strings global variable.
4. **Number**: A number that the user can modify.
5. **Text**: A string that the user can modify.
6. **Version**: This corresponds to the build number of the application. It’s updated whenever a new version of the shortcut is added and run by the user.

Preferences are available throughout your program’s execution via the `Preferences` variable.

![Preferences in Code and During Program Execution](https://atow.files.wordpress.com/2019/01/Preferences-in-Code-and-While-Running-Application.png?w=1280)

As you build your own application, you’ll add and remove items to the Default Preferences variable in the App global. You’ll add them to your preferences menu in the [pseudo-function "Application.settings"](#application-settings).

>When you make changes to the `Preferences` variable, remember to call **Set Variable** on `Preferences`. Otherwise, your changes won’t be saved back to the `Preferences` object. Also call the **Save File** action on `Preferences` to save your changes back to disk.

![Calling Set Variable and Save File actions when saving preferences.](https://atow.files.wordpress.com/2019/01/Calling-Set-Variable-and-Save-File-actions-when-saving-preferences..png?w=1280)

### Show Debugging Information
The preference `Debug` turns on additional debugging alerts when running the App Framework shortcut. It will stop execution at key moments during program execution, including the beginning and end of the Application Loop. You’ll be able to view the function that will be called next, the state of the `Current Number` and `Current Text` global variables, and more.

Use IF Debug Equals True blocks when developing your application so you can inspect what’s happening during challenging portions of your code. Also consider using my shortcut [Inspector](https://tow.com/shortcuts/inspector/), which lets you **view and modify** objects such as Dictionaries, Lists, Text, Numbers, and Booleans at runtime.

![Inspector: View and Modify Objects at Runtime While Developing Your Shortcuts](https://atow.files.wordpress.com/2019/01/35d3c4e8-bff9-45c6-b5ef-8e699146b078.png?w=1280)

<hr />

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

![Localized Strings](https://atow.files.wordpress.com/2019/01/Image.png?w=1280)

<hr />

<span id="assets"></span>
## Assets
Image assets for menus are stored as Base64 strings within the `Assets` global variable. In general, menu icons should be resized to between 82 pixels and 132 pixels wide. Note that when displayed as a vCard menu, your image will be cropped to a circle. Be mindful of image elements that go edge-to-edge, as they may be cropped.

![Assets Dictionary in App Framework](https://atow.files.wordpress.com/2019/01/Assets-Dictionary-in-App-Framework-1.png?w=1280)

<hr />

<span id="menus"></span>
## vCard Menus
Standard menus generated by Choose from the List or Choose from Menu actions are limited in what they can display. A trick many developers have been using in their shortcuts is supplying the Choose from List action with a list filled with Contact cards, also known as [vCards](https://en.m.wikipedia.org/wiki/VCard).

As you can see below, vCards can hold a lot of information; fields not visible to the user can still be retrieved using the "Get Details of Contacts" action.

![Get Details of Contacts](https://atow.files.wordpress.com/2019/01/Get-Details-of-Contacts.png?w=1280)

Each vCard is a text block that looks like this. The only required field is the `N` or `ORG` field. If you don’t have an image for your menu, don’t include the `PHOTO;ENCODING=b:` field. 

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

<hr />

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

>You may want to add this comment block to other long and complicated repeat loop.

<span id="flowchart"><span>
### Flowchart
The following diagram displays the flow of operations within the App Framework shortcut. Study this flowchart carefully so you understand how the loop functions.

![App Framework Application Flowchart](https://raw.githubusercontent.com/adamtow/shortcut-app-framework/master/flowchart.png)

Here is a simplified description of what’s happening during each iteration of the application loop:

1. At the beginning of the repeat loop, the variable `Function Name` is evaluated in a series of IF statements.
2. It looks within the appropriate [pseudo-class](#classes) via an IF/CONTAINS comparison and all class [pseudo-functions](#functions) via an IF/EQUALS comparison for a matching string. When found, it executes the code contained in the IF statement block.
3. At the end of the function, the [`Return` object](#return) must be properly set up.
4. Program executions goes to the end of the repeat loop, where the `Return` object is evaluated and global variables are reset.
5. Program execution returns to the top of the loop, where the process repeats itself.

![Pseudo-Functions in App Framework](https://atow.files.wordpress.com/2019/01/Pseudo-Functions-in-App-Framework.png?w=1280)

>Remember, at the end of the function, you must configure the [`Return` object](#return), which will be evaluated at the very end of the repeat loop.

<span id="return"><span>
### Return Object
The `Return` object contains information about the next function, global variables to assign, and other things your shortcut needs to operate. It is a dictionary that can hold whatever data you wish.

![Return Object in App Framework](https://atow.files.wordpress.com/2019/01/Return-Object.png?w=1280)

>You may want to handle errors better than in App Framework. Consider it an exercise to add an `Error Code` and `Error Number` fields to the `Return` dictionary. So, if an error occurred during the shortcut’s execution, you can use this information to raise an alert to the user.

Within this version of App Framework, the primary values that the developer must specify in the `Return` object are:

- **handled**: a boolean value. Set true if the function successfully ran. False otherwise. App Framework returns to the Program.start function if an error occurred.
- **next**: a string that is the next function to call. `Function Name` will be assigned the value of `next` at the end of the repeat loop.
- **args**: a dictionary or singular element that will be assigned to the `Arguments` global variable. This can then be used by the next function.
- **selection**: an array of elements. Selection is actually not used by App Framework, but it’s here to show how you can hold a list of items that the next function needs to operate on (i.e. a list of images, numbers, or strings).

![Repeat Loop Beginning and End](https://atow.files.wordpress.com/2019/01/Repeat-Loop-Beginning-and-End.png?w=1280)

<hr />

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
- `Current Number`
- `Current Text`
- `Function Name`
- `Return`
- `Arguments`
- `Selection`

![Pseudo-Global Variables](https://atow.files.wordpress.com/2019/01/Pseudo-Global-Variables.png?w=1280)

<hr />

<span id="local-vars"></span>
## Pseudo-Local Variables
Local variables are variables that are used solely within a specific [pseudo-function](#functions). For instance, in the `Program.step2` function, we create a vCard menu. We don’t use this menu variable anywhere else, so it makes sense to to distinguish it from variables in other functions.

![Pseudo-Local Variables in App Framework](https://atow.files.wordpress.com/2019/01/Pseudo-Local-Variables-in-App-Framework.png?w=1280)

To accomplish this, we prefix the variable name with the name of the function that it belongs to. The other named components of a local variable are in lowercase or [lower camel case](https://en.m.wikipedia.org/wiki/Camel_case). For example:

- `Program.step2.menu`
- `Program.step2.menuChoice`
- `Program.step2.menuType`
- `Program.step2.newText`

The local variables in the `Application.settings` function are named as follows:

- `Application.settings.menu`
- `Application.settings.language`
- `Application.settings.args`

As you can see in the screenshot below, it’s now easy to distinguish which variables belong to which function just by looking at their names.

![Pseudo-Global and Pseudo-Local Variables in App Framework](https://atow.files.wordpress.com/2019/01/Global-Variables-and-Local-Variables-in-App-Framework.png?w=1280)

<hr />

<span id="classes-vars"></span>
## Pseudo-Classes
Pseudo-classes combine related pseudo-functions under the same prefix. When evaluating the `Function Name` variable, the shortcut can skip unrelated functions belong to other classes.

![Pseudo-Classes in App Framework](https://atow.files.wordpress.com/2019/01/Pseudo-Classes-in-App-Framework.png?w=1280)

In App Framework, there are only two classes:

- Program
- Application

When you develop your own applications, you may have more pseudo-classes. [LaunchCuts](https://tow.com/shortcuts/launchcuts/), for instance, features the following classes:

- Folder
- Shortcuts
- Keywords
- Cache
- Application
- Preferences
- Settings

The start of a class is preceded by a comment with the following text block:

```
◻️🔵🔵◻️🔵◻️◻️
🔵◻️◻️◻️🔵◻️◻️
🔵◻️◻️◻️🔵◻️◻️
🔵◻️◻️◻️🔵◻️◻️
◻️🔵🔵◻️🔵🔵🔵
```

When you see this comment, you know we are defining a new pseudo-class. Be sure to add a comment explaining what the class does immediately following the IF/CONTAINS statement matching the class name with the global variable `Function Name`.

![Columned comments separate pseudo-classes](https://atow.files.wordpress.com/2019/01/Columned-comments-separate-pseudo-classes.png?w=1280)

We also separate pseudo-classes with columned comments. Choose an emoji that best represents what your class does when creating the column comments.

<hr />
 
<span id="functions"></span>
## Pseudo-Functions
Pseudo-functions are contained within a pseudo-class. Their names are prefixed by the name of the class:

- `Program.start`
- `Program.step2`
- `Program.step3`
- `Program.step4`
- `Program.actionMenuHandler`

You use functions within a class to organize your code in a logical manner. 

![Pseudo-Functions in App Framework](https://atow.files.wordpress.com/2019/01/Pseudo-Functions-in-App-Framework.png?w=1280)

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

- **Program.start**: This function doesn’t do too much, although you can use it to do some additional initialization. You could, for instance, evaluate the `Original Input` global variable and display something different depending on whether the shortcut was launch with input or not. This function is called when the user taps **Start Program** from `Application.welcome` or when the shortcut is run with input.
- **Program.step2**: The primary function in App Framework. This displays the menu containing the `Current Number` and `Current Text` preference variables. It also displays the Actions menu, and takes user input.
- **Program.step3**: This is a stub function. It isn’t called anywhere by the application, but you can add a menu item and update the `Return` object to run it.
- **Program.step4**: Another stub function. This one calls `Program.step5`, but that function doesn’t exist (so it should raise an error at the end of the Application Loop.
- **Program.actionMenuHandler**: This function handles choosing an Action menu from `Program.step2`: Copy to Clipboard, View Application Flow, License, Settings, and Exit.
- **Application.menus**: This function generates the Action menu used in `Program.step2`.
- **Application.welcome**: This function is called when the App Framework shortcut is run with no input.
- **Application.settings**: This function is called when the Settings menu item is chosen from `Program.step2` or `Application.welcome`. It handles updating the `Copy Locally`, `Debug`, and `Language` preferences. It also has a stub for checking for updates to the shortcut. Right now, it informs the user of the current version and build and redirects them to the RoutineHub.co page for App Framework. Your shortcut might call UpdateKit or another shortcut updater library.

<hr />

## Other Tips and Tricks
Here are some other tips and tricks that you’ll find useful when developing shortcut applications.

### Save File
Be sure to activate the **Overwrite If File Exists** slider when saving the `Preferences` global back to disk. If you don’t, Shortcuts will create another preferences file but won’t change the one that your shortcut relies on.

![Remember to check Overwrite If File Exists when saving preferences.](https://atow.files.wordpress.com/2019/01/Remember-to-check-Overwrite-If-File-Exists-when-saving-preferences..png?w=1280)

### Scrolling Through Your Code
Shortcut applications can get quite long, and it’s a pain to scroll through hundreds or thousands of lines of code at a time. One tip is to put the most important code at the top of bottom of your shortcut. You can jump to the top of the shortcut by tapping on your device’s status bar.

By tapping on an action in the Actions sidebar, the action will be added to the bottom of your shortcut. Remember to delete the action if you aren’t planning to use it. 

>I like to use the **Comment** action since it does nothing when run.

The faster your swipe, the faster Shortcuts will scroll through your code. By separating your classes with comments

### Restarting Shortcuts
When your shortcut gets very large, the Shortcuts application can become unstable and more prone to crashing. Before the application crashes, here are symptoms you can look out for:

- Scrolling becomes slow and jittery.
- Fields that have editable text become white and can’t be tapped on.
- Action blocks are shown visually overlapping each other.
- Tapping on an action to add it to the bottom of the shortcut takes a long time.

When you see any of the above, consider closing your shortcuts, waiting a minute, and force-quitting the Shortcuts app. That will free up any memory used up during your development period and make it available again for the Shortcuts app. Scrolling will become smoother and the other visual defects will be resolved (until it starts getting slow again).

### Make Regular Backups
There are [plenty of shortcuts](https://routinehub.co/search/?q=backup) that back up your shortcuts to iCloud Drive. Choose one of them and use it regularly. When doing development, make copies of your shortcut applications and suffix the name with:

`App Framework Backup YYYYMMDD_hhmm`

For instance:

- `App Framework Backup 20190110_2032`
- `App Framework Backup 20190109_0930`
- `App Framework Backup 20190108_1050`

Change the color of the shortcut icon so you don’t get confused with the current version you’re working on.

### Use GitHub for Localization and Documentation
Sign up for a GitHub account. Now that [free accounts can now create unlimited private repositories](https://blog.github.com/2019-01-07-new-year-new-github/), there’s no excuse to version track your code.

>It’s a little more difficult to get the actual shortcut into git, but there are some [intriguing developments](https://routinehub.co/shortcut/1486) to watch for in the future.

[App Framework is on GitHub](https://github.com/adamtow/shortcut-app-framework) and you’re welcome to send me a pull request for new localizations and corrections to the documentation.

### Turn on iCloud Sync
Sometimes Shortcuts crashes and displays a corrupted database error. This is alert looks very scary, and it’s not always obvious what button to press:

>Corrupt database
>
>The shortcuts database cannot be read because it is corrupt.
>
>You can email support with your lunch corrupted data base or reset your custom shortcuts. Your Shortcuts will be lost, but if you use iCloud sync they will be restored.
>
>- Email Support
>
>- Reset Shortcuts
>
>- Exit

Your mileage may vary, but I have found success in tapping Reset Shortcuts. This sometimes makes the Shortcuts app unusable (it launches and immediately crashes). When this happens, I perform turn the phone off and on again (Volume Up, Volume Down, hold Sleep/Wake button). When iOS reboots, and I open back up to the Shortcuts app, it’s completely empty. After about a minute, all of my shortcuts return, although the order of the shortcuts is completely hosed (so much for the **Sync Shortcut Order** preference in Settings)

![Empty Shortcuts Application after database corruption crash](https://atow.files.wordpress.com/2019/01/Empty-Shortcuts-Application-after-database-corruption-crash.png?w=1280)

<hr />

# License
MIT License
Copyright © 2018 Adam Tow • tow.com • @atow

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.