platform :ios, '11.0'

inhibit_all_warnings!

if ENV['TRAVIS']
  install! 'cocoapods', :share_schemes_for_development_pods => true
end

target 'The Blue Alliance' do
  use_frameworks!

  # React Native
  pod 'React', :path => 'subtrees/the-blue-alliance-react/node_modules/react-native', :subspecs => [
    'Core',
    'CxxBridge', # Include this for RN >= 0.47
    'DevSupport', # Include this to enable In-App Devmenu if RN >= 0.43
    'RCTText',
    'RCTNetwork',
    'RCTImage',
    'RCTWebSocket', # needed for debugging
    # Add any other subspecs you want to use in your project
  ]
  # Explicitly include Yoga if you are using RN >= 0.42.0
  pod "yoga", :path => "subtrees/the-blue-alliance-react/node_modules/react-native/ReactCommon/yoga"
  # Third party deps podspec link
  pod 'DoubleConversion', :podspec => 'subtrees/the-blue-alliance-react/node_modules/react-native/third-party-podspecs/DoubleConversion.podspec'
  pod 'glog', :podspec => 'subtrees/the-blue-alliance-react/node_modules/react-native/third-party-podspecs/glog.podspec'
  pod 'Folly', :podspec => 'subtrees/the-blue-alliance-react/node_modules/react-native/third-party-podspecs/Folly.podspec'

  # Deps
  pod 'BFRImageViewer'
  pod 'PINRemoteImage', '3.0.0-beta.13'
  pod 'PureLayout'
  pod 'XCDYouTubeKit', '~> 2.7'
  pod 'Zip', '~> 1.1'

  # Firebase
  pod 'Firebase/Analytics'
  pod 'Firebase/Auth'
  pod 'Firebase/Core'
  pod 'Firebase/Messaging'
  pod 'Firebase/Performance'
  pod 'Firebase/Storage'

  # Local Deps
  pod 'MyTBAKit', :path => 'Frameworks/MyTBAKit', :testspecs => ['Tests']
  pod 'TBAKit', :path => 'Frameworks/TBAKit', :testspecs => ['Tests']
  pod 'TBAOperation', :path => 'Frameworks/TBAOperation', :testspecs => ['Tests']

  # myTBA
  pod 'GoogleSignIn'

  # Crash reporting
  pod 'Fabric'
  pod 'Crashlytics'

  # Debugging
  pod 'Reveal-SDK', :configurations => ['Debug']

  target 'tba-unit-tests' do
    inherit! :search_paths

    pod 'iOSSnapshotTestCase', '4.0.1' # TODO: Update to 6.0
    pod 'MyTBAKitTesting', :path => 'Frameworks/MyTBAKit'
    pod 'TBAKitTesting', :path => 'Frameworks/TBAKit'
    pod 'TBATestingMocks', :path => 'Frameworks/TBATestingMocks'
    pod 'TBAOperationTesting', :path => 'Frameworks/TBAOperation'
  end
end

post_install do | installer |
  require 'fileutils'
  FileUtils.cp_r('Pods/Target Support Files/Pods-The Blue Alliance/Pods-The Blue Alliance-acknowledgements.plist',
  'the-blue-alliance-ios/Settings.bundle/Acknowledgements.plist', :remove_destination => true)

  installer.pods_project.targets.each do |target|
    target.build_configurations.each do |config|
      config.build_settings['WARNING_CFLAGS'] ||= ['"-Wno-nullability-completeness"']
    end

    if "#{target}" == "AppAuth"
      target.build_configurations.each do |config|
        config.build_settings['IPHONEOS_DEPLOYMENT_TARGET'] = '9.3'
      end
    end
  end
end
