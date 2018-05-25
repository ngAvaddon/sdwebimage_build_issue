# sdwebimage_build_issue

Setup:
----
TestApp1 set up to use ```pod 'SDWebImage/GIF'``` directly or through other pods.

Initially the issue was that main target used ```pod 'Appboy-iOS-SDK' ``` which pulled in SDWebImage with GIF support, but issue can be reproduced just using SDWebImage directly.

Issue description:
----
Project will build fine, but will to archive.

**Debug/Release:**

Xcode builds `SDWebImage` and `SDWebImage/GIF` into separate folders which results normal dependency and framework search path resolution.

Folder with SDWebImage:
![SDWebImage folder](/images/debug_sdwebimage.png?raw=true "")

Folder with SDWebImage/GIF:
![SDWebImage folder](/images/debug_sdwebimage_gif.png?raw=true "")


**Archive:**

Xcode builds all dependencies into `UninstalledProducts` folder and makes a symlinks to built frameworks while building all targets. That SDWebImage.framework is being overwritten and symlink points to same framework for SDWebImage and SDWebImage-GIF which results build failure.

Folder with SDWebImage:
![SDWebImage folder](/images/archive_sdwebimage.png?raw=true "")

Folder with SDWebImage/GIF:
![SDWebImage folder](/images/archive_sdwebimage_gif.png?raw=true "")

UninstalledProducts Folder:
![SDWebImage folder](/images/archive.png?raw=true "")


Build output SDWebImage:

```
ProcessInfoPlistFile /Users/user/Library/Developer/Xcode/DerivedData/TestApp1-eudslplchqkplofpqvoxvnskgbzp/Build/Intermediates.noindex/ArchiveIntermediates/TestApp1/IntermediateBuildFilesPath/UninstalledProducts/iphoneos/SDWebImage.framework/Info.plist Target\ Support\ Files/SDWebImage/Info.plist
    cd /Users/user/Projects/sdwebimage_build_issue/Pods
    export PATH="/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin:/Applications/Xcode.app/Contents/Developer/usr/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
    builtin-infoPlistUtility /Users/user/Projects/sdwebimage_build_issue/Pods/Target\ Support\ Files/SDWebImage/Info.plist -expandbuildsettings -format binary -platform iphoneos -o /Users/user/Library/Developer/Xcode/DerivedData/TestApp1-eudslplchqkplofpqvoxvnskgbzp/Build/Intermediates.noindex/ArchiveIntermediates/TestApp1/IntermediateBuildFilesPath/UninstalledProducts/iphoneos/SDWebImage.framework/Info.plist

SymLink /Users/user/Library/Developer/Xcode/DerivedData/TestApp1-eudslplchqkplofpqvoxvnskgbzp/Build/Intermediates.noindex/ArchiveIntermediates/TestApp1/BuildProductsPath/Debug-iphoneos/SDWebImage/SDWebImage.framework /Users/user/Library/Developer/Xcode/DerivedData/TestApp1-eudslplchqkplofpqvoxvnskgbzp/Build/Intermediates.noindex/ArchiveIntermediates/TestApp1/IntermediateBuildFilesPath/UninstalledProducts/iphoneos/SDWebImage.framework
    cd /Users/user/Projects/sdwebimage_build_issue/Pods
    export PATH="/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin:/Applications/Xcode.app/Contents/Developer/usr/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
    /bin/ln -sfh /Users/user/Library/Developer/Xcode/DerivedData/TestApp1-eudslplchqkplofpqvoxvnskgbzp/Build/Intermediates.noindex/ArchiveIntermediates/TestApp1/IntermediateBuildFilesPath/UninstalledProducts/iphoneos/SDWebImage.framework /Users/user/Library/Developer/Xcode/DerivedData/TestApp1-eudslplchqkplofpqvoxvnskgbzp/Build/Intermediates.noindex/ArchiveIntermediates/TestApp1/BuildProductsPath/Debug-iphoneos/SDWebImage/SDWebImage.framework
```

