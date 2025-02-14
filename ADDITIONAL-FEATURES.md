# Additional features of the Thunderhead SDK

## Table of Contents

* [Opt an end-user out of or into tracking](#opt-an-end-user-out-of-or-into-tracking)
  * [Opt an end-user out/in of all tracking](#opt-an-end-user-outin-of-all-tracking)
    * [Opt an end-user out of all tracking](#opt-an-end-user-out-of-all-tracking)
    * [Opt an end-user in for all tracking](#opt-an-end-user-in-for-all-tracking)
  * [Opt an end-user out/in of city/country level tracking](#opt-an-end-user-outin-of-citycountry-level-tracking)
    * [Opt an end-user out of city/country level tracking](#opt-an-end-user-out-of-citycountry-level-tracking)
    * [Opt an end-user in to city/country level tracking](#opt-an-end-user-in-to-citycountry-level-tracking)
* [Exclude an Interaction](#exclude-an-interaction) 
* [How to disable the codeless identity transfer support](#how-to-disable-the-codeless-identity-transfer-support)
* [Disable automatic Interactionxa detection](#disable-automatic-interaction-detection)
* [Programmatic Interactions and Properties API](#programmatic-interactions-and-properties-api)
    * [Send Interactions](#send-interactions)
        * [Send an Interaction request](#send-an-interaction-request)
        * [Send an Interaction request and retrieve the response](#send-an-interaction-request-and-retrieve-the-response)
    * [Send Properties](#send-properties)
        * [Send an Interaction request with Properties](#send-an-interaction-request-with-properties)
        * [Send Properties to a base Touchpoint](#send-properties-to-a-base-touchpoint)
    * [Send a response code](#send-a-response-code)
* [Retrieve a response for an automatically triggered Interaction request](#retrieve-a-response-for-an-automatically-triggered-interaction-request)
    * [Example: Retrieve a response for an automatic Activity Interaction](#example-retrieve-a-response-for-an-automatic-activity-interaction)
    * [Example: Retrieve a response for an automatic Fragment Interaction](#example-retrieve-a-response-for-an-automatic-fragment-interaction)
    * [Example: Retrieve a response for a manually assigned Interaction](#example-retrieve-a-response-for-a-manually-assigned-interaction)
    * [Optional response processing](#optional-response-processing)
* [Assign an Interaction to a View](#assign-an-interaction-to-a-view)
* [Ability to include identity transfer links](#ability-to-include-identity-transfer-links)
* [Ability to exclude identity transfer links](#ability-to-exclude-identity-transfer-links)
* [Disable automatic identity transfer](#disable-automatic-identity-transfer)
    * [Send deep link properties programmatically](#send-deep-link-properties-programmatically)
    * [Create a `URL` with a `one-tid` parameter to facilitate identity transfer](#create-a-url-with-a-one-tid-parameter-to-facilitate-identity-transfer)
    * [Create an `android.net.Uri` or `java.net.URI` with a `one-tid` parameter to facilitate identity transfer](#create-an-androidneturi-or-javaneturi-with-a-one-tid-parameter-to-facilitate-identity-transfer)
* [Disable automatic outbound link tracking](#disable-automatic-outbound-link-tracking)
    * [Programmatically trigger an outbound link tracking Interaction call](#programmatically-trigger-an-outbound-link-tracking-interaction-call)
* [Send a location object](#send-a-location-object)
* [Get Tid](#get-tid)
* [Configuring Logging](#configuring-logging)
    * [Turn all logs on](#turn-all-logs-on)
    * [Turn specific logs on](#turn-specific-logs-on)
    * [Turn logs for the Thunderhead SDK initialization process on](#turn-logs-for-the-thunderhead-sdk-initialization-process-on)
    * [Turn all logs off](#turn-all-logs-off)
* [Identify the SDK version](#identify-the-sdk-version)
* [Clear the user profile](#clear-the-user-profile)



### Opt an end-user out of or into tracking
The following methods allow you to opt a user out of various levels of tracking and also opt them back in based on your app's privacy configuration.

_Note_:
 * By default all `optOut` options are set to `optOut = false`


#### Opt an end-user out/in of all tracking
Use this option to opt an end-user out or in of all tracking

##### Opt an end-user out of all tracking
To opt an end-user out of all tracking, when the end-user does not give permission to be tracked in the client app, call the `oneConfigureOptOut` top-level Kotlin function or the `One.setOptOutConfiguration` Java method as shown below:

`Kotlin`
```kotlin
import com.thunderhead.mobile.oneConfigureOptOut

oneConfigureOptOut {
    optOut = true
}
```

`Java`
```java
import com.thunderhead.mobile.One;
import com.thunderhead.mobile.optout.OneOptOutConfiguration;

final OneOptOutConfiguration optOutConfiguration = new OneOptOutConfiguration.Builder()
            .optOut(true)
            .build();

One.setOptOutConfiguration(optOutConfiguration);
```

*Note:*
- When a user is opted out, tracking stops and locally queued data is removed.
- For instructions on completely removing a user's data from Thunderhead ONE or Salesforce Interaction Studio, see our [API Documentation](https://thunderheadone.github.io/one-api/#operation/delete).

##### Opt an end-user in for all tracking
To opt an end-user back in to all tracking, call the `oneConfigureOptOut` top-level Kotlin function or the `One.setOptOutConfiguration` Java method as shown below:

`Kotlin`
```kotlin
import com.thunderhead.mobile.oneConfigureOptOut

oneConfigureOptOut {
    optOut = false
}
```

`Java`
```java
import com.thunderhead.mobile.One;
import com.thunderhead.mobile.optout.OneOptOutConfiguration;

final OneOptOutConfiguration optOutConfiguration = new OneOptOutConfiguration.Builder()
            .optOut(false)
            .build();

One.setOptOutConfiguration(optOutConfiguration);
```

#### Opt an end-user out/in of city/country level tracking

Use the following options to opt an end-user out or in of all city/country level tracking.

##### Opt an end-user out of city/country level tracking
Examples of how to opt out of city/country level tracking

`Kotlin`
```kotlin
import com.thunderhead.mobile.oneConfigureOptOut
import com.thunderhead.mobile.optout.OneOptInOptions

val options = EnumSet.noneOf(OneOptInOptions::class.java)
oneConfigureOptOut {
    optOut = false
    optInOptions = options
}
```

`Java`
```java
import com.thunderhead.mobile.One;
import com.thunderhead.mobile.optout.OneOptOutConfiguration;
import com.thunderhead.mobile.optout.OneOptInOptions;

Set<OneOptInOptions> options = EnumSet.noneOf(OneOptInOptions.class);
final OneOptOutConfiguration optOutConfiguration = new OneOptOutConfiguration.Builder()
            .optOut(false)
            .optInOptions(options)
            .build();

One.setOptOutConfiguration(optOutConfiguration);
```

*Note:*
- By default a user is opted in and would need to be specifically opted out using the method mentioned above, depending on your specific privacy requirements.
- When a user is opted out, all opt in options are ignored.

##### Opt an end-user in to city/country level tracking
Examples of how to opt in to city/country level tracking

`Kotlin`
```kotlin
import com.thunderhead.mobile.oneConfigureOptOut
import com.thunderhead.mobile.optout.OneOptInOptions

val options = EnumSet.noneOf(OneOptInOptions::class.java)
options.add(OneOptInOptions.CITY_COUNTRY_DETECTION)
oneConfigureOptOut {
    optOut = false
    optInOptions = options
}
```

`Java`
```java
import com.thunderhead.mobile.One;
import com.thunderhead.mobile.optout.OneOptOutConfiguration;
import com.thunderhead.mobile.optout.OneOptInOptions;

Set<OneOptInOptions> options = EnumSet.noneOf(OneOptInOptions.class);
options.add(OneOptInOptions.CITY_COUNTRY_DETECTION);
final OneOptOutConfiguration optOutConfiguration = new OneOptOutConfiguration.Builder()
            .optOut(false)
            .optInOptions(options)
            .build();

One.setOptOutConfiguration(optOutConfiguration);
```

### Exclude an Interaction

Exclude a specific view from being automatically recognized as an Interaction, using the `excludeAutomaticInteraction` Kotlin extension function or `One.excludeAutomaticInteraction` Java method in an Activity's `onCreate` method or a Fragment's `onCreateView`, as shown below.

`Kotlin`
```kotlin
import com.thunderhead.mobile.excludeAutomaticInteraction
// rest of imports

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    findViewById<LinearLayout>(R.id.root)
        .excludeAutomaticInteraction()
}
```

`Java`
```java
import com.thunderhead.mobile.interactions.OneAutomaticInteractionExclusion;
// rest of imports
@Override
public View onCreateView(LayoutInflater inflater, ViewGroup container,
                         Bundle savedInstanceState) {
    rootView = inflater.inflate(R.layout.fragment_layout_2, null);
    final OneAutomaticInteractionExclusion rootViewExclusion = new OneAutomaticInteractionExclusion.Builder()
                        .view(rootView)
                        .build();
    One.excludeAutomaticInteraction(rootViewExclusion);
    return rootView;
}
```

### How to disable the codeless identity transfer support

To completely remove the codeless identity transfer functionality for Android, make the following updates:

1. Open the **top-level** `build.gradle` file and remove the following dependency reference.

```gradle 
classpath 'com.thunderhead.android:orchestration-plugin:6.0.1'
```

2. Open the **app-level** `build.gradle` file and remove the following references.

```gradle 
apply plugin: 'com.thunderhead.android.orchestration-plugin'
```

### Disable automatic Interaction detection

You can disable automatic Interaction detection by calling the `oneConfigureCodelessInteractionTracking` Kotlin top-level function or
the `One.setCodelessInteractionTrackingConfiguration` Java method with the appropriate configuration, as shown below:

`Kotlin`
```kotlin
import com.thunderhead.mobile.oneConfigureCodelessInteractionTracking

oneConfigureCodelessInteractionTracking {
    // disables Fragment/Activity Interaction Tracking
    disableCodelessInteractionTracking = true 
    // disables WebView URL Interaction Tracking
    disableWebViewInteractionTracking = true
    // disables Outbound Link Tracking
    disableOutboundLinkTracking = true
}
```

`Java`
```java
import com.thunderhead.mobile.One;
import com.thunderhead.mobile.codeless.OneCodelessInteractionTrackingConfiguration;

final OneCodelessInteractionTrackingConfiguration codelessInteractionTrackingConfiguration =
    new OneCodelessInteractionTrackingConfiguration.Builder()
        // disables Fragment/Activity Interaction Tracking
        .disableCodelessInteractionTracking(true)
        // disables WebView URL Interaction Tracking
        .disableWebViewInteractionTracking(true)
        // disables Outbound Link Tracking
        .disableOutboundLinkTracking(true)
        .build();
One.setCodelessInteractionTrackingConfiguration(codelessInteractionTrackingConfiguration);
```

When automatic Interaction detection is disabled, the SDK does not automatically send Interaction requests.
You must send Interaction requests manually, as and when needed, using the methods provided in the sections below.

Set this back to `false` at any point to restart automatic Interaction detection.

### Programmatic Interactions and Properties API

You can manually send Interaction Requests and Properties to ONE or Interaction Studio using
the `oneSendInteraction` Kotlin top-level function or the `One.sendInteraction` Java method.

The `oneSendInteraction` Kotlin top-level function uses Kotlin [Coroutines](https://kotlinlang.org/docs/reference/coroutines-overview.html). The `One.sendInteraction` Java method uses the Java `Future` model and returns a [Completable Future](https://developer.android.com/reference/java/util/concurrent/CompletableFuture).

#### Send Interactions

##### Send an Interaction request

Send an Interaction request programmatically by calling the `oneSendInteraction` Kotlin top-level function in a Coroutine, as shown below:

`Kotlin`
```kotlin
import com.thunderhead.mobile.interactions.OneInteractionPath
import com.thunderhead.mobile.oneSendInteraction
// rest of imports

scope.launch {
    oneSendInteraction {
        interactionPath = OneInteractionPath(URI("/interactionPath"))
    }
}
```

To capture errors, set the `throwErrors` parameter to `true` and wrap the method in a `try/catch` block, as shown below:

```kotlin
import com.thunderhead.mobile.interactions.OneInteractionPath
import com.thunderhead.mobile.oneSendInteraction
import com.thunderhead.mobile.responsetypes.OneAPIError
import com.thunderhead.mobile.responsetypes.OneSDKError
// rest of imports

scope.launch {
    try {
        oneSendInteraction(throwErrors = true) {
            interactionPath = OneInteractionPath(URI("/interactionPath"))
        }
    } catch (error: OneSDKError) {
        Log.e(TAG, "SDK Error: ${error.errorMessage}")
    } catch (error: OneAPIError) {
        Log.e(TAG, "Api Error: ${error.errorMessage}")
    }
}
```

Send an Interaction request programmatically by calling the `One.sendInteraction` Java method, as shown below:

`Java`
```java
import com.thunderhead.mobile.One;
import com.thunderhead.mobile.interactions.OneInteractionPath;
import com.thunderhead.mobile.interactions.OneRequest;
// rest of imports

final OneRequest sendInteractionRequest = new OneRequest.Builder()
        .interactionPath(new OneInteractionPath(URI.create("/interactionPath")))
        .build();
One.sendInteraction(sendInteractionRequest);
```

*Note:*
- Sends a `POST` request to Thunderhead ONE or Salesforce Interaction Studio. Only the `tid` from the response isused by the SDK; all other response objects are ignored.
- When sending Interaction requests programmatically please ensure the Interaction starts with a `/` and contains only letters, numbers, and/or dashes.
- When in Java be sure to perform the interaction request in a thread that is NOT the `Main` thread.

##### Send an Interaction request and retrieve the response

Send an Interaction request programmatically, access its response, and then process that response by calling the `oneSendInteraction` Kotlin top-level function in a Coroutine, as shown below:

`Kotlin`
```kotlin
import com.thunderhead.mobile.interactions.OneInteractionPath
import com.thunderhead.mobile.oneSendInteraction
import com.thunderhead.mobile.process
// rest of imports

scope.launch {
    val response = oneSendInteraction {
        interactionPath = OneInteractionPath(URI("/interactionPath"))
    }
    response?.process()
}
```

To capture errors, set the `throwErrors` parameter to `true` and wrap the method in a `try/catch` block, as shown below:

```kotlin
import com.thunderhead.mobile.interactions.OneInteractionPath
import com.thunderhead.mobile.oneSendInteraction
import com.thunderhead.mobile.process
import com.thunderhead.mobile.responsetypes.OneAPIError
import com.thunderhead.mobile.responsetypes.OneSDKError
// rest of imports

scope.launch {
    try {
        val response = oneSendInteraction(throwErrors = true) {
            interactionPath = OneInteractionPath(URI("/interactionPath"))
        }
        response?.process()
    } catch (error: OneSDKError) {
        Log.e(TAG, "SDK Error: ${error.errorMessage}")
    } catch (error: OneAPIError) {
        Log.e(TAG, "Api Error: ${error.errorMessage}")
    }
}
```

Send an Interaction request programmatically and process the response by calling the `One.sendInteraction` Java method and enqueue with a callback, as shown below:

`Java`
```java
import com.thunderhead.mobile.One;
import com.thunderhead.mobile.interactions.OneInteractionPath;
import com.thunderhead.mobile.interactions.OneRequest;
import com.thunderhead.mobile.responsetypes.OneAPIError;
import com.thunderhead.mobile.responsetypes.OneSDKError;
import com.thunderhead.mobile.responsetypes.OneResponse;
// rest of imports

final OneRequest sendInteractionRequest = new OneRequest.Builder()
        .interactionPath(new OneInteractionPath(URI.create("/interactionPath")))
        .build();

try {
    final OneResponse response = One.sendResponseCode(true, sendInteractionRequest).join();
    One.processResponse(response);
} catch (CompletionException error) {
    Log.e(TAG, error.getCause());
} catch(OneSDKError error) {
    Log.e(TAG, error.getErrorMessage());
} catch(OneAPIError error) {
    Log.e(TAG, error.getErrorMessage());
}
```

The response can be passed to the `One.processResponse` method, as shown above. This method returns the response to the SDK to process, attaching any activity capture, attribute capture, or optimize instructions to the interaction.

*Note:*
- Sends a `POST` request to Thunderhead ONE or Salesforce Interaction Studio.
- When sending Interaction requests programmatically, please ensure the Interaction starts with a `/` and contains only letters, numbers, and/or dashes.
- When in Java be sure to perform the interaction request in a thread that is NOT the `Main` thread.

#### Send Properties

Properties in the form of key/value pair strings can be sent to Thunderhead ONE or Salesforce Interaction Studio using the SDK's public methods.

##### Send an Interaction request with Properties

Send an Interaction request with Properties, programmatically, and ignore the response
by calling the `oneSendInteraction` Kotlin top-level function in a Coroutine, as shown below:

`Kotlin`
```kotlin
import com.thunderhead.mobile.interactions.OneInteractionPath
import com.thunderhead.mobile.oneSendInteraction
// rest of imports

scope.launch {
    oneSendInteraction {
        interactionPath = OneInteractionPath(URI("/interactionPath"))
        properties = mapOf("keyA" to "valueA", "keyB" to "valueB")
    }
}
```

To capture errors, set the `throwErrors` parameter to `true` and wrap the method in a `try/catch` block, as shown below:

```kotlin
import com.thunderhead.mobile.interactions.OneInteractionPath
import com.thunderhead.mobile.oneSendInteraction
import com.thunderhead.mobile.responsetypes.OneAPIError
import com.thunderhead.mobile.responsetypes.OneSDKError
// rest of imports

scope.launch {
    try {
        oneSendInteraction(throwErrors = true) {
            interactionPath = OneInteractionPath(URI("/interactionPath"))
            properties = mapOf("keyA" to "valueA", "keyB" to "valueB")
        }
    } catch (error: OneSDKError) {
        Log.e(TAG, "SDK Error: ${error.errorMessage}")
    } catch (error: OneAPIError) {
        Log.e(TAG, "Api Error: ${error.errorMessage}")
    }
}
```

Send an Interaction request with Properties, programmatically, and ignore the response
by calling the `One.sendInteraction` Java method, as shown below:

`Java`
```java
import com.thunderhead.mobile.One;
import com.thunderhead.mobile.interactions.OneCall;
import com.thunderhead.mobile.interactions.OneInteractionPath;
import com.thunderhead.mobile.interactions.OneRequest;
// rest of imports

final Map<String, String> properties = new HashMap<>();
properties.put("keyA", "valueA");
properties.put("keyB", "valueB");

final OneRequest sendInteractionRequest = new OneRequest.Builder()
        .interactionPath(new OneInteractionPath(URI.create("/interactionPath")))
        .properties(properties)
        .build();

try {
    One.sendInteraction(sendInteractionRequest);
} catch (ExecutionException e) {
    e.printStackTrace();
}
```

##### Send Properties to a base Touchpoint

Send Properties programmatically and ignore the response by calling the `oneSendProperties` Kotlin top-level function in a Coroutine, as shown below:

`Kotlin`
```kotlin
import com.thunderhead.mobile.interactions.OneInteractionPath
import com.thunderhead.mobile.oneSendProperties
// rest of imports

scope.launch {
    oneSendProperties {
        properties = mapOf("keyA" to "valueA", "keyB" to "valueB")
    }
}
```

To capture errors, set the `throwErrors` parameter to `true` and wrap the method in a `try/catch` block, as shown below:

```kotlin
import com.thunderhead.mobile.interactions.OneInteractionPath
import com.thunderhead.mobile.oneSendProperties
import com.thunderhead.mobile.responsetypes.OneAPIError
import com.thunderhead.mobile.responsetypes.OneSDKError
// rest of imports

scope.launch {
    try {
        oneSendProperties(throwErrors = true) {
            properties = mapOf("keyA" to "valueA", "keyB" to "valueB")
        }
    } catch (error: OneSDKError) {
        Log.e(TAG, "SDK Error: ${error.errorMessage}")
    } catch (error: OneAPIError) {
        Log.e(TAG, "Api Error: ${error.errorMessage}")
    }
}
```

Send Properties programmatically and ignore the response by calling the `One.sendProperties` Java method, as shown below:

`Java`
```java
import com.thunderhead.mobile.One;
import com.thunderhead.mobile.interactions.OneRequest;
// rest of imports

final Map<String, String> properties = new HashMap<>();
properties.put("keyA", "valueA");
properties.put("keyB", "valueB");

final OneRequest sendPropertiesRequest = new OneRequest.Builder()
        .properties(properties)
        .build();

try {
    One.sendProperties(sendPropertiesRequest);
} catch (ExecutionException e) {
    e.printStackTrace();
}
```

*Note:*
- Sends a `PUT` request to Thunderhead ONE or Salesforce Interaction Studio.
- Properties sent to a base Touchpoint are captured under a base (`/`) or wildcard (`/*`) Interaction in Thunderhead ONE or Salesforce Interaction Studio. The Attribute Capture Point API name in Thunderhead ONE, or Salesforce Interaction Studio, must match the key name sent above.

#### Send a response code

Send a response code by calling the `oneSendResponseCode` Kotlin top-level function, with the response code and the corresponding Interaction path as parameters, as shown below:

`Kotlin`
```kotlin
import com.thunderhead.mobile.interactions.OneInteractionPath
import com.thunderhead.mobile.oneSendResponseCode
// rest of imports

scope.launch {
    oneSendResponseCode {
        interactionPath = OneInteractionPath(URI("/interactionPath"))
        code = OneResponseCode("code")
    }
}
```

To capture errors, set the `throwErrors` parameter to `true` and wrap the method in a `try/catch` block, as shown below:

```kotlin
import com.thunderhead.mobile.interactions.OneInteractionPath
import com.thunderhead.mobile.oneSendResponseCode
import com.thunderhead.mobile.responsetypes.OneAPIError
import com.thunderhead.mobile.responsetypes.OneSDKError
// rest of imports

scope.launch {
    try {
        oneSendResponseCode(throwErrors = true) {
            interactionPath = OneInteractionPath(URI("/interactionPath"))
            code = OneResponseCode("code")
        }
    } catch (error: OneSDKError) {
        Log.e(TAG, "SDK Error: ${error.errorMessage}")
    } catch (error: OneAPIError) {
        Log.e(TAG, "Api Error: ${error.errorMessage}")
    }
}
```

Send a response code, by calling the `One.sendResponseCode` Java method, with the response code
and the corresponding interaction path as parameters, as shown below:

`Java`
```java
import com.thunderhead.mobile.One;
import com.thunderhead.mobile.interactions.OneResponseCodeRequest;
import com.thunderhead.mobile.interactions.OneInteractionPath;
import com.thunderhead.mobile.interactions.OneResponseCode;

final OneResponseCodeRequest responseCodeRequest = new OneResponseCodeRequest.Builder()
        .responseCode(new OneResponseCode("code"))
        .interactionPath(new OneInteractionPath(URI.create("/interactionPath")))
        .build();
try {
    One.sendResponseCode(responseCodeRequest);
} catch (ExecutionException e) {
    e.printStackTrace();
}
```

*Note:*
- Use this method when you are displaying optimizations programmatically and need to capture the user's response.
- Sends a `PUT` request to Thunderhead ONE or Salesforce Interaction Studio.
- When sending Interaction requests programmatically, please ensure the Interaction starts with a `/` and contains only letters, numbers, and/or dashes.

### Retrieve a response for an automatically triggered Interaction request

The Thunderhead SDK considers Android Activities and Fragments as Interactions. When configured correctly the SDK will _automatically_
send an Interaction request to ONE and process the response which may contain points (optimizations, capture, etc). If desired,
you can be notified of these automatic Interactions to take additional action on each Interaction request, by using the
automatic Interaction callback API.

**Notes**
* It is incumbent on you to then process the response in order for the Thunderhead SDK to perform automatic capture and optimization.
* Assigning a manual/custom Interaction to a view should be done _before_ setting an automatic Interaction callback.
* If you set a callback for an automatically triggered Interaction, you are advised to remove that callback as soon as it is no longer needed under your Activity's `onStop` method.

#### Example: Retrieve a response for an automatic Activity Interaction

`Kotlin`
```kotlin
import com.thunderhead.mobile.process
import com.thunderhead.mobile.setAutomaticInteractionCallback
import com.thunderhead.mobile.removeAutomaticInteractionCallback
// rest of imports

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
    }

    override fun onStart() {
        super.onStart()
        setAutomaticInteractionCallback {
            onError { error ->
                Log.e(TAG, "SDK Error", error)
            }

            onFailure { error ->
                Log.e(TAG, "API Error", error)
            }

            onSuccess { response ->
                Log.d(TAG, "Success: ${response.tid}")
                // Do something with response
                response.process()
            }
        }
    }

    override fun onStop() {
        super.onStop()
        removeAutomaticInteractionCallback()
    }

    companion object {
        const val TAG = "MainActivity"
    }
}
```

`Java`
```java
import com.thunderhead.mobile.One;
import com.thunderhead.mobile.interactions.OneCallback;
import com.thunderhead.mobile.responsetypes.OneAPIError;
import com.thunderhead.mobile.responsetypes.OneResponse;
import com.thunderhead.mobile.responsetypes.OneSDKError;
// rest of imports

public class MainActivity extends AppCompatActivity {
    private static final String TAG = "MainActivity";
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    @Override
    protected void onStart() {
        super.onStart();
        One.setAutomaticInteractionCallback(this, new OneCallback() {
            @Override
            public void onError(@NotNull OneSDKError error) {
                Log.e(TAG, "SDK Error", error);
            }

            @Override
            public void onFailure(@NotNull OneAPIError error) {
                Log.e(TAG, "API Error", error);
            }

            @Override
            public void onSuccess(@NotNull OneResponse response) {
                Log.d(TAG, response.getTid());
                // Do something with response
                One.processResponse(response);
            }
        });
    }

    @Override
    protected void onStop() {
        super.onStop();
        One.removeAutomaticInteractionCallback(this);
    }
}
```

#### Example: Retrieve a response for an automatic Fragment Interaction

`Kotlin`
```kotlin
import com.thunderhead.mobile.process
import com.thunderhead.mobile.setAutomaticInteractionCallback
import com.thunderhead.mobile.removeAutomaticInteractionCallback
// rest of imports

class MainActivity : FragmentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        if (savedInstanceState == null) {
            supportFragmentManager.beginTransaction()
                .add(R.id.fragment_container, TestFragment())
                .setReorderingAllowed(true)
                .commit()
        }
    }

    override fun onStart() {
        super.onStart()
        supportFragmentManager
            .findFragmentById(R.id.fragment_container)
            ?.setAutomaticInteractionCallback {
                onError { error ->
                    Log.e(TAG, "SDK Error", error)
                }

                onFailure { error ->
                    Log.e(TAG, "API Error", error)
                }

                onSuccess { response ->
                    Log.d(TAG, "Success: ${response.tid}")
                    // Do something with response
                    response.process()
                }
            }
    }

    override fun onStop() {
        super.onStop()
        supportFragmentManager
            .findFragmentById(R.id.fragment_container)
            ?.removeAutomaticInteractionCallback()
    }

    class TestFragment : Fragment() {
        override fun onCreateView(
            inflater: LayoutInflater,
            container: ViewGroup?,
            savedInstanceState: Bundle?
        ): View? = inflater.inflate(R.layout.fragment_test, container, false)
    }

    companion object {
        const val TAG = "MainActivity"
    }
}
```

`Java`
```java
import com.thunderhead.mobile.One;
import com.thunderhead.mobile.interactions.OneCallback;
import com.thunderhead.mobile.responsetypes.OneAPIError;
import com.thunderhead.mobile.responsetypes.OneResponse;
import com.thunderhead.mobile.responsetypes.OneSDKError;
// rest of imports

public class MainActivity extends FragmentActivity {
    private static final String TAG = "MainActivity";
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        if(savedInstanceState == null) {
            FragmentManager fragmentManager = getSupportFragmentManager();
            FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();
            fragmentTransaction.add(R.id.fragment_container, new TestFragment());
            fragmentTransaction.commit();
        }
    }

    @Override
    protected void onStart() {
        super.onStart();
        FragmentManager fragmentManager = getSupportFragmentManager();
        Fragment fragment = fragmentManager.findFragmentById(R.id.fragment_container);
        One.setAutomaticInteractionCallback(fragment, new OneCallback() {
            @Override

            @Override
            public void onError(@NotNull OneSDKError error) {
                Log.e(TAG, "SDK Error", error);
            }

            @Override
            public void onFailure(@NotNull OneAPIError error) {
                Log.e(TAG, "API Error", error);
            }

            public void onSuccess(@NotNull OneResponse response) {
                Log.d(TAG, response.getTid());
                // Do something with response
                One.processResponse(response);
            }
        });
    }

    @Override
    protected void onStop() {
        super.onStop();
        FragmentManager fragmentManager = getSupportFragmentManager();
        Fragment fragment = fragmentManager.findFragmentById(R.id.fragment_container);
        One.removeAutomaticInteractionCallback(fragment);
    }
}
```

#### Example: Retrieve a response for a manually assigned Interaction

`Kotlin`
```kotlin
import com.thunderhead.mobile.oneSetAutomaticInteractionCallback
import com.thunderhead.mobile.interactions.OneInteractionPath
import com.thunderhead.mobile.oneRemoveAutomaticInteractionCallback
import com.thunderhead.mobile.process
// rest of imports

oneSetAutomaticInteractionCallback(OneInteractionPath(URI("/ManualInteraction"))) {
    onError { error ->
        Log.e(TAG, "SDK Error", error)
    }

    onFailure { error ->
        Log.e(TAG, "API Error", error)
    }

    onSuccess { response ->
        Log.d(TAG, "Success: ${response.tid}")
        // Do something with response
        response.process()
    }
}

  override fun onStop() {
      super.onStop()
      oneRemoveAutomaticInteractionCallback(OneInteractionPath(URI("/ManualInteraction")))
```

`Java`
```java
import com.thunderhead.mobile.One;
import com.thunderhead.mobile.interactions.OneInteractionPath;
import com.thunderhead.mobile.responsetypes.OneAPIError;
import com.thunderhead.mobile.responsetypes.OneResponse;
import com.thunderhead.mobile.responsetypes.OneSDKError;
// rest of imports

One.setAutomaticInteractionCallback(new OneInteractionPath(URI.create("/ManualInteraction")), new OneCallback() {
    @Override
    public void onFailure(@NotNull OneAPIError error) {
        Log.e(TAG, "ApiError", error);
    }

    @Override
    public void onError(@NotNull OneSDKError error) {
        Log.e(TAG, "SdkError", error);
    }

    @Override
    public void onSuccess(@NotNull OneResponse response) {
        Log.d(TAG, "Success: ${response.tid}");
        // Do something with response
        One.processResponse(response);
    }
});

    @Override
    protected void onStop() {
        super.onStop();
        One.removeAutomaticInteractionCallback(new OneInteractionPath(URI.create("/ManualInteraction")));
    }
```
#### Optional response processing

The response can be passed to the `processResponse` method, as shown above. By calling this method the response is returned to the SDK to process, attaching any captures, trackers, and/or optimizations to the Interaction.

### Assign an Interaction to a View

Explicitly define a view as an Interaction by calling the `assignInteractionPath` Kotlin extension function
or the `One.assignInteractionPath` Java method with a valid desired Interaction path, as shown below:

`Kotlin`
```kotlin
import com.thunderhead.mobile.assignInteractionPath
import com.thunderhead.mobile.interactions.OneInteractionPath

findViewById<LinearLayout>(R.id.linear_layout)
    .assignInteractionPath(OneInteractionPath(URI("/viewAsInteraction")))
```

`Java`
```java
LinearLayout linearLayout = findViewById<LinearLayout>(R.id.linear_layout);
final OneInteractionPathAssignment oneInteractionPathAssignment = new OneInteractionPathAssignment.Builder()
        .view(linearLayout)
        .interactionPath(new OneInteractionPath(URI.create("/viewAsInteraction")))
        .build();

One.assignInteractionPath(oneInteractionPathAssignment);
```

This can be useful in the following cases:

1. If an activity with the same layout implements generic functionality and is used to represent various Interactions within the same application.
   For example, you may have a list view, that is reused across the application to display branch locations in one use case and cash point locations in a second use case.

`Kotlin`
```kotlin
import com.thunderhead.mobile.assignInteractionPath
import com.thunderhead.mobile.interactions.OneInteractionPath
// rest of imports

class LocationsList : ListActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val interactionPath = if (presenterType == CASH_POINT_LOCATION) {
            "/cashPointList"
        } else {
           "/branchList" 
        }
        getListView().assignInteractionPath(OneInteractionPath(URI(interactionPath)))
    }
}
```

`Java`
```java
public class LocationsList extends ListActivity {
  @Override
  public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        final OneInteractionPathAssignment.Builder assignmentBuilder = 
            new OneInteractionPathAssignment.Builder()
                .view(getListView());
        if (presenterType == CASH_POINT_LOCATION) {
            assignmentBuilder
                .interactionPath(new OneInteractionPath(URI.create("/cashPointList")));
        } else {
            assignmentBuilder
                .interactionPath(new OneInteractionPath(URI.create("/branchList")));
        }
        One.assignInteractionPath(assignmentBuilder.build());
    }
}
```

2. If a fragment implements generic functionality and may represent various Interactions.
   For example, in one case it may show a screen containing laptops and, in another, a screen containing cameras.

`Java`
```java 
@Override
public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
    View view = inflater.inflate(R.layout.products_tiles_view, container, false);
    final OneInteractionPathAssignment.Builder assignmentBuilder = 
        new OneInteractionPathAssignment.Builder()
            .view(view);
    if (category == Product.Category.LAPTOP) {
        assignmentBuilder
            .interactionPath(new OneInteractionPath(URI.create("/laptopsList")));
    } else if (category == Product.Category.CAMERA) {
        assignmentBuilder
            .interactionPath(new OneInteractionPath(URI.create("/camerasList")));
    }
    One.assignInteractionPath(assignmentBuilder.build());
    return view;
}
```

3. If an Interaction is represented by a custom view.

`Kotlin`
```kotlin
import com.thunderhead.mobile.assignInteractionPath
import com.thunderhead.mobile.interactions.OneInteractionPath
// rest of imports

fun showVariants() {
    if (varientsView == null) {
        variantsView = inflater.inflate(R.layout.variants_slide, mainPanelView, false)
        variantsView.assignInteractionPath(OneInteractionPath(URI("/variants")))
    }
}
```

`Java`
```java
void showVariants() {
    if (variantsView == null) {
        variantsView = inflater.inflate(R.layout.variants_slide, mainPaneView, false);
        final OneInteractionPathAssignment assignment = 
            new OneInteractionPathAssignment.Builder()
                .view(variantsView)
                .interactionPath(new OneInteractionPath(URI.create("/variants")))
                .build();

        One.assignInteractionPath(assignment);
    }
}
```

### Ability to include identity transfer links

The SDK appends a `one-tid` `URL` parameter to all links opened from a mobile app. To limit this behaviour, and allow the SDK to append a `one-tid` `URL` parameter only to a specific set of links. Include that set of links by assigning `oneIdentityTransferIncludeList` top level property or `One.setIdentityTransferIncludeList` Java method, as shown below:

`Kotlin`
```kotlin
import com.thunderhead.mobile.oneIdentityTransferIncludeList
import com.thunderhead.mobile.identitytransfer.OneIdentityTransferUriMatcher
// rest of imports

// This example shows how to include links for specific uri(s) which do have SSL protocols:
// https://www.google.com and https://www.uber.com
oneIdentityTransferIncludeList = setOf(
    OneIdentityTransferUriMatcher.ExactMatch(URI("https://www.google.com")),
    OneIdentityTransferUriMatcher.ExactMatch(URI("https://www.uber.com"))
)

// This example shows how to include the sub domains for wikipedia.org but not wikipedia.org directly.
oneIdentityTransferIncludeList = setOf(
    OneIdentityTransferUriMatcher.RegexMatch("https://(simple|en)\\.wikipedia\\.org".toRegex())
)

// This example shows how to retrieve the include list currently in use
val includeList = oneIdentityTransferIncludeList
```

`Java`
```java
// This example shows how to include links for specific uri(s) which do have SSL protocols:
// https://www.google.com and https://www.uber.com
Set<OneIdentityTransferUriMatcher> includeList = new HashSet<>();
try {
    includeList.add(new OneIdentityTransferUriMatcher.ExactMatch(URI.create("https://www.google.com")));
    includeList.add(new OneIdentityTransferUriMatcher.ExactMatch(URI.create("https://www.uber.com")));
} catch (Exception e) {
    e.printStackTrace();
}
One.setIdentityTransferIncludeList(includeList);

// This example shows how to include the sub domains for wikipedia.org but not wikipedia.org directly.
Set<OneIdentityTransferUriMatcher> includeList = new HashSet<>();
try {
    final Regex wikipediaSubDomains = new Regex("https://(simple|en)\\.wikipedia\\.org");
    includeList.add(new OneIdentityTransferUriMatcher.RegexMatch(wikipediaSubDomains));
} catch (Exception e) {
    e.printStackTrace();
}
One.setIdentityTransferIncludeList(includeList);

// This example shows how to retrieve the include list currently in use
Set<OneIdentityTransferUriMatcher> includeList = One.getIdentityTransferIncludeList();
```

*Note:*
- When a link is included, a `one-tid` is appended only to the allowed links, superseding any
  [excluded identity transfer links](#ability-to-exclude-identity-transfer-links).
- Setting the allowed list to an `empty list` clears the existing list.

### Ability to exclude identity transfer links

The SDK appends a `one-tid` `URL` parameter to all links opened from a mobile app. To limit this behaviour, and allow the SDK to append a `one-tid` `URL` parameter only to a specific set of links. Exclude the links by assigning list to the `oneIdentityTransferExcludeList` top level property or `One.setIdentityTransferExcludeList` Java method, as shown below:

`Kotlin`
```kotlin
import com.thunderhead.mobile.oneIdentityTransferExcludeList
import com.thunderhead.mobile.identitytransfer.OneIdentityTransferUriMatcher
// rest of imports

// This example shows how to exclude links for specific uri(s) which do not have SSL protocols:
// www.google.com and www.uber.com
oneIdentityTransferExcludeList = setOf(
    OneIdentityTransferUriMatcher.ExactMatch(URI("http://www.google.com")),
    OneIdentityTransferUriMatcher.ExactMatch(URI("http://www.uber.com"))
)

// This example shows how to exclude the sub domains for wikipedia.org but not wikipedia.org directly.
oneIdentityTransferExcludeList = setOf(
    OneIdentityTransferUriMatcher.RegexMatch("https://(simple|en)\\.wikipedia\\.org".toRegex())
)

// This example shows how to retrieve the exclude list currently in use
val excludeList = oneIdentityTransferExcludeList
```

`Java`
```java
// This example shows how to exclude links for specific uri(s) which do not have SSL protocols:
// www.google.com and www.uber.com
Set<OneIdentityTransferUriMatcher> excludeList = new HashSet<>();
try {
    excludeList.add(new OneIdentityTransferUriMatcher.ExactMatch(URI.create("http://www.google.com")));
    excludeList.add(new OneIdentityTransferUriMatcher.ExactMatch(URI.create("http://www.uber.com")));
} catch (Exception e) {
    e.printStackTrace();
}
One.setIdentityTransferExcludeList(excludeList);

// This example shows how to exclude the sub domains for wikipedia.org but not wikipedia.org directly.
Set<OneIdentityTransferUriMatcher> excludeList = new HashSet<>();
try {
    final Regex wikipediaSubDomains = new Regex("https://(simple|en)\\.wikipedia\\.org");
    excludeList.add(new OneIdentityTransferUriMatcher.RegexMatch(wikipediaSubDomains));
} catch (Exception e) {
    e.printStackTrace();
}
One.setIdentityTransferExcludeList(excludeList);

// This example shows how to retrieve the exclude list currently in use
Set<OneIdentityTransferUriMatcher> excludeList = One.getIdentityTransferExcludeList();
```

*Note:*
- If a link is excluded, a `one-tid` is appended to all links other than the excluded link, but
  is superseded by any [included identity transfer links](#ability-to-include-identity-transfer-links).
- Setting the exclude list to an `empty list` clears the existing list.

### Disable automatic identity transfer

If the Orchestration Plugin is enabled, the SDK adds a `one-tid` as a `URL` query parameter to web links opened in `WebView`, `CustomTabs` and external browsers (via `Intent`).
To disable this functionality, call the `oneConfigureIdentityTransfer` Kotlin top-level function or the `One.setIdentityTransferConfiguration` Java method, as shown below:

`Kotlin`
```kotlin
import com.thunderhead.mobile.oneConfigureIdentityTransfer

oneConfigureIdentityTransfer {
    disableIdentityTransfer = true
}
```

`Java`
```java
import com.thunderhead.mobile.One;
import com.thunderhead.mobile.identitytransfer.OneIdentityTransferConfiguration;

final OneIdentityTransferConfiguration identityTransferConfiguration =
    new OneIdentityTransferConfiguration.Builder()
        .disableIdentityTransfer(true)
        .build();
One.setIdentityTransferConfiguration(identityTransferConfiguration);
```

*Note:*
- This also disables the ability to pick up parameters, automatically, from deep links that open the app and prevents the SDK from adding a `one-tid` as a `URL` query parameter to web links opened from the app, resulting in the customer's identity not being transferred as they move across channels.

#### Send deep link properties programmatically

If you have disabled automatic identity transfer, you can still send all `URL` parameters received as part of a deep link by calling the `java.net.URI.processDeepLink` or `android.net.Uri.processDeepLink` Kotlin extension function or the `One.processDeepLink` Java method, as shown below:

`Kotlin`
```kotlin
import com.thunderhead.mobile.processDeepLink
// rest of imports

URI("myapp://MainActivity?customerKey=1").processDeepLink()
Uri.parse("myapp://MainActivity?customerKey=1").processDeepLink()
```

`Java`
```java
One.processDeepLink(URI.create("myapp://MainActivity?customerKey=1"));
```

*Note:*
- Sends a `PUT` request to Thunderhead ONE or Salesforce Interaction Studio.

#### Create a `URL` with a `one-tid` parameter to facilitate identity transfer

If you have disabled automatic identity transfer, you can still create a `URL` with a `one-tid` parameter to use in the app programmatically, by calling the `java.net.URL.generateIdentityTransferUrl()` Kotlin extension function or the `One.generateIdentityTransferUrl(URL)` Java method, as shown below:

`Kotlin`
```kotlin
import com.thunderhead.mobile.generateIdentityTransferUrl
// rest of imports

val urlWithOneTid = URL("http://mysite.com").generateIdentityTransferUrl()
```

`Java`
```java
URL url = new URL("http://mysite.com");
URL urlWithOneTid = One.generateIdentityTransferUrl(url);
```

Once you have the `urlWithOneTid`, pass this into the method which handles the opening of the `URL`.

*Note:* The above methods return `null` if the SDK is not configured or is in Admin Mode.

#### Create an `android.net.Uri` or `java.net.URI` with a `one-tid` parameter to facilitate identity transfer

If you have disabled automatic identity transfer, you can still create an `android.net.Uri` or `java.net.URI` with a `one-tid` parameter to use in the app programmatically, by calling the `java.net.URI.generateIdentityTransferUri()` or `android.net.Uri.generateIdentityTransferUri()` Kotlin extension functions or the `One.generateIdentityTransferUri(Uri|URI)` Java method as shown below:

`Kotlin`
```kotlin
import com.thunderhead.mobile.generateIdentityTransferUri
// rest of imports

val androidUriWithOneTid = Uri.parse("http://mysite.com").generateIdentityTransferUri()
val javaUriWithOneTid = URI("http://mysite.com").generateIdentityTransferUri()
```

`Java`
```java
Uri uri = Uri.parse("http://mysite.com");
Uri uriWithOneTid = One.generateIdentityTransferUri(uri);

URI javaUri = URI.create("http://mysite.com");
URI javaUriWithOneTid = One.generateIdentityTransferUri(javaUri);
```

Once you have the `uriWithOneTid`, pass this into the method which handles the opening of the `Uri`.

*Note:* The above methods return `null` if the SDK is not configured or is in Admin Mode.

### Disable automatic outbound link tracking

If the Orchestration Plugin is enabled, the SDK automatically sends an Interaction request to `/one-click` when a `URL` is opened in a `WebView`, `CustomTab` or external browser to facilitate last click attribution.

To disable this functionality, use the code below:

`Kotlin`
```kotlin
import com.thunderhead.mobile.oneConfigureCodelessInteractionTracking

oneConfigureCodelessInteractionTracking {
    disableOutboundLinkTracking = true
}
```

`Java`
```java
import com.thunderhead.mobile.One;
import com.thunderhead.mobile.codeless.OneCodelessInteractionTrackingConfiguration;

final OneCodelessInteractionTrackingConfiguration codelessInteractionTrackingConfiguration =
    new OneCodelessInteractionTrackingConfiguration.Builder()
        .disableOutboundLinkTracking(true)
        .build();

One.setCodelessInteractionTrackingConfiguration(codelessInteractionTrackingConfiguration);
```

#### Programmatically trigger an outbound link tracking Interaction call

If you have disabled automatic outbound link tracking, you can still track a `URL` or `Uri`, by calling the `java.net.URI.sendInteractionForOutboundLink` or `android.net.Uri.sendInteractionForOutboundLink` Kotlin extension functions or the `One.sendInteractionForOutboundLink` Java method, as shown below:

`Kotlin`
```kotlin
import com.thunderhead.mobile.sendInteractionForOutboundLink

URI("https://www.yourfullurl.com/").sendInteractionForOutboundLink()
Uri.parse("https://www.yourfullurl.com/").sendInteractionForOutboundLink()

URL("https://www.yourfullurl.com/").sendInteractionForOutboundLink()
```

`Java`
```java
// URL example
try {
    One.sendInteractionForOutboundLink(new URL("https://www.yourfullurl.com/"));
} catch (MalformedURLException e) {
    e.printStackTrace();
}

// URI example
try {
     One.sendInteractionForOutboundLink(Uri.parse("https://www.yourfullurl.com/"));
} catch (Exception e) {
    e.printStackTrace();
}

```
Pass the `URL` or `Uri`, to send an Interaction request to `/one-click` using the same logic as is available automatically.

*Note:*
- Sends a `POST` request to Thunderhead ONE or Salesforce Interaction Studio.
- Set up the `/one-click` Interaction request in Thunderhead ONE or Salesforce Interaction Studio, to capture the appropriate attributes and activity.

### Send a location object

To send a location object, pass the location object as a parameter to the `updateLocation` method, as shown below:

```java
One.processLocation(location);
```

Use the `LocationListener` callback method to call `updateLocation`, as shown below:

```java
LocationListener locationListener = new LocationListener() {
    public void onLocationChanged(Location location) {
        One.getInstance(Activity.this).updateLocation(location);
    }

    public void onStatusChanged(String provider, int status, Bundle extras){
    }

    public void onProviderEnabled(String provider) {
    }

    public void onProviderDisabled(String provider) {
    }
};
```

### Get Tid

To get the current `tid` used by the SDK, call:

`Kotlin`
```kotlin
oneGetTid()
```

`Java`
```java
One.getTid();
```

*Note:*
- Returns the `tid` assigned to the current user as a `String`.
- Retrieving the current `tid` can be useful if you want to monitor the current user in Thunderhead ONE or Salesforce Interaction Studio, or if you need to pass the identity of the current user to another system that sends data to Thunderhead ONE or Salesforce Interaction Studio.

### Configuring Logging

The Thunderhead SDK for Android provides an extensible logging configuration API for debug or reporting purposes. The API can be configured to
log any combination of Components (features or technical concepts such as Networking or Databases) to Log Levels (Verbose, Debug, etc). In addition,
custom log writers can be added to facilitate reporting if desired (ex. sending errors to Google Console).

By default, the Thunderhead SDK for Android logs ERROR and WARN messages for ANY component. Below are examples of other logging configurations.

#### Turn all logs on

*Example of configuring logging to VERBOSE Log Level for ANY Components of the Thunderhead SDK.*

`Kotlin`
```kotlin
import com.thunderhead.mobile.logging.OneLogComponent
import com.thunderhead.mobile.logging.OneLogLevel
import com.thunderhead.mobile.oneConfigureLogging
// rest of imports

oneConfigureLogging {
    levels = mutableSetOf(OneLogLevel.VERBOSE) 
    components = mutableSetOf(OneLogComponent.ANY)
}
```

`Java`
```java
import com.thunderhead.mobile.logging.OneLogComponent;
import com.thunderhead.mobile.logging.OneLogLevel;
import com.thunderhead.mobile.logging.OneLoggingConfiguration;
// rest of imports

final OneLoggingConfiguration oneLoggingConfiguration = OneLoggingConfiguration.builder()
    .log(OneLogLevel.VERBOSE)
    .log(OneLogComponent.ANY)
    .build();

One.setLoggingConfiguration(oneLoggingConfiguration);
```

#### Turn specific logs on

*Example of configuring logging to combination of ERROR and WARN levels for just NETWORKING and DATABASE Components of the Thunderhead SDK.*

`Kotlin`
```kotlin
import com.thunderhead.mobile.logging.OneLogComponent
import com.thunderhead.mobile.logging.OneLogLevel
import com.thunderhead.mobile.oneConfigureLogging
// rest of imports

oneConfigureLogging {
    levels = OneLogLevel.ERROR and OneLogLevel.WARN
    components = OneLogComponent.NETWORKING and OneLogComponent.DATABASE
}
```

`Java`
```java
import com.thunderhead.mobile.logging.OneLogComponent;
import com.thunderhead.mobile.logging.OneLogLevel;
import com.thunderhead.mobile.logging.OneLoggingConfiguration;
// rest of imports

final OneLoggingConfiguration oneLoggingConfiguration = OneLoggingConfiguration.builder()
    .log(OneLogLevel.ERROR)
    .log(OneLogLevel.WARN)
    .log(OneLogComponent.NETWORKING)
    .log(OneLogComponent.DATABASE)
    .build();

One.setLoggingConfiguration(oneLoggingConfiguration);
```

*Example of using a custom logger*

`Kotlin`
```kotlin
import com.thunderhead.mobile.logging.OneLogComponent
import com.thunderhead.mobile.logging.OneLogLevel
import com.thunderhead.mobile.oneConfigureLogging
import com.thunderhead.mobile.logging.OneLogWriter
// rest of imports

oneConfigureLogging {
    levels = mutableSetOf(OneLogLevel.VERBOSE) 
    components = OneLogComponent.NETWORKING and OneLogComponent.DATABASE
    logWriters = mutableSetOf(CustomLogger())
}

// custom logger
class CustomLogger : OneLogWriter() {
    override fun log(
        logLevel: OneLogLevel,
        component: OneLogComponent,
        message: String,
        throwable: Throwable?
    ) {
        Log.d("CustomLogger", "Component: ${component.name}\nMessage: $message", throwable)
    }
}
```

`Java`
```java
import com.thunderhead.mobile.logging.OneLogComponent;
import com.thunderhead.mobile.logging.OneLogLevel;
import com.thunderhead.mobile.logging.OneLoggingConfiguration;
import com.thunderhead.mobile.logging.OneLogWriter;
// rest of imports

final OneLoggingConfiguration oneLoggingConfiguration = OneLoggingConfiguration.builder()
    .log(OneLogLevel.VERBOSE)
    .log(OneLogComponent.NETWORKING)
    .log(OneLogComponent.DATABASE)
    .logTo(new CustomLogger())
    .build();

One.setLoggingConfiguration(oneLoggingConfiguration);

// custom logger
static class CustomLogger extends OneLogWriter {
        @Override
        public void log(
                @NotNull OneLogLevel logLevel,
                @NotNull OneLogComponent component,
                @NotNull String message,
                @Nullable Throwable throwable
        ) {
            Log.d("CustomLogger", "Component: " + component.name() + "\nMessage: "  + message, throwable);
        }
    }
```

#### Turn logs for the Thunderhead SDK initialization process on

The Thunderhead SDK performs initialization processes in an Android Content Provider which is instantiated before
the Application is created. This means the log configuration API cannot be invoked before the Thunderhead SDK
has finished its initialization process. To turn on logging for the initialization process of the Thunderhead SDK
a meta data element must be added to the android manifest. If the metadata element is not set no logging is configured.

*Metadata Info:*

`name` :  `com.thunderhead.android.InitLogLevel`
`value`: Comma separated list of `com.thunderhead.mobile.logging.OneLogLevel`

*Example for logging VERBOSE and above logs:*

```xml
<application>
    <!--Other application elements-->

    <meta-data
        android:name="com.thunderhead.android.InitLogLevel"
        android:value="VERBOSE" />
</application>
```

**Recommendation**
We recommend including the above metadata only in `DEBUG` builds to ensure no unnecessary logging
occurs in release. Therefore, only include this metadata in the `DEBUG` variant
`AndroidManifest.xml` and _NOT_ in the main `AndroidManifest.xml`. To learn more about how manifests
are merged, please see the [Android Documentation](https://developer.android.com/studio/build/manifest-merge).

#### Turn all logs off

To turn off logging, pass a set to OneLogLevel and a set to OneLogComponent with the values of NONE
*Example of turning logging off*

`Kotlin`
```kotlin
import com.thunderhead.mobile.logging.OneLogComponent
import com.thunderhead.mobile.logging.OneLogLevel
import com.thunderhead.mobile.oneConfigureLogging
// rest of imports

oneConfigureLogging {
    levels = mutableSetOf(OneLogLevel.NONE)
    components = mutableSetOf(OneLogComponent.NONE)
}
```

`Java`
```java
import com.thunderhead.mobile.logging.OneLogComponent;
import com.thunderhead.mobile.logging.OneLogLevel;
import com.thunderhead.mobile.logging.OneLoggingConfiguration;
// rest of imports

final OneLoggingConfiguration oneLoggingConfiguration = OneLoggingConfiguration.builder()
    .log(OneLogLevel.NONE)
    .log(OneLogComponent.NONE)
    .build();

One.setLoggingConfiguration(oneLoggingConfiguration);
```

*Note:*
- The `com.thunderhead.android.InitLogLevel` `AndroidManifest.xml` metadata value is only honored
  for the Thunderhead SDK initialization process. After initialization has finished, the logging
  configuration reverts to a default configuration mentioned above. If more logging is desired then use the logging
  configuration APIs to turn on logging as shown above.
- When setting a single `OneLogLevel`, the SDK will log any messages of that level and above.
    - The order from the bottom is: VERBOSE, DEBUG, ERROR, WARN, INFO, ASSERT
    - Example: Setting VERBOSE will log all messages.
    - Example: Setting INFO will log only INFO and ASSERT messages.
- When setting multiple `OneLogLevel`(s), the SDK will log only messages of those specific levels.
    - Example: Setting ERROR and WARN will only log message of ERROR and WARN levels and nothing else.
- When setting `OneLogComponent` to _only_ `ANY` all components will be logged in conjunction with the log level.
- When setting multiple `OneLogComponent`(s), the SDK will log only messages for those specific components.
    - Do not set set multiple `Components`(s) along side the `ANY` component. Choose only the components
      required or just use `ANY` but not both.

### Identify the SDK version

Find the current version of the SDK by calling:

`Kotlin`
```kotlin
val version = oneVersion
```

`Java`
```java
One.getVersion();
```

### Clear the user profile

Programmatically erase the user profile data by calling:

`Kotlin`
```kotlin
oneClearUserProfile()
```

`Java`
```java
One.clearUserProfile();
```

*Note:*
- Removes the stored `tid` only from local storage.
- For instructions on how completely remove a user's data from Thunderhead ONE or Salesforce Interaction Studio, see our [API Documentation](https://thunderheadone.github.io/one-api/#operation/delete).
