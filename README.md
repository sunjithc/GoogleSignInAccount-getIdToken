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

Sample code:

    import android.content.Context;
    import android.content.Intent;
    import android.os.Bundle;
    import android.util.Log;

    import androidx.annotation.NonNull;

    import com.google.android.gms.auth.api.signin.GoogleSignIn;
    import com.google.android.gms.auth.api.signin.GoogleSignInAccount;
    import com.google.android.gms.auth.api.signin.GoogleSignInClient;
    import com.google.android.gms.auth.api.signin.GoogleSignInOptions;
    import com.google.android.gms.common.ConnectionResult;
    import com.google.android.gms.common.api.ApiException;
    import com.google.android.gms.common.api.GoogleApiClient;
    import com.google.android.gms.tasks.Task;
    import com.travel.drnme.BaseActivity;
    import com.travel.drnme.R;
    import com.travel.drnme.utilities.Utils;

    import butterknife.ButterKnife;
    import butterknife.OnClick;

    public class LoginActivity_ extends BaseActivity implements
        GoogleApiClient.OnConnectionFailedListener {
    private static final String TAG = LoginActivity_.class.getSimpleName();
    private static final int RC_SIGN_IN = 9001;
    GoogleSignInClient mGoogleSignInClient;

    public static void start(Context context) {
        start(context, false);
    }

    public static void start(Context context, boolean clearTop) {
        Intent intent = new Intent(context, LoginActivity_.class);
        if (clearTop) {
            intent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP | Intent.FLAG_ACTIVITY_NEW_TASK);
        }
        context.startActivity(intent);
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_login);
        ButterKnife.bind(this);

        // Configure sign-in to request the user's ID, email address, and basic
       // profile. ID and basic profile are included in DEFAULT_SIGN_IN.
        GoogleSignInOptions gso = new GoogleSignInOptions.Builder(GoogleSignInOptions.DEFAULT_SIGN_IN)
                .requestIdToken(getString(R.string.server_client_id))
                .requestEmail()
                .build();
        // Build a GoogleSignInClient with the options specified by gso.
        mGoogleSignInClient = GoogleSignIn.getClient(this, gso);


    }

    @Override
    protected void onStart() {
        super.onStart();
        // Check for existing Google Sign In account, if the user is already signed in
        // the GoogleSignInAccount will be non-null.
        GoogleSignInAccount account = GoogleSignIn.getLastSignedInAccount(this);
    }


    @OnClick(R.id.ll_gmailLogin)
    public void onGoogleClick() {

        signIn();
    }

    private void signIn() {
        Intent signInIntent = mGoogleSignInClient.getSignInIntent();
        startActivityForResult(signInIntent, RC_SIGN_IN);
    }


    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data) {
        super.onActivityResult(requestCode, resultCode, data);
        if (requestCode == RC_SIGN_IN) {
            Task<GoogleSignInAccount> task = GoogleSignIn.getSignedInAccountFromIntent(data);
            handleSignInResult(task);
        }

    }

    private void handleSignInResult(Task<GoogleSignInAccount> completedTask) {
        try {
            GoogleSignInAccount account = completedTask.getResult(ApiException.class);
            // Signed in successfully, show authenticated UI.
            Log.d("getIdToken", account.getIdToken());
            updateUI(account);

        } catch (ApiException e) {
            // The ApiException status code indicates the detailed failure reason.
            // Please refer to the GoogleSignInStatusCodes class reference for more information.
            Log.w(TAG, "signInResult:failed code=" + e.getStatusCode());
        }
    }
    }


