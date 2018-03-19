source 'https://github.com/artsy/Specs.git'
source 'https://github.com/CocoaPods/Specs.git'

# Uncomment the next line to define a global platform for your project
platform :ios, '11.0'

use_frameworks!
inhibit_all_warnings!

# normal pods
pod 'AlamofireNetworkActivityIndicator', '~> 2.1'
pod 'HTMLString', '~> 4.0.1'
pod 'NYTPhotoViewer', '~> 1.1.0'
pod 'SDWebImage/GIF', '~> 4.0.0'
pod 'SnapKit', '~> 4.0.0'
pod 'TUSafariActivity', '~> 1.0.0'
pod 'SwiftLint'
pod 'Fabric'
pod 'Crashlytics'
pod 'Tabman', '~> 1.1'
pod 'Firebase/Core'
pod 'Firebase/Database'

# fork
pod 'Apollo', :git => 'https://github.com/orta/apollo-ios.git', :branch => 'support_local'

# prerelease pods
pod 'IGListKit/Swift', :git => 'https://github.com/Instagram/IGListKit.git', :branch => 'swift'
pod 'StyledText', :git => 'https://github.com/GitHawkApp/StyledText.git', :branch => 'master'
pod 'Highlightr', :git => 'https://github.com/GitHawkApp/Highlightr.git', :branch => 'master'
pod 'MMMarkdown', :git => 'https://github.com/GitHawkApp/MMMarkdown.git', :branch => 'master'
pod 'FlatCache', :git => 'https://github.com/GitHawkApp/FlatCache.git', :branch => 'master'
pod 'MessageViewController', :git => 'https://github.com/GitHawkApp/MessageViewController.git', :branch => 'master'
pod 'ContextMenu', :git => 'https://github.com/GitHawkApp/ContextMenu.git', :branch => 'master'

# debugging pods
pod 'FLEX', '~> 2.0', :configurations => ['Debug', 'TestFlight']

# Local Pods w/ custom changes
pod 'SwipeCellKit', :path => 'Local Pods/SwipeCellKit'
pod 'GitHubAPI', :path => 'Local Pods/GitHubAPI'
pod 'GitHubSession', :path => 'Local Pods/GitHubSession'

pod 'GitDawg', :path => '../GitDawg'
pod 'yoga', :podspec => 'Local Pods/yoga.podspec.json'

target 'Freetime' do

end

target 'FreetimeTests' do
	pod 'FBSnapshotTestCase'
end

def edit_pod_file(file, old_code, new_code)
  code = File.read(file)
  if code.include?(old_code)
    FileUtils.chmod("+w", file)
    File.write(file, code.sub(old_code, new_code))
  end
end

post_install do |installer|
  system("sh tools/generateAcknowledgements.sh")

  installer.pod_targets.each do |target|
    # Build React Native with RCT_DEV disabled
    next unless target.pod_name == 'React'
    target.native_target.build_configurations.each do |config|
      config.build_settings['GCC_PREPROCESSOR_DEFINITIONS'] ||= ['$(inherited)']
      config.build_settings['GCC_PREPROCESSOR_DEFINITIONS'] << 'RCT_DEV=0'
      config.build_settings['GCC_PREPROCESSOR_DEFINITIONS'] << 'RCT_DEBUG=0'
    end
  end

  # https://github.com/facebook/react-native/pull/14664
  animation_view_file = "Pods/React/Libraries/NativeAnimation/RCTNativeAnimatedNodesManager.h"
  animation_view_old_code = "import <RCTAnimation/RCTValueAnimatedNode.h>"
  animation_view_new_code = 'import "RCTValueAnimatedNode.h"'
  edit_pod_file animation_view_file, animation_view_old_code, animation_view_new_code

  # There's an extra dev-only import
  animation_view_file = "Pods/React/React/Modules/RCTDevSettings.mm"
  animation_view_old_code = '#import "RCTPackagerClient.h"'
  animation_view_new_code = '#if RCT_DEV\n#import "RCTPackagerClient.h"\n#endif'
  edit_pod_file animation_view_file, animation_view_old_code, animation_view_new_code

end
