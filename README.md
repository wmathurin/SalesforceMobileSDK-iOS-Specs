![Build Status](https://forcedotcom.github.io/SalesforceMobileSDK-TestResults/Specs-results/master/latest/buildstatus.svg)

# Salesforce Mobile SDK for iOS - CocoaPods Specs Repository

CocoaPods podspec repository for the Salesforce Mobile SDK for iOS.

## Overview

This repository contains [CocoaPods](https://cocoapods.org/) podspec files for all versions of the Salesforce Mobile SDK for iOS. It serves as a **specs source** that enables CocoaPods to resolve and install iOS SDK dependencies.

## What are CocoaPods Specs?

CocoaPods specs are metadata files (written in Ruby) that describe how to download, build, and integrate iOS libraries. This repository acts as a catalog that CocoaPods queries when resolving dependencies.

## Using This Specs Repository

### In Your Podfile

Add this repository as a source in your `Podfile`:

```ruby
source 'https://cdn.cocoapods.org/'  # Official CocoaPods specs
source 'https://github.com/forcedotcom/SalesforceMobileSDK-iOS-Specs.git'  # Salesforce SDK specs

target 'MyApp' do
  use_frameworks!

  # iOS SDK libraries
  pod 'MobileSync', '~> 13.2.0'
  pod 'SmartStore', '~> 13.2.0'
  pod 'SalesforceSDKCore', '~> 13.2.0'

  # Or for hybrid apps
  pod 'SalesforceHybridSDK', '~> 13.2.0'

  # Or for React Native
  pod 'SalesforceReact', '~> 13.2.0'
end
```

### Install Dependencies

```bash
pod install
```

CocoaPods will:
1. Clone this specs repository
2. Find the requested library versions
3. Download the SDK from GitHub
4. Integrate it into your Xcode project

## Available Libraries

All Salesforce Mobile SDK libraries for iOS are available through this specs repository:

| Library | Description |
|---------|-------------|
| **SalesforceSDKCommon** | Shared utilities, crypto, and base protocols |
| **SalesforceAnalytics** | Telemetry and event tracking |
| **SalesforceSDKCore** | OAuth, REST API, account management, push notifications |
| **SmartStore** | Encrypted SQLite storage with SQLCipher |
| **MobileSync** | Data synchronization framework |
| **SalesforceHybridSDK** | Cordova hybrid app support |
| **SalesforceReact** | React Native bridge |
| **SalesforceFileLogger** | File-based logging |
| **AgentforceSDK** | Agentforce agent functionality |
| **SalesforceSwiftSDK** | Modern Swift SDK layer |

## Repository Structure

Podspecs are organized by library name and version:

```
SalesforceMobileSDK-iOS-Specs/
├── MobileSync/
│   ├── 13.2.0/
│   │   └── MobileSync.podspec
│   ├── 13.1.0/
│   │   └── MobileSync.podspec
│   └── ...
│
├── SmartStore/
│   ├── 13.2.0/
│   │   └── SmartStore.podspec
│   └── ...
│
└── (other libraries follow same pattern)
```

## Version Compatibility

All SDK libraries share the same version number for each release:

| SDK Version | iOS Min | Xcode | Release Date |
|------------|---------|-------|--------------|
| 13.2.0     | 17.0    | 15+   | Latest       |
| 13.1.0     | 16.0    | 15+   | -            |
| 13.0.0     | 16.0    | 15+   | -            |

See [release notes](https://github.com/forcedotcom/SalesforceMobileSDK-iOS/releases) for detailed version history.

## For SDK Developers

### Publishing New Versions

If you're a maintainer publishing a new SDK version:

1. **Prepare iOS Repo**: Update and tag `SalesforceMobileSDK-iOS`
2. **Update Specs**: Run the update script in this repo:
   ```bash
   ./update.sh -b v13.2.0 -v 13.2.0
   ```
3. **Validate**: Lint podspecs before committing:
   ```bash
   pod spec lint MobileSync/13.2.0/MobileSync.podspec \
     --sources='https://github.com/forcedotcom/SalesforceMobileSDK-iOS-Specs.git,https://cdn.cocoapods.org/'
   ```
4. **Commit and Push**:
   ```bash
   git add .
   git commit -m "Release v13.2.0"
   git push origin master
   ```

### Testing Podspecs Locally

Before publishing, test podspecs locally:

```ruby
# In your test app's Podfile
pod 'MobileSync', :podspec => '/path/to/SalesforceMobileSDK-iOS-Specs/MobileSync/13.2.0/MobileSync.podspec'
```

## Alternative: Swift Package Manager

If you prefer Swift Package Manager over CocoaPods, see the [SalesforceMobileSDK-iOS-SPM](https://github.com/forcedotcom/SalesforceMobileSDK-iOS-SPM) repository.

## Documentation

- **Mobile SDK Developer Guide**: https://developer.salesforce.com/docs/platform/mobile-sdk/guide
- **iOS SDK Documentation**: https://forcedotcom.github.io/SalesforceMobileSDK-iOS
- **CocoaPods Guides**: https://guides.cocoapods.org/
- **Podspec Syntax**: https://guides.cocoapods.org/syntax/podspec.html

## Related Repositories

- **iOS SDK** (source code): https://github.com/forcedotcom/SalesforceMobileSDK-iOS
- **iOS SPM** (Swift Package Manager): https://github.com/forcedotcom/SalesforceMobileSDK-iOS-SPM
- **iOS Hybrid**: https://github.com/forcedotcom/SalesforceMobileSDK-iOS-Hybrid
- **Templates**: https://github.com/forcedotcom/SalesforceMobileSDK-Templates

## Support

- **Issues**: [GitHub Issues](https://github.com/forcedotcom/SalesforceMobileSDK-iOS-Specs/issues)
- **Questions**: [Salesforce Stack Exchange](https://salesforce.stackexchange.com/questions/tagged/mobilesdk)
- **Community**: [Trailblazer Community](https://trailhead.salesforce.com/trailblazer-community/groups/0F94S000000kH0HSAU)

## License

Salesforce Mobile SDK License. See [LICENSE](LICENSE) file for details.
