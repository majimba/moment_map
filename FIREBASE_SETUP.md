# Firebase Setup Guide

Follow these steps to set up Firebase for your Moment Map app:

## Step 1: Create a Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click **"Add project"** or select an existing project
3. Enter your project name (e.g., "moment-map")
4. Follow the setup wizard (you can disable Google Analytics if you don't need it)

## Step 2: Register Your Web App

1. In your Firebase project, click the **Web icon** (`</>`) to add a web app
2. Register your app with a nickname (e.g., "Moment Map Web")
3. **Copy the Firebase configuration object** that appears

## Step 3: Enable Firebase Services

### Authentication
1. Go to **Authentication** → **Get Started**
2. Enable **Email/Password** sign-in method (or others as needed)

### Firestore Database
1. Go to **Firestore Database** → **Create database**
2. Choose **Start in test mode** (for development) or **Production mode**
3. Select a location for your database

### Storage (if needed)
1. Go to **Storage** → **Get Started**
2. Start in test mode (for development)
3. Choose a location

## Step 4: Get Your Configuration Values

1. Go to **Project Settings** (gear icon) → **General** tab
2. Scroll down to **"Your apps"** section
3. Click on your web app
4. You'll see your Firebase config object with these values:
   - `apiKey`
   - `authDomain`
   - `projectId`
   - `storageBucket`
   - `messagingSenderId`
   - `appId`

## Step 5: Set Up Environment Variables

1. Copy `.env.example` to `.env`:
   ```bash
   cp .env.example .env
   ```

2. Open `.env` and replace the placeholder values with your actual Firebase config:
   ```
   VITE_FIREBASE_API_KEY=AIzaSy...
   VITE_FIREBASE_AUTH_DOMAIN=your-project-id.firebaseapp.com
   VITE_FIREBASE_PROJECT_ID=your-project-id
   VITE_FIREBASE_STORAGE_BUCKET=your-project-id.appspot.com
   VITE_FIREBASE_MESSAGING_SENDER_ID=123456789
   VITE_FIREBASE_APP_ID=1:123456789:web:abc123
   ```

## Step 6: Use Firebase in Your App

Import Firebase services in your components:

```javascript
import { auth, db, storage } from './firebase/config';
```

### Example: Using Authentication
```javascript
import { signInWithEmailAndPassword } from 'firebase/auth';
import { auth } from './firebase/config';

// Sign in
signInWithEmailAndPassword(auth, email, password)
  .then((userCredential) => {
    console.log('Signed in:', userCredential.user);
  })
  .catch((error) => {
    console.error('Error:', error);
  });
```

### Example: Using Firestore
```javascript
import { collection, addDoc } from 'firebase/firestore';
import { db } from './firebase/config';

// Add a document
const addData = async () => {
  try {
    const docRef = await addDoc(collection(db, 'moments'), {
      title: 'My Moment',
      timestamp: new Date(),
    });
    console.log('Document written with ID:', docRef.id);
  } catch (error) {
    console.error('Error adding document:', error);
  }
};
```

## Security Rules

Don't forget to set up proper security rules in Firebase Console:

- **Firestore**: Go to Firestore Database → Rules
- **Storage**: Go to Storage → Rules

For development, you can use test mode, but **always secure your rules before deploying to production**.

## Troubleshooting

- Make sure your `.env` file is in the root directory
- Restart your dev server after changing `.env` values
- Check that all environment variables start with `VITE_` (required for Vite)
- Verify your Firebase project settings match your `.env` file

