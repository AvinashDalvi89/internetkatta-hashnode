## How to add more simulator in Xcode 12 ?

Today's I came across an issue while upgrading Xcode 11 to 12. Not able to find an option to add devices which support iOS older versions like 12,13 etc. Because Xcode 12 came with iOS 14 default version. When we selected to add a new Simulator then the iOS option was coming only iOS 14 no other option to add. While doing google found one  [StackOverflow question](https://stackoverflow.com/questions/64075441/xcode-12-download-more-simulators-runtime-is-empty/65413382#65413382) which was having same issue. But all of the answers do not look straight forward. 

So, I tried the simplest thing which generally all apps have an option to search inside the app. Below are steps to do the same. 


1.  Open Xcode 12 app
2.  Go to menu -> Xcode -> Preferences 
![Preferences](https://cdn.hashnode.com/res/hashnode/image/upload/v1608660828088/C8iJReiBN.png)
3.  Go to component tab shown like below picture 👇🏻
![Component](https://cdn.hashnode.com/res/hashnode/image/upload/v1608661664791/xHkHTkFUA.png)
4. Then start installing whichever iOS version would like to install. Once its install iOS version
5. To add a simulator. Go to -> Xcode menu -> Open Developer tool -> Simulator 
6.  Simulator menu -> File -> New Simulator. It will show a list of iOS available for the device. 
![Screenshot 2020-12-22 at 11.47.02 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1608661033370/erBZc6hv0.png)
7. Select Device name -> Select Device model -> iOS version -> Create Simulator.


References : 
https://stackoverflow.com/questions/64075441/xcode-12-download-more-simulators-runtime-is-empty/65413382#65413382

Hope this helps you. Thanks for reading this blog. If any feedback feel free to reach out to me.