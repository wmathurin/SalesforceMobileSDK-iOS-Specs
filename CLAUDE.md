# CLAUDE.md — Salesforce Mobile SDK for iOS - CocoaPods Specs Repository

---

## About This Project

The Salesforce Mobile SDK for iOS Specs repository is a **CocoaPods Specs Repository** containing podspec files for all versions of the iOS Mobile SDK libraries. It serves as the distribution mechanism for CocoaPods dependency management.

**Key constraint**: This is a **public specs repository** that enables CocoaPods to resolve and install iOS SDK dependencies. Every podspec published here must be valid, tested, and match the corresponding SDK release.

## Repository Role in SDK Architecture

This repository is the **CocoaPods distribution layer**:

```
SalesforceMobileSDK-iOS (source code)
  ├── Podspec files (at repo root)
  └── Libraries (libs/)
           │
           ▼
    release/release.js
    (copies podspecs)
           │
           ▼
SalesforceMobileSDK-iOS-Specs (this repo)
  ├── Library folders with version subdirectories
  └── update.sh (helper script)
           │
           ▼
    CocoaPods (pod install)
           │
           ▼
    iOS Apps and Templates
    (consume SDK via Podfile)
```

## Repository Structure

```
SalesforceMobileSDK-iOS-Specs/
├── SalesforceAnalytics/          # Analytics library specs
│   ├── 13.2.0/
│   │   └── SalesforceAnalytics.podspec
│   ├── 13.1.0/
│   │   └── SalesforceAnalytics.podspec
│   └── ...
│
├── SalesforceSDKCommon/          # Common utilities specs
│   ├── 13.2.0/
│   │   └── SalesforceSDKCommon.podspec
│   └── ...
│
├── SalesforceSDKCore/            # Core SDK specs
│   ├── 13.2.0/
│   │   └── SalesforceSDKCore.podspec
│   └── ...
│
├── SmartStore/                   # SmartStore specs
│   ├── 13.2.0/
│   │   └── SmartStore.podspec
│   └── ...
│
├── MobileSync/                   # MobileSync specs
│   ├── 13.2.0/
│   │   └── MobileSync.podspec
│   └── ...
│
├── SalesforceHybridSDK/          # Hybrid SDK specs
│   ├── 13.2.0/
│   │   └── SalesforceHybridSDK.podspec
│   └── ...
│
├── SalesforceFileLogger/         # File logger specs
│   ├── 13.2.0/
│   │   └── SalesforceFileLogger.podspec
│   └── ...
│
├── AgentforceSDK/                # Agentforce SDK specs
├── AgentforceService/            # Agentforce Service specs
├── cmark_gfm/                    # Markdown parser specs (Agentforce dependency)
├── swift-markdown-ui/            # Swift markdown UI specs (Agentforce dependency)
├── NetworkImage/                 # Network image specs (Agentforce dependency)
├── SalesforceCache/              # Cache specs (Agentforce dependency)
├── SalesforceLogging/            # Logging specs (Agentforce dependency)
├── SalesforceNavigation/         # Navigation specs (Agentforce dependency)
├── SalesforceNetwork/            # Network specs (Agentforce dependency)
├── SalesforceUser/               # User specs (Agentforce dependency)
└── update.sh                     # Helper script to update specs
```

## How CocoaPods Uses This Repo

### As a Specs Source

Apps and templates reference this repo in their Podfile:

```ruby
source 'https://cdn.cocoapods.org/'  # Official CocoaPods specs
source 'https://github.com/forcedotcom/SalesforceMobileSDK-iOS-Specs.git'  # SDK specs

target 'MyApp' do
  use_frameworks!
  pod 'MobileSync', '~> 13.2.0'
end
```

When `pod install` runs:
1. CocoaPods clones this specs repo (if not already cached)
2. Searches for `MobileSync/13.2.0/MobileSync.podspec`
3. Reads the podspec to determine dependencies and source locations
4. Downloads and integrates the SDK libraries

### Podspec Structure

Each podspec file defines:

