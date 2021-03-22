# JavaFX Gradle 7 Plugin

Simplifies working with JavaFX 11+ for gradle projects. This is is a fork I created to have a plugin version compatible with Gradle 7.

[![BSD-3 license](https://img.shields.io/badge/license-BSD--3-%230778B9.svg)](https://opensource.org/licenses/BSD-3-Clause)

## Important Note

If you are not using Java 16/Gradle 7, I recommend using the official [javafxplugin](javafxgradle7plugin) created by the JavaFX developpers. I created thsi fork because the changes I made cannot be merged into mainline without breaking the plugin for many users of the original plugin.

Background: Gradle 6.x is still incompatible with JDK 16. Gradle 7 works with JDK 16, but the original JavaFX plugin does not. The incompatibiliy is caused by the Java modularity plugin, that is used in the JavaFX plugin. As Gradle 7 comes with out-of-the box support for Jigsaw modules, that plugin isn't needed anyway, so I changed the JavaFX plugin to not rely on the modularity plugin.

This also means that using the Java modularity plugin will break your Gradle build, so just remove it. Refer to '[Building Modules for the Java Module System](https://docs.gradle.org/7.0-milestone-3/userguide/java_library_plugin.html#sec:java_library_modular)' in the Gradle documentation for more information.

## Getting started

To use the plugin, apply the following two steps:

### 1. Apply the plugin

##### Using the `plugins` DSL:

**Groovy**

    plugins {
        id 'com.dua3.javafxgradle7plugin' version '0.0.9'
    }

**Kotlin**

    plugins {
        id("com.dua3.javafxgradle7plugin") version "0.0.9"
    }


##### Alternatively, you can use the `buildscript` DSL:

**Groovy**

    buildscript {
        repositories {
            maven {
                url "https://plugins.gradle.org/m2/"
            }
        }
        dependencies {
            classpath group: 'com.dua3.gradle', name: 'javafx-gradle7-plugin', version: "0.0.9"
        }
    }
    apply plugin: 'com.dua3.javafxgradle7plugin'

**Kotlin**

    buildscript {
        repositories {
            maven {
                setUrl("https://plugins.gradle.org/m2/")
            }
        }
        dependencies {
            classpath("com.dua3.gradle:javafx-gradle7-plugin:0.0.9")
        }
    }
    apply(plugin = "org.openjfx.javafxplugin")

### 2. Specify JavaFX modules

Specify all the JavaFX modules that your project uses:

**Groovy**

    javafx {
        modules = [ 'javafx.controls', 'javafx.fxml' ]
    }

**Kotlin**

    javafx {
        modules("javafx.controls", "javafx.fxml")
    }
    
### 3. Specify JavaFX version

To override the default JavaFX version, a version string can be declared.
This will make sure that all the modules belong to this specific version:

**Groovy**

    javafx {
        version = '12'
        modules = [ 'javafx.controls', 'javafx.fxml' ]
    }

**Kotlin**

    javafx {
        version = "12"
        modules("javafx.controls", "javafx.fxml")
    }

### 4. Cross-platform projects and libraries

JavaFX modules require native binaries for each platform. The plugin only
includes binaries for the platform running the build. By declaring the 
dependency configuration **compileOnly**, the native binaries will not be 
included. You will need to provide those separately during deployment for 
each target platform.

**Groovy**

    javafx {
        version = '12'
        modules = [ 'javafx.controls', 'javafx.fxml' ]
        configuration = 'compileOnly'
    }

**Kotlin**

    javafx {
        version = "12"
        modules("javafx.controls", "javafx.fxml")
        configuration = "compileOnly"
    }

### 5. Using a local JavaFX SDK

By default, JavaFX modules are retrieved from Maven Central. 
However, a local JavaFX SDK can be used instead, for instance in the case of 
a custom build of OpenJFX.

Setting a valid path to the local JavaFX SDK will take precedence:

**Groovy**

    javafx {
        sdk = '/path/to/javafx-sdk'
        modules = [ 'javafx.controls', 'javafx.fxml' ]
    }

**Kotlin**

    javafx {
        sdk = "/path/to/javafx-sdk"
        modules("javafx.controls", "javafx.fxml")
    }
