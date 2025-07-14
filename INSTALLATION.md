# 🚀 BD Lead Manager - Installation Guide

## 📋 Prerequisites

Before installing BD Lead Manager, ensure you have the following:

### **Development Environment**
- **Xcode 15.0+**: Latest version of Xcode
- **iOS 15.0+**: Minimum deployment target
- **macOS 13.0+**: Required for Xcode 15
- **Apple Developer Account**: For testing and distribution

### **External Services**
- **Firebase Project**: For authentication and data storage
- **App Store Connect**: For app distribution and subscriptions
- **StoreKit Configuration**: For subscription testing

## 🛠️ Installation Steps

### **Step 1: Clone the Repository**
```bash
git clone <repository-url>
cd bulk3-copy-12
```

### **Step 2: Configure Firebase**

#### **Create Firebase Project**
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Create a new project or select existing project
3. Enable the following services:
   - **Authentication**: Email/password and Apple Sign-In
   - **Firestore Database**: Real-time database
   - **Analytics**: Usage tracking (optional)

#### **Download Configuration File**
1. In Firebase Console, go to Project Settings
2. Download `GoogleService-Info.plist`
3. Add the file to your Xcode project:
   - Drag `GoogleService-Info.plist` into Xcode
   - Ensure it's added to the main target
   - Verify it appears in the project navigator

#### **Configure Firestore Security Rules**
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // User data isolation
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
    
    // Quota tracking
    match /quotas/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
    
    // Subscription data
    match /subscriptions/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

### **Step 3: Configure StoreKit**

#### **Create StoreKit Configuration File**
1. In Xcode, go to File → New → File
2. Select "StoreKit Configuration File"
3. Name it `BDLeads23.storekit`
4. Add your subscription products:

```json
{
  "identifier": "basic_monthly",
  "type": "AutoRenewable",
  "displayName": "Basic Plan - Monthly",
  "description": "500 messages per month with dashboard access",
  "price": "9.99",
  "subscriptionPeriod": "P1M"
}
```

#### **Configure Subscription Products**
Add the following products to your StoreKit configuration:

- **Free Plan**: No product ID (handled in-app)
- **Basic Monthly**: `basic_monthly`
- **Basic Yearly**: `basic_yearly`
- **Pro Monthly**: `pro_monthly`
- **Pro Yearly**: `pro_yearly`

### **Step 4: Configure Xcode Project**

#### **Project Settings**
1. Open `bulk3.xcodeproj` in Xcode
2. Select the main target
3. Configure the following:

**General Tab:**
- **Display Name**: BD Lead Manager
- **Bundle Identifier**: Your unique bundle ID
- **Version**: 1.0.0
- **Build**: 346

**Signing & Capabilities:**
- **Team**: Your Apple Developer Team
- **Bundle Identifier**: Unique identifier
- **Capabilities**: 
  - In-App Purchase
  - Push Notifications (if needed)

#### **Info.plist Configuration**
Ensure your `Info.plist` contains:

```xml
<key>CFBundleURLTypes</key>
<array>
  <dict>
    <key>CFBundleTypeRole</key>
    <string>None</string>
    <key>CFBundleURLName</key>
    <string></string>
    <key>CFBundleURLSchemes</key>
    <array/>
  </dict>
</array>
<key>com.apple.developer.applesignin</key>
<array>
  <string>Default</string>
</array>
```

### **Step 5: Install Dependencies**

#### **Swift Package Manager Dependencies**
The project uses the following dependencies (already configured):

- **Firebase iOS SDK**: Authentication and Firestore
- **SwiftData**: Local data persistence
- **StoreKit**: Subscription management

#### **Verify Dependencies**
1. In Xcode, go to File → Add Package Dependencies
2. Ensure all dependencies are resolved
3. Build the project to verify everything compiles

### **Step 6: Configure App Store Connect**

