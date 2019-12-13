pipeline {

    agent none

    parameters {
        gitParameter branchFilter: 'origin/(.*), name: 'Tag', type: 'PT_TAG', description: 'The tag to build'
    }

    stages {
        stage('Checkout') {
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
    }

}
