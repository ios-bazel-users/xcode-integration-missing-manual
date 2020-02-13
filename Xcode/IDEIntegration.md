## Ad-hoc build system IDE features

### Xcode iOS application icon displaying: schemes, and targets

Xcode displays the app icon ion in schemes and targets. There are 2 ways to
provide this information into Xcode:

_Using an `appiconset` inside an asset catalog_. The icon set is provided to the
application and Xcode through a `Copy bundle resources` build phase and the
environment variable `ASSETCATALOG_COMPILER_APPICON_NAME`. Both must be set.

_Setting CFBundleIconFile in the `Info.plist`_. Put the path to the icon file
here.

Inside of Bazel, neither of these settings will are used, and the source of
truth resides in the `ios_application` macro.


### LLDB integration: breakpoints and more

In order for LLDB to correctly symbolicate and breakpoints to work, the debug
symbols must be provided to LLDB.

There are a handful of ways to achieve this, which can be used independently or
together.

_dSYM generation_. Generate a dsym for each of the applications and stick it
inside of the application.

_Use a dummy path when writing dwarf info into object files and re-mapping_. This is
best used for code that is built remotely. The idea is to write a stub path into
all object files, and then re-map the path to the absolute path of the source
code that Xcode is operating with. In clang, use the flag `-fdebug-prefix-map`,
and then an lldbinit to remap. e.g.,
[XCHammer](https://github.com/pinterest/xchammer) automatically code-generate
ans LLDB: `settings set target.source-map
"/__SHARED_CACHABLE_PINTEREST_BAZEL_WORKSPACE_DIR__"
"/Users/jerrymarino/Projects/ios"` and
`__SHARED_CACHABLE_PINTEREST_BAZEL_WORKSPACE_DIR__` is written into all objects
by the `-fdebug-prefix-map` flag.

_Binary patching_ the binaries after build time. This tactic simply replaces the
paths in binaries with the expected ones and/or perhaps a dummy path.



