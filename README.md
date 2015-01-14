# Sharpen - Automated Java->C# coversion

[![master](https://img.shields.io/travis/imazen/sharpen/master.svg?label=master)](https://travis-ci.org/imazen/sharpen/builds) [![develop](https://img.shields.io/travis/imazen/sharpen/develop.svg?label=develop)](https://travis-ci.org/imazen/sharpen/builds) [![commandline](https://img.shields.io/travis/imazen/sharpen/commandline.svg?label=commandline)](https://travis-ci.org/imazen/sharpen/builds) 

Sharpen is a library and command-line tool for automating Java to C# code conversion. You can provide configuration classes to control a wide range of class and functionality mapping.

Sharpen doesn’t provide a compatibility runtime (i.e, an implementation of all java functionality on top of .NET), but it does provide some utility classes to meet the most common needs. 

It’s likely that you will need to create a configuration class to customize and perfect your conversion, and you may need to apply patches to the result as well.

Sharpen was originally created by db40 [svn source here](https://source.db4o.com/db4o/trunk) in the format of an Eclipse plugin, but it has since been refactored to work from the command line and on build servers.


### Building and testing sharpen itself

1. Clone this repository
2. Install Java 7 and maven
3. Run ‘mvn clean test’ to test
4. Run ‘mvn install ’ to generate .jar files in /sharpen.core/target

### Running sharpen

1. `mvn install` should have created a file named `sharpencore-0.0.1-SNAPSHOT-jar-with-dependencies.jar`. This is a self-contained copy of sharpen that can be run anywhere.
2. Run `java -jar sharpencore-0.0.1-SNAPSHOT-jar-with-dependencies.jar SOURCEPATH -cp JAR_DEPENDENCY_A JAR_DEPENDENCY_B`  
    Each dependecy needed by the java source should be specified as a full path to the jar file. SOURCEPATH should also be a full path.
3. Run -help for syntax

### Sharpen allows for configuration through code

Sharpen’s command-line options don’t let you fully override all conversion options and behaviour. For example if you need to change mapping of primitive types or allow/deny mapping between iterators and enumerators, ...

#### Creating external config class

Your external configuration class must:
* inherit [Configuration](sharpen.core/src/sharpen/core/Configuration.java) class;
* must be publicly visible;
* must have a public constructor;

An example configuration project can be found here https://github.com/ydanila/sharpen_imazen_config.

#### Using your custom config class

Name your jar file `<configuration class name>`.sharpenconfig.jar in the sharpen directory. Then specify the full configuration name via the command line parameter `-runtimeTypeName` (or via the options file).

##Calling Sharpen from command line

* If you don't have or don't need custom options for the configuration. Just call it from Sharpen directory with specified java source directory. Project will be converted using default configuration.
```
java -jar sharpen-jar-with-dependencies.jar C:/java_src/
```
* If you need to override some conversion options, you can use configuration file which contains all required data for this. For example, for the [XMP core port](https://github.com/ydanila/n-metadata-extractor/tree/xmp-core) it could be used like this.
```
java -jar sharpen-jar-with-dependencies.jar C:/java_src/ @sharpen-all-options-without-configuration
```
* If you also need to specify conversion configuration, you can do this using command line option. For example, for the [XMP core port](https://github.com/ydanila/n-metadata-extractor/tree/xmp-core) with this prebuilt [Sharpen configuration](https://github.com/ydanila/sharpen_imazen_config) it could be used like this.
```
java -jar sharpen-jar-with-dependencies.jar C:/java_src/ -configurationClass sharpen.config.MEConfiguration @sharpen-all-options-without-configuration
```
Configuration also could be specified in options file. In this case, for the [XMP core port](https://github.com/ydanila/n-metadata-extractor/tree/xmp-core) with this prebuilt [Sharpen configuration](https://github.com/ydanila/sharpen_imazen_config) it could be used like this.
```
java -jar sharpen-jar-with-dependencies.jar C:/java_src/ @sharpen-all-options
```