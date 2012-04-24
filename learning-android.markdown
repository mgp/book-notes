## Learning Android

by Marko Gargenta

### Chapter 3
* pg 20: The mainfest file explains what the application consits of, what its main building blocks are, what permissions it requires, and so on.
* pg 21: The layout XML is responsible for the layout of widgets, while the strings XML is responsible for their textual content.

### Chapter 4
* pg 29: An activity transitioning from a _starting_ state to a _running_ state is an expensive operation.
* pg 30: An activity that is visible but not in focus is _paused_. Once not visible, it is in a _stopped_ state.
* pg 30: A destroyed activity is no longer in memory. There's no guarantee that it will be stopped before being destroyed, so do all important work en route to the paused state.
* pg 31: An explicit intent specifies the receiving component, while an implicit intent specifies only the type of receiver.
* pg 32: Services are either _starting_, _running_, or _destroyed_, and this life cycle is controlled by the developer and not so much by the system.
* pg 34: Broadcast receivers are a system-wide publish-subscribe system. Receivers are not actively running in memory, but do get to run some code when triggered.

### Chapter 6
* pg 48: Use XML to declare everything about the UI that is static, and then a programmatic approach to define what happens when the user interacts with its widgets.
* pg 49: Layouts allocate space for their children, which can be views or other layouts in turn.
* pg 50: If you nest multiple `LinearLayout` instances, consider `RelativeLayout`. Heavy nesting has negative effects on time to start the activity, CPU, and battery consumption.
* pg 50: `AbsoluteLayout` positions its children at absolute coordinates on the screen. Although simple, it breaks when screen size, density, or orientation changes.
* pg 54: Specifying `fill_parent` for `layout_height` or `layout_width` uses all available space, while `wrap_content` only uses as much space as is necessary.
* pg 55: The `layout_gravity` specifies how a widget is positioned within its layout, while `gravity` specifies how the widget's content is positioned within the widget itself.
* pg 57: The `setContentView` inflates from XML, or parses the XML and creates a corresponding Java object for each element.
* pg 62: Beware that if your project doesn't have `R.java` generated, then Organize Imports will import the `android.R` class.
* pg 64: A terminal running `adb logcat` at all times is faster for debugging than switching to the DDMS perspective in Eclipse.
* pg 67: The `AsyncTask` class is used to help handle long operations that need to report to the UI thread when progress is made or finally completed.
* pg 75: Do not use extensions when referring to other file resources; Android figures out the best file format automatically.
* pg 77: Android optionally expands on the RGB color set with the alpha channel, so you can express values for each channel in hexadecimal as `#ARGB` or `#AARRGGBB`.
* pg 79: Alternative resources, like strings in different languages or images with different pixel density, work by specifying the qualifiers in the names of their resource folders.
* pg 81: The Hierarchy Viewer tool attaches to any device or emulator and introspect the structure of the current view. For performance, aim for flat, nested layouts.

### Chapter 7
* pg 87: A subclass of `PreferenceActivity` uses `addPreferencesFromResource()` instead of `setContentView()` to set its content from an XML file containing preferences.
* pg 88: Any building block like an activity, service, broadcast receiver, or content provider must be defined in the `AndroidManifest.xml` file.
* pg 90: The "title condensed" attribute of a menu item is shown instead of the title attribute if space is limited.
* pg 91: The `onCreateOptionsMenu()` is only called once to inflate the menu, and doesn't get called again until the activity is destroyed.
* pg 96: The `/sdcard` partition is a poorly structured, free-for-all partition that is a suitable place to store large files such as music, photos, or videos.
* pg 97: The `data` subfolder of the `/data` partition contains subfolders corresponding to each application, each named by the package used to sign the corresponding application.

### Chapter 8
* pg 101: An unbound service runs independently of activities. A bound service provides more specific APIs through Android Interface Definition Language, or AIDL.
* pg 102: As long as any part of your app is running, the `Application` object will be created, and so is a good place for common state.
* pg 104: You must add an `android.name` attribute to the `application` element in `AndroidManifest.xml` to specify your subclass of `Application`.
* pg 107: The `onStartCommand` method is called whenever the service receives a `startService` intent, and unlike `onCreate` and `onDestroy`, can be called repeatedly.

### Chapter 9
* pg 120: The single-file nature of SQLite makes security straightforward, as it boils down to filesystem security.
* pg 122: Class `SQLiteDatabase` supports prepared statements for `INSERT`, `UPDATE`, `DELETE`, and `SELECT`; all other SQL statements must be executed directly.
* pg 124: The versioning provided by `SQLiteOpenHelper` simplifies recognizing when the schema has changed and tables must be altered.
* pg 127: The database is stored in the `databases` subdirectory in your application directory along the `/data/data` path.
* pg 129: On the command line, `sqlite3` will not complain if the file you refer to does not exist, and will simply create a new database.

### Chapter 10
* pg 138: A `ScrollView` contains only one direct child, and should have its width and height specified as `fill_parent`.
* pg 141: The `startManagingCursor()` method of `Activity` manages the cursor's life cycle the same way it manages its own.
* pg 143: The `ListActivity` is convenient where the built-in `ListView` is the only widget in the activity.
* pg 148: The method `DateUtils.getRelativeTimeSpan` provides a human-readable relative time since a given timestamp.
* pg 150: The `ViewBinder` assigned to an adapter specifies what data should be bound in the default manner, and what data requires a custom bind.
* pg 151: The category `android.intent.category.LAUNCHER` must be added to an activity's `<intent_filter>` for the application to be shown in the launcher drawer.
* pg 156: The `onMenuOpened()` callback allows you to customize menu items before the menu is displayed.

