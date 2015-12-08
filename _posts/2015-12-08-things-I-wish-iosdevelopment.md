---
layout: post
title:  "Things I wish I had known before starting iOS development — Part 1"
---
My designer handed me a nifty looking onboarding flow to complete just days before the launching of our app. The flow resembled the Evernote iOS app.

![_config.yml]({{ site.baseurl }}/images/iospost1/evernote.gif)

I jumped straight into coding the flow on Xcode using page view controllers and scroll view. I was able to complete the flow in two days taking help from Stack Overflow and Google. When I showed the flow to a friend who is also an iOS developer, he told me that I could have completed this in an hour if I would have used this [open source project](https://github.com/mamaral/Onboard).
This and many other experiences have certainly turned me into a better iOS engineer over the past one year. I would like to share my experience so that others do not have to repeat the same mistakes on their path to turn into an iOS ninja.

#Focus on the fundamentals

When I started with iOS development I straight away took this free [awesome course](http://web.stanford.edu/class/cs193p/cgi-bin/drupal/) offered by Stanford University. Although I learned a lot from this course, it did not focus much on teaching me the fundamentals of the language used for iOS development which was primarily Objective-C at that time. As I started writing code for my own apps, I realised that I have missed a lot of fundamentals of the language which resulted in buggy code.

If you do not have enough programming experience in an Object Oriented Language, I recommend you to read a good book on the language before diving into iOS App development. My personal favorites are [Big Nerd Ranch Guide](https://www.bignerdranch.com/we-write/objective-c-programming/) for Objective-C and [The Apple’s Guide for Swift](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/).

![_config.yml]({{ site.baseurl }}/images/iospost1/bignerd.jpeg)

#Github is your best friend

I absolutely love the iOS open source community. They have shown their awesomeness with tonnes of great projects like AFNetworking, Restkit, JSQMessages etc. You must learn to take advantage of the work that this community has done in the past.

Before coming up with your own library or solution for a problem search on Github or Google for similar solutions. It is highly likely that a fellow developer has already open sourced a project that might fit your needs.

Feel free to reach out to the community via [Facebook Groups](https://www.facebook.com/profile.php?id=467547859922136) or [Slack chat](https://ios-developers.slack.com/). They will be more than willing to answer your questions. Skim through the code of some good open source projects to learn how the experienced developers organize their code and start following similar design patterns for your own code.

Some of the most amazing iOS resources on Github can be found here.

* [awesome-ios](https://github.com/vsouza/awesome-ios)
* [awesome-swift](https://github.com/matteocrippa/awesome-swift)
* [awesome-ios-ui](https://github.com/cjwirth/awesome-ios-ui)

If you are looking for the iOS best practices to follow in your own projects look at the [following](https://github.com/futurice/ios-good-practices).

#Know your tools
Most iOS developers use Xcode as the primary tool for development. Xcode has a lot of powerful features like Storyboards, Auto Layout which if learned can increase your productivity to great extents. A lot of developers avoid Storyboards due to some limitations, but I personally regard them as a great way to quickly layout your screens.

Learn navigating through XCode using shortcut keys. Though it might seem like a little time saved in the present, it consolidates into a lot of time saved in the future. These are some of the solutions I use consistently that have helped me to speed up my development process.

1. Use [Cocoapods](https://cocoapods.org/) for dependency management. It will make the work of your team much easier.

2. Learn how to setup [continuos integration](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/xcode_guide-continuous_integration/) early in your project so that you do not have to repeat redundant steps.

3. Use [Testflight](https://developer.apple.com/testflight/) for beta distribution of your apps. With Apple buying Testflight, it has become very easy for anyone with a iTunes Connect account to distribute beta builds using Testflight.

4. Integrate [Crashlytics](https://try.crashlytics.com/) in your app to get crash reports as they happen.

5. If you do not want to setup your own backend server go for the amazing service offered by [Parse](https://parse.com/).

#Read some great blogs and newsletters

I already told you about the awesomeness of the iOS open source community. A lot of great blogs have been started by some experience iOS developers which host great content every week. Some of my favorites are:

1. [**Cocoa With Love**](http://cocoawithlove.com/) : Arguably the best iOS Blog by Matt Gallagher. Matt’s way of doing things is nothing short of masterful.

2. [**iOS Dev Weekly**](http://iosdevweekly.com/) : Technically not a blog but insanely awesome updates every week by Dave Verwer.

3. [**NS Hipster**](http://nshipster.com/) : NSHipster is a journal of the overlooked bits in Objective-C and Cocoa. Updated weekly by Mattt Thompson.

4. [**Ray Wenderlich**](http://www.raywenderlich.com/) : Ray Wenderlich’s Blog (Really good for beginners)

Some other notable blogs are [Peter Steinberger](http://petersteinberger.com/), [Matt Gemmell](http://mattgemmell.com/) and [Natash The Robot](http://natashatherobot.com/).

The insights you get by reading these blogs will certainly help you to turn into a better iOS developer.

# Design too can be easy

A lot of us developers tend to run away from the design aspects of iOS. We treat design to be out of our grasp and leave it all to designers. But with a little effort you can learn to design your own apps.

Nowadays the line between the designers and developers is blurring as a lot of indie iOS developers tend to design, develop and market their apps with success. I will talk about the marketing aspects in the next part but if you want to design your own iOS apps, learn the [Sketch tool](http://bohemiancoding.com/sketch/). It is super easy to learn and made specifically for designing apps and websites.

You can find tonnes of Sketch resources and plugins online that will make your work even more fun and easier. Once you have your designs ready you can tie them together using this amazing tool called [Marvel](https://marvelapp.com/).

In the next part, I will talk about the approach you must take while building your own apps as well as the marketing tricks for an iOS app.
