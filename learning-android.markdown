## Learning Android

by Marko Gargenta

### Chapter 3
* pg 20: The mainfest file explains what the application consits of, what its main building blocks are, what permissions it requires, and so on.
* pg 21: The layout XML is responsible for the layout of widgets, while the strings XML is responsible for their textual content.

### Chapter 4
* pg 29: An activity transitioning from a _starting_ state to a _running_ state is an expensive operation.
* pg 30: An activity that is visible but not in focus is _paused_. Once not visible, it is in a _stopped_ state.
* pg 30: A destroyed activity is no longer in memory. There’s no guarantee that it will be stopped before being destroyed, so do all important work en route to the paused state.
* pg 31: An explicit intent specifies the receiving component, while an implicit intent specifies only the type of receiver.
* pg 32: Services are either _starting_, _running_, or _destroyed_, and this life cycle is controlled by the developer and not so much by the system.
* pg 34: Broadcast receivers are a system-wide publish-subscribe system. Receivers are not actively running in memory, but do get to run some code when triggered.

### Chapter 6
* pg 48: Use XML to declare everything about the UI that is static, and then a programmatic approach to define what happens when the user interacts with its widgets.
* pg 49: Layouts allocate space for their children, which can be views or other layouts in turn.
* pg 50: If you nest multiple `LinearLayout` instances, consider `RelativeLayout`. Heavy nesting has negative effects on time to start the activity, CPU, and battery consumption.
* pg 50: `AbsoluteLayout` positions its children at absolute coordinates on the screen. Although simple, it breaks when screen size, density, or orientation changes.
* pg 54: Specifying `fill_parent` for `layout_height` or `layout_width` uses all available space, while `wrap_content` only uses as much space as is necessary.
* pg 55: The `layout_gravity` specifies how a widget is positioned within its layout, while `gravity` specifies how the widget’s content is positioned within the widget itself.
* pg 57: The `setContentView` inflates from XML, or parses the XML and creates a corresponding Java object for each element.
* pg 62: Beware that if your project doesn’t have `R.java` generated, then Organize Imports will import the `android.R` class.
* pg 64: A terminal running `adb logcat` at all times is faster for debugging than switching to the DDMS perspective in Eclipse.
* pg 67: The `AsyncTask` class is used to help handle long operations that need to report to the UI thread when progress is made or finally completed.
* pg 75: Do not use extensions when referring to other file resources; Android figures out the best file format automatically.
* pg 77: Android optionally expands on the RGB color set with the alpha channel, so you can express values for each channel in hexadecimal as `#ARGB` or `#AARRGGBB`.
* pg 79: Alternative resources, like strings in different languages or images with different pixel density, work by specifying the qualifiers in the names of their resource folders.
* pg 81: The Hierarchy Viewer tool attaches to any device or emulator and introspect the structure of the current view. For performance, aim for flat, nested layouts.
