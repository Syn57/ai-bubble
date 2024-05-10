# Notes: Convention Extensions
## AndroidInstrumentedTest.kt
By default, the Android Gradle plugin generates Android instrumentation tests for all library modules, even if the library module does not have any Android test source files. This can be unnecessary and can slow down the build process.
The disableUnnecessaryAndroidTest() function allows you to disable the generation of Android instrumentation tests for library modules that do not have any Android test source files. This can help to improve the build speed of your project.
Here is a breakdown of the code:
- ``internal fun LibraryAndroidComponentsExtension.disableUnnecessaryAndroidTests()`` : This is the signature of the extension function. It takes a single parameter, project, which is a reference to the current Gradle project.
- ``beforeVariants {}`` : This block of code is executed before any Android variants are created.
- ``it.enableAndroidTest = it.enableAndroidTest`` : This line of code simply assigns the value of it.enableAndroidTest to itself. This has the effect of preserving the original value of it.enableAndroidTest.
- ``&& project.projectDir.resolve("src/androidTest").exists()`` : This line of code checks if the directory src/androidTest exists in the project directory. If it does not exist, then the enableAndroidTest property is set to false.

Overall, this code snippet disables Android tests for library modules if the src/androidTest directory does not exist. This is useful because library modules typically do not need to have Android tests. Android tests are typically written for applications, not libraries.
## Badge.kt
The badging task is a Gradle task that is used to generate and check the badging information for an Android app bundle. **Badging information** is a set of metadata that is embedded in an APK file. This information can include things like the app's name, version, and icon. Badging information is used by the Android system to display information about the app to users.
- Generate badging information:
  The badging task can be used to generate the badging information for an APK file. This information is extracted from the APK file using the aapt2 tool.
- Check badging information:
  The badging task can also be used to check if the generated badging information matches the expected badging information. This can be useful for ensuring that the badging information for the app bundle is correct and consistent across different builds.

Here are some of the benefits of using the badging task:
- The badging task can be cached, which means that it only needs to be executed once for each unique combination of inputs. This can improve build performance, especially for large app bundles.
- The badging task can help to reduce the risk of errors by automatically generating and checking the badging information for the app bundle.
- The badging task can help to ensure that the badging information for the app bundle is consistent across different builds. This can be important for maintaining a consistent user experience.
## Compose.kt
Here is a breakdown of the code:
- ``configureAndroidCompose( )`` : This function takes a CommonExtension object as input. The CommonExtension object is used to configure common properties for all Android modules in a project. The function applies a series of configurations to the CommonExtension object.
- ``buildFeatures`` : The buildFeatures block is used to configure build features for the Android modules. The compose = true line enables Jetpack Compose for the modules.
- ``composeOptions`` : The composeOptions block is used to configure Compose-specific options for the Android modules. The kotlinCompilerExtensionVersion line sets the version of the Kotlin compiler extension for Compose.
- ``dependencies`` : The dependencies block is used to add dependencies to the Android modules. The val bom = libs.findLibrary("androidx-compose-bom").get() line retrieves the bill of materials (BOM) for the androidx Compose libraries.
  The add("implementation", platform(bom)) and add("androidTestImplementation", platform(bom)) lines add the BOM as a dependency for both the implementation and Android test configurations.
- ``testOptions`` : The testOptions block is used to configure test options for the Android modules. The unitTests block configures options for unit tests.
  The isIncludeAndroidResources = true line enables the inclusion of Android resources in unit tests. This is required for Robolectric tests.
- ``tasks.withType<KotlinCompile>().configureEach { ... }`` : This block iterates over all tasks of type KotlinCompile and configures them. The kotlinOptions block is used to configure Kotlin compiler options. The ``freeCompilerArgs = freeCompilerArgs + buildComposeMetricsParameters()`` line adds the buildComposeMetricsParameters() method to the list of free compiler arguments.

There are a few reasons why you might want to use the code snippet you provided to configure Compose for your Android project, rather than just configuring it in the app module's build.gradle file:
1. Consistency:
   If you have multiple Android modules in your project, using the code snippet will ensure that Compose is configured the same way in all of them. This can help to improve consistency and reduce the risk of errors.
2. Modularity:
   The code snippet allows you to configure Compose in a centralized location, outside of any specific module. This can be useful if you want to keep your module-level build.gradle files clean and focused on module-specific configurations.
3. Dependency management:
   The code snippet uses the libs object to retrieve the version of the androidx Compose BOM. This can help to ensure that you are using the latest version of Compose across all of your modules.
4. Future-proofing:
   The code snippet uses the CommonExtension object to configure Compose. This object is designed to be used with the Android Gradle Plugin's new modularization features, which are still under development. By using the CommonExtension, you can future-proof your project and make it easier to migrate to the new modularization features when they are released.
## ProjectExt.kt
The code snippet you provided defines an extension property on the Project class called libs. This extension property returns a VersionCatalog object named "libs".
Version catalogs are a new feature in Android Gradle Plugin 8.0 that allow you to define and manage dependencies in a central location. This can help to improve the consistency and readability of your build scripts. The libs extension property provides a convenient way to access the dependencies defined in the "libs" version catalog from anywhere in your build scripts.
```sh
val Project.libs  get(): VersionCatalog = extensions.getByType<VersionCatalogsExtension>().named("libs")
```
So then you can simply just call the ``libs.`` to get the VersionCatalog like this:
```sh 
kotlinCompilerExtensionVersion = libs.findVersion("androidxComposeCompiler").get().toString()
```