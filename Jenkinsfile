pipeline {

    agent none

    parameters {
        booleanParam(
            name: "BUILD_TAG",
            description: "Build a tagged commit.",
            defaultValue: false)

        gitParameter branchFilter: 'origin/(.*), name: 'Tag', type: 'PT_TAG'
    }

    stages {

        stage('clean') {
            steps{
                cleanWs()
            }
        }

        stage('Checkout-tag') {
            agent { label 'armhf' }
            when { expression { return params.BUILD_TAG } }
            steps {
                checkout([$class: 'GitSCM',
                          branches: [[name: "${params.TAG}"]],
                          doGenerateSubmoduleConfigurations: false,
                          extensions: [],
                          gitTool: 'Default',
                          submoduleCfg: [],
                          userRemoteConfigs: [[url: 'https://github.com/synexxus/at91bootstrap.git']]
                        ])
            }
        }

        stage('Checkout-non release') {
            agent { label 'armhf' }
            when { expression { return !params.BUILD_TAG } }
            steps {
                checkout([$class: 'GitSCM',
                          branches: [[name: "v3.9.0-ledgateway"]],
                          doGenerateSubmoduleConfigurations: false,
                          extensions: [],
                          gitTool: 'Default',
                          submoduleCfg: [],
                          userRemoteConfigs: [[url: 'https://github.com/synexxus/at91bootstrap.git']]
                        ])
            }
        }

        stage('build') {
            agent { label 'armhf' }
            steps {
                sh 'make ledgateway_defconfig'
                sh 'make'
                archiveArtifacts 'binaries/ledgateway-nandflashboot-uboot-3.9.0.bin'
            }
        }
    }

}
