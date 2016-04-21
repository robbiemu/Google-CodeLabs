# Google CodeLabs - [Build a Material Design App with the Android Design Support Library](https://codelabs.developers.google.com/codelabs/material-design-style/index.html#0)
_see also [Material Design](https://www.google.com/design/spec/material-design)_

What you’ll learn 
* How to use the Android Design Support Library.
* How to apply material design to your Android app.
* The following key material design components and principles:
* Themes and colors to create tangible surfaces and print-like design
* App layout best practices for improved navigability
* Animation and touch feedback to express meaningful motion.

## Overview
Installed and viewed finished project. It was remarkably barebones.

## Themes, Colors and Typography

_Themes in general allow you to default default color palettes for your app, or to color an individual activity or View, etc._ 
There are a dark and light Material design theme.

— [Themes](http://developer.android.com/guide/topics/ui/themes.html) are specified in res/values/styles.xml and then set app-wide at the manifest or applied with the `style="@style/.."` attribute. 

_see also [Material Palette](http://www.materialpalette.com/) generator_

## Layout and Animation

### Tangible Surfaces
    Material should be responsive if there was a reason to place it on the screen.
### Bold Elements
    Large, simple, (flat) presentation that quickly brings the user towards usages. 
### Meaningful Motion
    Animation should be present to relay meaning, not absent nor decorative to the point of being an independent part of the app.

* Pattern: we used an appbarlayout with a toolbar and tablayout to keep the top bar responsive and active. These are housed in an outer coordinator layout.
* Pattern: We attached the tab's fragments to a viewpager so the user can swipe to change tabs.
* Beyond just material design, we also used fragments for each tab.

## Style each View and add a RecyclerView

### Tangible Surfaces
    From scrollviews and tabs to content, the tendancy should be to use surfaces for touch related events.
### Cards
    The design of complex information like a card, with name, title, and actionable links, etc, should respect
    the basic design goals of surfaces. Its behavior should be typical for material.

* Beyond just material design, we used a recyclerview in each tab. We extended RecyclerView.ViewHolder to inflate our fragments, and extended the RecyclerView.Adpater to bind a ViewHolder . Then we finished the changes in the fragment classes by having them load the recyclerview. 

_In the example, no data is passed in, and so the constructor is empty. Normally the data is passed in there. Outbound calls to update/etc data can go through a DataSetObserver (which observes a Cursor) which can be attached with `registerDataSetObserver`. Data can be updated to the adapter following [this recipe](http://stackoverflow.com/questions/31367599/how-to-update-recyclerview-adapter-data#answer-31368367):_

1) REMOVE: There are 4 steps to remove an item from a RecyclerView

    list.remove(position);
    recycler.removeViewAt(position);
    mAdapter.notifyItemRemoved(position);                 
    mAdapter.notifyItemRangeChanged(position, list.size());
These line of codes work for me.

2) UPDATE THE DATA: The only things I had to do is

    mAdapter.notifyDataSetChanged();

## Page Elements

###Tangible Surfaces
This unit reviews the use of more surfaces that are tangible by design.
###Bold Elements
Particuraly in the Large header section in the nav slider, and the round FAB, bold presentational decisions are meant to help the user find the flow of use cases for the activity.

I'd also add a third segment:
###Motion Provides meaning
The use of so many animations in the app at this point conveys the flow of use cases through the UX. For example, after seeing a snackbar alert for a bookmarked card or shared intent, we antisipate and look for the alert when adding a favorite, and seing it confirms that we are where we intend to be in the use of the app.

* Beyond just material design, we used a DrawerLayout + NavigationView to create a slide-out actions list (a hot list of actions you cna take with the app).
* Further, we also used the snackbar to serve as a placeholder for actions from the card view.
* In terms of Material design, it is a common pattern found in Google apps and follows the keylines and metrics for lists.
* Material design: The use of a fab to trigger the most likely action for a view. (see [fab guidelines](https://www.google.com/design/spec/components/buttons-floating-action-button.html)) Note:
   * Not every screen needs a floating action button. A floating action button represents the primary action in an application.
   * Only one floating action button is recommended per screen to increase its prominence. It should represent only the most common action.
   * The floating action button doesn’t need to be placed in the lower-right corner of the screen. Consider placing it in the upper-left or upper-right, overlapping with the Toolbar.
   * A mini-size floating action button can also be used, for situations where you need continuity with other visual elements on screen.
   * Placing the FAB in the Coordinator layout allows it to animate in avoidance of the snackbar, when it is also triggered.
* Material design: In this unit, the wealth of response options built in speaks to an inherit preference to action density in activities. 

### The drawer & nav layouts

When using a navbar, the top system bar should remain, even if only semi-transparent.

1. The actual activity must be encapsulated in a Drawer Layout
2. The layout's tree node must sit beside the NavigationView. It must specify an `app:headerlayout` and an `app:menu`. The menu is the slideout's list (`<menu>` of `[<group>]<item>...`), the headerlayout is the top section of the slider. Examples:
#### navheader.xml
    <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="@dimen/navheader_height"
        android:background="?attr/colorPrimaryDark"
        android:orientation="vertical"
        android:padding="@dimen/md_keylines">
    </LinearLayout>
### menu_navigation.xml
    <menu xmlns:android="http://schemas.android.com/apk/res/android">
        <group android:checkableBehavior="single">
            <item
                android:icon="@drawable/ic_home_black_24dp"
                android:tint="@color/button_grey"
                android:title="One" />
            <item
                android:icon="@drawable/ic_favorite_black_24dp"
                android:tint="@color/button_grey"
                android:title="Two" />
            <item
                android:icon="@drawable/ic_bookmark_border_black_24dp"
                android:tint="@color/button_grey"
                android:title="Three" />
        </group>
    </menu>
3. tying the navigation to the activity with `navigationView.setNavigationItemSelectedListener` will let your nav handle clicks to items it contains.
4. Add `mDrawerLayout.openDrawer(GravityCompat.START);` to the actiivty (in `@Override onOptionsItemSelected`) to make the navigation drawer move when you touch the menu.
5. add a style in `values-21` to keep the system bar:
    <resources>
        <style name="AppTheme" parent="AppTheme.Base">
            <item name="android:windowDrawsSystemBarBackgrounds">true</item>
            <item name="android:statusBarColor">@android:color/transparent</item>
        </style>
    </resources>
    
## Detail view with collapsing toolbar

###Meaningful motion
The collapsing toolbar guides the user through the usecase of digging into the details in the detail view. The default transition animation from list to detail actiivty adds to the stack I mentioned earlier.
###Print-like design
By using the toolbar as a image sleave (giving substantial initial layout space to the main item image), we help create a flat, textual, printlike design. 

* Like with any android app, we use an intent to move to a detail view from the item views. The title is set to the item's name
* Material design: in the detail view, we replace a typical toolbar with a CollapsingToolbarLayout to animate the toolbar out of the way as the user focuses down the details.
* Material design: the fully expanded toolbar is given the item's image for its background view

