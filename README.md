<<<<<<< HEAD
# CometChat UIKit Builder - React Native

<p align="center">
  <img alt="CometChat" src="https://assets.cometchat.io/website/images/logos/banner.png">
</p>

CometChat UIKit Builder is a powerful React Native application that allows you to quickly configure and customize your CometChat-powered app by scanning a QR code. The scanned QR code includes theme settings, UI configurations, feature toggles, and other app-level preferences that will automatically be applied to your app.

## ✨ Features

📱 **QR Code Scanning**: Easily scan QR codes to fetch and apply app configurations instantly  
🎨 **Dynamic Theme Customization**: Apply custom themes, colors, and typography styles in real-time  
🔧 **Feature Configuration**: Enable/disable chat features, calling capabilities, and UI components  
📋 **Layout Management**: Configure sidebar, tabs, and chat type preferences  
⚡ **Seamless Integration**: Works directly with CometChat's React Native UI Kit  
🔄 **Real-time Updates**: Instantly apply scanned configurations without app restart  
💾 **Persistent Settings**: Automatically save and restore configurations using AsyncStorage  

## 🛠 Requirements

- **Node.js** 18 or higher
- **React Native** 0.77 or later
- **iOS**: iOS 12.0+, Xcode 14+, CocoaPods
- **Android**: Android 5.0+, Android Studio

## 📦 Installation

### 1. Download the Code
Download the source code from the repository and extract it to your desired location.

### 2. Install Dependencies
```bash
cd uikit-builder-app-react-native
npm install
```

### 3. iOS Setup
```bash
cd ios
pod install
cd ..
```

### 4. Configure CometChat Credentials
Open `src/utils/AppConstants.tsx` and add your CometChat credentials:

```typescript
export const AppConstants = {
  appId: 'YOUR_APP_ID',
  authKey: 'YOUR_AUTH_KEY', 
  region: 'YOUR_REGION',
  // other properties...
}
```

### 5. Run the Application
```bash
# Start Metro bundler
npm start

# Run on iOS
npm run ios

# Run on Android  
npm run android
```

## 🚀 Integration in Your Existing React Native App

To integrate the UIKit Builder configuration system into your existing React Native app:

### 1. Install Required Dependencies
```bash
npm install zustand @react-native-async-storage/async-storage
```

### 2. Copy Configuration Files
Copy these two essential files from the downloaded code to your existing React Native project:

- `src/config/store.ts` - Configuration store management
- `src/config/config.json` - Default configuration template

Place them in your project at the same path: `yourProject/src/config/`

### 3. Import and Use Configuration Store

In your main App component, import the configuration store:

```typescript
import React from 'react';
import { CometChatThemeProvider } from '@cometchat/chat-uikit-react-native';
import { useConfig } from './src/config/store';

const App = () => {
  // Get theme configuration from store
  const styleConfig = useConfig(state => state.settings.style);
  
  // Create theme object based on configuration
  const theme = {
    light: {
      color: {
        primary: styleConfig.color.brandColor,
        textPrimary: styleConfig.color.primaryTextLight,
        textSecondary: styleConfig.color.secondaryTextLight,
      },
      // Apply typography and other settings
    },
    dark: {
      color: {
        primary: styleConfig.color.brandColor,
        textPrimary: styleConfig.color.primaryTextDark,
        textSecondary: styleConfig.color.secondaryTextDark,
      },
    },
  };

  return (
    <CometChatThemeProvider theme={theme}>
      {/* Your existing app components */}
    </CometChatThemeProvider>
  );
};
```

### 4. Access Feature Configurations

In your existing CometChat components, you can access and apply feature configurations from the store. The configuration will automatically control which features are enabled or disabled based on the settings in `config.json` or any scanned QR code data.

## 📁 Project Structure

```
src/
├── components/
│   ├── conversations/     # Chat and messaging components
│   ├── calls/            # Voice/video calling components  
│   ├── groups/           # Group management components
│   ├── users/            # User management components
│   └── login/            # Authentication components
├── config/
│   ├── store.ts          # Zustand store for configuration
│   └── config.json       # Default configuration template
├── navigation/           # App navigation setup
├── utils/               # Helper utilities and constants
└── assets/              # Icons and images
```

