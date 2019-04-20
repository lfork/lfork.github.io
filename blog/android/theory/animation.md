
# 1、参考资料 https://developer.android.com/guide/topics/resources/animation-resource

Animation resources
An animation resource can define one of two types of animations:

+ 属性动画  Property Animation
Creates an animation by modifying an object's property values over a set period of time with an Animator.

+ 视图动画 View Animation
 There are two types of animations that you can do with the view animation framework:

补间动画 Tween animation: Creates an animation by performing a series of transformations on a single image with an Animation
帧动画 Frame animation: or creates an animation by showing a sequence of images in order with an AnimationDrawable.

//图片变化类的动画资源放到drawable 目录
//形状变化的资源放到anim目录

# 2、属性动画


# 3、视图动画

## 3.1 帧动画

An object used to create frame-by-frame animations, defined by a series of Drawable objects, which can be used as a View object's background.

The simplest way to create a frame-by-frame animation is to define the animation in an XML file, placed in the res/drawable/ folder, and set it as the background to a View object. Then, call start() to run the animation.

An AnimationDrawable defined in XML consists of a single <animation-list> element and a series of nested <item> tags. Each item defines a frame of the animation. See the example below.

### 编写 spin_animation.xml file in res/drawable/ folder:

```XML
 <!-- Animation frames are wheel0.png through wheel5.png
     files inside the res/drawable/ folder -->
 <animation-list android:id="@+id/selected" android:oneshot="false">
    <item android:drawable="@drawable/wheel0" android:duration="50" />
    <item android:drawable="@drawable/wheel1" android:duration="50" />
    <item android:drawable="@drawable/wheel2" android:duration="50" />
    <item android:drawable="@drawable/wheel3" android:duration="50" />
    <item android:drawable="@drawable/wheel4" android:duration="50" />
    <item android:drawable="@drawable/wheel5" android:duration="50" />
 </animation-list>
```

### Here is the code to load and play this animation.
```java

 // Load the ImageView that will host the animation and
 // set its background to our AnimationDrawable XML resource.
 ImageView img = (ImageView)findViewById(R.id.spinning_wheel_image);
 img.setBackgroundResource(R.drawable.spin_animation);

 // Get the background, which has been compiled to an AnimationDrawable object.
 AnimationDrawable frameAnimation = (AnimationDrawable) img.getBackground();

 // Start the animation (looped playback by default).
 frameAnimation.start();
 
```

### 更多资源 Drawable Animation https://developer.android.com/guide/topics/graphics/drawable-animation.html

## 3.2 补间动画（插值器）

### 资源文件
```xml
<set xmlns:android="http://schemas.android.com/apk/res/android"
    android:shareInterpolator="false">
    <scale
        android:interpolator="@android:anim/accelerate_decelerate_interpolator"
        android:fromXScale="1.0"
        android:toXScale="1.4"
        android:fromYScale="1.0"
        android:toYScale="0.6"
        android:pivotX="50%"
        android:pivotY="50%"
        android:fillAfter="false"
        android:duration="700" />
    <!-- Scale 比例缩放 -->
    <!-- fillAfter 变化后是否还原 -->
    <set
        android:interpolator="@android:anim/accelerate_interpolator"
        android:startOffset="700">
        <scale
            android:fromXScale="1.4"
            android:toXScale="0.0"
            android:fromYScale="0.6"
            android:toYScale="0.0"
            android:pivotX="50%"  
            android:pivotY="50%"
            android:duration="400" />
            <!-- pivotX 基点，基于哪个点 -->
        <rotate
            android:fromDegrees="0"
            android:toDegrees="-45"
            android:toYScale="0.0"
            android:pivotX="50%"
            android:pivotY="50%"
            android:duration="400" />

            <!-- toDegrees 负号表示逆时针 -->
    </set>
</set>

```


### 常用插值器

Interpolators
An interpolator is an animation modifier defined in XML that affects the rate of change in an animation. This allows your existing animation effects to be accelerated, decelerated, repeated, bounced, etc.

An interpolator is applied to an animation element with the android:interpolator attribute, the value of which is a reference to an interpolator resource.

All interpolators available in Android are subclasses of the Interpolator class. For each interpolator class, Android includes a public resource you can reference in order to apply the interpolator to an animation using the android:interpolator attribute. The following table specifies the resource to use for each interpolator:

| Interpolator class | 	Resource ID |
|---- | --- |
|AccelerateDecelerateInterpolator	|@android:anim/accelerate_decelerate_interpolator
|AccelerateInterpolator	|@android:anim/accelerate_interpolator
|AnticipateInterpolator	|@android:anim/anticipate_interpolator
|AnticipateOvershootInterpolator	|@android:anim/anticipate_overshoot_interpolator
|BounceInterpolator	|@android:anim/bounce_interpolator
|CycleInterpolator	|@android:anim/cycle_interpolator
|DecelerateInterpolator	|@android:anim/decelerate_interpolator
|LinearInterpolator	|@android:anim/linear_interpolator
|OvershootInterpolator	|@android:anim/overshoot_interpolator
Here's how you can apply one of these with the android:interpolator attribute:

```xml
<set android:interpolator="@android:anim/accelerate_interpolator">
    ...
</set>
```

# 4 其他
## Android动画中属性fillafter和fillbefore的正确理解 https://blog.csdn.net/nihaoma9999/article/details/46790173

很多文章都对fillafter和fillbefore这两个属性进行过描述，大致都是说：



/******动画结束时，停留在最后一帧**********
```XML
<set android:fillAfter="true" android:fillBefore="false">
```

/******动画结束时，停留在第一帧**********
```XML
<set android:fillAfter="false" android:fillBefore="true">
```


且注：xml设置在scale标签里面设置是无效的，注意是set标签



-----------------------------------------------------------------------------------------------------华丽丽的分割线--------------------------------------------------------------------------------------------------------

但这里面很多人尝试使用并不好用，原因就是注没看明白。

正确解释是fillafter和fillbefore这两个属性是不能够在</alpha>,</scale>,</translate>,</rotate>中设置的，

这样设置是无效的，要能正确显示效果需要这样设置：

```XML
<?xml version="1.0" encoding="utf-8"?>  
   <set xmlns:Android="http://schemas.android.com/apk/res/android"  
    android:fillEnabled="true"  
    android:fillAfter="true">  
    <translate    
        android:interpolator="@android:anim/cycle_interpolator"  
        android:fromXDelta="0"  
        android:toXDelta="100"  
        android:fromYDelta="0"  
        android:toYDelta="100"  
        android:duration="2000"  
        >   
    </translate>   
   
</set>  
```

这里面可能犯的错误是属性放置的位置有错误，再就是没有添加下面语句，这些都是导致效果不能实现的原因

android:fillEnabled="true"
要是Java则：
```java
    setFillAfter(true);  
    setFillBefore(false);
```

