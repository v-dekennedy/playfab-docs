---
title: PlayFab Party Release Notes
author: ScottMunroMS
description: Release notes for PlayFab Party
ms.author: scmunro
ms.date: 06/22/2021
ms.topic: article
ms.prod: playfab
keywords: playfab, party, release notes, multiplayer, networking
ms.localizationpriority: medium
---

# PlayFab Party release notes

PlayFab Party had a significant (up to 90%) price drop on 10/13/2020. You can view the updated Party rates on the [Pricing page](https://playfab.com/pricing). For more information about the price drop, see our [blog post](https://blog.playfab.com/blog/starting-today-save-up-to-90-using-playfab-party).

## 1.7.5

### Bug fixes

- Fixed an issue where some 16kHz microphones were not working.
- Fixed an issue where microphone permission changes were not handled on Windows.
- Fixed a memory leak in some `PartyManager::CreateNewNetwork()` failure conditions.
- Fixed an occasional crash in `PartyLocalEndpoint::GetEndpointStatistics()`.

## 1.7.0

### New profiling hooks and chat control indicators

- Developers interested in where time is spent in internal library functions can now install optional method entrance and exit callbacks to hook into their preferred high-performance instrumentation method. For more information, see [`PartyManager::SetProfilingCallbacksForMethodEntryExit`](reference/classes/PartyManager/methods/partymanager_setprofilingcallbacksformethodentryexit.md). In this release the callbacks are only supported for Windows, Xbox One XDK, and Microsoft Game Core platforms.
- New NoRemoteInput and RemoteAudioInputMuted chat control indicators provide more granularity on currently silent audio state. For more information, see [`PartyChatControlChatIndicator`](reference/enums/partychatcontrolchatindicator.md).

### Endpoint message behavior improvements

- The ['PartySendMessageQueuingConfiguration'](reference/structs/partysendmessagequeuingconfiguration.md) `timeoutInMilliseconds` field is now also evaluated by the transparent cloud relay if forced to queue messages before forwarding because of differing network conditions or responsiveness of the remote targets.
- When fully authenticated into a network, sending to a 0-entry array will correctly target the exact set of all remote endpoints the library currently reports in the network at that time. Endpoints being created while the message is in the process of transmitting are no longer potentially included.

## 1.6.1

### Bug fixes

- Fixed a bug where a crash may occur when a chat control is connected to a network while that same chat control is disconnecting from another network.

## 1.6.0

### New thread control and text moderation features

- The library's work can now be run manually on game-controlled threads. For more information, see [`PartyManager::SetWorkMode`](reference/classes/PartyManager/methods/partymanager_setworkmode.md).
- Offensive text chat can now optionally be filtered. For more information, see [Using text moderation](concepts-text-moderation.md).

### Explicit enum numbering in the header

- Enum values in the header now have explicit numbering.

## 1.5.13

### Bug fixes

- Fixed a bug where audio is cutting out on iOS devices using Bluetooth headsets.
- Fixed a bug where an incorrect error code is generated when the app doesn't have permission to activate a microphone on Windows platforms.
- Fixed a bug where an unhealthy device is never refreshed unless something else forces a refresh.
- Fixed a bug where a crash may occur when dereferencing a send channel's user data after the source endpoint associated with that channel has become invalid.
- Fixed a bug where clients experience silent failures if a remote chat control doesn't have a language code.

## 1.5.10

### Bug fixes

- Fixed a bug where the library may fail to initialize on some Windows devices due to a mismatch between the processor affinity of the process and the library's default thread affinity.
- Fixed a bug where the library may not provide errors when an operation fails due to an internal web request failure.
- Fixed a bug where a crash may occur when direct peer connectivity is enabled and the library attempts to establish direct peer connectivity to another device.
- Fixed a bug that may result in crackly or distorted audio.

## 1.5.1

### Bug fix

- Fixed a bug where the library may fail to activate the microphone on iOS.

## 1.5.0

### New direct peer-to-peer connectivity, latency, and speech-to-text features

- Direct peer-to-peer connectivity is now supported in the Windows 10 and Microsoft Game Core versions of the library. For more information, see [Using direct peer-to-peer connectivity](concepts-direct-peer-connectivity.md).
- The round trip latency between the local device and a remote device can now be queried through the library. For more information, see [`PartyEndpointStatistic::AverageDeviceRoundTripLatencyInMilliseconds`](reference/enums/partyendpointstatistic.md)
- Speech-to-text profanity masking can now be disabled. For more information, see [`PartyVoiceChatTranscriptionOptions::DisableProfanityMasking`](reference/enums/partyvoicechattranscriptionoptions.md).

### Android and iOS audio bug fixes

- Android: Bluetooth manager will be initialized the first time `PartyLocalDevice::CreateChatControl()` is called, rather than when `PartyManager::Initialize()` is called.
- iOS: Fixed a minor sound artifact when `PartyManager::Initialize()` is called.

## 1.4.13

### Windows 8.1 dependency issue

This release removes an unnecessary dependency on api-ms-win-core-version-l1-1-1.dll which prevented previous versions from working on Windows 8.1

## 1.4.8

### TLS1.2

- The transcription stack has been updated to use TLS1.2 on Windows 7, Android, and iOS. Please upgrade if you make use of any of these platforms as TLS1.1 support will be deprecated by [Azure Speech Services](https://azure.microsoft.com/updates/azuretls12/) beginning in September 2020. All other platforms already support TLS1.2 and no upgrade is necessary.

### Bug fixes
- Fixed a bug where the `languageCode` field in the `PartyCreateChatControlCompletedStateChange` struct was not being populated.
- Fixed a bug which was artificially inflating the latency measurements reported by `PartyManager::GetRegions()`.
- Fixed a bug which allowed `PartyManager::SetMemoryCallbacks()` to be called at unsafe times.
- Fixed a bug where calling `PartyManager::DestroyLocalUser()` with a `PartyLocalUser` in a `PartyNetwork` would generate a `PartyLocalUserRemovedStateChange` with an incorrect value in the removedReason field, `PartyLocalUserRemovedReason::RemoveLocalUser`, instead of the correct value, `PartyLocalUserRemovedReason::DestroyLocalUser`.

### Misc changes
- The bandwidth used by chat data has been reduced.
- More descriptive error codes and error messages are now provided when sandbox issues are encountered on Xbox.
- `PartyManager::Initialize()` will now fail if an empty PlayFab Title ID is provided.
- Passing invalid PlayFab Entity Tokens to `PartyManager::CreateLocalUser()` will now result in more descriptive error messages in the state change results for operations which rely on a valid token.
- Typos in the header documentation have been fixed.
- A more descriptive error code and error message is now provided when an invalid region is passed to `PartyManager::CreateNewNetwork()`.
- The documentation for the lifetimes of `PartyString` values has been clarified for the structures and interfaces in Party.h.
- The documentation for `PartyManager::Cleanup()` was clarified to explain it is not a thread-safe call.
- A more descriptive error code and error message are provided when `PartyManager::ConnectToNetwork()` asynchronously fails with internet connectivity errors.

## 1.3.0

### Chat API Changes

* The real-time audio manipulation functions, which can be used to modify outgoing or incoming voice chat audio, are implemented for Windows and Xbox. For more information, see [Using real-time audio manipulation to apply custom voice effects](concepts-realtime-audio-manipulation.md).
* The chat permission options have more options for optionally configuring text-to-speech and microphone audio permissions independently. For more information, see [`PartyChatPermissionOptions`](reference/enums/partychatpermissionoptions.md).
* This breaks compatibility with previous releases of the Xbox Live Helper library. This release is compatible with version 1.2.0 of the Xbox Live Helper library. For more information, see the [Xbox Live Helper Library release notes](party-xboxlive-relnotes.md).
* Each transcription state change now indicates whether it represents text-to-speech or microphone audio. For more information, see [`PartyVoiceChatTranscriptionReceivedStateChange`](reference/structs/partyvoicechattranscriptionreceivedstatechange.md).


## 1.2.2

### iOS Changes

- Adds support for the volume control API.

## 1.2.0

### Android Changes

- Adds support for the volume control API.
- Removes audio focus handling from the library.  Host applications are now expected to implement their own focus handling logic.

## 1.0.2

- Fixed crash in background telemetry

## 1.0.1

### Party API Changes

#### PartyManager::SetMemoryCallbacks Changes

This release of Party adds fixes for `PartyManager::SetMemoryCallbacks()` and also restrictions about when this API is safe to call. Check the reference documentation of the API in Party.h for details.

#### Removal of PartyStateChangeResult::TitleCreateNetworkThrottled

The `PartyStateChangeResult` value `TitleCreateNetworkThrottled` has been removed from the API, since the Party library will never generate it.

## 0.7.0-prerelease

### Windows Packaging Changes

This release of Party introduces a new NuGet package, [Microsoft.PlayFab.PlayFabParty.Cpp.Windows](https://www.nuget.org/packages/Microsoft.PlayFab.PlayFabParty.Cpp.Windows), which replaces and deprecates the NuGet packages specific to Windows 10 and Windows 7 (Microsoft.PlayFab.PlayFabParty.Cpp.Win10 and Microsoft.PlayFab.PlayFabParty.Cpp.Win7, respectively). The new unified Windows NuGet package contains two new DLLs, PartyWin.dll (supports Windows 8.1 and up) and PartyWin7.dll (only for use on Windows 7). With the new Windows unified NuGet package, the correct Party DLL is loaded based on runtime detection of the OS version, so both PartyWin.dll and PartyWin7.dll should be included in the game distribution package.

### Android Changes

The Android flavor now uses a shared object for Party (libparty.so) instead of a static library (libparty.a).

This release also contains Android-specific bug fixes for audio device selection.

### iOS Changes

The iOS flavor of Party now has the framework package included for dynamically loading libparty instead of the statically built libparty.a.

### API Changes

#### UpdateEntityToken API

This release of Party makes a change related to the handling of PlayFab entity tokens. In the previous version, the game provided Party with a user's entity token in the `PartyManager::CreateLocalUser()` API. Thereafter, Party internally refreshed the entity token and kept it up to date.

In this version, the internal token refreshing behavior has been removed and is replaced by a new API, `PartyLocalUser::UpdateEntityToken()`. The caller is now responsible for monitoring the expiration of the entity token provided to `PartyManager::CreateLocalUser()` and `PartyLocalUser::UpdateEntityToken()`. When the token is nearing or past the expiration time a new token should be obtained by performing a PlayFab login operation and provided to the Party library by calling `PartyLocalUser::UpdateEntityToken()`. It is recommended to acquire a new token when the previously supplied token is halfway through its validity period. On platforms that may enter a low power state or otherwise cause the application to pause execution for a long time, preventing the token from being refreshed before it expires, the token should be checked for expiration once execution resumes.

The rough flow is as follows:

1. The game calls a `LoginWith*` PlayFab API
1. The response from PlayFab contains the entity token and also the expiration time
1. Provide the token information to Party with the `PartyManager::CreateLocalUser()` API, as before
1. [New] Make note of the expiration time in order to know when to refresh it
1. [New] At halfway through the token's expiration time (or soonest opportunity after that time), acquire a fresh token by calling `LoginWith*` again and track the new token's expiration time
1. [New] Call `PartyLocalUser::UpdateEntityToken()` to pass in the new token to Party

Additional notes:
- The act of acquiring an entity token does not invalidate any previously obtained entity tokens. They remain valid until they expire.
- The internal refreshing functionality was removed because most games are expected to make their own PlayFab calls and so having both the game and the Party library fetching entity tokens causes unnecessary service load to basically manage two sets of tokens.

#### Chat API Changes

The `PartyVoiceChatTranscriptionReceivedStateChange` now includes a `languageCode` field, which indicates the language of the transcription.

The `PartyChatTextReceivedStateChange` now includes a `languageCode` field, which may indicate the language of the chat text. The `languageCode` field will be populated when chat text translation has been enabled via `PartyLocalChatControl::SetTextChatOptions()`.

## 0.6.0-prerelease

Added support for iOS, Android, and Nintendo Switch platforms.

For more information, see the following links:

[iOS/Android](https://github.com/PlayFab/PlayFabParty/releases)

[Nintendo Switch](https://github.com/PlayFab/PlayFabPartySwitch/releases)


## 0.5.0-prerelease

### API Changes

#### Accessible Chat
* Added support for text-to-speech narration. (see `PartyLocalChatControl::SetTextToSpeechProfile()`)
* Added more options for controlling when speech-to-text occurs. (see `PartyLocalChatControl::SetTranscriptionOptions()`)
* Added text-to-text translation to the API, although it is not yet supported. ( see `PartyLocalChatControl::SetTextChatOptions()`)
* Reduced text-to-speech bandwidth and memory usage.

#### Network access control
* `PartyNetwork::SetAccessControlList()` and related methods have been removed from the API. Use the new `PartyInvitation` class and related methods to create open invitations or grant access to specific PlayFab users to your Party networks.


## 0.4.6-prerelease

### API Changes

Added new public API `PartyLocalUser::UpdateEntityToken()`.


## 0.2.0-prerelease

### API Changes

* `PartyManager::Initialize` now requires a valid PlayFab Title ID to be passed in.