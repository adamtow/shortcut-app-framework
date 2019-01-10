# shortcut-app-framework

[App Framework](https://tow.com/shortcuts/framework/) is a starting point for shortcut developers to create applications in Shortcuts.

By Adam Tow • [tow.com](tow.com) • [@atow](https://twitter.com/atow/)

App Framework features:

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

The App Framework shortcut is full of comments detailing exactly how the framework operates. Study and learn from App Framework and you’ll be writing your own shortcut applications in no time!

![App Dictionary in App Framework](https://atow.files.wordpress.com/2019/01/App-Dictionary-in-App-Framework.png?w=1280)

<hr />

<span id="settings"></span>
## Application Settings
Application settings and defaults are stored in a dictionary called "App". Here you’ll find things such as:

- App Name
- Shortcut Name
- RoutineHub ID
- Current Version
- Build Number
- Default Preferences
- Default Language
- etc.

![App-Dictionary-in-App-Framework](https://atow.files.wordpress.com/2019/01/App-Dictionary-in-App-Framework.png?w=1280)

The App variable functions as a [pseudo-global variable](#global-vars) and is available throughout your entire application lifecycle.

<hr />

<span id="preferences"></span>
## Preferences
Preferences are stored in the Shortcuts directory of iCloud Drive in a folder named "App.App Name". For instance:

`iCloud Drive/Shortcuts/App Framework/config.json`

Default Preferences, found in the App global, are loaded the first time the shortcut is launched by the user. In this framework, we have six preferences:

1. **Copy Locally**: When copying text, copy to the local clipboard or the user’s global iCloud clipboard (for Handoff).
2. **Debug**: Displays alerts when moving through the application. Useful for understanding what [pseudo-function](#functions) is being called at the time.
3. **Language**: The language that will be used to generate the Localized Strings global variable.
4. **Number**: A number that the user can modify.
5. **Text**: A string that the user can modify.
6. **Version**: This corresponds to the build number of the application. It’s updated whenever a new version of the shortcut is added and run by the user.

![Preferences in Code and During Program Execution](https://atow.files.wordpress.com/2019/01/Preferences-in-Code-and-While-Running-Application.png?w=1280)

As you build your own application, you’ll add and remove items to the Default Preferences variable in the App global. You’ll add them to your preferences menu in the [pseudo-function "Application.settings"](#application-settings).

<hr />

<span id="localization"></span>
## Localization
App Framework is ready to be translated into your preferred language. It comes with a default English localization and a test Emoji localization. All you need to do is replace the strings in the default English file with your strings and add your localized strings dictionary to the `All Localizations` global variable.

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
Image assets for menus and such are stored as Base64 strings within the Assets global variable. Menu icons should generally be between 82 pixels and 132 pixels in size. vCard menus crop the image within a circle, so be mindful of image elements that go edge to edge; they may be cropped.

![Assets Dictionary in App Framework](https://atow.files.wordpress.com/2019/01/Assets-Dictionary-in-App-Framework-1.png?w=1280)

<hr />

<span id="menus"></span>
## vCard Menus
Standard Choose from List or Choose from Menu actions are limited in what can be displayed in Shortcuts. A trick many developers is supplying the Choose from List action with a list formatted as Contacts, or [vCards](https://en.m.wikipedia.org/wiki/VCard).

As you can see below, vCards can hold a lot of information; fields not visible to the user can still be retrieved using the "Get Details of Contacts" action.

![From vCard text to Pretty Shortcut Menu](https://atow.files.wordpress.com/2019/01/9e72b73f-69c7-413a-b3e2-19cbc10684f7.png?w=1280)

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

### Creating a vCard Menu
To create a vCard menu, follow these steps:

1. Create an empty list object.
2. Add your vCard text items to the list using the **Add to Variable** action.
3. Run the **Combine Text** action on the list with `Spaces` as the separator when you have added all of the items.
4. Place a **Set Name** action and give the menu a name. You **must add** `.vcf` to the end of the name.
5. Add a **Get Variable** action.
6. Tap **Magic Variable**.
7. Choose the **Set Name** action that you created in Step 4.
8. Tap on the variable in the **Get Variable** action and make sure that the variable is being interpreted as a Contact. Your menu **will not** display properly if you do not do this.
9. Add the **Choose from List** action immediately after the **Get Variable** action.

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
The following diagram displays the flow of operations within the App Framework shortcut.

![App Framework Application Flowchart](https://raw.githubusercontent.com/adamtow/shortcut-app-framework/master/flowchart.png)

At the beginning of the repeat loop, the variable `Function Name` is evaluated in a series of IF statements. It looks within the appropriate [pseudo-class](#classes) and all class [pseudo-functions](#functions) for a matching string. When a match is found, it executes the code in the IF statement.

![Pseudo-Functions in App Framework](https://atow.files.wordpress.com/2019/01/Pseudo-Functions-in-App-Framework.png?w=1280)

At the end of the function, you must configure the [`Return` object](#return), which will be evaluated at the very end of the repeat loop.

<span id="return"><span>
### Return Object
The `Return` object contains information about the next function, global variables to assign, and other things your shortcut needs to operate. It is a dictionary that can hold whatever data you wish.

![Return Object in App Framework](https://atow.files.wordpress.com/2019/01/Return-Object.png?w=1280)

>You may want to handle errors better than in App Framework. Consider it an exercise to add an Error Code and Error Number fields to the Return dictionary. If an error occurred during the shortcut’s execution, display the error to the user. 

In App Framework, the primary values that the developer must specify in the `Return` object are:

- **handled**: a boolean value. Set true if the function successfully ran. False otherwise. App Framework returns to the Program.start function if an error occurred.
- **next**: a string that is the next function to call. `Function Name` will be assigned the value of `next` at the end of the repeat loop.
- **args**: a dictionary or singular element that will be assigned to the `Arguments` global variable. This can then be used by the next function.
- **selection**: an array of elements. Not used by App Framework, but you can use selection to hold items that the next function needs to operate on (i.e. a list of images or text items, for instance).

![Repeat Loop Beginning and End](https://atow.files.wordpress.com/2019/01/Repeat-Loop-Beginning-and-End.png?w=1280)

<hr />

<span id="global-vars"></span>
## Pseudo-Global Variables
Shortcuts do not distinguish between global and local variables. All variables are considered global when writing shortcuts. 

However, not all of the variables that you define will be accessible by every function, even though they will appear in the list of variables you can choose from in actions like **Get Variable**. This is because we are defining certain variables within a [pseudo-class](#classes) or [pseudo-function](#functions).

For variables that you will be using everywhere in your shortcut, App Framework uses write them out as an uppercase string:

- App
- All Localizations
- Localized Strings
- Current Number
- Current Text
- Function Name
- Return
- Arguments
- Selection

![Pseudo-Global Variables](https://atow.files.wordpress.com/2019/01/Pseudo-Global-Variables.png?w=1280)

<hr />

<span id="local-vars"></span>
## Pseudo-Local Variables
Local variables are those defined as being used within a pseudo-function. For instance, in the `Program.step2` function, we create the vCard menu. We don’t need to use the menu variable in any other function, so we need a way to distinguish it from variables in other functions.

![Pseudo-Local Variables in App Framework](https://atow.files.wordpress.com/2019/01/Pseudo-Local-Variables-in-App-Framework.png?w=1280)

To that end, we prefix the variable name with the name of the function that it belongs to. For instance:

- Program.step2.menu
- Program.step2.menuChoice
- Program.step2.menuType
- Program.step2.newText

In another function, we follow the same convention:

- Application.settings.menu
- Application.settings.language
- Application.settings.args

As you can see in the screenshot below, it’s easy to distinguish which variables are accessible from which function by looking at the variable name.

![Pseudo-Global and Pseudo-Local Variables in App Framework](https://atow.files.wordpress.com/2019/01/Global-Variables-and-Local-Variables-in-App-Framework.png?w=1280)

<hr />

<span id="classes-vars"></span>
## Pseudo-Classes

Pseudo-classes combine related pseudo-functions under the same prefix. When evaluating the `Function Name` variable, the shortcut can skip unrelated functions belong to other classes.

![Pseudo-Classes in App Framework](2019/01/Pseudo-Classes-in-App-Framework.png?w=1280)

In App Framework, there are only two classes:

- Program
- Application

When you develop your own applications, you may have more pseudo-classes. LaunchCuts, for instance, features the following classes:

- Folder
- Preferences
- Shortcuts
- Keywords
- Cache
- Application
- Settings

The start of a class is preceded by a comment with the following text block:

```
◻️🔵🔵◻️🔵◻️◻️
🔵◻️◻️◻️🔵◻️◻️
🔵◻️◻️◻️🔵◻️◻️
🔵◻️◻️◻️🔵◻️◻️
◻️🔵🔵◻️🔵🔵🔵
```

When you see this comment, you know we’re about to define a new Pseudo-Class. Be sure to add a comment explaining what the class does immediately following the IF/CONTAINS statement matching the class name with the global variable `Function Name`.

<hr />
 
<span id="functions"></span>
## Pseudo-Functions
Pseudo-functions are contained within a Pseudo-Class. Their names are prefixed by the name of the class:

- Program.start
- Program.step2
- Program.step3
- Program.step4
- Program.actionMenuHandler

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

- **Program.start**: This function doesn’t do too much, although you can use it to do some additional initialization. You could, for instance, evaluate the `Original Input` global variable and display something different depending on whether the shortcut was launch with input or not.
- **Program.step2**: The primary function in App Framework. This displays the menu containing the Current Number and Current Text preference variables. It also displays the Actions menu, and takes user input.


