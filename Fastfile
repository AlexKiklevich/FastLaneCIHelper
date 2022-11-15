# This file contains the fastlane.tools configuration
# You can find the documentation at https://docs.fastlane.tools
#
# For a list of all available actions, check out
#
#     https://docs.fastlane.tools/actions
#
# For a list of all available plugins, check out
#
#     https://docs.fastlane.tools/plugins/available-plugins
#

# Uncomment the line if you want fastlane to automatically update itself
# update_fastlane

fastlane_version "2.171.0"
default_platform(:ios)

# Use this lane to build Any eWallet Brand
# Automatically sync your iOS keys and profiles across all your team members using git
# Builds and packages iOS apps for you

# Using:
# cd ../ProjectPath
# fastlane buildBrand --env SchemeName

desc "Build One Brand"
  lane :buildBrand do

    api_key = app_store_connect_api_key(
      key_id: ENV["KEY_ID"],
      issuer_id: ENV["ISSUER_ID"],
      key_filepath:  "/Users/" + Etc.getlogin + ENV["KEY_FILEPATH"],
      in_house: false
    )

    match(
      api_key: api_key,
      git_branch: ENV["BRANCH_NAME"], 
      app_identifier: ENV["APP_IDENTIFIER"],
      type: "appstore", 
      readonly: true
    )

    # build your iOS app
    gym(
      scheme: ENV["SCHEME"],
      export_method: "app-store",
      clean: true,
      output_directory: "../Output/eWallet",
      output_name: ENV["SCHEME"]
    )

  end


# Use this lane to deploy Any eWallet Brand
# Automatically sync your iOS keys and profiles across all your team members using git
# Builds and packages iOS apps for you

# Using:
# cd ../ProjectPath
# fastlane deployBrand --env SchemeName

desc "Deploy One Brand"
  lane :deployBrand do

    api_key = app_store_connect_api_key(
      key_id: ENV["KEY_ID"],
      issuer_id: ENV["ISSUER_ID"],
      key_filepath:  "/Users/" + Etc.getlogin + ENV["KEY_FILEPATH"],
      in_house: false
    )

    current_build_number = latest_testflight_build_number(
                             api_key: api_key,
                             app_identifier: ENV["APP_IDENTIFIER"]
                           )

    increment_build_number(
      build_number: current_build_number.to_i + 1
    )

    sh "fastlane buildBrand --env " + "\"" + ENV["SCHEME"] + "\""
    
    ENV["DELIVER_ITMSTRANSPORTER_ADDITIONAL_UPLOAD_PARAMETERS"] = "-t Aspera"

    pilot(
      api_key: api_key,
      ipa: "../Output/eWallet/" + ENV["SCHEME"] + ".ipa",
      skip_waiting_for_build_processing: true
    )

  end

#######################################################
# Sync Certificates
#######################################################

# Use this lane if needed download all certs
# Automatically sync your ALL Brands iOS keys and profiles across all your team members using git

# Using:
# cd ../ProjectPath
# fastlane checkCerts

desc "Check certificates"
  lane :checkCerts do |options|

    sh "fastlane checkCert --env \"Bank_Alpha\" two_factor_auth:#{options[:two_factor_auth] ? true : false}"
    sh "fastlane checkCert --env \"Bank_Beta\" two_factor_auth:#{options[:two_factor_auth] ? true : false}" 
    sh "fastlane checkCert --env \"Bank_New_Design_Alpha\" two_factor_auth:#{options[:two_factor_auth] ? true : false}"
    # sh "fastlane checkCert --env Demo two_factor_auth:#{options[:two_factor_auth] ? true : false}"
    sh "fastlane checkCert --env Monevium two_factor_auth:#{options[:two_factor_auth] ? true : false}"

  end


# Use this lane if needed add new certs
# Automatically sync your ALL Brands iOS keys and profiles across all your team members using git
# Push new iOS keys and profiles to git if needed

# Using:
# cd ../ProjectPath
# fastlane updateCerts

desc "Update certificates"
  lane :updateCerts do |options|

    sh "fastlane updateCert --env \"Bank_Alpha\" two_factor_auth:#{options[:two_factor_auth] ? true : false}"
    sh "fastlane updateCert --env \"Bank_Beta\" two_factor_auth:#{options[:two_factor_auth] ? true : false}"
    sh "fastlane updateCert --env \"Bank_New_Design_Alpha\" two_factor_auth:#{options[:two_factor_auth] ? true : false}"
    # sh "fastlane updateCert --env Demo two_factor_auth:#{options[:two_factor_auth] ? true : false}"
    sh "fastlane updateCert --env Monevium two_factor_auth:#{options[:two_factor_auth] ? true : false}"

  end



