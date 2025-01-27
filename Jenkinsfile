pipeline {
    agent {
        label 'AGENT-1'
    }
    options {
        timeout(time: 30, unit: 'MINUTES') // This pipeline must take only 30 min to complete or else the pipeline will be failed if it exeeds more than 30 min.
        disableConcurrentBuilds() // It won't allow two parallel builds at a time.
        retry(1) // It will retry the entire pipeline on failure based on the specified number in the braces.
    }
    environment {
        DEBUG = 'true'
        appVersion = '' // This is global variable, we can use this across pipeline in any stage
    }
    stages {
        stage('Read the version') {
            steps {
                script {
                    def packageJson = readJSON file: 'package.json' // pipeline utility steps plugin need to be installed
                    appVersion = packageJson.version
                    echo "App version: ${appVersion}"
                }
            }
        }
        stage('Install dependencies') {
            steps {
                sh 'npm install'
            } 
        }
        stage('Docker Build') {
            when {
                expression { env.GIT_BRANCH == 'origin/main' } // This stage will run when the branch is main only, if there is any other branch then the pipeline will skip this stage
            }
            steps {
                sh """
                docker build -t jagadeesh3373/backend:${appVersion} .
                docker images
                """
            }
        }
        // stage('Print Params') {
        //     steps {
        //         echo "Hello ${params.PERSON}"
        //         echo "Biography: ${params.BIOGRAPHY}"
        //         echo "Toggle: ${params.TOGGLE}"
        //         echo "Choice: ${params.CHOICE}"
        //         echo "Password: ${params.PASSWORD}"
        //     }
        // }
        // stage('Approval') {
        //     input {
        //         message "Should we continue?"
        //         ok "Yes, we should."
        //         submitter "alice,bob"
        //         parameters {
        //             string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
        //             }
        //         }
        //     steps {
        //         echo "Hello, ${PERSON}, nice to meet you."
        //     }
        // }
    }
    post {
        always {
            echo "This section runs always"
            deleteDir() // Used to delete previous builds to reduce memory usage
        }
        success {
            echo "This section runs when pipeline is success"
        }
        failure {
            echo "This section runs when pipeline is failure"
        }
    }
}