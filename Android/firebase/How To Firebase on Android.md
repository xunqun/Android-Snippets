# How To Firebase on Android
## Installation
		
We publish builds of our Android and Java SDK to the Maven central repository. To install the library inside Android Studio, you can simply declare it as dependency in your build.gradle file:
		
		dependencies {
		    compile 'com.firebase:firebase-client-android:2.5.2+'
		}
		
If you are getting a build error complaining about duplicate files you can choose to exclude those files by adding the packagingOptions directive to your build.gradle file:

		android {
		    ...
		    packagingOptions {
		        exclude 'META-INF/LICENSE'
		        exclude 'META-INF/LICENSE-FIREBASE.txt'
		        exclude 'META-INF/NOTICE'
		    }
		}
##Premissions

		<uses-permission android:name="android.permission.INTERNET" />
		
##Setup

###In Application

		@Override
		public void onCreate() {
		    super.onCreate();
		    Firebase.setAndroidContext(this);
		    // other setup code
		}
		
## Read & Write

###Initialization

		Firebase myFirebaseRef = new Firebase("https://<YOUR-FIREBASE-APP>.firebaseio.com/");
		
###Writing Data
	myFirebaseRef.child("message").setValue("Do you have data? You'll love Firebase.");
	
### Reading Data
	myFirebaseRef.child("message").addValueEventListener(new ValueEventListener() {
	  @Override
	  public void onDataChange(DataSnapshot snapshot) {
	    System.out.println(snapshot.getValue());  //prints "Do you have data? You'll love Firebase."
	  }
	  @Override public void onCancelled(FirebaseError error) { }
	});	
	
## Authenticate Your Users
### Create a User
		myFirebaseRef.createUser("bobtony@firebase.com", "correcthorsebatterystaple", new Firebase.ValueResultHandler<Map<String, Object>>() {
		    @Override
		    public void onSuccess(Map<String, Object> result) {
		        System.out.println("Successfully created user account with uid: " + result.get("uid"));
		    }
		    @Override
		    public void onError(FirebaseError firebaseError) {
		        // there was an error
		    }
		});