# Reproduction of guava signature verification failure

1. ./gradlew assembleDebug

## Expected

Success

## Actual

Failures in signature verification
```
* What went wrong:
Execution failed for task ':app:checkDebugDuplicateClasses'.
> Dependency verification failed for configuration ':app:debugRuntimeClasspath'
  One artifact failed verification: guava-32.1.3-android.jar (com.google.guava:guava:32.1.3-jre) from repository MavenRepo
  If the artifacts are trustworthy, you will need to update the gradle/verification-metadata.xml file. For more on how to do this, please refer to https://docs.gradle.org/8.7/userguide/dependency_verification.html#sec:troubleshooting-verification in the Gradle documentation.
```

HTML report then has details that module `com.google.guava:guava:32.1.3-jre` artifact
`guava-32.1.3-android.jar` is missing checksums. That is because Gradle tries to find
`guava-32.1.3-android.jar.asc` which is missing, so signing verification calls back to
checksum verification.

It seems to happen due to guava trying to do "clever" redirect
```
      "files": [
        {
          "name": "guava-32.1.3-android.jar",
          "url": "../32.1.3-android/guava-32.1.3-android.jar"
        }
      ],
```
