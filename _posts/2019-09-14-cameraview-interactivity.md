---
title: "Recording a camera overlay on Android with CameraView: an interactive example."
date: 2019-09-14T21:42:56+02:00
comments: true
draft: false
---

On Android, recording an overlay on top of the camera has been, until recently, an exceedingly difficult task.  
[CameraView](https://github.com/natario1/CameraView) by [@natario1](https://github.com/natario1) is a great an Android camera library, it's easy to setup yet highly configurable. I've contributed to help introduce, in the new version 2 of the library, the following feature: a simple way to record or take pictures with an overlay/watermark on top of the camera. In this blog post I will show you how to use this new feature.

Here's a sneak peak of what we'll build:

<p>
<img src="https://github.com/randiisan/CameraView-overlay-demos/raw/master/media/ciao.gif" alt="GIF of the video recording while drawing 'Ciao' on top of the camera preview." width="250" vspace="20" hspace="5">
<img src="https://raw.githubusercontent.com/randiisan/CameraView-overlay-demos/master/media/ciao_screenshot.png" alt="Screenshot of the app UI with 'Ciao' written on top of the camera preview." width="250" vspace="20" hspace="5">
</p>

It's basically [Android Draw](https://github.com/divyanshub024/AndroidDraw) on top of the camera preview.  
Most of the usages of a camera overlay that I can think of, don't require interactivity and, as you will see, adding interactivity will not be straightforward. The reason I built this example is to show you what you can create with the library.   

The full code is available on Github [here](https://github.com/randiisan/CameraView-overlay-demos/tree/master/FreeDrawing) so I won't go through every line of code step by step, I want to tell you about the ideas and the thought process behind it.

## A basic camera Activity

The starting point will be a basic camera activity featuring:

- full screen camera preview
- a video recording button
- a button to take pictures  
- a button to flip the camera  

If this is the first time you hear about CameraView, you can head over to [the documentation](https://natario1.github.io/CameraView/) and try to create this activity by yourself to get familiar with CameraView.

Otherwise, you can just grab the code from Github [here](https://github.com/randiisan/CameraView-overlay-demos/tree/master/BasicWatermark). Consider this little project a snapshot, a starting point from which I'll build this example and future examples regarding CameraView. I'll refer to it as BasicWatermark.

To build BasicWatermark, I basically followed the [getting started](https://natario1.github.io/CameraView/about/getting-started.html) guide, added a bunch of buttons and copied the `PicturePreviewActivity` and `VideoPreviewActivity` from CameraView's demo.

### Quick intro on overlays in CameraView

In a gist, the way CameraView implement overlays is by having the `CameraView` act as a `FrameLayout`, whose children `View`s get recorded.  
Here's just a quick example I made up to see this new overlay feature in action.

Given that your `CameraView` in your `activity_main.xml` (or any other layout file) looks like this:

```xml
<com.otaliastudios.cameraview.CameraView
    android:id="@+id/camera"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:keepScreenOn="true"
    app:cameraAudio="off" />
```

Add some message on top of the camera, by using a `TextView` as a child of `CameraView`:

```xml
<com.otaliastudios.cameraview.CameraView
    android:id="@+id/camera"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:keepScreenOn="true"
    app:cameraAudio="off">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginTop="16dp"
        android:text="I will be recorded!"
        android:textColor="@android:color/white"

        app:layout_drawOnPreview="true"
        app:layout_drawOnPictureSnapshot="true"
        app:layout_drawOnVideoSnapshot="true"

        />

</com.otaliastudios.cameraview.CameraView>
```

That's it!

As you can see you can choose whether to show the overlay in the camera preview `app:layout_drawOnPreview`, in pictures `app:layout_drawOnPictureSnapshot` and/or videos `app:layout_drawOnVideoSnapshot`.

## Painting on screen

The open source library [Android Draw](https://github.com/divyanshub024/AndroidDraw) provides an easy to use `View`, the `DrawView`, that allows painting on the screen.

Our goal is to allow the user to paint on top of the camera preview, the drawing will show up in pictures and recorded videos.  

Again, I assume as a starting point a copy of the [BasicWatermark](https://github.com/randiisan/CameraView-overlay-demos/tree/master/BasicWatermark) Android Studio project.

After importing Android Draw as a dependency, the first thing that comes to mind is to simply add the `DrawView` as a child of `CameraView`. Like the following:

```xml
<com.otaliastudios.cameraview.CameraView
    android:id="@+id/camera"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:keepScreenOn="true"
    app:cameraAudio="off">

    <com.divyanshu.draw.widget.DrawView
        android:id="@+id/draw_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_drawOnPreview="true"
        app:layout_drawOnPictureSnapshot="true"
        app:layout_drawOnVideoSnapshot="true"
        />

</com.otaliastudios.cameraview.CameraView>
```

You'll notice that it doesn't work.  
It's as if the `DrawView` is not receiving and touch event because the `CameraView` is intercepting them, we can test this hypothesis by adding in our `MainActivity`'s `onCreate` method the following lines:

```java
// camera is our CameraView.
camera.setOnTouchListener(new View.OnTouchListener() {
    @Override
    public boolean onTouch(View v, MotionEvent event) {
        Log.d(TAG, "onTouch: Camera touched.");
        return false;
    }
});

// drawView is our DrawView.
drawView.setOnTouchListener(new View.OnTouchListener() {
    @Override
    public boolean onTouch(View v, MotionEvent event) {
        Log.d(TAG, "onTouch: DrawView touched.");
        return false;
    }
});
```

By tapping a few times on the screen, you will see the following in the LogCat.

![LogCat showing CameraView intercepting touch events](/assets/img/2019-09-14-cameraview-interactivity/logcat.png)

The CameraView is intercepting our touch events!

### Interactivity

I've found a workaround, it's not very elegant but it works.

- create a `ForwardTouchesView` that holds a reference to a `View` and forwards `onTouchEvent` calls to it.  
- make your layout a `FrameLayout` which allows stacking `View`s. The children will take the following order (from front to back) ConstraintLayout (or whatever layout that holds your UI) > ForwardTouchesView > CameraView with DrawView as overlay.
- have the `ForwardTouchesView` forward touch events to the `DrawView`.

By doing so the `ForwardTouchesView`, since it is on top of the `CameraView`, will intercept touch events from the `CameraView` and forward them to the `DrawView` instead.

In practice, here's the code for the `ForwardTouchesView`.

```java
public class ForwardTouchesView extends View {

    @Nullable
    private View forwardTo;

	// the following three methods are constructors for custom View
    public ForwardTouchesView(Context context) {
        super(context);
    }
    public ForwardTouchesView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
    }
    public ForwardTouchesView(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }

    public void setForwardTo(@Nullable View v) {
        this.forwardTo = v;
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        if (forwardTo != null) {
            return forwardTo.onTouchEvent(event);
        }
        return super.onTouchEvent(event);
    }

    @Override
    public boolean performClick() {
        return super.performClick();
    }
}
```

And here's how your layout file should look like:

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <com.otaliastudios.cameraview.CameraView
        android:id="@+id/camera"
        android:keepScreenOn="true"
        app:cameraAudio="off"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        >

        <com.divyanshu.draw.widget.DrawView
            android:id="@+id/draw_view"
            android:layout_width="match_parent"
            android:layout_height="match_parent"

            app:layout_drawOnPreview="true"
            app:layout_drawOnPictureSnapshot="true"
            app:layout_drawOnVideoSnapshot="true"
            />

    </com.otaliastudios.cameraview.CameraView>


    <!-- replace with your package name! -->
    <com.ran3000.cameraviewdemo.freedrawing.ForwardTouchesView
        android:id="@+id/forwardTouches"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        />


    <androidx.constraintlayout.widget.ConstraintLayout
        android:layout_height="match_parent"
        android:layout_width="match_parent"
        >

        <!--your UI goes here-->

    </androidx.constraintlayout.widget.ConstraintLayout>

</FrameLayout>
```

![Layers of views, explains the above code more clearly.](/assets/img/2019-09-14-cameraview-interactivity/layers.png)

And finally in your activity's `onCreate` call:

```java
forwardTouchesView.setForwardTo(drawView);
```

Now it should be working! Here's what you can do:

<img src="/assets/img/2019-09-14-cameraview-interactivity/blackpaint.jpg" alt="Smiling face drawn on top of the camera preview." width="250" vspace="20" hspace="5">

A this point I wanted a little bit more options for the drawing, so I added a sidebar that allows the user to select a paint color and to clear the canvas.
I won't go through the details, you can take a look at the [complete code](https://github.com/randiisan/CameraView-overlay-demos/tree/master/FreeDrawing).

### Weird crashes

By playing with it, I noticed that sometimes, in the middle of a video recording, the app crashes a video with a `java.util.ConcurrentModificationException` thrown by CameraView's `VideoEncoder`.  
Unfortunately I haven't been able to solve this without having to fork Android Draw. You can change your Android Draw dependency to `implementation 'com.github.RAN3000:AndroidDraw:da6512c'` and sync to fix this quickly.  
(You might have to take a look at [this StackOverflow page](https://stackoverflow.com/questions/13565082/how-can-i-force-gradle-to-redownload-dependencies) if it doesn't work, check that in the `DrawView`'s code there are some `synchronized` calls)

The reason of the exception is that CameraView, when drawing on the video recording, calls `DrawView`'s `onDraw()` method.  
In the latter method, a `LinkedHashMap` containing the drawn paths is looped over. This results in a exception when a new path is added by, for example, drawing with your finger: it happens in a different thread, the UI thread.  

I have no idea if the following is the best option but I managed to fix this problem by adding some synchronization.

That's something you might have to keep in mind if your overlay is a custom view with complex behavior like `DrawView`.

## The end 
First of all here are, again, the links to:

- [CameraView](https://github.com/natario1/CameraView)
- [Android Draw](https://github.com/divyanshub024/AndroidDraw)

I want to thank [@natario1](https://github.com/natario1) the author of CameraView for letting me contribute to the repo and for the advice he directly and indirectly gave me. 

You have any question/critique/feedback feel free to leave a comment or hit me up at [giacomoran@gmail.com](mailto:giacomoran@gmail.com).

