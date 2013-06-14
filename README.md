# Presentation notes for RubyConf 2012.


* [RubyConf 2012](http://rubyconf2012.busyconf.com/schedule/full)
* 3rd November 2012 in Denver
* Speaker: Yuichiro MASUI <masui@masuidrive.jp>
* Reviewer: Aaron Petterson and ebi-chan.


## MobiRuby

![slide 1](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.001-resized.jpg)

Today, I’d like to talk about MobiRuby.  It’s my first English presentation.


## Hi! I'm @masuidrive

![slide 2](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.002-resized.jpg)
 ![slide 4](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.004-resized.jpg)

Nice to meet you. I’m Yuichiro MASUI. 
Please call me Ichi.
I come from Tokyo. 
My twitter and github accounts are masuidrive.

I really love programming and write code anytime, anywhere.
Especially with the Ruby language.

But my most popular Open Source product is a PHP-based Wiki engine.

Japanese Rubyists know me as "Furo-grammer". Furo means hot-tub. I code while in the hot-tub.

I’m working at a Japanese startup called FrogApps that has released a food photo-sharing app for iPhone and Android.
But that app is not built on MobiRuby. It is a native app.

Until September, I had worked at Appcelerator, which released Titanium Mobile. It’s a Javascript-based iOS/Android development framework.


## What's MobiRuby?

![slide 5](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.005-resized.jpg)


But now I’m working on MobiRuby. It’s a competitor of Titanium Mobile.
MobiRuby is my private project, not a business.

MobiRuby is an iOS app development environment built on mruby.
You can create iOS apps using Ruby.
MobiRuby provides a bridge between Cocoa and mruby.
You can call native Cocoa classes and functions in Ruby, instead of using Objective-C.

Currently, MobiRuby only supports iOS. But I have plan for an Android version.
I have confirmed that mruby can run on Android devices. 

I’ve already released a MobiRuby-based game app. 

[Demo]
This puzzle game is built on MobiRuby and my Ruby code.
It uses a UIImageView and UIAnimation.

It’s really smooth, the same speed as a native application.
[/Demo]

You can download this application from iOS AppStore.
Please search for “mobiruby” in the store.


## mruby

![slide 6](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.006-resized.jpg)

mruby is the latest Ruby implementation by Matz.
I heard about mruby for the first time in 2010. At that time, it was called RiteVM.

mruby is designed for embedded environments, which means it uses a smaller amount of memory and has a modular structure.

mruby is more compact than other Ruby implementations.
On OSX, the mruby binary is around 460 kilobytes.  CRuby is over 2 megabytes.

mruby can be even smaller because it has a modular structure.
If you want to remove the Math module, you can do so easily.

So, mruby supports multiple virtual machines. CRuby doesn't support that yet.

Still, mruby has many limitations. mruby is built on an ISO specification which means mruby doesn’t support Thread and many classes.
Almost all Ruby standard libraries are not supported.
So, Rubygems isn’t supported either.

And mruby still alpha version. Sometimes, I find mruby bugs.


## Vision

![slide 7](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.007-resized.jpg)

MobiRuby provides Ruby power to mobile devices.
Especially mobile phones.

Do you know “wax”?
It’s a Lua-based iOS application development toolchain, similar to MobiRuby. 

Lua is a popular language for embedded systems.
The syntax is similar to ActionScript.

Wax was born about 3 years ago, but it isn’t popular yet.
Why? Because of the Lua language. 

There bridge library is one of glue language.
That needs powerful language support.
I think Lua doesn’t have enough dynamic programming capabilities. 

I think meta-programming is the most important and powerful capability of Ruby.
Ruby’s meta-programming is good for wrapping native libraries
and making a DSL for iOS.


## Hello world

![slide 8](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.008-resized.jpg)

This is the hello world code for MobiRuby.

iOS developers can understand this sample code.
It’s really similar to Objective-C.


## Hello world - 1st half

![slide 9](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.009-resized.jpg)

In the first half, the code defines a new class, MyAlertView, which inherits from the UIAlertView Cocoa class.

And defines a new delegate method through the define method.
mruby doesn't support keyword arguments yet. It’s a 2.0 feature.
And mruby’s hash is non-ordered. It’s same as Ruby 1.8.

MyAlertView is defined in Ruby code. So it’s a Ruby class and an Objective-C class.
Both Ruby and Objective-C can access MyAlertView.



## Hello world - 2nd half

![slide 10](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.010-resized.jpg)

In the second half, I created a MyAlertView instance and show this alert box.

When you call an Objective-C method,
You need to start with underscores as the slide shows.

In the current version, if you want to make an iOS app in MobiRuby,
you need to understand Objective-C and Cocoa libraries.

Is it non-sense? I think so, too.


## Next step

![slide 11](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.011-resized.jpg)

I am aiming for this code in the next version.
It looks like truly Ruby code.

ActiveRecord wrapped SQL and a good DSL for ruby programmers.
So MobiRuby will wrap Cocoa libraries.

First, I will focus on making a Cocoa bridge in mruby, because it is the baseline for MobiRuby.
After that, I’ll create new APIs.


## MobiRuby stack

![slide 12](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.012-resized.jpg)

Now, I will talk about MobiRuby internals.

MobiRuby is made up of five components.


## mruby

![slide 13](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.013-resized.jpg)

mruby is main component of MobiRuby.
It was patched, and some of the configurations were changed.


## mruby-cfunc

![slide 14](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.014-resized.jpg)

The first component is mruby-cfunc.

It’s an interface between C-level functions and mruby.


## mruby-cfunc

![slide 15](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.015-resized.jpg)

mruby-cfunc provides an interface between mruby and C-level functions.

It’s the same as Ruby/DL library on CRuby.


## mruby-cfunc code

![slide 16](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.016-resized.jpg)

This code calls the “puts” C function from Ruby.
Internally, it converts a Ruby string into a C string, looks up the puts function pointer and calls the function.

mruby can call all C functions without specific extensions.
This component is independent from MobiRuby.
If you want to use mruby-cfunc in your mruby project, you can use it.


## mruby-cocoa

![slide 17](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.017-resized.jpg)

The 2nd component is mobiruby-cocoa.

It’s an interface between Objective-C and Cocoa.


## mruby-cocoa

![slide 18](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.018-resized.jpg)

mruby-cocoa is bridge for Cocoa, which is the iOS and OSX framework.

This library provides transparent communication between mruby and Cocoa.


# Bridge Cocoa runtime

![slide 19](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.019-resized.jpg)

mruby-cocoa allows for the creation of new instances of Cocoa classes in mruby.
You can also inherit from existing Cocoa classes in mruby.

In the earlier “hello world” sample, we inherited from UIAlertView to create MyAlertView. UIAlertView is a Cocoa class, and MyAlertView is defined in Ruby.
Cocoa and ruby class can manipulate each other transparently.

Natural interface, delegation and block functions are supported in this library.



## Memory management

![slide 20](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.020-resized.jpg)

In this library, the hardest part is memory management.

Objective-C uses reference counting. mruby uses mark & sweep.

It’s hard to free objects correctly in both environments.

Now, all Objective-C classes “release” method are overridden. 
It’s an awful bottleneck for performance.

It effects all Cocoa objects that are included and are not related to mruby objects.
It still has a problem that it cannot detect circular references.

I’ll fix it at some point in the future.  Probably I’ll rewrite it in assembler.



## mobiruby-common

![slide 21](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.021-resized.jpg)

The 4th component is mobiruby-common.

It’s for a future release when we will release the Android version.


## mobiruby-common

![slide 22](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.022-resized.jpg)

This library provides common utilities among iOS and Android.

Currently, it provides ‘require’ and ‘load’ methods only.
Standard mruby doesn’t support require or load methods.


## mobiruby-ios

![slide 23](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.023-resized.jpg)

The last component is mobiruby-ios.


## mobiruby-ios

![slide 24](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.024-resized.jpg)

mobiruby-ios is the main part of MobiRuby.
It provides iOS specific utilities that are included for Xcode integration.

In the first version, this component is poor.
I don’t know Xcode very deeply.
I need help to understand how to configure Xcode.


## Wrapped APIs

![slide 25](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.025-resized.jpg)

In this feature, MobiRuby will provide good wrapper APIs.
It will have some compatibility between iOS and Android.

When you use these APIs, you can use Cocoa APIs at the same time.


## Road map

![slide 26](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.026-resized.jpg)

I don’t have a detailed plan because it’s my private project.
I don’t have a boss or marketing section.
I aim to have the first production-level version by the end of Q1 2013.

This version will support almost all Objective-C features.
I plan to start writing API and tutorial documents.

After that, I’ll start the next version. It will have wrapped APIs.
So you can code in a Ruby style.


## Current status

![slide 27](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.027-resized.jpg)

I have already released a MobiRuby game. Apple did not reject this app.

In Sept, I finally released the MobiRuby alpha version. It’s the first public version.
I will keep updating it. Now, I want to focus on writing test code.
Until recently, I wrote a few tests only.
2 weeks ago, I started using Travis-CI. 

I think the hardest part is Thread because mruby doesn’t support Thread.
I wrote a limited thread feature and some sample code for use cases.

Currently, MobiRuby is not for ordinary Ruby programmers.
If you know Objective-C and have an interest in mruby,
Please join this project.



## FAQ

![slide 28](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.028-resized.jpg)

I have 3 frequency questions.


### What's different from RubyMotion?

![slide 29](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.029-resized.jpg)

First question, “What’s different from RubyMotion?”

RubyMotion is ...

* LLVM-based and compiles to native code.
* Only for iOS.
* Extends Ruby syntax
* Not Open Source

Maybe we have the same goal.
But the approach is different.

If you want to make an app soon, I recommend using RubyMotion.
It’s stable and faster than the current version of MobiRuby.


### Can I use RubyGems?

![slide 30](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.030-resized.jpg)

2nd question, “Can I use RubyGems?”

No, mruby doesn’t have compatibility with the CRuby ext. APIs. You will need to write a new extension for mruby.

bovi and matz have discussed about mrbgems.


### Can I use existing Cocoa libraries?

![slide 31](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.031-resized.jpg)

3rd question, “Can I use existing Cocoa libraries?”

Yes, you can use almost all existing libraries.

I think CocoaPods will be a good partner.
CocoaPods is a library manager for Xcode.


### Questions?

![slide 32](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.032-resized.jpg)

If you have a question, please tweet to @mobiruby or post to github issues.



## Contributors wanted

![slide 33](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.033-resized.jpg)


I want a teammate. I'm alone now.

If you have any interest in this project, please contact me.


## Thank you

![slide 34](https://raw.github.com/masuidrive/mobiruby-rubyconf2012/master/slides/MobiRuby-RubyConf.032-resized.jpg)

* http://mobiruby.org
* http://github.com/mobiruby
* http://twitter.com/mobiruby 
* http://fb.me/mobiruby

Thank you very much!
