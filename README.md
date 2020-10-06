# CavyTester

The `CavyTester` app exercises the [Cavy API](https://github.com/pixielabs/cavy)
in a native mobile environment to test against regressions and make it safer to
modify existing features or add new ones.

You will need to install [Cavy CLI](https://github.com/pixielabs/cavy-cli) to
run the tests.

## ⚙️ Running with Cavy in Development

`CavyTester` can be run on its own to demonstrate Cavy running in a sample React
Native app. However, it's also valuable to have it run against a local version
of the `cavy` library when testing changes to Cavy itself. To do so, take the
following steps:

1. Clone the [Cavy repo](https://github.com/pixielabs/cavy):

    `git clone https://github.com/pixielabs/cavy.git`

2. Clone this repo:

    `git clone https://github.com/pixielabs/cavy-tester.git`

3. Install dependencies:

    `cd cavy-tester`

    `yarn install`

3. Install [wml](https://github.com/wix/wml#readme) - an alternative to
  symlinking (which React Native does not support):

    `yarn global add wml`

4. Link your local version of Cavy to `CavyTester`'s node modules:

    `wml add {PATH_TO_CAVY} node_modules/cavy`

5. Install [CocoaPods](https://guides.cocoapods.org/using/getting-started.html),
  then:

    `cd ios`

    `pod install`

    `cd ..`

6. Run the tests!

    `cavy run-ios --dev`

    or

    `cavy run-android --dev`

You should then be able to make changes to your local version of Cavy, whilst
running CavyTester tests in the background 🎉

## ✏️ Adding new Tests

1. Create a new file under `src/scenarios` with a concise name describing your
   scenario. You can copy `src/scenarios/exists.js` as a starting point.
2. Export the following variables from your file:
    - `key`: A unique identifier for your set of tests. You can probably just
      use the filename.
    - `Screen`: A React component which renders the UI necessary to exhibit the
      behavior you're testing.
    - `label`: A short description of your scenario. Used for labeling the
      scenario in `CavyTester`'s menu.
    - `spec`: A Cavy spec which validates your scenario.
3. In `scenarios/index.js`, import your scenario and add it to the `scenarios`
  array

Example `CavyTester` scenario for the `spec.exists` functionality:

```jsx
// exists.js
import React from 'react';
import { Text } from 'react-native';
import { useCavy } from 'cavy';

// `key` is descriptive, concise, and alphanumeric
export const key = 'Exists';

const testId = `${key}.element`; // uses `key` as testId namespace

// `Screen` component is as minimal as necessary to validate scenario
export const Screen = () => {
  const generateTestHook = useCavy();
  return <Text ref={generateTestHook(testId)}>I am text that is present</Text>;
};

// `label` describes the scenario being tested
export const label = 'spec.exists checks for element';

export const spec = spec =>
  // uses `key` for `describe` string
  spec.describe(key, () =>
    // uses `label` for `it` string
    spec.it(label, async () => {
      // validate scenario
      await spec.exists(testId);
    })
  );

```

## 🎉 Shout outs

Thanks goes to [Ryan Stelly](https://github.com/FLGMwt) for `CavyTester`'s
original design and implementation 🙌