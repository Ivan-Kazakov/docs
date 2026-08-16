# Google Play Data Safety — Zen Blocks Sudoku

**Last reviewed:** August 16, 2026
**Package:** `com.btrtgds.zenblocksudoku`

A working reference for filling in **Play Console → App content → Data safety**, and for the neighbouring
App content sections that the form depends on. It records not just the answers but *why* each one is what
it is, so the next review does not have to derive them again — and so the answers stay in step with
[PRIVACY.md](PRIVACY.md), which is written for players and says the same things in prose.

This is a developer reference, not a legal document. Google's guidance is the authority; the developer is
solely responsible for the answers submitted.

---

## Why there is no Google sign-in

The Game has no accounts and no sign-in, and the ad-free unlock does not need one.

A one-time purchase made through Google Play Billing is recorded by Google Play against the Google account
signed in on the device. The Game asks Play "does this account own `remove_ads` for this package?" and Play
answers — that query *is* the authentication, and Play performs it, not the Game. It runs at launch, on every
return from the background, and behind the Restore button in Settings, so reinstalling the Game or moving to a
new device restores the unlock with no action from the player and no account of ours.

The answer is cached on the device so an owner can start ad-free with no network. The cache is never the
authority: a query that *fails* leaves it untouched, and only a successful query that says "not owned" (a
refund) can clear it. Revoking a paid unlock because the network was down is the failure mode the design
exists to prevent.

Adding Google Sign-In would create a second, weaker source of truth for something Play already answers
authoritatively, and would add a user identifier to declare in the form below. The Play Billing API's
`setObfuscatedAccountId` / `setObfuscatedProfileId` are deliberately not used, for the same reason — they
exist for games with their own servers and their own accounts.

---

## What the app actually does with data

- **No servers of ours, no analytics, no crash reporting, no accounts.**
- The only component that sends anything off the device is the **Google Mobile Ads SDK** (banner + optional
  rewarded video), together with the **User Messaging Platform** consent SDK that gates it.
- **Google Play Billing** talks to Google Play, not to us. We learn one bit: whether the unlock is owned.
- Everything the Game itself saves — the unfinished game, the best score, the settings, the cached ownership
  bit — is written to two files on the device and never leaves it.

Google Play requires data collected by third-party SDKs to be declared exactly as if the developer collected
it. So the entire declaration below is the Mobile Ads SDK's.

---

## Data safety form

**Does your app collect or share any of the required user data types?** → **Yes**

### Data types to declare

All five rows are the Google Mobile Ads SDK. All are both *collected* and *shared*, all carry the same three
purposes, and none is processed ephemerally.

| Play form category | Data type | Collected | Shared | Required or optional | Purposes |
|---|---|---|---|---|---|
| Location | Approximate location | Yes | Yes | Required | Advertising or marketing; Analytics; Fraud prevention, security, and compliance |
| App activity | App interactions | Yes | Yes | Required | same three |
| App info and performance | Crash logs | Yes | Yes | Required | same three |
| App info and performance | Diagnostics | Yes | Yes | Required | same three |
| Device or other IDs | Device or other IDs | Yes | Yes | Optional — "Users can choose whether this data is collected" | same three |

Where each row comes from — Google publishes the SDK's disclosure as four items, which map onto the form's
categories like this:

- **IP address → Approximate location.** The form has no "IP address" type. AdMob derives coarse geography
  from the IP address, so Approximate location is the conservative and defensible answer. *This is the one
  row that is a judgement call rather than a direct restatement of Google's table.*
- **User product interactions → App interactions.**
- **Diagnostic information → Crash logs + Diagnostics.** Declared because the Game does not call the SDK's
  `disableSDKCrashReporting`; if that ever changes, Crash logs can be revisited.
- **Android ad ID and app set ID → Device or other IDs.** Marked *optional* because the UMP consent form gives
  players in the EEA, UK and Switzerland a real choice, and because the advertising ID can be reset or turned
  off in Android's own settings.

### Other questions on the form

