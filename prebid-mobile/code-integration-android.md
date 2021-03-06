---
layout: page
title: Code Integration
description: Code Integration
pid: 1
top_nav_section: prebid-mobile
nav_section: prebid-mobile-android
---

<div class="bs-docs-section" markdown="1">

# Code Integration for Android

{:.no_toc}

Get started with Prebid Mobile by creating a [Prebid Server account]({{site.github.url}}/prebid-mobile/prebid-mobile-pbs.html).

### Use Maven?

Easily include the Prebid Mobile SDK using Maven. Simply add this line to your gradle dependencies:

```
compile 'org.prebid:prebid-mobile-sdk:0.0.1'
```

### Build framework from source

Build Prebid Mobile from source code. After cloning the repo, from the root directory run

```
./buildprebid.sh
```

to output the PrebidMobile framework for Android.

## Ad Unit Setup for Android
{:.no_toc}

Register Prebid Mobile ad units as early as possible in the application's lifecycle. Each ad unit has an `adUnitId` which is an arbitrary unique identifier of the developer's choice.

The steps for using Prebid Mobile are as following:

1. Create the ad units with ad unit ids and add sizes for banner ad units
2. Add a server side configuration for each ad unit to Prebid Server Adapter
3. Set targeting parameters for the ad units (Optional)
4. Register the ad units with the adapter to start bid fetching process

### How to create ad units?

Create the ad units that represent the ad spaces in your app using following APIs:

```java

ArrayList<AdUnit> adUnits = new ArrayList<AdUnit>();
 
// Configure a Banner Ad Unit with size 320x50
BannerAdUnit adUnit1 = new BannerAdUnit("YOUR-AD-UNIT-ID-HERE", "YOUR-CONFIG-ID-HERE");
adUnit1.addSize(320, 50);
 
// Configure an Interstitial Ad Unit
InterstitialAdUnit adUnit2 = new InterstitialAdUnit("YOUR-INTERSTITIAL-AD-UNIT-ID-HERE", "YOUR-INTERSTITIAL-CONFIG-ID-HERE");
 
// Add them to the list 
adUnits.add(adUnit1);
adUnits.add(adUnit2);

```

### Initialize the SDK

Once configuration is done, use the following API to initialize Prebid Mobile and start fetching Prebid ads for your list of ad units:

```java
// Register ad units for prebid.
try {
    Prebid.init(getApplicationContext(), adUnits, "YOUR-ACCOUNT-ID-HERE");
} catch (PrebidException e) {
    e.printStackTrace();
}
```

## Set Ad Server Targeting
{:.no_toc}

The final step for implementing Prebid Mobile is to attach bid keywords on the ad object. You can either attach bids immediately or wait for ads before attaching bids. To attach bids immediately use the following API.

```java
Prebid.attachBids(YOUR-AD-REQUEST-HERE, YOUR-AD-UNIT-ID-HERE, this.getActivity());
```

To wait for ads before attaching bids, implement the following listener.

```java
@Override
public void onAttachComplete(Object adObj) {
    if (adView2 != null && adObj != null && adObj instanceof PublisherAdRequest) {
        adView2.loadAd((PublisherAdRequest) adObj);
        Prebid.detachUsedBid(adObj);
    }
}
```

Prebid Mobile will immediately tell your app whether it has a bid or not without waiting. If it does have a bid, the code below will attach the bids to the ad request by applying keyword targeting. Use the table below to see which ad objects are supported currently.

{: .table .table-bordered .table-striped }
| Primary Ad Server | Ad Object Type | Ad Object                  | Load Method                                        |
|-------------------|----------------|----------------------------|----------------------------------------------------|
| DFP               | Banner         | `PublisherAdView`          | `public void loadAd(PublisherAdRequest adRequest)` |
| DFP               | Interstitial   | `PublisherInterstitialAd`  | `public void loadAd(PublisherAdRequest adRequest)` |
| MoPub             | Banner         | `MoPubView`                | `public void loadAd()`                             |
| MoPub             | Interstitial   | `MoPubInterstitial`        | `public void load()`                               |



</div>