Build output SDWebImage-GIF:
```
ProcessInfoPlistFile /Users/user/Library/Developer/Xcode/DerivedData/TestApp1-eudslplchqkplofpqvoxvnskgbzp/Build/Intermediates.noindex/ArchiveIntermediates/TestApp1/IntermediateBuildFilesPath/UninstalledProducts/iphoneos/SDWebImage.framework/Info.plist Target\ Support\ Files/SDWebImage-Core-GIF/Info.plist
    cd /Users/user/Projects/sdwebimage_build_issue/Pods
    export PATH="/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin:/Applications/Xcode.app/Contents/Developer/usr/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
    builtin-infoPlistUtility /Users/user/Projects/sdwebimage_build_issue/Pods/Target\ Support\ Files/SDWebImage-Core-GIF/Info.plist -expandbuildsettings -format binary -platform iphoneos -o /Users/user/Library/Developer/Xcode/DerivedData/TestApp1-eudslplchqkplofpqvoxvnskgbzp/Build/Intermediates.noindex/ArchiveIntermediates/TestApp1/IntermediateBuildFilesPath/UninstalledProducts/iphoneos/SDWebImage.framework/Info.plist

SymLink /Users/user/Library/Developer/Xcode/DerivedData/TestApp1-eudslplchqkplofpqvoxvnskgbzp/Build/Intermediates.noindex/ArchiveIntermediates/TestApp1/BuildProductsPath/Debug-iphoneos/SDWebImage-Core-GIF/SDWebImage.framework /Users/user/Library/Developer/Xcode/DerivedData/TestApp1-eudslplchqkplofpqvoxvnskgbzp/Build/Intermediates.noindex/ArchiveIntermediates/TestApp1/IntermediateBuildFilesPath/UninstalledProducts/iphoneos/SDWebImage.framework
    cd /Users/user/Projects/sdwebimage_build_issue/Pods
    export PATH="/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/usr/bin:/Applications/Xcode.app/Contents/Developer/usr/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
    /bin/ln -sfh /Users/user/Library/Developer/Xcode/DerivedData/TestApp1-eudslplchqkplofpqvoxvnskgbzp/Build/Intermediates.noindex/ArchiveIntermediates/TestApp1/IntermediateBuildFilesPath/UninstalledProducts/iphoneos/SDWebImage.framework /Users/user/Library/Developer/Xcode/DerivedData/TestApp1-eudslplchqkplofpqvoxvnskgbzp/Build/Intermediates.noindex/ArchiveIntermediates/TestApp1/BuildProductsPath/Debug-iphoneos/SDWebImage-Core-GIF/SDWebImage.framework

```

Error log:
----
```/Users/user/Library/Developer/Xcode/DerivedData/TestApp1-eudslplchqkplofpqvoxvnskgbzp/Build/Intermediates.noindex/ArchiveIntermediates/TestApp1/IntermediateBuildFilesPath/TestApp1Framework.build/Debug-iphoneos/TestApp1Framework.build/Objects-normal/arm64/ImageTest.bc

<module-includes>:1:9: note: in file included from <module-includes>:1:
#import "Headers/SDWebImage-Core-GIF-umbrella.h"
        ^
/Users/user/Library/Developer/Xcode/DerivedData/TestApp1-eudslplchqkplofpqvoxvnskgbzp/Build/Intermediates.noindex/ArchiveIntermediates/TestApp1/BuildProductsPath/Debug-iphoneos/SDWebImage/SDWebImage.framework/Headers/SDWebImage-Core-GIF-umbrella.h:40:9: note: in file included from /Users/user/Library/Developer/Xcode/DerivedData/TestApp1-eudslplchqkplofpqvoxvnskgbzp/Build/Intermediates.noindex/ArchiveIntermediates/TestApp1/BuildProductsPath/Debug-iphoneos/SDWebImage/SDWebImage.framework/Headers/SDWebImage-Core-GIF-umbrella.h:40:
#import "FLAnimatedImageView+WebCache.h"
        ^
/Users/user/Library/Developer/Xcode/DerivedData/TestApp1-eudslplchqkplofpqvoxvnskgbzp/Build/Intermediates.noindex/ArchiveIntermediates/TestApp1/BuildProductsPath/Debug-iphoneos/SDWebImage/SDWebImage.framework/Headers/FLAnimatedImageView+WebCache.h:16:9: error: 'FLAnimatedImage.h' file not found
#import "FLAnimatedImage.h"
        ^
/Users/user/Projects/sdwebimage_build_issue/TestApp1Framework/TestApp1Framework/ImageTest.swift:2:8: error: could not build Objective-C module 'SDWebImage'
import SDWebImage
       ^
```


Temporarily fix:
----

use ```pod 'SDWebImage/GIF'``` everywhere.

```pod install``` then will add all required folders into framework search paths and will have dependency frameworks linked.
