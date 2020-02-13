## Build system integration

In Xcode, there are 2 build systems: Legacy and XCBuild.

As of Xcode 11, XCBuild is the default build system selected by Xcode. As these
build systems have quite different implementations and integration points, the
document should discuss both.


## XCBuild architecture

XCBuild runs a build system daemon as a child process inside of Xcode. Xcode and
the build service communicate via MessagePack protocol.
[XCBuildKit](https://github.com/jerrymarino/xcbuildkit) aims to provide a way to
integrate with XCBuild, and add features like the progress bar.


## XCBuild manual code signing

By default, XCBuild requires code signing.

Prior to building, it emits the following error:

```
An empty identity is not valid when signing a binary for the product type 'Application'.
```

Set the following setting to disable code signing errors and manually sign:
```
CODE_SIGNING_ALLOWED = NO
CODE_SIGN_STYLE = manual
```


## Legacy manual build code signing

In legacy build, to disable code signing requires overriding 2 settings:

```
CODE_SIGNING_REQUIRED = NO
CODE_SIGN_STYLE = manual
```