| Question | Answer | Why |
|---|---|---|
| Is all of the user data collected by your app encrypted in transit? | **Yes** | Google states the Mobile Ads SDK encrypts everything it sends over TLS. |
| Do you provide a way for users to request that their data be deleted? | **No** | There are no accounts and no server-side data of ours to delete. Uninstalling removes everything stored locally; the advertising identifier is reset in Android settings. |
| Privacy policy URL | Link to [PRIVACY.md](PRIVACY.md) | Must be reachable and must match this declaration. |

### What is deliberately **not** declared

- **The two files the Game writes on the device** — `settings.cfg` (settings, best score, cached ad-free bit)
  and `savegame.cfg` (board, score, mode). Google exempts "user data accessed by your app that is only
  processed locally on the user's device and not sent off device". Nothing in either file leaves the device.
- **Purchase history and payment information.** Payment is handled end to end by Google Play, which is the
  payment-processing exemption; and the Game only reads the ownership answer from Play and caches it locally,
  which is the on-device exemption. There is no server of ours that receives, stores or analyses purchase
  data. If a reviewer ever queries this, that is the answer.
- **The vibration permission.** A permission, not a data type.

---

## Neighbouring App content sections

The Data safety form is not accepted in isolation; these must agree with it.

| Section | Answer |
|---|---|
| **Advertising ID** | **Yes**, the app uses an advertising ID. Purposes: advertising or marketing; analytics; fraud prevention, security and compliance. The Mobile Ads SDK declares `com.google.android.gms.permission.AD_ID` in the manifest itself, and a mismatch between that permission and this answer is a common rejection. |
| **Ads** | **Yes**, the app contains ads (banner + optional rewarded video). |
| **Target audience and content** | **13 and over**, not directed to children. Declaring a child age band would pull the app into the Families policy and require child-directed treatment in AdMob. Matches PRIVACY.md. |
| **Content rating questionnaire** | Declare that the app contains advertising and offers in-app purchases. |
| **Financial features** | None (the ad-free unlock is a normal in-app purchase, not a financial feature). |
| **Government apps / News** | No. |
| **Data deletion** | Not applicable — no accounts. |

---

## Release checklist

The declarations above describe a build that actually ships the ad and billing code. Before the store build:

1. **Both plugins enabled** in `project.godot` → `[editor_plugins] enabled=` (AdMob and GodotGooglePlayBilling),
   and the **Android Build Template installed** — without either, the AAB carries no ad or billing library and
   the Game silently runs with no ads and nothing for sale, which would make this entire declaration wrong in
   the opposite direction.
2. **AdMob app id** — `admob/general/android/app_id` set to the real id from the AdMob console (it defaults to
   Google's public *test* app id).
3. **Live ad units** — `AdUnits.USE_TEST_UNITS = false` and `LIVE_BANNER` / `LIVE_REWARDED` filled in. Live ads
   in a debug build are invalid traffic and get AdMob accounts suspended, so this flag is flipped only for the
   build that goes to the store.
4. **`remove_ads`** created *and activated* as a one-time managed product in Play Console — otherwise the price
   lookup returns nothing and the purchase button never appears.
5. **`version/code`** increased in the store export preset. Google Play rejects an upload whose versionCode is
   not higher than the last published one.
6. **Verify the pack** contains `addons/GodotGooglePlayBilling/BillingClient.gd` and
   `addons/admob/gdscript/src/api/MobileAds.gd`, then verify the built AAB's merged manifest contains
   `android.permission.INTERNET`, `com.android.vending.BILLING`, `com.google.android.gms.permission.AD_ID` and
   the AdMob `APPLICATION_ID` meta-data.

---

## Sources

- [Google Play data disclosure — Google Mobile Ads SDK (Android)](https://developers.google.com/admob/android/privacy/play-data-disclosure)
- [Provide information for Google Play's Data safety section](https://support.google.com/googleplay/android-developer/answer/10787469)
- [Zen Blocks Sudoku Privacy Policy](PRIVACY.md)
