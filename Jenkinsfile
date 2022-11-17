

pipeline {
    agent any
    tools {
        maven 'maven'
    }
    stages {
        stage('Compile') {
            steps {
                echo 'COMPILE'
                sh "mvn clean compile -e"
            }
        }
        stage('Test') {
            steps {
                echo 'TEST'
                sh "mvn clean test -e"            
            }
        }
        stage('Package') {
            steps {
                echo 'PACKAGE'
                sh "mvn clean package -e"            
            }
        }
        stage('sonar') {
           steps{
                echo 'SONAR' 
                withSonarQubeEnv(credentialsId:'access_token_sq',installationName:'MySonar') { 
                    sh 'mvn sonar:sonar'
                }
          }
        } 
        stage('uploadNexus v0.0.1') {
           steps{
            step(
             [$class: 'NexusPublisherBuildStep',
                 nexusInstanceId: 'devops-usach-nexus',
                 nexusRepositoryId: 'devops-usach-nexus',
                 packages: [[$class: 'MavenPackage',
                       mavenCoordinate: [artifactId: 'DevOpsUsach2020', groupId: 'com.devopsusach2020', packaging: 'jar', version: '0.0.1'],
                       mavenAssetList: [
                          [classifier: '', extension: 'jar', filePath: "${WORKSPACE}/build/DevOpsUsach2020-0.0.1.jar"]
                       ] 
                   ]
                 ]
               ]
             )
           }
        }
        stage('download from nexus'){
          steps{
               echo "DOWNLOAD"
               sh "curl -X GET ${NEXUS_AUTH} http://54.183.170.50:3002/repository/devops-usach-nexus/com/devopsusach2020/DevOpsUsach2020/0.0.1/DevOpsUsach2020-0.0.1.jar"
            }
        }       
        
        stage('uploadNexus v1.0.0') {
           steps{
            step(
             [$class: 'NexusPublisherBuildStep',
                 nexusInstanceId: 'devops-usach-nexus',
                 nexusRepositoryId: 'devops-usach-nexus',
                 packages: [[$class: 'MavenPackage',
                       mavenCoordinate: [artifactId: 'DevOpsUsach2020', groupId: 'com.devopsusach2020', packaging: 'jar', version: '1.0.0'],
                       mavenAssetList: [
                          [classifier: '', extension: 'jar', filePath: "${WORKSPACE}/build/DevOpsUsach2020-0.0.1.jar"]

                       ]
                   ]
                 ]
               ]
             )
           }
        }

    }    
}