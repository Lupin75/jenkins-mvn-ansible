pipeline {
    agent {
        docker {
            image 'maven:3.8.1-adoptopenjdk-11'
            args '-v /root/.m2:/root/.m2'
        }
    }
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
            }
        }
    }
}
// pipeline {
//   agent {
//     node {
//       label 'sushan'
//     }

//   }
//   stages {
//     stage('Build') {
//       steps {
//         sh 'mvn clean install'
//         sh 'cp ./target/test-1.0-SNAPSHOT-jar-with-dependencies.jar /tmp'
//       }
//     }

//     stage('Test') {
//       steps {
//         ansiblePlaybook(playbook: 'ansible/java-app-setup.yml', inventory: 'ansible/hosts', limit: 'localhost', vaultCredentialsId: "java-app-vault")
//         sh 'mvn test "-Dtestcase/test=Test.Runner"'
//         archiveArtifacts 'testcase/target/surefire-reports/*html'
//         ansiblePlaybook(playbook: 'ansible/java-app-reset.yml', inventory: 'ansible/hosts', limit: 'localhost', vaultCredentialsId: "java-app-vault")
//       }
//     }

//     stage('Deploy') {
//       steps {
//         ansiblePlaybook(playbook: 'ansible/java-app-setup.yml', inventory: 'ansible/hosts', limit: 'production', vaultCredentialsId: "java-app-vault")
//         input 'Finished using mvn-app?'
//         ansiblePlaybook(playbook: 'ansible/java-app-reset.yml', inventory: 'ansible/hosts', limit: 'production', vaultCredentialsId: "java-app-vault")
//       }
//     }

//   }
// }
