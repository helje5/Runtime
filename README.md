![Runtime](https://github.com/wickwirew/Runtime/blob/master/Resources/Runtime.png)

![Build](https://travis-ci.org/wickwirew/Runtime.svg?branch=master)
![Swift 4.0](https://img.shields.io/badge/Swift-4.0-green.svg)
[![CocoaPods compatible](https://img.shields.io/cocoapods/v/Runtime.svg)](#installation)
[![Swift Package Manager compatible](https://img.shields.io/badge/SPM-4.0-green.svg)](#installation)
[![License](http://img.shields.io/:license-mit-blue.svg)](http://doge.mit-license.org)

Runtime is a Swift library to give you more runtime abilities, including getting type metadata, setting properties via reflection, and type construction for native swift objects.

## TypeInfo
`TypeInfo` exposes metadata about native Swift structs, protocols, classes, tuples and enums. It captures the properties, generic types, inheritance levels, and more.
### Example
Lets say you have a User struct:
```swift
struct User {
  let id: Int
  let username: String
  let email: String
}
```
To get the `TypeInfo` of `User`, all that you have to do is:
```swift
let info = try typeInfo(of: User.self)
```

## Property Info
Within the `TypeInfo` object, it contains a list of `PropertyInfo` which represents all properties for the type. `PropertyInfo` exposes the name and type of the property. It also allows the getting and setting of a value on an object.
### Example
Using the same `Person` object as before first we get the `TypeInfo` and the property we want.
```swift
let info = try typeInfo(of: User.self)
let property = try info.property(named: "username")
```
To get a value:
```swift
let username = try property.get(from: user)
```
To set a value:
```swift
try property.set(value: "newUsername", on: &user)
```
It's that easy! 🎉

## Factory
Runtime also supports building an object from it's `Type`. Both structs and classes are supported.

To build a `User` object:
```swift
let user = try createInstance(type: User.self)
```

## Function Info
`FunctionInfo` exposes metadata about functions. Including number of arguments, argument types, return types, and whether it can throw an error.
### Example
```swift
func doSomething(a: Int, b: Bool) throws -> String { 
  return "" 
}

let info = functionInfo(of: doSomething)
```

## FAQ
Q: When getting and setting a value does it work typeless? (i.e. object casted as `Any`)

A: Yes! The whole library was designed with working typeless in mind.

Q: When creating a new instance of a class is it still protected by ARC?

A: Yes! The retain counts are set properly so ARC can do its job. 

## Installation
Runtime is available through [CocoaPods](http://cocoapods.org). To install
it, simply add the following line to your Podfile:
```ruby
pod 'Runtime'
```

Runtime is available through [Swift Package Manager](https://swift.org/package-manager/). To install
it, simply add the following line to your `Package.swift`,  `dependencies` keys:
```swift
.package(url: "https://github.com/wickwirew/Runtime.git",
         from: "0.1.1"),
```
and `Runtime` to your target's dependencies. Full example:

```swift
// swift-tools-version:4.0
// The swift-tools-version declares the minimum version of Swift required to build this package.

import PackageDescription

let package = Package(
  name: "testruntime",
  dependencies: [
    .package(url: "https://github.com/wickwirew/Runtime.git",
             from: "0.1.1"),
  ],
  targets: [
    .target(name: "testruntime", dependencies: [ "Runtime" ])
  ]
)
```

## Contributions
Contributions are welcome and encouraged!

## Learn
Want to know how it works? 
[Here's an article](https://medium.com/@weswickwire/creating-a-swift-runtime-library-3cc92fc486cc) on how it was implemented.

Want to learn about Swift memory layout?
[Mike Ash](https://github.com/mikeash) gave an awesome [talk](https://academy.realm.io/posts/goto-mike-ash-exploring-swift-memory-layout/) on just that.

## License
Runtime is available under the MIT license. See the LICENSE file for more info.
