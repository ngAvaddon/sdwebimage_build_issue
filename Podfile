inhibit_all_warnings!
use_frameworks!

platform :ios, '10.0'

workspace 'TestApp1.xcworkspace'

target 'TestApp1' do
#    pod 'Appboy-iOS-SDK'                            #https://github.com/Appboy/appboy-ios-sdk
    pod 'SDWebImage/GIF'                                #https://github.com/rs/SDWebImage
end

target 'TestApp1Framework' do
    project './TestApp1Framework/TestApp1Framework.xcodeproj'
    pod 'SDWebImage'                                #https://github.com/rs/SDWebImage
end
