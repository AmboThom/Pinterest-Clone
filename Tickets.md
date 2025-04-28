# Ticket 1 - Remove Test Credentials from Login Screen

## Type
Bug

## Description
Test credentials are visible on the login screen. This is a privacy concern and must be addressed by removing the exposed information from the UI.

## Solution
**File:** `frontend/screens/LoginScreen.js`  
**Action:** Remove lines 47–52, which display hardcoded test credentials.

### Code to remove:
```jsx
<Text style={styles.testCredentials}>
    Test credentials:{'\n'}
    Email: john@example.com{'\n'}
    Password: password123{'\n\n'}
    Note: Enter the password without any prefix
</Text>
```


# Ticket 3 - Fix Broken or Missing Icons on Main Screen

## Type  
Bug

## Description  
Several icons on the main screen are either missing or incorrectly rendered. This affects the visual consistency and usability of the interface. We need to make sure all expected icons are properly displayed and sourced.

## Solution  
First, let’s check the error being thrown when we run React and open the browser:

```
WARNING in ./node_modules/react-native-paper/lib/module/components/MaterialCommunityIcon.js:37:4
Module not found: Can't resolve 'react-native-vector-icons/MaterialCommunityIcons'
```

This is the first piece of the puzzle — it’s saying it can’t find the files we're trying to use:
```
Module not found: Can't resolve 'react-native-vector-icons/MaterialCommunityIcons'
```

So, let’s fix that first.

### Step 1: Install the required module
Run:
```bash
npm install react-native-vector-icons
```

The app should run a bit smoother now, but the icons still don’t show…

---

### Step 2: Move into `HomeScreen.js`

Let’s go to where the icons are managed. For example:

```jsx
// Lines 94 - 99
<IconButton
  icon="account-circle"
  size={24}
  onPress={() => setMenuVisible(true)}
  iconColor="#E60023"
/>
```

The issue here is how we’re referencing the icons. I changed it to:

```jsx
<IconButton 
  icon={() => <MaterialCommunityIcons name="account-circle" size={24} color="#E60023" />}
  onPress={() => setMenuVisible(true)}  
/>
```

And don’t forget to add this import at the top:
```js
import { MaterialCommunityIcons } from '@expo/vector-icons';
```

This worked because we moved from string-based icon names to using the actual `MaterialCommunityIcons` component. This is better because:
- It ensures the icons actually load
- We get more control over their size, color, etc.
- It’s the recommended way to use icons with `react-native-paper` + Expo

The string method relies on correct mapping to the font files, and when that breaks (like in our case), nothing shows. The component method bypasses that and just works.

---

### Continuing on...

```jsx
// Lines 137 - 143
<FAB
  icon="plus"
  style={styles.fab}
  onPress={() => navigation.navigate('CreatePin')}
  color="#fff"
  backgroundColor="#E60023"
/>
```

Change to:

```jsx
<FAB
  icon={() => <MaterialCommunityIcons name="plus" size={24} color="#fff" />}
  style={styles.fab}
  onPress={() => navigation.navigate('CreatePin')}
  customSize={56}
/>
```

**Note:** `backgroundColor` was moved into the `styles.fab` section.

---
# Ticket 9
### Other changes:

```jsx
// Line 108
leadingIcon="account"
// to:
leadingIcon={() => <MaterialCommunityIcons name="account" size={24} color="#000" />}

// Line 116
leadingIcon="cog"
// to:
leadingIcon={() => <MaterialCommunityIcons name="cog" size={24} color="#000" />}

// Line 124
leadingIcon="logout"
// to:
leadingIcon={() => <MaterialCommunityIcons name="logout" size={24} color="#000" />}
```

And finally…

```jsx
// Line 88
iconColor="#E60023"
// was just setting a color with no icon — replace it with:
icon={() => <MaterialCommunityIcons name="magnify" size={24} color="#E60023" />}
```