## ⚙️ Configuration Schema

The QR code should contain a JSON object with the following structure:

```json
{
  "builderId": "unique-builder-id",
  "settings": {
    "chatFeatures": {
      "coreMessagingExperience": {
        "typingIndicator": true,
        "threadConversationAndReplies": true,
        // ... other messaging features
      },
      "deeperUserEngagement": {
        "mentions": true,
        "reactions": true,
        // ... engagement features  
      }
    },
    "callFeatures": {
      "voiceAndVideoCalling": {
        "oneOnOneVoiceCalling": true,
        "groupVideoConference": true,
        // ... calling features
      }
    },
    "layout": {
      "withSideBar": true,
      "tabs": ["chats", "calls", "users", "groups"],
      "chatType": "both"
    },
    "style": {
      "theme": "system",
      "color": {
        "brandColor": "#6852D6",
        "primaryTextLight": "#141414",
        // ... color configuration
      },
      "typography": {
        "font": "roboto",
        "size": "default"
      }
    }
  }
}
```

## 🎯 Use Cases

- **App Customization**: Allow users to personalize their chat experience
- **Brand Consistency**: Apply company themes and colors instantly
- **Feature Testing**: Enable/disable features for A/B testing
- **Client Demos**: Quickly showcase different configurations to clients
- **Theme Marketplace**: Share and distribute custom themes via QR codes

## 📱 Sample QR Codes

You can generate QR codes containing configuration JSON to test different themes and features. Use any QR code generator with the JSON configuration as the content.

## 🔧 Advanced Configuration

### Custom Font Integration

1. Add your custom font files to:
   - iOS: `ios/YourApp/Resources/Fonts/`
   - Android: `android/app/src/main/assets/fonts/`

2. Update the font mapping in `App.tsx`:

```typescript
const FONT_MAP = {
  'your-custom-font': {
    regular: Platform.OS === 'ios' ? 'YourFont-Regular' : 'your_font_regular',
    medium: Platform.OS === 'ios' ? 'YourFont-Medium' : 'your_font_medium',
    bold: Platform.OS === 'ios' ? 'YourFont-Bold' : 'your_font_bold',
  }
};
```

### Persistent Configuration

The app automatically saves scanned configurations to AsyncStorage. To manually manage persistence:

```typescript
import AsyncStorage from '@react-native-async-storage/async-storage';
import { useConfigStore } from './src/config/store';

// Save current config
const saveConfig = async () => {
  const config = useConfigStore.getState().config;
  await AsyncStorage.setItem('@app_config', JSON.stringify(config));
};

// Load saved config
const loadConfig = async () => {
  const savedConfig = await AsyncStorage.getItem('@app_config');
  if (savedConfig) {
    useConfigStore.getState().updateConfig(JSON.parse(savedConfig));
  }
};
```

## 📖 Help and Support

For issues running the project or integrating with our UI Kits:

