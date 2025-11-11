Reducing your build times is achievable by recycling your native build artifacts. In this guide, I will walk you through the steps required to implement this process effectively.

### The native builds are expensive

Every time you trigger the CI to create a new build, it will create new IPA and APK files. This can take a lot of time, depending on your project size. These builds will contain the latest JS bundle. Most of the time, you don’t change native code, so why not keeping the previous IPA/APK and just replace the new JS bundle?

### When to go native

In this article we will use CircleCI and Fastlane to do the job, most of the code is shell based, so this can be adjusted to any other CI service.

The first step is know when a new native build is required, we will run a `git diff`and here are the paths and files to check:

- No cached IPA/APK found - this means that even if our diff contains JS code only, we cannot recycle a non-existent native build.
- Any change in `/ios`.
- Any change in `/android`.
- Any change in `/src/assets`, in theory this could also be recycled, but we will keep it simple for now.
- Any change in `/.env*`. Depending on the env lib that you use, these env variables can be used inside native code.
- Any change in `/patches`, if you’re using `patch-package`, you’re probably patching native code.
- Any change in `/yarn.lock`, this one is useful to trigger Android builds, on iOS we already know if a native dependency was changed because the `/ios/Podfile.lock` will be changed.

The order of these checks is important here.

Here is the CircleCI command for this:

```
 is-native-build-required:
    parameters:
      platform:
        type: enum
        enum: ['android', 'ios']
    steps:
      - run:
          name: Check if native builds are required
          command: |
            if [ << parameters.platform >> == "ios" ]; then
                if [ -f ./${PRODUCT_NAME}.ipa ]; then
                    echo "Cached iOS .ipa found."
                else
                    echo "No cached iOS .ipa found. Creating new a native iOS build."
                    echo "export NATIVE_IOS_BUILD_REQUIRED=true" >> "$BASH_ENV"
                    exit 0
                fi
            fi
            if [ << parameters.platform >> == "android" ]; then
                if [ -f ./android/app/build/outputs/apk/release/mobile_app.apk ]; then
                    echo "Cached Android .apk found."
                else
                    echo "No cached Android .apk found. Creating new a native Android build."
                    echo "export NATIVE_ANDROID_BUILD_REQUIRED=true" >> "$BASH_ENV"
                    exit 0
                fi
            fi
            commit_id_1="<< pipeline.git.base_revision >>"
            commit_id_2="<<pipeline.git.revision>>"
            echo "Base commit: $commit_id_1"
            echo "Head commit: $commit_id_2"
            if [ "$commit_id_1" == "$commit_id_2" ]; then
                commit_id_1="$commit_id_2~1"
            fi
            if [ -z "$commit_id_1" ] || [ -z "$commit_id_2" ]; then
                echo "No commit range detected. Creating new native builds for iOS and Android."
                echo "export NATIVE_IOS_BUILD_REQUIRED=true" >> "$BASH_ENV"
                echo "export NATIVE_ANDROID_BUILD_REQUIRED=true" >> "$BASH_ENV"
                exit 0
            fi
            changed_files=$(git diff --name-only "$commit_id_1" "$commit_id_2")
            echo "Changed files: $changed_files"
            if echo "$changed_files" | grep -qE '^ios/'; then
                echo "Changes detected in the /ios directory. New native iOS build will be created."
                echo "export NATIVE_IOS_BUILD_REQUIRED=true" >> "$BASH_ENV"
            else
                echo "No changes detected in the /ios directory."
                echo "export NATIVE_IOS_BUILD_REQUIRED=false" >> "$BASH_ENV"
            fi
            if echo "$changed_files" | grep -qE '^android/'; then
                echo "Changes detected in the /android directory. New native Android build will be created."
                echo "export NATIVE_ANDROID_BUILD_REQUIRED=true" >> "$BASH_ENV"
            else
                echo "No changes detected in the /android directory."
                echo "export NATIVE_ANDROID_BUILD_REQUIRED=false" >> "$BASH_ENV"
            fi
            if echo "$changed_files" | grep -qE '^src/assets'; then
                echo "Changes detected in the /src/assets directory. New native iOS and Android builds will be created."
                echo "export NATIVE_IOS_BUILD_REQUIRED=true" >> "$BASH_ENV"
                echo "export NATIVE_ANDROID_BUILD_REQUIRED=true" >> "$BASH_ENV"
            fi
            if echo "$changed_files" | grep -qE '^.env'; then
                echo "Changes detected in one of the .env* files. New native iOS and Android builds will be created."
                echo "export NATIVE_IOS_BUILD_REQUIRED=true" >> "$BASH_ENV"
                echo "export NATIVE_ANDROID_BUILD_REQUIRED=true" >> "$BASH_ENV"
            fi
            if echo "$changed_files" | grep -qE '^patches'; then
                echo "Changes detected in the /patches directory. New native iOS and Android builds will be created."
                echo "export NATIVE_IOS_BUILD_REQUIRED=true" >> "$BASH_ENV"
                echo "export NATIVE_ANDROID_BUILD_REQUIRED=true" >> "$BASH_ENV"
            fi
            # A new JS lib is added/changed and that lib might contain Android native code. 
            if echo "$changed_files" | grep -qE '^yarn.lock'; then
                echo "Changes detected in the yarn.lock file. New native Android build will be created."
                echo "export NATIVE_ANDROID_BUILD_REQUIRED=true" >> "$BASH_ENV"
            fi
```
and it can be used like this:

