#Firebase Authentication
##When a user authenticates to a Firebase app, three things happen:

 - Information about the user is returned in callbacks on the client device. This allows you to customize your app's user experience for that specific user.
 - The user information returned contains a uid (a unique ID), which is guaranteed to be distinct across all providers, and to never change for a specific authenticated user.
 - The value of the auth variable in your app's Security and Firebase Rules becomes defined. This variable is null for unauthenticated users, but for authenticated users it is an object containing the user's unique (auth.uid) and potentially other data about the user. This allows you to securely control data access on a per-user basis.

 ## Enabling Providers

- Go to the Account Dashboard.
- Select your Firebase app.
- Choose the Login & Auth tab.
- Find the Authentication Providers section.
- Select a tab for a provider and enable authentication.
- If you're using a social login provider, like Facebook, copy your client ID and secret into the form.

## Monitoring Authentication
	Firebase ref = new Firebase("https://<YOUR-FIREBASE-APP>.firebaseio.com");
	ref.addAuthStateListener(new Firebase.AuthStateListener() {
	    @Override
	    public void onAuthStateChanged(AuthData authData) {
	        if (authData != null) {
	            // user is logged in
	        } else {
	            // user is not logged in
	        }
	    }
	});
	
You can also check the user's authentication state by calling the synchronous getAuth() function:

	AuthData authData = ref.getAuth();
	if (authData != null) {
	  // user authenticated
	} else {
	  // no user authenticated
	}

To stop listening to authentication state changes, you can use removeAuthEventListener().

	ref.removeAuthStateListener(authStateListener);
	
##Logging Users In

	Firebase ref = new Firebase("https://<YOUR-FIREBASE-APP>.firebaseio.com");
	// Create a handler to handle the result of the authentication
	Firebase.AuthResultHandler authResultHandler = new Firebase.AuthResultHandler() {
	    @Override
	    public void onAuthenticated(AuthData authData) {
	        // Authenticated successfully with payload authData
	    }
	    @Override
	    public void onAuthenticationError(FirebaseError firebaseError) {
	        // Authenticated failed with error firebaseError
	    }
	};
	// Authenticate users with a custom Firebase token
	ref.authWithCustomToken("<token>", authResultHandler);
	// Alternatively, authenticate users anonymously
	ref.authAnonymously(authResultHandler);
	// Or with an email/password combination
	ref.authWithPassword("jenny@example.com", "correcthorsebatterystaple", authResultHandler);
	// Or via popular OAuth providers ("facebook", "github", "google", or "twitter")
	ref.authWithOAuthToken("<provider>", "<oauth-token>", authResultHandler);
	
## Logging Users Out

	ref.unauth();
	
## Storing User Data
	ref.authWithPassword("jenny@example.com", "correcthorsebatterystaple",
	    new Firebase.AuthResultHandler() {
	    @Override
	    public void onAuthenticated(AuthData authData) {
	        // Authentication just completed successfully :)
	        Map<String, String> map = new HashMap<String, String>();
	        map.put("provider", authData.getProvider());
	        if(authData.getProviderData().containsKey("displayName")) {
	            map.put("displayName", authData.getProviderData().get("displayName").toString());
	        }
	        ref.child("users").child(authData.getUid()).setValue(map);
	    }
	    @Override
	    public void onAuthenticationError(FirebaseError error) {
	        // Something went wrong :(
	    }
	});
A users node will be created in your database that looks something like this:

	{
	  "users": {
	    "6d914336-d254-4fdb-8520-68b740e047e4": {
	      "displayName": "alanisawesome",
	      "provider": "password"
	    },
	    "002a448c-30c0-4b87-a16b-f70dfebe3386": {
	      "displayName": "gracehop",
	      "provider": "password"
	    }
	  }
	}
	
##Handling Errors

	Firebase ref = new Firebase("https://<YOUR-FIREBASE-APP>.firebaseio.com");
	ref.authWithPassword("jenny@example.com", "correcthorsebatterystaple",
	    new Firebase.AuthResultHandler() {
	    @Override
	    public void onAuthenticated(AuthData authData) { /* ... */ }
	    @Override
	    public void onAuthenticationError(FirebaseError error) {
	        // Something went wrong :(
	        switch (error.getCode()) {
	            case FirebaseError.USER_DOES_NOT_EXIST:
	                // handle a non existing user
	                break;
	            case FirebaseError.INVALID_PASSWORD:
	                // handle an invalid password
	                break;
	            default:
	                // handle other errors
	                break;
	        }
	    }
	});