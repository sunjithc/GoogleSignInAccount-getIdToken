# GoogleSignInAccount-getIdToken-
How to get GoogleSignInAccount getIdToken() 


getIdToken () :- Returns an ID token that you can send to your server if GoogleSignInOptions.Builder.requestIdToken(String) was configured; null otherwise.
ID token is a JSON Web Token signed by Google that can be used to identify a user to a backend.

Add Google Play services
In your project's top-level build.gradle file, ensure that Google's Maven repository is included:

allprojects {
    repositories {
        google()

        // If you're using a version of Gradle lower than 4.1, you must instead use:
        // maven {
        //     url 'https://maven.google.com'
        // }
    }
}

Then, in your app-level build.gradle file, declare Google Play services as a dependency:


apply plugin: 'com.android.application'
    ...

    dependencies {
        implementation 'com.google.android.gms:play-services-auth:19.0.0'
    }
Configure a Google API Console project
To configure a Google API Console project, click the button below, and specify your app's package name when prompted. You will also need to provide the SHA-1 hash of your signing certificate. See Authenticating Your Client for information.

Configure a project-> https://developers.google.com/identity/sign-in/android/start-integrating / https://console.cloud.google.com/apis/credentials?pli=1

Get your backend server's OAuth 2.0 client ID
If your app authenticates with a backend server or accesses Google APIs from your backend server, you must get the OAuth 2.0 client ID that was created for your server. To find the OAuth 2.0 client ID:

Open the Credentials page in the API Console.
The Web application type client ID is your backend server's OAuth 2.0 client ID.

![Screenshot 2021-03-22 at 5 55 29 PM](https://user-images.githubusercontent.com/12294662/111991738-a35bd280-8b3a-11eb-99af-2003ef93778a.png)


