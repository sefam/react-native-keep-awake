This React Native package allows you to prevent the screen from going to sleep while your app is active. It's useful for things like navigation or video playback, where the user expects the app to remain visible over long periods without touch interaction.

## Installation

As the first step, install this module:

```
npm install --save react-native-keep-awake
```

### RNPM

The plugin can be installed using [rnpm](https://github.com/rnpm/rnpm).

```
rnpm link react-native-keep-awake
```

After RNPM finishes, you will have to make a small change to your `MainActivity.java` for android support:

```java
// MainActivity.java, inside the getPackages() method:

    // old
    new KeepAwakePackage(),

    // new
    new KeepAwakePackage(this),
```

### Manual install

#### iOS

1. In XCode, in the project navigator, right click `Libraries` ➜ `Add Files to [your project's name]`
2. Go to `node_modules` ➜ `react-native-keep-awake` and add `KCKeepAwake.xcodeproj`
3. In XCode, in the project navigator, select your project. Add `libKCKeepAwake.a` to your project's `Build Phases` ➜ `Link Binary With Libraries`
4. Click `KCKeepAwake.xcodeproj` in the project navigator and go the `Build Settings` tab. Make sure 'All' is toggled on (instead of 'Basic'). In the `Search Paths` section, look for `Header Search Paths` and make sure it contains both `$(SRCROOT)/../../react-native/React` and `$(SRCROOT)/../../../React` - mark both as `recursive`.


#### Android

1. Open up `android/app/src/main/java/[...]/MainActivity.java
  - Add `import com.corbt.keepawake.KeepAwakePackage;` to the imports at the top of the file
  - Add `new KeepAwakePackage(this)` to the list returned by the `getPackages()` method

3. Append the following lines to `android/settings.gradle`:

	```groovy
	include ':react-native-keep-awake'
	project(':react-native-keep-awake').projectDir = new File(rootProject.projectDir, 	'../node_modules/react-native-keep-awake/android')
	```

4. Insert the following lines inside the dependencies block in `android/app/build.gradle`:

	```groovy
  compile project(':react-native-keep-awake')
  ```

## Usage

There are two ways to use this package: by rendering it as a component, or by explicitly calling
the `KeepAwake.activate()` and `KeepAwake.deactivate()` static methods. Both are demonstrated below.

```jsx

import React, { Component } from 'react';
import { View, Text } from 'react-native';

import KeepAwake from 'react-native-keep-awake';

// Method 1
const MyComponent extends Component {
  render() {
    if (this.props.screenShouldBeAwake) {
      return (
        <View>
          <Text>Screen will be kept awake</Text>
          <KeepAwake />
        </View>
      )
    } else {
      return (
        <View>
          <Text>Screen can sleep</Text>
        </View>
      );
    }
  }
}

// Method 2
function changeKeepAwake(shouldBeAwake) {
  if (shouldBeAwake) {
    KeepAwake.activate();
  } else {
    KeepAwake.deactivate();
  }
}

```

In method 1, the screen will be locked awake when the `<KeepAwake />` component is mounted, and the lock will be disabled when the component is unmounted. Method 2 gives you more explicit control of when the lock is active or not.