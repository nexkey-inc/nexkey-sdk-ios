# NexkeyCore

## Usage

```swift
import NexkeyCore

let client = NexkeyCore.shared

// 1a. Initialization with an `apiKey` and `apiSecretKey` 
client.initialize(with: apiKey, apiSecretKey: apiSecret)

// 1b. Or with a session token based approach
client.initialize(with: Constants.nexkeyAPIKey)

client.signIn(
  with: identifier,
  password: password,
  error: { [weak self] (error) in
    debugPrint("ERROR: \(error.localizedDescription)")
    self?.handleError(error, popsBack: false)
  },
  completion: { [weak self] json in
    let result = json.dictionaryValue["result"] as? [String: Any]
    if let sessionToken = result?["sessionToken"] as? String {
      self?.sessionTokenBlock?(sessionToken)
    }
  }
)
client.initialize(with: apiKey, sessionToken: sessionToken)

// 2. Getting all user keys:
client.getAllUserKeys(
  error: { [weak self] error in
    debugPrint("ERROR: \(error.localizedDescription)")
    self?.handleError(error)
  },
  completion: { [weak self] json in
    let result = json.dictionaryValue["result"] as? [[String: Any]]
    self?.locks = result
    self?.pullToRefresh.endRefreshing()
  }
)

// 3. If you want to get an information about the status
client.delegate = self

// 4. For unlocking the lock remotely 
client.remoteUnlock(
  lockId: lockId,
  update: Update<HubStatus>(owner: self) { status in
    debugPrint(status)
  },
  error: { [weak self] error in
    debugPrint("ERROR: \(error.localizedDescription)")
    self?.dismiss(animated: true, completion: nil)
    self?.handleError(error)
  },
  completion: { (json) in
    debugPrint("INFO: \(json)")
  }
)
```

## Requirements

* iOS 9.0+
* Xcode 8.0+

## Installation

### CocoaPods

[CocoaPods](https://cocoapods.org/) is a dependency manager for Cocoa projects.

To install NexkeyCore, simply add the following line to your Podfile:

```ruby
pod 'NexkeyCore'
```

## Author

* [Nexkey, Inc.](https://github.com/nexkey-inc) ([@nexkeyinc](https://twitter.com/nexkeyinc))

## FAQ

# Changelog

See [CHANGELOG](CHANGELOG.md).
