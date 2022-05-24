pipeline {
    options {
        // set a timeout of 30 minutes for this pipeline
        timeout(time: 30, unit: 'MINUTES')
    }
    agent {
      node {
        label 'master'
      }
    }

    stages {

        stage('stage 1') {
            steps {
                script {
                    openshift.withCluster() {
                        openshift.withProject() {
                                echo "stage 1: using project: ${openshift.project()} in cluster ${openshift.cluster()}"
                                sh 'oc create deployment newapp --image quay.io/mayank123modi/mayanknginximage'
                        }
                    }
                }
            }
        }

        stage('stage 2') {
            steps {
                sh 'echo hello from stage 2!'
                sh 'oc expose deployment newapp --port 80 --type ClusterIP'
            }
        }

        stage('manual approval') {
            steps {
                timeout(time: 60, unit: 'MINUTES') {
                    input message: "Move to stage 3?"
                    sh 'oc expose svc newapp'
                }
            }
        }

        stage('stage 3') {
            steps {
                sh 'echo hello from stage 3!. This is the last stage...'
                sh 'echo deployment, service, route were created. app is up!'
            }
        }

    }
}