```ruby
Pod::Spec.new do |s|
  s.name         = "MobileSync"
  s.version      = "13.2.0"
  s.summary      = "Salesforce Mobile SDK for iOS - MobileSync"
  s.homepage     = "https://github.com/forcedotcom/SalesforceMobileSDK-iOS"
  s.license      = { :type => "Salesforce.com Mobile SDK License" }
  s.author       = { "Wolfgang Mathurin" => "wmathurin@salesforce.com" }

  s.platform     = :ios, "17.0"
  s.source       = { :git => "https://github.com/forcedotcom/SalesforceMobileSDK-iOS.git",
                     :tag => "v#{s.version}",
                     :submodules => false }

  s.dependency 'SmartStore', "~>#{s.version}"

  s.source_files = 'libs/MobileSync/MobileSync/Classes/**/*.{h,m,swift}'
  s.public_header_files = 'libs/MobileSync/MobileSync/Classes/**/*.h'
  s.resource_bundles = { 'MobileSync' => ['libs/MobileSync/MobileSync/Resources/**'] }

  s.requires_arc = true
end
```

## Update Process

### The `update.sh` Script

This script helps copy podspecs from the iOS repo during release:

**Usage**:
```bash
./update.sh -b <branch> -v <version>

# Example:
./update.sh -b v13.2.0 -v 13.2.0
```

**What it does**:
1. Clones `SalesforceMobileSDK-iOS` at specified branch/tag
2. Copies podspec files from iOS repo root
3. Creates version subdirectories in this repo
4. Places podspecs in appropriate locations

**Example**:
```bash
# From iOS repo root:
MobileSync.podspec

# Copied to Specs repo:
MobileSync/13.2.0/MobileSync.podspec
```

### Manual Release Process

The typical workflow for publishing a new SDK version:

1. **In iOS Repo**: Create and test podspecs at repo root
2. **Tag iOS Repo**: `git tag v13.2.0`
3. **Run update.sh**: In this repo, run `./update.sh -b v13.2.0 -v 13.2.0`
4. **Verify**: Check that podspecs are in correct version directories
5. **Commit and Push**:
```bash
git add .
git commit -m "Release v13.2.0"
git push origin master
```
6. **Consumers Update**: Apps run `pod update` to get new version

## Podspec Validation

Before publishing, podspecs should be validated:

```bash
# Lint a specific podspec
pod spec lint MobileSync/13.2.0/MobileSync.podspec

# Lint with dependencies
pod spec lint MobileSync/13.2.0/MobileSync.podspec \
  --sources='https://github.com/forcedotcom/SalesforceMobileSDK-iOS-Specs.git,https://cdn.cocoapods.org/'
```

## Libraries Distributed

iOS SDK libraries available via this specs repo:

### Core Mobile SDK Libraries

| Library | Purpose |
|---------|---------|
| **SalesforceSDKCommon** | Shared utilities, crypto, base protocols |
| **SalesforceAnalytics** | Telemetry and event tracking |
| **SalesforceSDKCore** | OAuth, REST API, account management, push notifications |
| **SmartStore** | Encrypted SQLite storage (SQLCipher) |
| **MobileSync** | Data synchronization framework |
| **SalesforceHybridSDK** | Cordova hybrid app support |
| **SalesforceFileLogger** | File-based logging |

### Agentforce Libraries

| Library | Purpose |
|---------|---------|
| **AgentforceSDK** | Agentforce agent functionality |
| **AgentforceService** | Agentforce service layer |
| **cmark_gfm** | Markdown parser (dependency) |
| **swift-markdown-ui** | Swift markdown UI (dependency) |
| **NetworkImage** | Network image loading (dependency) |
| **SalesforceCache** | Caching utilities (dependency) |
| **SalesforceLogging** | Logging framework (dependency) |
| **SalesforceNavigation** | Navigation utilities (dependency) |
| **SalesforceNetwork** | Network layer (dependency) |
| **SalesforceUser** | User management (dependency) |

## Development Workflow

### Normal Development (Not This Repo)

**Important**: You typically **do not** make changes directly in this repo except during releases.

**For SDK development**:
1. Make changes in `SalesforceMobileSDK-iOS` repository
2. Update podspecs in iOS repo root
3. Test locally: `pod install --project-directory=<app>`
4. During release: Use `update.sh` to copy podspecs here

### Testing Podspecs Locally

