xtend-gradle-plugin
===================

[![Build Status](https://travis-ci.org/xtext/xtend-gradle-plugin.svg?branch=master)](https://travis-ci.org/xtext/xtend-gradle-plugin)

A gradle plugin for building Xtend projects, **even with the new Android build system!**

Getting Started
------
Apply the latest [org.xtend.xtend](http://plugins.gradle.org/plugin/org.xtend.xtend) or  [org.xtend.xtend-android](http://plugins.gradle.org/plugin/org.xtend.xtend-android) plugin. Then add the Xtend library.

```groovy
plugins {
  id "org.xtend.xtend" version "0.4.10"
}

repositories.jcenter()

dependencies {
  compile 'org.eclipse.xtend:org.eclipse.xtend.lib:2.8.+'
}
```

Use ```gradle build``` to compile and ```gradle eclipse``` to generate Eclipse metadata.

Features
--------

- Compiles Xtend sources to Java
- Enhances Java classes with Xtend debug information
- Automatically downloads the correct Xtend compiler based on which version of xtend.lib you use
- Supports both normal Java projects and the new Android build system
- Hooks into 'gradle eclipse', so the Xtend compiler is configured for your project when you import it into Eclipse


Options
--------

The compile task has [lots of options](https://github.com/oehme/xtend-gradle-plugin/blob/master/xtend-gradle-plugin/src/main/java/org/xtend/gradle/tasks/XtendOptions.xtend). All of them have good defaults.

```groovy
tasks.withType(org.xtend.gradle.tasks.XtendCompile) {
  // default is UTF-8
  options.encoding = "UTF-16"
}
```

The eclipse settings task can be configured similarly.

```groovy
tasks.withType(org.xtend.gradle.tasks.XtendEclipseSettings) {
  options.xtendAsPrimaryDebugSource = true
  options.hideSyntheticVariables = false
}
```

You can also change the output folder for each source set.

```groovy
sourceSets {
  main.xtendOutputDir = 'xtend-gen'
  test.xtendOutputDir = 'test/xtend-gen'
}
```

Reducing the runtime footprint
------------------------------

If you develop for Android or some other restrictive platform where the number of classes or methods matter a lot, then there are several ways to reduce the runtime footprint of your Xtend projects.

If your project does not define new active annotations, then you don't need the macro API from xtend.lib. You can use the smaller xbase.lib instead:

```groovy
dependencies {
  compile 'org.eclipse.xtext:org.eclipse.xtext.xbase.lib:2.7.+'
}
```

If your code and all of its dependencies work without Google Guava, then you can use xbase.lib.slim, which comes with an inlined and stripped down version of Guava.

```groovy
dependencies {
  compile 'org.eclipse.xtext:org.eclipse.xtext.xbase.lib.slim:2.7.+'
}
```

If you have dependencies which only contain active annotations, you can put them on the xtendCompileOnly classpath. Active annotations are expanded at compile time, so they are no longer needed at runtime.

```groovy
dependencies {
  xtendCompileOnly 'com.github.oehme.xtend:xtend-contrib:0.4.5'
}
```

Limitations
-----------

This plugin supports Xtend 2.5.4 and higher. For the android plugin, version 0.13 or higher is supported.

Also, the behaviour and API are still subject to change. Please file issues here if anything doesn't work as you would expect.