#### **Create App Record**
1. Go to [App Store Connect](https://appstoreconnect.apple.com/)
2. Create a new app record
3. Configure app metadata:
   - **App Name**: BD Lead Manager
   - **Bundle ID**: Your bundle identifier
   - **SKU**: Unique SKU for your app

#### **Configure Subscription Products**
1. In App Store Connect, go to Features → In-App Purchases
2. Create subscription products matching your StoreKit configuration
3. Configure pricing and availability
4. Submit for review

### **Step 7: Build and Test**

#### **Build the Project**
```bash
# Clean build
xcodebuild clean -project bulk3.xcodeproj -scheme bulk3

# Build for simulator
xcodebuild build -project bulk3.xcodeproj -scheme bulk3 -destination 'platform=iOS Simulator,name=iPhone 15'
```

#### **Run in Simulator**
1. Select iOS Simulator as target device
2. Press Cmd+R to build and run
3. Test the following:
   - App launch and loading
   - Firebase authentication
   - StoreKit subscription testing
   - Core functionality

## 🔧 Configuration Options

### **Environment Configuration**

#### **Development Environment**
```swift
// In BDRAXApp.swift
#if DEBUG
// Development-specific configuration
ProductionLogger.enableDebugLogging()
#endif
```

#### **Production Environment**
```swift
// Disable debug logging for production
ProductionLogger.enableDebugLogging = false
```

### **Firebase Configuration**

#### **Authentication Methods**
Enable the following in Firebase Console:
- **Email/Password**: Standard email authentication
- **Apple Sign-In**: iOS native authentication
- **Anonymous Auth**: Optional for testing

#### **Firestore Collections**
The app uses the following Firestore collections:
- `users`: User profile data
- `quotas`: Message usage tracking
- `subscriptions`: Subscription status
- `analytics`: Usage analytics (optional)

### **StoreKit Configuration**

#### **Testing Subscriptions**
1. Use StoreKit testing in Xcode
2. Configure test accounts in App Store Connect
3. Test subscription flows:
   - Purchase
   - Renewal
   - Expiration
   - Restoration

## 🚨 Troubleshooting

### **Common Issues**

#### **Firebase Configuration**
- **Issue**: Firebase not connecting
- **Solution**: Verify `GoogleService-Info.plist` is properly added
- **Check**: Firebase project settings and bundle ID match

#### **StoreKit Issues**
- **Issue**: Subscriptions not working
- **Solution**: Verify StoreKit configuration file
- **Check**: Product IDs match App Store Connect

#### **Build Errors**
- **Issue**: SwiftData compilation errors
- **Solution**: Ensure iOS 15+ deployment target
- **Check**: Xcode version compatibility

#### **Authentication Issues**
- **Issue**: Firebase auth not working
- **Solution**: Verify Firebase project configuration
- **Check**: Authentication methods enabled in Firebase Console

### **Debug Configuration**

#### **Enable Debug Logging**
```swift
// In BDRAXApp.swift
ProductionLogger.enableDebugLogging()
```

#### **Network Debugging**
```swift
// Check Firebase connectivity
connectivityService.checkFirebaseConnection()
```

## 📱 Testing

### **Unit Testing**
```bash
# Run unit tests
xcodebuild test -project bulk3.xcodeproj -scheme bulk3 -destination 'platform=iOS Simulator,name=iPhone 15'
```

### **UI Testing**
```bash
# Run UI tests
xcodebuild test -project bulk3.xcodeproj -scheme bulk3UITests -destination 'platform=iOS Simulator,name=iPhone 15'
```

### **Manual Testing Checklist**
- [ ] App launches successfully
- [ ] Firebase authentication works
- [ ] User registration and login
- [ ] Subscription purchase flow
- [ ] Core features (Dashboard, Campaigns, CRM, Contacts)
- [ ] Data persistence and sync
- [ ] Offline functionality
- [ ] Error handling

## 🚀 Deployment

### **Archive for App Store**
1. Select "Any iOS Device" as target
2. Product → Archive
3. Distribute App through App Store Connect
4. Submit for review

### **TestFlight Distribution**
1. Upload build to App Store Connect
2. Create TestFlight group
3. Add test users
4. Distribute for beta testing

---

**Follow these steps carefully to ensure BD Lead Manager is properly configured and ready for development and deployment.** 