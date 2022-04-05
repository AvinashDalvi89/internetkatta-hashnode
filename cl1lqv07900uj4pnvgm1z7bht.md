## How to solve location policy rejection for play store

Hello Devs, 

I am writing this blog to help others because I spent more than 30 days trying to get back the missing google app from the play store. In the month of Feb 2022 the app was suddenly removed from the play store. I got to know this after 10 days when a customer raised a complaint and QA found an app missing in the play store.  It had a big business impact. This blog is the story from the app’s rejection to approval & the struggles faced to get it finally approved. 

I am not from an android app developer background so struggle was difficult. I used [titanium](https://titaniumsdk.com/) to build native app. Titanium is a framework built to create both iOS and Android apps using Javascript code ( code can be shared ). To read about Titanium see bottom reference link. 

Android app was rejected by Google Play store citing reasons for not adhering to their [location policy](https://support.google.com/googleplay/android-developer/answer/9799150?hl=en-GB#zippy=)
![app_removal_notice.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649052179275/P96HrPtcm.png)
![policy-page.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649052294646/3sxJxgOTU.png)

me and my colleague started investigating into the sudden disappearance of app from play store, got to see a removal mail and status on play store mentioning android app was removed due to location permission issue.

### Reason why app got removed 
Google updated its latest location policy. Though our app in fact does not use device background location, with this policy update showed signs of using background location which is not adhering to play store rules. This requires explicit mentioning of apps not using background location to users.

### Action 1 - Validate if app uses background location in any scenario : 

Check if the app uses any of the background locations by trying different scenarios. Like:

- Clicking on ‘locate me’ option and then moving 1KM from current location 
- Keeping the app open in the background and seeing whether the app is automatically updating location or not.

On confirming the app is not using background location, a new build with the same code was generated and uploaded again (we never did any recent release of the app, our previous app release was long months ago). 

24th Feb 2022:  Uploaded a new build (X.X.X.277 - build number YYY). This got rejected with the same reason - app not adhering to location policy with no further details. By these two rejections, we got clarity on two areas to look into - one's location policy and other is developer programme policies. 
![second-rejection.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649053290215/qmcJd2ZxQ.png)


### Action 2: Submission of developer & location policy

We submitted 
1. [Developer programme policy](https://play.google.com/about/developer-distribution-agreement.html) Google Play and
2. Location policy form
3. Our android app usage on these policies
4. Explaining how & why location feature is required in android app, with submitting documents of mobile demo page

25th feb & 28th Feb : A series of trials and rejections happened. In these trials, new builds were not generated, we chose to resubmit old builds with explanations on app usage of background location as mentioned in Action 1.

![third-rejection.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649053440572/SVWEZEqTq.png)

**Login credentials missing**: It was found that the login credential given in app content is missing. Submitted a sample login credential in play store under Play console → App content → App access → Manage 

**Note** : This action might be optional and vary case to case but make sure if this causes the same for you or not. The Google team was taking wrong credentials after giving the right details. 

![login-credentials-missing.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649053591390/-2wC9kkvS.png)

Then I decided to deep dive more into their policy statements to see what is expected & where our app is missing details 
![location-policy-guide.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649053784345/ZDdPpLHx6.png)

![location-policy-guide-2.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649053814986/EVyPVeDaS.png)

### Action 3: 

Tried reaching out to contact support over chat and mail right from the start of this issue, but no response. We are allowed to communicate only through developer console chat. No details provided on what is exactly wrong in the app etc, other than the same repeat statement ‘app not adhering to policy’. 

Also in this period,  we found the play store team is trying to access wrong credentials (typo error) due to which app got rejected again. 

 
### Action 4 - Declaring no background usage: 

With learnings on policies due to deep dive understanding, we wanted to declare ‘no’ for background usage under the location policy page & update the location permission policy form to foreground use only. 

For this, we tried accessing background location in the app but it wont work. Tied the same with 2 kms distance & location won't update automatically. 

Tried clicking the location button -  it didn't work the first time, then hit this same button twice after which location got updated. The location permission policy form should be updated to foreground use only. 

We declared NO for background usage under the location policy page and resubmitted the same build. 

Play store → App content → Sensitive permissions and APIs → Manage  

![location-permission-choice.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649054975495/FUboE7ImG.png)

![rejection-mail-4.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649054988885/9tyIdL-E0.png)


### Action 5 - Explicit mention of not using Background location: 

We redeclared location policy to not using background location,  with more details on why location is one feature in the app. With added details on what our app exactly does with location data and explicit mention of not using `ACCESS_BACKGROUND_LOCATION`

**Code change**: Also added in code remove node if there for `ACCESS_BACKGROUND_LOCATION` Permission review Google Play for access to background location with reference from StackOverflow answer in `tiapp.xml` along with this changes we added  `<override-permissions>true</override-permissions>` in tiapp.xml after `</deployment-targets>`


```
<uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" 
  tools:node="remove" />
```
Permission updates: Explicitly added only those API permissions which required access location. Titanium works anywhere in code titanium.geolocation api to get geo location data.  While creating build it will automatically add in AndroidManifest.xml file 

```
<uses-permission
        android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission
        android:name="android.permission.ACCESS_COARSE_LOCATION" />
```

Again the app got rejected on 8th March 2022. 

### Action 6 - Titanium support: 

We had a long discussion with the Titanium dev support team over [titanium slack](https://tidev.slack.com/). [Safer and More Transparent Access to User Location](https://android-developers.googleblog.com/2020/02/safer-location-access.html) 
After checking all possibilities of background location issues in the app. Background location access checklist : [Access location in the background  |  Android Developers](https://developer.android.com/training/location/background) 

**Following are the observations**:

- Checked for `ACCESS_BACKGROUND_LOCATION` as we are not using this API ( Search through all code and build code plus Android manifest file) .
- Every time the locate button is clicked, the location details are deregistering the Location event. No longer is the location event alive.
- As per titanium framework also we are not using `<service>` tag which is used for background location.
- Only we are using `ACCESS_COARSE_LOCATION` and `ACCESS_FINE_LOCATION` but it doesn't cause background location use. When the app is not used or killed it doesn't ask for background location. This API is fine to use.
- Uploaded new build 410 (1.9.0.304) in play store ( production API pointing )
- Submitted for developer form for login details into app
- Submitted again location policy changes are the same as before.
- Wrote separate mail to google support team as below screenshot.Next set of action planned :
-Tried debugging app using `adb logcat` running connected to your device, have the app go into background mode and trigger a location update. If you don't see a your log in the update, then that's not running in the background;
- Tried using https://f-droid.org/packages/io.github.muntashirakon.AppManager/ this app manager to see any hidden permission in code. 

Situation was again the same rejection on 9th March 2022. 

### Action 7 - Demo submission on app not using background location: 

We recorded a new video with more explanation about location usage and how we are not using background location. Submitted app. Again rejection. 

### Action 8 - location permission : 

We went through Google Location policy video multiple times [Google Play Policy - Declared permissions](https://www.youtube.com/watch?time_continue=3&v=b0I1Xq_iSK4&feature=emb_logo) and in-app disclosures then decided to make changes in app about message and adding inside app asking for permission instead just saying “Geolocation is disabled” 

Replace existing dialog message to below message which was explicitly saying why location permission is required. 

![Screenshot 2022-04-04 at 12.44.59 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649056538803/J1V35P7Og.png)
added this new function to give in app permission after the first popup came for accept and deny.

![Screenshot 2022-04-04 at 12.45.18 PM.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649056545257/vXIVKUQCS.png)


```
getLocationPermission = function (callback){
 var permissions = [
        "Titanium.Geolocation.AUTHORIZATION_WHEN_IN_USE",
        "Titanium.Geolocation.AUTHORIZATION_ALWAYS",
    ];
var whenInUse = Titanium.Geolocation.hasLocationPermissions(Titanium.Geolocation.AUTHORIZATION_WHEN_IN_USE);
var whenAlways = Titanium.Geolocation.hasLocationPermissions(Titanium.Geolocation.AUTHORIZATION_ALWAYS);

	if (whenAlways || whenInUse) {
		Titanium.API.info("has permission");
		callback();
		return;
	}
	Titanium.Geolocation.requestLocationPermissions(permissions, function(e) {
        console.log(e);
		if (e.success) {
			// yes
			Titanium.API.info("ok")
			callback();
		} else if (OS_ANDROID) {
			// no
		} else {
			// no
		}
	});
}
``` 
**Note** : This code reference may different framework to framework.  This is sample input to find in your code. 

Also replaced this line `Titanium.Geolocation.addEventListener("location", this.handleLocationEvent);` to   `Titanium.Geolocation.getCurrentPosition(this.handleLocationEvent)` to avoid any possibility due to listener activity. `this.handleLocationEvent` is a custom function to get location data and send it back.  

With above changes uploaded next build number on 14th March 2022. But after this many changes still got rejected on 15th March 2022. 


### Action 9 - removing an older build : 

I had word with the manager about this rejection. He pointed out one important thing which I had ignored -Along with one build number ( example 123 build another build 124 )  there was another build which got removed the first time. 

For that build ( Open testing track ) we got a good response as per mail. Assuming this is not an issue and considering this I submitted the final build number to the production track.

This time we again got rejection mail for latest build but something was different in their messaging. In rejection mail they didn’t mention any build number 

### Action 10: Update Location permission policy to ‘NO’: 

This above message gave us some hint towards the approval process but meanwhile we tried to reach out to many google android dev experts and get suggestions but whichever they mention everything was implemented already. Also tried to reach the google team by taking network help. 

Along with this, Submitted Location permission policy to No. In our understanding their message only mentioned build number and open testing track build was approved with no issue. This gave a hint that only this action might get us approval. 

![location-no.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1649138219103/lotVU9OWu.png)

Big YES. Hurray we got approval mail from google play team. 

![popeye-thumbs-up.gif](https://cdn.hashnode.com/res/hashnode/image/upload/v1649138292034/yJDaAJTtv.gif)

## Summary : 

Android app got rejected because it was not giving explicit messages to users while asking for location permission. We just ask that the user Geolocation is currently disabled. Enable geolocation for this app to use this feature. This alone was not enough to provide clarity on questions such as 
- Why does the app request access to location ? 
- What happens after enabling the location feature? 
- How is this location data used?

Along with this we suspect listener activity was causing background location access ( There is no foolproof answer on this ). You can try to remove the listener and upload the build. 

As part of final approved build number we changed to get `Titanium.Geolocation.getCurrentLocation` from `Titanium.Geolocation.addEventListener("location", this.handleLocationEvent);` 

Also added two popup messages as mentioned in Action 8 to give more explicit messages to users and asking permission in the app. These actions got approval for the app in the play store.  Try to see this video multiple times and implement similar things will save your time. 
<iframe width="560" height="315" src="https://www.youtube.com/embed/b0I1Xq_iSK4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Hope it will solve your issue if you come across a similar problem. Feel free to comment if you have any issues. If you like my blog please don't forget to like the article. It will encourage me to write more helpful articles. You can reach out to me over my twitter handle @aviboy2006

### References : 

- [Code reference to add dialog box in titanium](https://titaniumsdk.com/api/titanium/ui/alertdialog.html#three-button-alert-dialog) 
- [Fix suggested from titanium side](https://jira-archive.titaniumsdk.com/TIMOB/TIMOB-28049) 
- [Titanium framework](https://titaniumsdk.com/)
