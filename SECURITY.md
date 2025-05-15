<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>GCM_SENDER_ID</key>
	
	<key>PLIST_VERSION</key>
	<string>1</string>
	<key>BUNDLE_ID</key>
	<string>Fortress.org</string>
	<key>PROJECT_ID</key>
	<string>fortress-7a7ec</string>
	<key>STORAGE_BUCKET</key>
	<string>fortress-7a7ec.firebasestorage.app</string>
	<key>IS_ADS_ENABLED</key>
	<false></false>
	<key>IS_ANALYTICS_ENABLED</key>
	<false></false>
	<key>IS_APPINVITE_ENABLED</key>
	<true></true>
	<key>IS_GCM_ENABLED</key>
	<true></true>
	<key>IS_SIGNIN_ENABLED</key>
	<true></true>
	<key>GOOGLE_APP_ID</key>
	<string>1:734067921066:ios:396ead8a94105588c280d3</string>
</dict>
</plist>
	
	// Firestore.rules
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    match /users/{userId} {
      allow read, write: if request.auth.uid == userId;
    }

    match /tasks/{taskId} {
      allow read: if request.auth != null;
      allow write: if false;
    }

    match /rewards/{rewardId} {
      allow read: if request.auth != null && 
                   get(/databases/$(database)/documents/users/$(request.auth.uid)).data.tier >= resource.data.requiredTier;
    }
  }
}