Before releasing, test podspecs locally:

```bash
# In an app's Podfile, use local path
pod 'MobileSync', :path => '/path/to/SalesforceMobileSDK-iOS'

# Or use local specs repo
pod 'MobileSync', :podspec => '/path/to/SalesforceMobileSDK-iOS-Specs/MobileSync/13.2.0/MobileSync.podspec'
```

### Fixing Published Podspecs

If a podspec has issues after publishing:

**Preferred**: Publish a patch version (e.g., 13.2.1)
- Update iOS repo
- Create new tag
- Run update.sh with new version
- Publish new version

**Last Resort**: Modify existing podspec (not recommended)
- Only for critical bugs
- Requires all users to clear CocoaPods cache
- Document in release notes

## Code Standards

Since this is primarily a distribution repo:

### Podspec Quality
- **Valid Ruby**: Ensure proper Ruby syntax
- **Complete metadata**: Author, license, homepage all filled
- **Correct dependencies**: All pod dependencies specified with versions
- **Source accuracy**: Git tag matches version
- **Platform requirements**: Minimum iOS version specified
- **File paths**: Source files and headers correctly specified

### Version Consistency
- **All libraries**: Same version number across all SDK libraries (e.g., all 13.2.0)
- **Tag format**: Use `v<version>` format (e.g., `v13.2.0`)
- **Semantic versioning**: Follow semver (major.minor.patch)

## Code Review Checklist

When reviewing changes (typically during release):

- [ ] **Version consistency**: All SDK libraries have same version
- [ ] **Podspec validity**: Run `pod spec lint` on all changed podspecs
- [ ] **Git tags exist**: Source tags exist in iOS repo
- [ ] **Dependency versions**: Pod dependencies reference correct versions
- [ ] **File paths**: Source files and resources paths are correct
- [ ] **No breaking changes**: Unless major version bump
- [ ] **Release notes**: Document changes for this version
- [ ] **Testing**: Podspecs tested in sample app before publishing

## Agent Behavior Guidelines

### Do
- Understand this is a distribution repository
- Direct SDK code changes to `SalesforceMobileSDK-iOS` repo
- Help validate podspec syntax and structure
- Verify version consistency across libraries
- Test podspecs locally before committing

### Don't
- Don't make SDK code changes here (wrong repo)
- Don't publish without human approval
- Don't modify existing published podspecs without discussion
- Don't change version numbers manually (should match iOS repo tags)
- Don't skip validation (`pod spec lint`)

### Escalation — Stop and Flag for Human Review
- Any publication of new podspecs (release event)
- Modification of existing published podspecs
- Changes to dependency versions
- New libraries added to specs repo
- Breaking changes in podspecs
- CocoaPods Trunk publication

## Key Domain Concepts

- **CocoaPods**: Dependency manager for iOS projects (like npm for Node.js)
- **Podspec**: Ruby file describing a library (name, version, source, dependencies)
- **Specs Repo**: Git repository containing podspecs for CocoaPods to query
- **Pod**: A library distributed via CocoaPods
- **Podfile**: Manifest file in iOS projects listing pod dependencies
- **Pod Install**: Command that resolves and installs dependencies
- **Source**: URL of specs repo (can have multiple sources)
- **Subspecs**: Optional sub-libraries within a pod

## Version History

Version numbers follow the iOS SDK release cycle:

| Version | Release Date | Notes |
|---------|-------------|-------|
| 13.2.0  | Latest      | Current stable |
| 13.1.0  | -           | Previous release |
| 13.0.0  | -           | Major release |

See [iOS repo release notes](https://github.com/forcedotcom/SalesforceMobileSDK-iOS/releases) for detailed version history.

## Related Documentation

- **iOS SDK**: See `SalesforceMobileSDK-iOS/CLAUDE.md` for SDK development
- **CocoaPods Guides**: https://guides.cocoapods.org/
- **Podspec Syntax**: https://guides.cocoapods.org/syntax/podspec.html
- **iOS-SPM**: See `SalesforceMobileSDK-iOS-SPM/CLAUDE.md` for Swift Package Manager distribution
- **Mobile SDK Developer Guide**: https://developer.salesforce.com/docs/platform/mobile-sdk/guide
