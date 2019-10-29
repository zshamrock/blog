{:title "Gradle 'Getting Started' template"
:layout :post
:tags ["practice"]
}
Gradle is an alternative building tool to the widely used Maven. In this post I would not describe what Gradle is and why you should switch to it. It is already very well explained on the official web site [gradle.org](https://gradle.org), personally it feels more concise, developer friendly and productive build tool comparing to Maven.

Although official web site has a nice and good enough user guide documentation, it still might take some time to build all the necessary pieces together. So, here I share my Gradle 'Getting Started' which contains all the minimum necessary parts for every product you would like to build to include.

The key features of the provided template are:
 - JaCoCo coverage
 - Checkstyle/PMD/FindBugs/JDepend integration
 - Spock for testing (Groovy based)
 - `checkCoverage` custom task to run tests and JaCoCo coverage together

```
description = 'fill product description'

apply plugin: 'java'
// groovy is required for Spock
apply plugin: 'groovy'

apply from: 'gradle/code-coverage.gradle'
apply from: 'gradle/code-quality.gradle'

sourceCompatibility = targetCompatibility = 1.8

version = '1.0.0'

sourceSets {
    main {
        java {
            srcDir 'src/java'
        }
    }
    test {
        groovy {
            srcDir 'test/groovy'
        }
    }
}

ext {
    groovyMajorVersion = 2.4
    groovyMinorVersion = 3
}

repositories {
    mavenCentral()
}

dependencies {
    testCompile "org.codehaus.groovy:groovy-all:${groovyMajorVersion}.${groovyMinorVersion}"
    testCompile "org.spockframework:spock-core:1.0-groovy-${groovyMajorVersion}"
}

task wrapper(type: Wrapper) {
    gradleVersion = '2.5'
}

task checkCoverage(dependsOn: ['test', 'jacocoTestReport']) {
    description 'Runs all tests and JaCoCo coverage'
    doLast {
        def reports = [test, jacocoTestReport].collect({
            def buildPath = buildDir.path
            def buildDirName = buildPath[buildPath.lastIndexOf(File.separator)..-1]
            '.' + buildDirName + it.reports.html.destination.path - buildPath
        })
        println "\n\tReports are avaialable at ${reports[0]} and ${reports[1]}"
    }
}
```

where *gradle/code-coverage.gradle* is:

```
apply plugin: 'jacoco'

jacoco {
    toolVersion = '0.7.5.201505241946'
}

```

and *gradle/code-quality.gradle* is:

```
apply plugin: 'jdepend'
apply plugin: 'pmd'
apply plugin: 'checkstyle'
apply plugin: 'findbugs'

jdepend {
    toolVersion = '2.9.1'
}

jdependMain {
    reports {
        xml.enabled = false
        text.enabled = true
    }
}

pmd {
    toolVersion = '5.3.2'
}

checkstyle {
    toolVersion = '6.7'
}

findbugs {
    toolVersion = '3.0.1'
}

findbugsMain {
    reports {
        xml.enabled = false
        html.enabled = true
    }
}
```

The complete source code with directory structure and Checkstyle configuration file could be found on GitHub: [https://github.com/zshamrock/gradle-getting-started-template](https://github.com/zshamrock/gradle-getting-started-template)
