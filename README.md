# CavyTester

The `CavyTester` app exercises the [Cavy API](https://github.com/pixielabs/cavy)
in a native mobile environment to test against regressions and make it safer to
modify existing features or add new ones.

You will need to install [Cavy CLI](https://github.com/pixielabs/cavy-cli) to
run the tests.

## ⚙️ Running with Cavy in Development

CavyTester can be run on its own to demonstrate Cavy running in a sample React
Native app. However, it's also valuable to have it run against a local version
of the `cavy` library when testing changes to Cavy itself. To do so, take the
following steps:

[TODO - these steps need testing]

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