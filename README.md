# ActivityAndFragmentLifecycle
## Lesson 4: Activity And Fragment Lifecycle
This is the fourth android project in [Udacity: Developing Android Apps with Kotlin course.](https://classroom.udacity.com/courses/ud9012)
- Basic understanding of Activity and fragment lifecycle 
- Logging message in Log cat 
- Using Lifecycle observer pattern 
- Using Android Debug Bridge(ADB) 

## Highlight for lesson 4:
### Activity Lifecycle 
![Diagram of Activity Lifecycle](https://developer.android.com/codelabs/kotlin-android-training-lifecycles-logging/img/9be2255ff49e0af8.png)

----
###General Definitions
Visible Lifecycle: The part of the Lifecycle between onStart and onStop when the Activity is visible. 
Focus: An Activity is said to have focus when it's the activity the user can interact with. 
Foreground: When the activity is on screen. 
Background: When the activity is fully off screen, it is considered in the background.
——
### Adding Log message 
Steps:
1. Use the log keyword ``` Log.i("MainActivity", "onCreate called") ```
### Lifecycle Library 
In 2017, Google announced the lifecycle library, with the goal of simplifying working with the activity and fragment lifecycle. 
Before the lifecycle library, the only way to interact with a fragment or activity Lifecycle was through the callback methods like onCreate, onStart, onResume and so on.   The lifecycle library introduced the Android Lifecycle as an actual object.  example: ```
fun onButtonClick(){
if(this.lifecycle.currentState.isAtLeast(Lifecycle.State.STARTED)){
// do something 
}
}
```
This new object can be queried and checked for state. 
What’s more, the lifecycle library also introduced, the LifecycleOwner interface and the LifecycleObserver interface.   LifecycleOwners are anything that has a lifecycle, activities and fragments are prefect example of LifecycleOwners.

LifecycleObservers observers the life cycle of the a lifecycle owner, such as an activity. 
——
### Lifecycle Observation 
####Observer Pattern This is a software design pattern that involves an object called the subject, which keeps track of a list of other objects called observers. 

The observers are said to be watching or observing the subject. When the state of the subject changes, it notifies its list of all observers.   Example of lifecycle library can be used to change the responsibility of starting and stopping the timer for the activity to the dessertTimer class. 

Steps:
1. Make DessertTimer a LifecycleObserver: In order to achieve this, DessertTimer should implement a LifecycleObserver, take in a Lifecycle as a parameter and establish observer relationship in init block. ``` class DessertTimer(lifecycle: Lifecycle) : LifecycleObserver {
    init {
        lifecycle.addObserver(this)
    } } ```
2. Annotate startTimer and stopTimer with @OnLifecycleEvent and the correct event: ``` @OnLifecycleEvent(Lifecycle.Event.ON_START)
fun startTimer() {...}

@OnLifecycleEvent(Lifecycle.Event.ON_STOP)
fun stopTimer() {...} ``` 3. Pass in 'this' MainActivity's lifecycle so that it is observed: ``` dessertTimer = DessertTimer(this.lifecycle) ``` ——
### Android Debug Bridge (ADB)
It’s a command-line tool that lets you send instruction to emulators and devices. 
Adding ADB to your path: 1. When you enter “add” in the terminal. And you see the message “command not found”. Follow step 2.  2. Find the platform-tools folder which contains adb. 
You can find the location of the SDK by going to Tools -> SDK Manager. ADB is located in this location, followed by platform-tools/ so in the example above, you could find adb in:
/Users/lmf/Library/Android/sdk/platform-tools/  3. Add adb to your path: Adding a path variable is done using the terminal on Mac/Linux.
1. Open a Terminal
2. Create a .bash_profile file if you don’t have one already. This is a configuration file for bash) - it’s executed when you start bash:
    * touch ~/.bash_profile
3. Open up the ~/.bash_profile file in your preferred text editor:
    * open ~/.bash_profile
4. Add the following to your .bash_profile file and save:
    * export PATH=<Path to platform-tools>:$PATH
5. Either restart your terminal, or enter:
    * source ~/.bash_profile
6. Ensure you can run adb by typing:
    * adb
7. You should see output, including something like: ```  Android Debug Bridge version 1.0.40 Version 4986621 ``` 
Terminating a process:
1. Make sure you are using a device or emulator running at least API level 28. This is easier to set up for an emulator.
2. Make sure adb is installed. If not, see instructions below.
3. Make sure your app is in the background - this only happens when the app is in the background. You can do this by hitting the home button.
4. Run the command:  ``` adb shell am kill com.example.android.dessertpusher```
——
 ### OnSaveInstanceState onSaveInstanceState is a callback where you could save data that you might need in case the Android OS destroys your app.   onSaveInstanceState occurs after the activity has been stopped. Its called when your activity goes into the background and not when the app is actually restarting. 

Its is like a safety measure which gives a chance to save a small amount of information to a bundle as your activity exits in the foreground. 

Steps:
1. Override onSaveInstanceState and onRestoreInstanceState: You can use the keyboard shortcut Ctrl + O to do this. For onSaveInstanceState, you can use the version that has one parameter, the Bundle.
2. Save the data in onSaveInstanceState: You should save the revenue, dessertsSold and dessertTimer.secondsCount in the state Bundle. Here's an example of saving the revenue: ```outState.putInt(KEY_REVENUE, revenue)``` Note that KEY_REVENUE is a string constant.
3. Retrieve and use the data in onCreate if you're restarting to Activity: Check that the Bundle is not null in onCreate, and then retrieve and use the data. For example: ``` if (savedInstanceState != null) {
        revenue = savedInstanceState.getInt(KEY_REVENUE, 0)
     } ``` ——
### Configuration Changes
A configuration change is where the state of the device changes so radically that the system decides the easiest way to resolve this is to rebuild the activity.   It uses the same onSavedInstanceState bundle to restore the activity, which is why implementing onSavedInstanceState fixed that rotation problem. 

Example of configuration changes include:
- User changed device language
- User plugs in physical keyboard
- User rotates device 
