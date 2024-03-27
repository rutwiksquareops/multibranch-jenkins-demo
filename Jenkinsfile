pipeline {
    agent {
        kubernetes {
            label 'jenkinsrun'
            defaultContainer 'builder'
            yaml """
apiVersion: v1
kind: Pod
spec:
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: "cicd-services"
            operator: "In"
            values:
            - "true"
  containers:
  - name: builder
    image: squareops/jenkins-build-agent:v4
    securityContext:
      privileged: true
    volumeMounts:
      - name: builder-storage
        mountPath: /var/lib/docker
    resources:
      requests:
        cpu: 200m
        memory: 300Mi
      limits:
        cpu: 2000m
        memory: 2500Mi
  volumes:
    - name: builder-storage
      emptyDir: {}
"""
        }
    }

    options {
        buildDiscarder logRotator( 
            daysToKeepStr: '16', 
            numToKeepStr: '10'
        )
    }

    stages {
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
                sh """
                echo "Cleaned Up Workspace For Project"
                """
            }
        }

        stage('Code Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: '*/main']], 
                    userRemoteConfigs: [[url: 'https://github.com/spring-projects/spring-petclinic.git']]
                ])
            }
        }

        stage('Unit Testing') {
            steps {
                sh """
                echo "Running Unit Tests"
                """
            }
        }

        stage('Code Analysis') {
            steps {
                sh """
                echo "Running Code Analysis"
                """
            }
        }

        stage('Build Deploy Code') {
            when {
                branch 'develop'
            }
            steps {
                sh """
                echo "Building Artifact"
                """

                sh """
                echo "Deploying Code"
                """
            }
        }
    }   
}
