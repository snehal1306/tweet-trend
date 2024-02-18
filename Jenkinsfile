def registry = 'https://sneh1306.jfrog.io/'
pipeline {
    agent {
        node {
            label 'maven1'
        }
    }
environment {
    PATH = "/opt/apache-maven-3.9.6/bin:$PATH"
    }
    stages {
        stage("build"){
            steps {
                echo "----------- build started ------------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                echo "----------- build complted ----------"
            }
        }
        stage("test"){
            steps{
                echo "--------- unit test started -------------------"
                sh 'mvn surefire-report:report'
                echo "--------- unit test completed -----------------"
            }
        }
        stage('SonarQube analysis'){
            environment{
                scannerHome = tool 'sneh-sonar-scanner'
            }
            steps{
                script{
                    echo "----------starting sonar-----------"
                    withSonarQubeEnv('sneh-sonarqube-server'){
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                    echo "-----------ending sonar--------------"
                }
            }
        }
        stage("Quality Gate"){
            steps {
                script {
                timeout(time: 1,unit:'HOURS'){
            def qg = waitForQualityGate()
            if (qg.status != 'OK'){
                error "pipeline aborted due to quality gate failure: ${qg.status}"
            }
                }
                }
            }
        }
       
    }   
}   
           
    
    