```
steps:
    - is-native-build-required:
        platform: android
```

### Save and restore a cached IPA/APK

Here are three important things that we need here:

- Cache the IPA/APK after a native build was finished.
- Store the build in a different location for each environment and platform.
- Have a way to manually reset this cache.
- Use the latest cached build for a specific platform (iOS/Android) and for a specific environment (dev/staging/prod).

CircleCI doesn’t have the best tool for this job, but something close enough, is to use the `save_cache` and `restore_cache` commands.

Here is how to do this for the Android builds:

```
save-android-apk-cache:
    parameters:
      env:
        type: enum
        enum: ['staging', 'prod']
    steps:
      - save_cache:
          name: Save Android .apk cache
          # Including the commit hash in the cache key to ensure that the cache is invalidated on every build
          key: android-apk-<< parameters.env >>-v1-<< pipeline.git.revision >>
          paths:
            - ./android/app/build/outputs/apk/release/mobile_app.apk
```
This command has one param, `env`, to know for which environment we save the APK.

The `key` param from the `save_cache` is constructed using:

- `env` - a variable to know for which environment we store.
- `v1` - a hard coded string, to be able to manually reset the cache by changing it to v2, v3, […]
- `pipeline.git.revision` - the git SHA that is being built. This is added automatically by CircleCI, it may have a different name on other CI services. We want this to make sure we use the latest cached build, even if it’s a recycled one. Ideally, would be to recycle only the native builds and ignore the recycled ones.

The `paths` param is used to tell CircleCI where is the file/dir that we need to cache, here you can specify multiple paths if you output multiple APKs.

The next step is to restore this cache when needed, we will use this command:

```
restore-android-apk-cache:
    parameters:
      env:
        type: enum
        enum: ['staging', 'prod']
    steps:
      - restore_cache:
          name: Restore cached Android .apk if exists
          # Not including the commit id, this will fetch the latest cache for the given env
          key: android-apk-<< parameters.env >>-v1
```
Really similar with the previous command except the `key` param from `restore_cache`. We don’t add the last part. CircleCI will choose the latest cache entry that matches the first parts of the key.

