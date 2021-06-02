<h1>Background synchronisation</h1>

SwissCovid Check-ins have to synchronize for potential matches regularly in the background due to the decentralized architecture. Even if the user does not open the app, the app should periodically download the list of traceKeys from the backend and check them for potential matches. However, background processes are unreliable on mobile phones, especially on iOS. They might be delayed by moren than a day, or not run at all. As long as ExposureNotifications are activated, the frameworks of Apple and Google make sure that the SwissCovid app can run every two hours in the background. As soon as ExposureNotifications are turned off, the app looses this privilege and therefore need a separate mechanism for users using only the SwissCovid Check-in functionality.

<h1>Android</h1>

On Android for the background synchronisation the normal WorkManager API is used, if ExposureNotifications are enabled or not. In addition the user is asked to disable battery optimizations for the SwissCovid app to prevent the app from being put into app standby mode. Some vendors have additional more aggressive power optimization mechanisms, but most of them were deactivated for the SwissCovid app specifically. As long as ExposureNotifications are active, the [Google Play Services](https://developers.google.com/android/exposure-notifications/release-notes#new_six-hour_wakeupservice_cadence) make sure every six hours that our app is running or restart it if it was killed. We can not provide this guarantee if ExposureNotifications are deactivated, but if the user is using the SwissCovid check-in feature, the app has to be opened from time to time.

<h1>iOS</h1>

On iOS several methods are used to unsure background processing time. If ExposureNotification is turned on we can rely on the scheduling of background processing tasks by apple. In the case where check-ins are used and ExposureNotifications are turned off we have to use silent push notifications to guarantee a regular synchronization of trace keys.

## Background processing on iOS

SwissCovid uses two different mechanisms to schedule background processes depending on the iOS version:

* On devices running iOS 13 and above, Apple's [BackgroundTasks framework](https://developer.apple.com/documentation/backgroundtasks) is used. It allows to schedule tasks that will opportunistically be executed in the background, depending on various conditions such as frequency of app usage, battery level and network connectivity. There is no guarantee that tasks will reliably be scheduled. To maximize chances of execution, SwissCovid schedules both [BGProcessingTasks](https://developer.apple.com/documentation/backgroundtasks/bgprocessingtask) *and* [BGAppRefreshTasks](https://developer.apple.com/documentation/backgroundtasks/bgapprefreshtask), both of which trigger a sync with the server to download new data and locally check for new exposures.
* On devices running iOS 12 and below, the BackgroundTasks framework is not available. Apple [provides a mechanism](https://developer.apple.com/documentation/exposurenotification/supporting_exposure_notifications_in_ios_12_5) for these devices to receive background execution time. This is only usable if ExposureNotification is enabled.

These two mechanisms are only reliable if ExposureNotification is enabled.

## Silent Pushes
For users that have ExposureNotifications deactivated, the background tasks are not reliable enough. Therefore, SwisCovid uses in this case a different, more reliable mechanism of triggering syncs: silent pushes. Push messages are sent by the server and will always result in processing time being allocated to the app. Thereby ensuring that the app will always perform a timely sync with the server and can check for notifications.

### Apple Push Notification Service
Apple provides the Apple Push Notification Service to send messages to iPhones. The architecture overview of Apple's Push Notification service can be found [here](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html#//apple_ref/doc/uid/TP40008194-CH8-SW1). Apps can ask the system to be registered with APNs. Once an app does, it receives an app-specific push token that it can then send to the app backend server. Usually, apps also send a unique app-instance token that is randomly generated upon app installation, identifying a specific app instance. The backend can then use the push token to send push payloads to APNs, which in turn will send a push notification to the registered device.

### What data is sent via push
SwissCovid uses pushes just as a mechanism to trigger a sync request. _SwissCovid pushes do not to actually send  data_.  The payload of the push is empty. Moreover, the push is configured to be "silent", meaning that it is delivered to the device without triggering a visible or audible notification. Upon receiving the push, the app downloads new data from the server and checks for possible exposures. If it finds any, it then triggers a new, local notification that alerts the user and prompts them to open the app.

A push payload looks like this:
```
{
    "aps" : {
        "content-available": 1
    }
}
```

### When does the SwissCovid app register for silent pushes

As we only need the silent push feature as long as the user is using the check-in feature but has ExposureNotifications deactivated, the app only registers those users to receive silent pushes. The logic for registering and de-registering is as follows:

#### Registration

* Upon check-out (creation of a new check-in entry) the app checks if ExposureNotifications are deactivated. If so, the app registers itself for receiving silent pushes, unless it is already registered.

* If the user manually deactivates the ExposureNotifications from the SwissCovid app, the app checks if there are check-ins stored. If so, the app registers itself for receiving silent pushes.

* On every app start, the app checks if there are check-ins stored, ExposureNotifications are deactivated and the user is not in the isolation state (where ExposureNotifications are always deactivated). If so, the app registers itself for receiving silent pushes, unless it is already registered or the registration is older than two weeks.

#### De-Registration

* If the user manually activates the ExposureNotifications from the SwissCovid app, the app de-registers itself for receiving silent pushes, if it was currently registered.

* On every app start, the app checks if ExposureNotifications are activated. If so and the app is registered for silent pushes, it de-registers itself.


### Data stored on SwissCovid servers
For each device that registers for push notifications, the SwissCovid backend stores the push token used to authenticate with APNs. The server uses the push token to send push notifications to the app.

For each push token, the server also stores a random app-instance token. This token is also stored inside the app. No other apps have access to this app-instance token. The app-instance token is used only for updating or deleting the push token associated with an app. Updates are needed when Apple for some reason invalidates the old push token. The app then sends the new push token and the app-instance token to server. Finally, the app-instance token is used to unregister the app from push notifications.

The app-instance token is not communicated to the backend server at any other time  and can therefore not be used to track users. No other data is stored related to apps and users on the SwissCovid servers.
