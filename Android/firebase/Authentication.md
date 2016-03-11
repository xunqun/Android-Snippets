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