- 📚 **Documentation**: [CometChat React Native UI Kit](https://www.cometchat.com/docs/ui-kit/react-native/5.0/getting-started)
- 🎫 **Support Tickets**: [Create a Support Ticket](https://help.cometchat.com/hc/en-us)
- 💬 **Real-time Support**: [CometChat Dashboard](http://app.cometchat.com/)
- 🐛 **Issues**: [GitHub Issues](https://github.com/cometchat-team/uikit-builder-app-react-native/issues)

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

⚡ **With CometChat UIKit Builder, give your users the power to instantly configure your app's theme, features, and settings just by scanning a QR code!**
=======
Sakshi Ashtekar Assignment Cometchat - 
Findings are from MAC Chrome browser - 
1. Dashboard - 
https://drive.google.com/file/d/12rEv3oYcpetz1TAeTBxj3OFvWqKPyHNp/view?usp=sharing
- Navbar:
Expected: The navbar should have a proper, consistent height and adequate spacing (bottom margin/padding) to clearly separate it from the main dashboard content. 
Actual: The navbar visually appears to lack a bottom margin, causing it to merge with the content below the "Get Started / Integrate" section i.e. the Dashboard .
Bugs or Errors: The lack of clear separation between the fixed header (navbar) and the scrollable content creates a cluttered appearance. This is likely a CSS error in the header component's styling (missing margin-bottom, padding-bottom, or an improperly set z-index).
Suggestions for Improvement: Implement a border-bottom or a sufficient margin-bottom on the header/navbar component. 

- Dashboard Content Display and Scrolling:
Expected: The main dashboard content should be fully visible within the viewport at 100% resolution, or if the content is taller than the viewport, only the content area should be scrollable (not the entire window). 
Actual: The dashboard content is cut off from the top (likely behind the header/navbar) and from the bottom. The dashboard itself is scrollable, but the content inside is not. 
Bugs or Errors: This suggests a CSS layout bug related to - Missing padding-top or margin-top on the main content area or Viewport Sizing - The main content's container is likely set to a fixed or max-height that is smaller than the required content, and the scroll behavior is not set up correctly.
Suggestions for Improvement - Add a padding-top to the main content wrapper equal to the navbar's total height to prevent the content from sliding underneath the fixed header.




2. UI kit builder - React Native -
- Sidebar and Chat window Trimming:
https://drive.google.com/file/d/1-Yvgtekm_8a1Hr2vwDMpcPN-RyVmLfQz/view?usp=sharing
https://drive.google.com/file/d/1BUilwsLcUJp_SvEiMG7-8Hkga6sAObMa/view?usp=sharing
Expected: 
   1. The left sidebar, containing the configuration options (Layout, Tabs, Users, Groups, etc.), should be scrollable if its content exceeds the viewport height. 
   2. Both the left configuration sidebar and the right theme sidebar should be collapsable or have a    reducible width option to give more space to the main preview area (the chat window).
   3. The central chat preview window should display its content fully without any trimming from left or right.
Actual: 
   1. The content at the bottom of the sidebar (e.g., the 'Groups' card-group) is not fully visible, indicating a lack of scrollability for the container or that the content is being cut off. 
   2. The sidebars appear to be fixed-width, with no visible controls to minimise them or close them causing the main chat window to hide behind them for both with with sidebar and without sidebar options. 
   3. The chat window in the middle is cut off from both the left and right sides. The 'Chats' list and the messages window appears trimmed.
Bugs or Errors: 
    1. This suggests a CSS overflow issue. 
    3. This is a CSS layout or responsiveness bug.
Suggestions for Improvement: 
   1. A scrollbar should appear when content is overflowing. 
   2. Add a small button (e.g., an arrow/chevron icon) near the edge of each sidebar to allow the user to collapse it into a minimised state or fully hide it.
   3. Implement Flexbox/Grid Layout: The central preview should dynamically take up 100% of the remaining available width after the fixed-width sidebars are considered.

- New group modal placement in UI kit builder:
https://drive.google.com/file/d/1Ka3eJIsR7aYOcReEFXGKwzZYsPthLCNn/view?usp=sharing
 Expected: The "New Group" modal should be perfectly centered and fully visible within its viewing area (or the app's screen).  It should contain a field to upload group image. 
 Actual: The modal is incorrectly positioned, suggesting the modal's container fails to center or constrain itself correctly to the screen boundaries. No option to upload group’s Dp/Image.
 Bugs or Errors: This is a Positioning/Centering Bug (CSS layout).



3. Documentation -
https://drive.google.com/file/d/1g6sylmTymSV-uTMGMKUbrsvioFkB2Kju/view?usp=sharing
Expected: The generated README should include clear, step-by-step instructions for the necessary Xcode project configuration. 
Actual: The documentation has missing steps to properly configure the iOS project, specifically the need to change the Bundle Identifier from the default CometChat name to a unique value to resolve Xcode error 70 which I encountered while building the exported UI kit builder.





5. Actual UI Kit implementation -
Missing Search Bar on Export: 
https://drive.google.com/file/d/1s63q8-UZibmMUDXy53j0V_VAjDvI8ylg/view?usp=sharing
Expected - When the UI Kit is exported, all features and UI elements visible in the builder, including the Search Bar, should be present and functional.
Actual - The Search Bar is visible in the UI Kit builder environment but is completely absent in the exported code when the application is run.
Bugs or Errors:  The Search component itself (or its integration logic) may not have been correctly included in the final exported bundle or project structure.


- Group image(DP) missing on groups tab:
https://drive.google.com/file/d/1dLSlsSdldyMq7TFSQfYWmOzjVQz0o2lD/view?usp=sharing
https://drive.google.com/file/d/1s63q8-UZibmMUDXy53j0V_VAjDvI8ylg/view?usp=sharing
 Expected: Group images should render consistently across all views that list groups (Chats Tab and Groups Tab) if an image is available. 
 Actual: The Chats Tab successfully renders the 'Hiking Group' image while the Groups Tab (which lists groups specifically) renders a placeholder/text initials ('HI') instead of the actual image for the same group.
 Suggestions for Improvement: 1. Ensure both the Chats list and the Groups list utilise the exact same reusable component for displaying group list items to guarantee consistency in rendering logic. 
   2. Trace the data flow to verify that the group object being passed to the Groups Tab list item contains the icon or imageUrl field.
   3. Verify the conditional logic that switches between displaying the image and displaying the initials (‘HI’).


- Group Leave and automatic Rejoin:
https://drive.google.com/file/d/1rilGOKrXTrv9DjY7hS35rmO6juIrrveN/view?usp=sharing - 
Expected: When a user leaves a group, the group should immediately disappear from both the Chats Tab and the Groups Tab. The user should only be able to rejoin via an explicit action (like accepting an invite or clicking a join button, depending on the group type). 
Actual: The group successfully disappears from the user's Chats Tab (correct behavior), but it remains visible on the Groups Tab (bug 1). When the user clicks on the group in the Groups Tab, they are automatically added back to the group without confirmation (bug 2).
Bugs or Errors: 
Bug 1: The client-side logic is failing to remove the group object from the local data store that feeds the Groups Tab when the leaveGroup API call succeeds. 
Bug 2: Security/Logic Flaw (Auto-Rejoin): When a user clicks a group they are not a member of (the group in the Groups Tab is stale data), the app is executing an unconditional joinGroup API call instead of checking the membership status or prompting the user to join.
Suggestions for improvement:
Bug 1: Update all local and global data after a action like leaving the group is performed.
Bug 2: Implement Membership Check: When a group item is clicked from the Groups Tab, the app must first check the user's membership status: 
If the user is a member -> open the chat window.
If the user is not a member -> display a "Join Group" button or a confirmation prompt before executing the joinGroup API call.


Admin kick reversal: 
https://drive.google.com/file/d/11Z1isvNhgl_d0MjklcNoZ-5F4P-S0Pw3/view?usp=sharing - 
Expected: When an Admin kicks a member, that member should not be allowed to rejoin until permitted from the admin and prevented from rejoining by simply clicking the group item again even though the group might be public. Rejoining should require an explicit "Join" action.
Actual: The kicked person is able to automatically enter the group again just by re-clicking the group in the Groups list.
Bugs or Errors: The application fails to update the user's membership status after the Admin's kickMember API call. 
Suggestions for improvement: 
Public Groups: Kicked members might be allowed to rejoin, but the default action should still be a clear "Join" button or confirmation, not an automatic join.
2.   When the user clicks a group item from the list, the action must first confirm that the user is a current member before opening the chat. If they are not a member (or have been kicked/banned), the group screen should display the appropriate "Banned" or "Join Group" status.



Unclear feature -
Not sure what the feature change app credentials does here as even if I enter my app credentials from comet chat, it still shows the same default data. 
>>>>>>> f58c20410ce73a9ba415a06804295dda2232ae29