#######################################################
# Sync Certificates Helpers
#######################################################

# Automatically sync your Any Brand iOS keys and profiles across all your team members using git

# Using:
# cd ../ProjectPath
# fastlane checkCert --env SchemeName

desc "Check certificate"
  lane :checkCert do |options|

    if !options[:two_factor_auth]

      match(
        git_branch: ENV["BRANCH_NAME"],
        team_id: ENV["DEV_PORTAL_TEAM_ID"],
        app_identifier: ENV["APP_IDENTIFIER"],
        type: "appstore", 
        readonly: true
      )

      match(
        git_branch: ENV["BRANCH_NAME"],
        team_id: ENV["DEV_PORTAL_TEAM_ID"],
        app_identifier: ENV["APP_IDENTIFIER"],
        type: "development", 
        readonly: true
      )
    else 

      api_key = app_store_connect_api_key(
        key_id: ENV["KEY_ID"],
        issuer_id: ENV["ISSUER_ID"],
        key_filepath:  "/Users/" + Etc.getlogin + ENV["KEY_FILEPATH"],
        in_house: false
      )

      match(
        api_key: api_key,
        git_branch: ENV["BRANCH_NAME"],
        app_identifier: ENV["APP_IDENTIFIER"],
        type: "appstore", 
        readonly: true
      )

      match(
        api_key: api_key,
        git_branch: ENV["BRANCH_NAME"],
        app_identifier: ENV["APP_IDENTIFIER"],
        type: "development", 
        readonly: true
      )

    end
  end

# Automatically sync your Any Brand iOS keys and profiles across all your team members using git
# Push new iOS keys and profiles to git if needed

# Using:
# cd ../ProjectPath
# fastlane updateCert --env SchemeName

desc "Update certificate"
  lane :updateCert do |options|

    if !options[:two_factor_auth]

      match(
        git_branch: ENV["BRANCH_NAME"],
        team_id: ENV["DEV_PORTAL_TEAM_ID"], 
        app_identifier: ENV["APP_IDENTIFIER"],
        type: "appstore",
        readonly: false
      )

      match(
        git_branch: ENV["BRANCH_NAME"],
        team_id: ENV["DEV_PORTAL_TEAM_ID"],
        app_identifier: ENV["APP_IDENTIFIER"], 
        type: "development", 
        readonly: false
      )
    else 
      api_key = app_store_connect_api_key(
        key_id: ENV["KEY_ID"],
        issuer_id: ENV["ISSUER_ID"],
        key_filepath:  "/Users/" + Etc.getlogin + ENV["KEY_FILEPATH"],
        in_house: false
      )

      match(
        api_key: api_key,
        git_branch: ENV["BRANCH_NAME"], 
        app_identifier: ENV["APP_IDENTIFIER"],
        type: "appstore",
        readonly: false
      )

      match(
        api_key: api_key,
        git_branch: ENV["BRANCH_NAME"],
        app_identifier: ENV["APP_IDENTIFIER"], 
        type: "development", 
        readonly: false
      )

    end
  end

#######################################################
# TeamCity Build (Based on env scheme)
#######################################################

desc "Deploy One Brand"
  lane :deployBrandTeamCity do |options|
    evaluated_build_number = 0

    api_key = app_store_connect_api_key(
      key_id: ENV["KEY_ID"],
      issuer_id: ENV["ISSUER_ID"],
      key_filepath: "/Users/" + Etc.getlogin + ENV["KEY_FILEPATH"],
      in_house: false
    )

    if options[:auto_increment]
      current_build_number = latest_testflight_build_number(
                              api_key: api_key,
                              app_identifier: ENV["APP_IDENTIFIER"],
                            )
      evaluated_build_number = [current_build_number.to_i, options[:build].to_i].max + 1          
      increment_build_number(
        build_number: evaluated_build_number
      )
    else
      evaluated_build_number = options[:build].to_i
      increment_build_number(
        build_number: evaluated_build_number
      )
    end

    evaluated_version=""
    unless options[:version].to_s.strip.empty?
      
      sh "sed -i -e 's/APP_VERSION = .*/APP_VERSION = #{options[:version]}/g' ../" + ENV["Base_XCConfig"]
      evaluated_version = options[:version]
    else 
      evaluated_version = sh "sed -n -e '/APP_VERSION/ s/.*\= *//p' ../" + ENV["Base_XCConfig"]
    end
    
  
     sh "fastlane buildBrandTeamCity --env " + "\"" + ENV["SCHEME"] + "\""
    
    ENV["DELIVER_ITMSTRANSPORTER_ADDITIONAL_UPLOAD_PARAMETERS"] = "-t Aspera"

    pilot(
      api_key: api_key,
      ipa: "../Output/eWallet/" + ENV["SCHEME"] + ".ipa",
      skip_waiting_for_build_processing: true
    )

  end