The iOS part is really similar with this one, you can see it [here](https://gist.github.com/cristiangu/d8e24f88f580519238d681cb09def782#file-config-yml-L4-L25).

The `restore-android-apk-cache`, `restore-ios-ipa-cache` must be called before `is-native-build-required` and `save-android-apk-cache`, `save-ios-ipa-cache` must be called after the build part is finished.

### Have the new version and build number ready

Before refreshing the APK/IPA files, we need to know the next version and build number, depending on your setup this may vary but a good practice for React Native projects is to use the `version` field from `package.json`. Let’s define a new command in CircleCI to read this and keep it in an env variable.

```
read-app-version:
    steps:
      - run:
          name: Read app version from package.json
          command: echo "export APP_VERSION=$(jq -r '.version' package.json)" >> $BASH_ENV

```
The build number should be fetched from Firebase/TestFlight/AppStore Connect/Google Play and incremented or updated as you wish, this will not be covered here. Fastlane has a lot of tools for this:

- [latest_testflight_build_number](https://docs.fastlane.tools/actions/latest_testflight_build_number/#latest_testflight_build_number)
- [firebase_app_distribution_get_latest_release](https://github.com/firebase/fastlane-plugin-firebase_app_distribution/blob/master/lib/fastlane/plugin/firebase_app_distribution/actions/firebase_app_distribution_get_latest_release.rb)
- [google_play_track_version_codes](https://docs.fastlane.tools/actions/google_play_track_version_codes/#google_play_track_version_codes)

### Refresh the recycled APK file

Having the cached builds ready to be used and the flag env variables (`NATIVE_ANDROID_BUILD_REQUIRED`, `NATIVE_IOS_BUILD_REQUIRED`) to know when to use them, we can start on Android and refresh the APK file with the new JS bundle.

This implementation may vary from project to project but here is what we want:

- Unarchive the APK file using `apktool`.
- Delete the old JS bundle from there.
- Create a new un-minified JS bundle using the current source files.
- Compile the JS bundle to bytecode (if you’re using Hermes).
- Copy the new JS bundle inside the APK dir.
- Update APK’s build number and version.
- Zip back the APK dir.
- Sign the APK file.
- Move it back in the cached path defined in the `save_cache` command, so CircleCI will be able to find it.

This time, we will do it inside `Fastfile`, but this is basically a shell script so feel free to adjust it for your project.

```
# [...]

new_build_number = latest_release[:buildVersion].to_i + 1
if ENV["NATIVE_ANDROID_BUILD_REQUIRED"] == "true"
    # Do your regular fastlane steps
    # [...]
else 
    UI.message("Skipping native Android build")
    refresh_apk_js_bundle(
        build_number: new_build_number.to_s
    )
    # Distribute the new build, the path is "./mobile_app.apk"
end

# [...]
```

The `refresh_apk_js_bundle` action can be defined in the same directory with your Fastfile, `actions/refresh_apk_js_bundle.rb`.

This fastlane action will use `wget`, `yq`, `apktool` and `bundletool`, if your CI machine does not contain these, you can install them by creating a new file inside the `fastlane` dir, `install-tools-android.sh`.

```
# Install wget
apt update -qq && apt install -qq -y --no-install-recommends wget

# Install yq
version=v4.45.1
binary=yq_linux_amd64
wget https://github.com/mikefarah/yq/releases/download/${version}/${binary}.tar.gz -O - |\
  tar xz && mv ${binary} /usr/bin/yq

# Install apktool
apktool_version=2.11.0
rm -rf /usr/local/bin/apktool.jar
rm -rf /usr/local/bin/apktool
sudo -E sh -c 'wget https://bitbucket.org/iBotPeaches/apktool/downloads/apktool_$apktool_version.jar -O /usr/local/bin/apktool.jar'
sudo chmod +r /usr/local/bin/apktool.jar
sudo sh -c 'wget https://raw.githubusercontent.com/iBotPeaches/Apktool/master/scripts/linux/apktool -O /usr/local/bin/apktool'
sudo chmod +x /usr/local/bin/apktool
echo "apktool $apktool_version has been installed successfully"

# Install bundletool
bundletool_version=1.18.0
rm -rf /usr/local/bin/bundletool.jar
sudo -E sh -c 'wget https://github.com/google/bundletool/releases/download/$bundletool_version/bundletool-all-$bundletool_version.jar -O /usr/local/bin/bundletool.jar'
sudo chmod +x /usr/local/bin/bundletool.jar

```
Now having these tools ready, we can refresh the APK file. In the first part we call the `install-tools-android.sh` script, use `apktool` to unzip the APK file and delete the old JS bundle file.

```
 require 'fastlane/actions/gradle'
        sh('bash ./fastlane/install-tools-android.sh')

        unzipped_path = "./mobile_app"
        js_bundle_path = "#{unzipped_path}/assets/index.android.bundle"
        
        sh("mv ./android/app/build/outputs/apk/release/mobile_app.apk ./mobile_app.apk")
        sh("rm -rf ./android/app/build/outputs/apk/release/mobile_app.apk")

        sh("apktool d mobile_app.apk")
        sh("rm -rf mobile_app.apk")
        sh("rm -rf #{js_bundle_path}")
```
In the next part we create a new JS bundle, compile it to bytecode using Hermes and copy the new bundle inside the APK dir.

```
 UI.message("Creating a new unminified JS bundle")
        sh("yarn react-native bundle \
          --dev false \
          --minify false \
          --platform android \
          --entry-file index.js \
          --reset-cache \
          --bundle-output index.android.bundle \
          --sourcemap-output index.android.bundle.map")

        UI.message("Compiling JS bundle to Hermes bytecode")
        sh("node_modules/react-native/sdks/hermesc/linux64-bin/hermesc \
          -O -emit-binary \
          -output-source-map \
          -out=index.android.bundle.hbc \
          index.android.bundle")
        sh("rm -f index.android.bundle")
        sh("mv index.android.bundle.hbc index.android.bundle")
        
        UI.message("Copying JS bundle to APK assets")
        sh("cp index.android.bundle #{js_bundle_path}")
```
The last steps are to upload the new source maps to your favorite crash reporting service, set the new version and build number inside `apktool.yml`, create the new APK and sign it.

```
# Upload the new source maps to a crash reporting tool.
        #other_action.sentry_upload_sourcemaps(
        #  bundle_name: "index.android.bundle",
        #  release: "mobile-app@#{ENV["APP_VERSION"]}+#{params[:build_number]}",
        #)
        
        UI.message("Setting app version and build number")
        UI.message("App version: #{ENV["APP_VERSION"]}")
        UI.message("Build number: #{params[:build_number]}")
        sh("cd #{unzipped_path} && yq -i '.versionInfo.versionName = \"#{ENV["APP_VERSION"]}\"' apktool.yml")
        sh("cd #{unzipped_path} && yq -i '.versionInfo.versionCode = #{params[:build_number]}' apktool.yml")
        
        UI.message("Zipping APK")
        sh("apktool b #{unzipped_path} -o mobile_app.apk")

        UI.message("Resigning APK")
        # TODO: Update to zipalign -P 16 when upgrading to a newer React Native 
        # https://github.com/facebook/react-native/issues/45054
        sh("zipalign -P 4 -f -v 4 ./mobile_app.apk ./mobile_app-aligned.apk")
        sh("rm -rf ./mobile_app.apk")
        sh("mv './mobile_app-aligned.apk' './mobile_app.apk'")

        # Make sure to use the same signing configuration as the one used in the build.gradle
        sh("apksigner sign -v --v2-signing-enabled true --ks ./android/app/release.keystore --ks-key-alias #{ENV['ANDROID_KEYSTORE_ALIAS']} --ks-pass pass:'#{ENV['ANDROID_KEYSTORE_PRIVATE_KEY_PASSWORD']}' --key-pass pass:'#{ENV['ANDROID_KEYSTORE_PASSWORD']}' mobile_app.apk")
        
        # Copy  APK to the android build folder so it can cached by the CI at the end of the build
        sh("cp ./mobile_app.apk ./android/app/build/outputs/apk/release/mobile_app.apk")
```
Complete source for the `refresh_apk_js_bundle.rb` can be found [here](https://gist.github.com/cristiangu/287212f0e0abbb5cba479824bf4508ab).

### Refresh the recycled IPA file

The iOS part is similar with the Android one, first we unzip the IPA file, create a new JS bundle, compile it to bytecode using Hermes and copy it back in the original location.

```
require 'fastlane/actions/resign'
        unzipped_path = "./ipa_unzipped"
        js_bundle_dir = "#{unzipped_path}/Payload/mobile_app.app"
        js_bundle_path = "#{js_bundle_dir}/main.jsbundle"
        sh("unzip mobile_app.ipa -d #{unzipped_path}")
        sh("rm -rf mobile_app.ipa")
        sh("rm -rf #{js_bundle_path}")

        UI.message("Creating a new unminified JS bundle")
        sh("yarn react-native bundle \
          --dev false \
          --minify false \
          --platform ios \
          --entry-file index.js \
          --reset-cache \
          --bundle-output main.jsbundle \
          --sourcemap-output main.jsbundle.map")
        
        UI.message("Compiling JS bundle to Hermes bytecode")
        sh("ios/Pods/hermes-engine/destroot/bin/hermesc \
          -O -emit-binary \
          -output-source-map \
          -out=main.jsbundle.hbc \
          main.jsbundle")

        sh("rm -f main.jsbundle")
        sh("mv main.jsbundle.hbc main.jsbundle")

        UI.message("Copying JS bundle to IPA")
        sh("cp main.jsbundle #{js_bundle_path}")
```
Next, we sign the IPA using a `signing_identity` and `provisioning_profile(s)`defined by Fastlane in some ENV variables. You can replace `com.bundle.identifier` with your own identifier. This fastlane action has a new parameter, `match_type`, it can have the following values: `adhoc`, `appstore`, `development` or `enterprise`.

```
 #other_action.sentry_upload_sourcemaps(
        #  bundle_name: "main.jsbundle",
        #  release: "mobile-app@#{ENV["APP_VERSION"]}+#{params[:build_number]}",
        #)

        sh("cd #{unzipped_path} && zip -r ../mobile_app.ipa ./*")

        UI.message("Resigning IPA, setting app version and incrementing build number")
        UI.message("IPA file: mobile_app.ipa")
        UI.message("App version: #{ENV["APP_VERSION"]}")
        UI.message("Build number: #{params[:build_number]}")
        Fastlane::Actions::ResignAction.run(
            ipa: "./mobile_app.ipa",
            short_version: ENV["APP_VERSION"],
            bundle_version: params[:build_number],
            signing_identity: ENV["sigh_com.bundle.identifier_#{params[:match_type]}_certificate-name"],
            provisioning_profile: {
                "com.bundle.identifier" => ENV["sigh_com.bundle.identifier_#{params[:match_type]}_profile-path"],
                "com.bundle.identifier.NotificationService" => ENV["sigh_com.bundle.identifier.NotificationService_#{params[:match_type]}_profile-path"]
            },
            # Use the local app entitlement file(s), useful for multiple entitlement files. (App + Extensions)
            use_app_entitlements: true
        )
```
Complete source for the `refresh_ipa_js_bundle.rb` can be found [here](https://gist.github.com/cristiangu/17b2be4759634f0eef7b56bca514e383).

For the iOS lane, this action can be used like this:

```
# [...]

latest_release = firebase_app_distribution_get_latest_release(app: firebase_app_id)
if ENV["NATIVE_IOS_BUILD_REQUIRED"] == "true"
    update_project_settings(
        profile_type: "AdHoc",
        build_number: (latest_release[:buildVersion].to_i + 1).to_s
    )
    # Do your regular fastlane steps
    # [...]
else
    UI.message("Skipping native iOS build")
    refresh_ipa_js_bundle(
        match_type: "adhoc",
        build_number: (latest_release[:buildVersion].to_i + 1).to_s
    )
    # Distribute the new build, the path is "./mobile_app.ipa"
end

# [...]
```

### Good to know

- On Android, this will not work for `.aab` files, they are really different compared to an APK.
- Would be nice to create a new env variable to store the artifact file name and use it inside the shell scripts in both fastlane actions, `sh("something #{ENV["PRODUCT_NAME"]}.apk")`
- The `save_cache` and `restore_cache` commands are saving every new build even if it’s a recycled one. Unfortunately, there is no way to delete the old cache or skip certain build steps in CircleCI. This cache [expires automatically after 15 days](https://circleci.com/docs/caching/).
- The APK sign logic is duplicated on Android, if you change something inside `./android/app/build.gradle` make sure to also update the `refresh_apk_js_bundle.rb` file.
- This articles assumes that you use the default JS bundle file names, `index.android.bundle` and `main.jsbundle`.
- If your project is using `productFlavors` & `flavorDimensions` on Android and multiple `build schemes` & `build configs` on iOS, more customizations are needed to accommodate the things explained above.

### Sources

- [config.yml](https://gist.github.com/cristiangu/d8e24f88f580519238d681cb09def782)
- [install-tools-android.sh](https://gist.github.com/cristiangu/573fc71c79203cf027cef5ed42a5e11e)
- [refresh_apk_js_bundle.rb](https://gist.github.com/cristiangu/287212f0e0abbb5cba479824bf4508ab)
- [refresh_ipa_js_bundle.rb](https://gist.github.com/cristiangu/17b2be4759634f0eef7b56bca514e383)

### That was it

Recycling native build artifacts can significantly reduce build times and improve the efficiency of your CI/CD pipeline, especially for React Native projects. By leveraging this, you can save valuable time.

While the approach outlined in this article focuses on CircleCI and Fastlane, the scripts are platform agnostic or at least easy adjustable to other CI/CD tools and workflows.