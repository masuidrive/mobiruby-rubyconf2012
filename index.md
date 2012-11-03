# Presentation notes for RubyConf 2012.

* [http://rubyconf2012.busyconf.com/schedule/full](http://rubyconf2012.busyconf.com/schedule/full)
* 3rd November 2012 in Denver
* Speaker: Yuichiro MASUI <masui@masuidrive.jp>
* Reviewer: Aaron Petterson and ebi-chan.



## MobiRuby

Today I’d like to talk about MobiRuby, it’s my first English presentation.


## Hi! I'm @masuidrive

Nice to meet you. I’m Yuichiro MASUI. 
I came from Tokyo. 
Please call me Ichi.
My twitter and github account is masuidrive.

I really love programming and write code anytime anywhere.
Especially with the Ruby language.

But my most popular Open source product is PHP based Wiki engine.

Japanese Rubists know me as Furo-grammer. Furo means hot-tub. I’m coding while in the hot-tub.

I’m working at Japanese startup FrogApps that has released food photo sharing app for iPhone and Android.
But that app are not built on MobiRuby. there are native apps.

Until September, I had worked at Appcelerator what released Titanium Mobile. it’s Javascript based iOS/Android development framework.


## What's MobiRuby?

But now I’m working on MobiRuby. It’s competitor of Titanium Mobile.
MobiRuby is my private project, not business.

MobiRuby is iOS app development environment built on mruby.
You can create iOS app using Ruby.
MobiRuby provides bridge between Cocoa and mruby.
You call native Cocoa classes and functions instead of Objective-C.

Currently, MobiRuby supported iOS only. But I have plan for Android version.
I confirmed mruby can run on Android devices. 

I’ve already released MobiRuby based game app. 

[Demo]
This puzzle game is built on MobiRuby and my Ruby code.
It used UIImageView and UIAnimation.

It’s really smooth. same as native application.
[/Demo]

You can download this application from iOS AppStore.
Please search for “mobiruby” in the store.


## mruby

mruby is latest Ruby implementation by Matz.
I heard mruby for first time in 2010. At that time, it was called RiteVM.

mruby is designed for embedded environments.
It means small memory and modular structure.

mruby is more compact than other Ruby implements.
On OSX, mruby binary is around 460 kilo bytes.  CRuby is over 2 mega bytes.

mruby can be even smaller. mruby has modular structure.
If you want to remove Math module, you can do it easily.

So, mruby supports multiple virtual machines. CRuby doesn't support yet.

Instead, mruby has many limitations. mruby is built on ISO specification.
It means mruby doesn’t support Thread and many classes.
Almost, all Ruby standard libraries are not supported.
So, Rubygems isn’t supported too.

And mruby still alpha version. Sometimes I find mruby bugs.


## Vision

MobiRuby provides Ruby power to mobile devices.
Especially mobile phones.

Do you know “wax”? w-a-x.
It’s Lua based iOS application development toolchain. similar to MobiRuby. 

Lua is popular language around embedded systems.
The syntax is near ActionScript.

Wax was born about 3 years ago. but It isn’t popular yet.
Why? The cause is Lua language. 

There bridge library is one of glue language.
That needs for powerful language support.
I think Lua doesn’t have enough dynamic programming capabilities. 

I think meta-programming is most important and powerful capability in Ruby.
Ruby’s meta-programming can make good wrapping native libraries
and make DSL for iOS.


## Hello world

This is hello world code of MobiRuby.

iOS developers can understand this sample code.
It’s really similar to Objective-C.


## Hello world - 1st half

In first half, the code defined new class MyAlertView that inherited from 
UIAlertView Cocoa class.

And defined new delegate method through define method.
mruby hasn’t support keyword arguments yet. It’s 2.0 feature.
And mruby’s hash is non-ordered. It’s same as Ruby 1.8.

MyAlertView defined on Ruby code. So It’s Ruby class and Objective-C class.
Ruby and Objective-C both side can touch to MyAlertView.



## Hello world - 2nd half

In second half, created MyAlertView instance. and show this alert box.

When you call Objective-C method,
You need to start with underscores as the slide shows.

On the current version, If you want iOS app on MobiRuby,
You need to understand Objective-C and Cocoa libraries.

Is it non-sense? I think so, too.


## Next step

I am aiming for this code on next version.
It looks like truly Ruby code.

ActiveRecord wrapped SQL and provide good DSL to ruby programmers.
So MobiRuby will wrap Cocoa libraries.

First, I focus to make Cocoa bridge on mruby, it’s for baseline of MobiRuby.
After that, I’ll create new APIs.


## MobiRuby stack

I will talk about MobiRuby internals.

MobiRuby is made up of five components.


## mruby - MobiRuby stack

mruby is main component of MobiRuby.
It was patched, and some configuration changed.


## mruby-cfunc - MobiRuby stack

first component is mruby-cfunc.

It’s an interface with C level function and mruby.


## mruby-cfunc

mruby-cfunc provides an interface to mruby and C level functions.

It’s same as Ruby/DL library on CRuby.


## mruby-cfunc code

This code calls the “puts” C function from Ruby.
Internally, it converts Ruby string to C string, lookup puts function pointer and calls the function.

mruby can call all C functions without specific extensions.
This component is independent from MobiRuby.
If you want to use mruby-cfunc in your mruby project, You can use it.


## mruby-cocoa - MobiRuby stack

2nd component is mobiruby-cocoa.

it’s an interface to Objective-C and Cocoa.


## mruby-cocoa

mruby-cocoa is bridge for Cocoa, which is the iOS and OSX framework.

This library provide transparency communication with mruby and Cocoa.


# Bridge Cocoa runtime

mruby-cocoa provided create new instances of Cocoa Class on mruby.
You can also inherit from existing Cocoa class on mruby.

In earlier “hello world” sample, we inherited from UIAlertView to MyAlertView. UIAlertView is Cocoa class, and MyAlertView defined in Ruby.
Cocoa and ruby class can manipulate transparency.

Natural interface, delegation and block functions supported in this library.



## Memory management

In this library, the hardest part is memory management.

Objective-C uses reference counting. mruby uses mark & sweep.

It’s hard to free object correctly in both environments.

Now, all Objective-C classes “release” method are overridden. 
It’s an awful bottleneck for performance.

It effects all Cocoa object that are included and not related with mruby object.
It still have problem that it cannot detect circular reference.

I’ll fix at some future. probably I’ll rewrite in assembler.



## mobiruby-common - MobiRuby stack

4th component is mobiruby-common.

it’s for future release when we will release Android  version.


## mobiruby-common

This library provides common utilities among iOS and Android version.

Currently, it provides ‘require’ and ‘load’ method only.
Standard mruby doesn’t support require and load method.


## mobiruby-ios - MobiRuby stack

The last component is mobiruby-ios.


## mobiruby-ios

mobiruby-ios is the main part of MobiRuby.
it provides iOS specific utilities that are included Xcode integration.

In the first version, this component is poor.
I don’t know Xcode deeply.
I need help about Xcode configurations.


## Wrapped APIs

In the feature, MobiRuby will provide good wrapper APIs.
it has some compatibility between iOS and Android.

When you use these APIs, you can use Cocoa APIs at the same time.


## Road map

I don’t have a detailed plan. so it’s my private project.
I don’t have a boss and marketing section.
I aim first production level version until end of Q1 2013.

this version supports almost all Objective-C features.
I plan to start writing API and tutorial documents.

After that, I’ll touch next version. It will have wrapped APIs.
So you can code in Ruby style.


## Current status

I already released MobiRuby game. Apple never reject this app.

In Sept, I finally released MobiRuby alpha version. It’s first public version.
I’m keeping to update. Now I focus on writing test code.
Until recently, I wrote a few tests only.
I used travis-CI from 2 weeks ago. 

I think MobiRuby’s hardest part is Thread.
Because mruby doesn’t support Thread.
I wrote limited thread feature and writing sample code for use cases.

Currently, MobiRuby is not for ordinary Ruby programmers.
If you know Objective-C and have an interest in mruby,
Please join to this project.



## FAQ

I have 3 frequency questions.


### What's different from RubyMotion?

First question, “What’s different from RubyMotion?”

RubyMotion is ...

* LLVM based and compile to native code.
* Only for iOS.
* Extended Ruby syntax
* Not Open source

Maybe we have same goal.
But approach is different.

If you want to make app soon, I recommend to use RubyMotion.
It’s stable and faster than current MobiRuby.


### Can I use RubyGems?

2nd question, “Can I use RubyGems?”

No, mruby doesn’t have compatibility with CRuby ext. APIs.

You need to write new extension for mruby.

bovi and matz has discussed about mrbgems.


### Can I use existing Cocoa libraries?

3rd question, “Can I use existing Cocoa libraries?”

Yes, you can use almost all existing libraries.

I think CocoaPods will be a good partner.
CocoaPods is library manager for Xcode.



## Contributors wanted

I want to team mate. I'm alone now.

If you have interest to this project, please contact to me.


## Questions?

If you have a question, please tweet to @mobiruby or post to github issues.


## Thank you

Thank you very much!