#######################################################
# TeamCity Build (trigger on git push)
#######################################################

desc "Build Demo"
  lane :buildDemoTeamCity do
    sh "fastlane buildBrandTeamCity --env Demo"
  end

desc "Build One Brand"
  lane :buildBrandTeamCity do

    api_key = app_store_connect_api_key(
      key_id: ENV["KEY_ID"],
      issuer_id: ENV["ISSUER_ID"],
      key_filepath: "/Users/" + Etc.getlogin + ENV["KEY_FILEPATH"],
      in_house: false
    )

    update_code_signing_settings(
      code_sign_identity:"Apple Distribution",
      build_configurations: "Release",
      use_automatic_signing: true,
      targets:"eWallet"
    )
    

    match(
      api_key: api_key,
      git_branch: ENV["BRANCH_NAME"], 
      app_identifier: ENV["APP_IDENTIFIER"],
      type: "appstore", 
      readonly: true
    )
   
    # build your iOS app
    gym(
      scheme: ENV["SCHEME"],
      export_method: "app-store",
      clean: true,
      output_directory: "../Output/eWallet",
      output_name: ENV["SCHEME"]
    )

  end

#######################################################
# Builds for iOS Simulator 
#######################################################

lane :buildAutotestBrand do |options|

  increment_build_number(
      build_number: options[:build_number]
    )

    xcbuild(
      workspace: "../eWallet/eWallet.xcworkspace",
      scheme: ENV["SCHEME"],
      configuration: ENV["SCHEME"],
      xcargs: "-sdk iphonesimulator",
      derivedDataPath: "../Output/" + ENV["DATA_PATH"] + "/" + ENV["SCHEME"]
    )

    sh "cd .. && find ../Output/" + ENV["DATA_PATH"] + "/" + ENV["SCHEME"] + " -name " + ENV["SCHEME"] + ".app -maxdepth 0 " + "-exec rm -rf {} \\+"
    sh "cd .. && find ../Output/" + ENV["DATA_PATH"] + "/" + ENV["SCHEME"] + " -name *.app -exec cp -r {} ../Output/" + ENV["DATA_PATH"] + "/" + ENV["SCHEME"] + ".app \\;"
    sh "cd .. && rm -rf ../Output/" + ENV["DATA_PATH"] + "/" + ENV["SCHEME"]

end

lane :uploadDSYMToFirebaseCrashlytics do |options|

  sh "rm -rf ../firebase"
  sh "mkdir -m 777 ../firebase"

  output_dir = Dir.pwd + "/../firebase"
  gsp_plist = Dir.pwd + "/../Configuration/GoogleService-Info/" + "/GoogleService-Info-" + ENV["APP_IDENTIFIER"] + ".plist"

  gsp_script = Dir.pwd + "/../Pods/FirebaseCrashlytics/upload-symbols"

  api_key = app_store_connect_api_key(
    key_id: ENV["KEY_ID"],
    issuer_id: ENV["ISSUER_ID"],
    key_filepath: "/Users/" + Etc.getlogin + ENV["KEY_FILEPATH"],
    in_house: false
  )
  
  current_build_number = latest_testflight_build_number(
                            api_key: api_key,
                            app_identifier: ENV["APP_IDENTIFIER"],
                          )

  download_dsyms(
    app_identifier: ENV["APP_IDENTIFIER"],
    team_id: ENV["AS_CONNECT_TEAM_ID"],
    version: options[:version],
    build_number: options[:auto_detect] ? current_build_number : options[:build],
    output_directory: "firebase"
  )

  sh "#{gsp_script} -gsp #{gsp_plist} -p ios #{output_dir}"
  sh "rm -r ../firebase"

end