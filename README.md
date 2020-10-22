# Latin Android Summit 2020
https://latinandroidsummit.virtualconference.com/

## No fear to tame the automatic deploys

You’ve may heard of CI/CD, which stands for Continuous Integration and Continuous Delivery. 
Both relate to broader topic which is DevOps — a set of practices that’s the main goal is to create and release software in smaller pieces, faster and more reliably, to make release of the software fast. 

And such can be reached by automating its building process. 

To achieve it CI/CD practice can be introduced which covers frequently commits to the main application branch, testing it and then releasing it.

Some stuff that we talked about...

### Fastlane
- https://fastlane.tools/

### DevOps - Understanding and applying CI/CD pipeline for Android developers 🚀 - Part 1
 - https://dev.to/mustafakhaled/devops-understanding-and-applying-ci-cd-pipeline-for-android-developers-part-1-3o7l
 - 
### Git Feature Branch Workflow
The core idea behind the Feature Branch Workflow is that all feature development should take place in a dedicated branch instead of the main branch. This encapsulation makes it easy for multiple developers to work on a particular feature without disturbing the main codebase. It also means the main branch will never contain broken code, which is a huge advantage for continuous integration environments.

 - https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow

###  Configurando CI/CD com Github Actions e Firebase App Distribution para projetos Android
- https://medium.com/android-dev-br/configurando-ci-cd-com-github-actions-e-firebase-app-distribution-para-projetos-android-8df02096610b

### Distribute Android apps to testers using the Firebase console
- https://firebase.google.com/docs/app-distribution/android/distribute-console

### Bitrise — Firebase App Distribution step
- https://medium.com/@arekk/bitrise-firebase-app-distribution-step-9f9eb558fb89
- https://github.com/d-date/bitrise-step-firebase-app-distribution

### Firebase App Distribution - Using BitRise CLI
 - https://github.com/guness/bitrise-step-firebase-app-distribution
 - https://app.bitrise.io/cli

### GitHub Actions for Android: First Approach
 - https://medium.com/@wkrzywiec/github-actions-for-android-first-approach-f616c24aa0f9

### Android GitHub Actions Setup
 - https://www.coletiv.com/blog/android-github-actions-setup/


### Workflow syntax for GitHub Actions
- https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions

Take a look at this two Github Actions workflows:

-   [pre-merge](https://github.com/cortinico/kotlin-android-template/blob/master/.github/workflows/pre-merge.yaml)  To build, test, run detekt & ktlint on all the modules of the project.
-   [gradle-wrapper-validation](https://github.com/cortinico/kotlin-android-template/blob/master/.github/workflows/gradle-wrapper-validation.yml)  To validate the hash of the Gradle wrapper.

Those workflows will run automatically for every new Pull Request and for every push.

Github Actions integrates well with Github templates: when you create a new repo from my template, you will have a CI setup already running, and you don’t need to set up extra services or create other accounts.

### ktlint - pre-commit
 - https://ktlint.github.io/
 - https://github.com/pinterest/ktlint/blob/master/ktlint/src/main/resources/ktlint-git-pre-commit-hook-android.sh

```shell
#!/bin/sh
# https://github.com/shyiko/ktlint pre-commit hook
git diff --name-only --cached --relative | grep '\.kt[s"]\?$' | xargs ktlint --android --relative .
if [ $? -ne 0 ]; then exit 1; fi

```

###  detekt & lint


That’s where  **static analyzers**  can help spot bugs before the code goes to production. I always configure those two static analyzers on my projects:

-   [**(Android) Lint**](https://developer.android.com/studio/write/lint): A language-agnostic analyzer that will help you spot Android-specific bugs (e.g., unused resources or hard-coded text).
-   [**Detekt**](https://github.com/detekt/detekt): A Kotlin specific analyzer that can help you find potential errors or anti-patterns in your Kotlin code.

Lint is available out of the box directly inside Android Studio.

Detekt is instead a command-line tool and can be included in the build with a  [Gradle plugin](https://detekt.github.io/detekt/groovydsl.html).

### Kotlin style guide
This document serves as the complete definition of Google’s Android coding standards for source code in the Kotlin Programming Language.
https://developer.android.com/kotlin/style-guide

## Code Signing


### Where to Store Android KeyStore File in CI/CD Cycle?
 - https://android.jlelse.eu/where-to-store-android-keystore-file-in-ci-cd-cycle-2365f4e02e57


### Step-by-step guide to Android code signing and code signing with Codemagic
 - https://blog.codemagic.io/the-simple-guide-to-android-code-signing/

### Android code signing using Android Sign step
 - https://devcenter.bitrise.io/code-signing/android-code-signing/android-code-signing-using-bitrise-sign-apk-step/

 There are also some of the best our there such as
   - GitHub Actions
   - CircleCI
   - TravisCI
   - Bitbucket Pipelines
   - Azure Pipelines

## Scaling Android Deployment with Bitbucket Pipelines and Fastlane
 - https://bitbucket.org/blog/scaling-android-deployment-with-bitbucket-pipelines-and-fastlane

## Automate publishing your Android application with Bitbucket Pipelines and Gradle
 - https://bitbucket.org/blog/automate-publishing-your-android-application-with-bitbucket-pipelines-and-gradle

```yml
# bitbucket-pipelines.yml
pipelines:
  branches:
    master:
      - parallel:
        - step:
            name: Build
            image: bitbucketpipelines/android-ci-image
            caches:
              - gradle
            script:
              - echo "$SIGNING_JKS_FILE" | base64 -d > android-signing-keystore.jks
              - ./gradlew assembleRelease
            artifacts:
              - app/build/outputs/**
  
        - step:
            name: Build Debug
            caches:
              - gradle
            image: bitbucketpipelines/android-ci-image
            script:
              - echo "$SIGNING_JKS_FILE" | base64 -d > android-signing-keystore.jks
              - ./gradlew assembleDebug
            artifacts:
            - app/build/outputs/**
```

## Build Android Apps with GitLab CI/CD
 - https://medium.com/@seanghay/build-android-apps-with-gitlab-ci-cd-5fec4c301a2f

## gitlab ci/cd - Android (Part-1)
 - https://medium.com/faun/android-setup-for-gitlab-ci-cd-part-1-32bedaba0d5a


```shell
#.gitlab-ci.yml 

image: seanghay/android-ci

before_script:
  - chmod +x ./gradlew

stages:
  - build

cache:
  paths:
    - .gradle/wrapper
    - .gradle/caches

assembleDebug:
  stage: build
  script:
    - ./gradlew assembleDebug
    - cp app/build/outputs/apk/debug/app-debug.apk app-debug.apk
  artifacts:
    paths:
      - app-debug.apk
```


