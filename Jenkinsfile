#!groovy
pipeline {
    agent {
        kubernetes {
            label 'keypop-gradle'
            yaml javaBuilder('1.0')
        }
    }
    stages {
        stage('Prepare settings') {
            steps{
                container('java-builder') {
                    script {
                        keypopGradleVersion = sh(script: 'grep "^version" java/keypop-gradle/gradle.properties | cut -d= -f2 | tr -d "[:space:]"', returnStdout: true).trim()
                        deployKeypopGradle = env.GIT_URL == 'https://github.com/eclipse-keypop/keypop-ops.git' && env.GIT_BRANCH == "main" && env.CHANGE_ID == null
                    }
                }
            }
        }
        stage('Import keyring'){
            when {
                expression { deployKeypopGradle }
            }
            steps{
                container('java-builder') {
                    withCredentials([file(credentialsId: 'secret-subkeys.asc', variable: 'KEYRING')]) {
                        sh 'import_gpg "${KEYRING}"'
                    }
                }
            }
        }
        stage('Keypop Gradle Plugin: Build and test') {
            steps{
                container('java-builder') {
                    configFileProvider(
                        [configFile(
                            fileId: 'gradle.properties',
                            targetLocation: '/home/jenkins/agent/gradle.properties')]) {
                        dir('java/keypop-gradle') {
                            sh './gradlew clean assemble --info --stacktrace'
                        }
                        catchError(buildResult: 'UNSTABLE', message: 'There were failing tests.', stageResult: 'UNSTABLE') {
                            dir('java/keypop-gradle') {
                                sh './gradlew test --info --stacktrace'
                            }
                        }
                        junit allowEmptyResults: true, testResults: 'java/keypop-gradle/build/test-results/test/*.xml'
                    }
                }
            }
        }
        stage('Keypop Gradle Plugin: Upload to sonatype') {
            when {
                expression { deployKeypopGradle }
            }
            steps{
                container('java-builder') {
                    configFileProvider([configFile(
                            fileId: 'gradle.properties',
                            targetLocation: '/home/jenkins/agent/gradle.properties')]) {
                        dir('java/keypop-gradle') {
                            sh './gradlew publish --info --stacktrace'
                        }
                    }
                }
            }
        }
    }
}