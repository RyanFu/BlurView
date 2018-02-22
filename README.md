# BlurView

![alt tag](https://github.com/Dimezis/BlurView/blob/master/BlurScreenshot.png)

Dynamic iOS-like blur of underlying Views for Android. 
Includes library and small example project.

BlurView can be used as a regular FrameLayout. It blurs its underlying content and draws it as a background for its children.
BlurView redraws its blurred content when changes in view hierarchy are detected (draw() called). 
It honors its position and size changes, including view animation and property animation.

## [Demo App at Google Play](https://play.google.com/store/apps/details?id=com.eightbitlab.blurview_sample)

## How to use
```XML
  <eightbitlab.com.blurview.BlurView
      android:id="@+id/blurView"
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      app:blurOverlayColor="@color/colorOverlay">

       <!--Any child View here, TabLayout for example-->

  </eightbitlab.com.blurview.BlurView>
```

```Java
    float radius = 20;

    View decorView = getWindow().getDecorView();
    //Activity's root View. Can also be root View of your layout (preferably)
    ViewGroup rootView = (ViewGroup) decorView.findViewById(android.R.id.content);
    //set background, if your root layout doesn't have one
    Drawable windowBackground = decorView.getBackground();

    blurView.setupWith(rootView)
           .windowBackground(windowBackground)
           .blurAlgorithm(new RenderScriptBlur(this))
           .blurRadius(radius);
```

Always try to choose the closest possible root layout to BlurView. This will greatly reduce the amount of work needed for creating View hierarchy snapshot.

## Vector drawables issue
In version 1.4.0 a new API was added - `setHasFixedTransformationMatrix(boolean)`.
You can set it to true to optimize position calculation before blur.
By default, BlurView takes into account its translation, rotation and scale before each draw call.
If you are not changing these properties (for example, during animation), this behavior can be changed
to calculate them only once during initialization.

This also resolves the issue with <a href="https://github.com/Dimezis/BlurView/issues/41">distortion of vector drawables</a>

## Supporting API < 17
If you need to support API < 17, you can include

```Groovy
implementation 'com.eightbitlab:supportrenderscriptblur:1.0.1'
```

setup BlurView with

```Java
blurAlgorithm(new SupportRenderScriptBlur(this))
```

and enable RenderScript support mode

```Groovy
 defaultConfig {
        renderscriptTargetApi 27 //must match target sdk and build tools
        renderscriptSupportModeEnabled true
 }
```

## Important
BlurView can be used only in a hardware-accelerated window.
Otherwise, blur will not be drawn. It will fallback to a regular FrameLayout drawing process.

## Performance
It takes 1-4ms on Nexus 5 and Nexus 4 to draw BlurView with the setup given in example project

## Gradle
```Groovy
implementation 'com.eightbitlab:blurview:1.4.0'
```

License
-------

    Copyright 2016 Dmitry Saviuk

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
