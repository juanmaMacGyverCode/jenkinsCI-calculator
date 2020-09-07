pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage("Compile") {
            steps {
                sh "./gradlew compileJava"
            }
        }
        stage("Unit test") {
            steps {
                sh "./gradlew test"
            }
        }
        stage("Code coverage") {
            steps {
        	    sh "./gradlew jacocoTestReport"
        	 	publishHTML (target: [
         	        reportDir: 'build/reports/jacoco/test/html',
         			reportFiles: 'index.html',
         			reportName: 'JacocoReport'
         	    ])
         		sh "./gradlew jacocoTestCoverageVerification"
         	}
        }
        stage("Static code analysis") {
            steps {
                sh "./gradlew checkstyleMain"
                publishHTML (target: [
                	reportDir: 'build/reports/checkstyle/',
                	reportFiles: 'main.html',
                	reportName: 'Checkstyle Report'
                ])
            }
        }
        stage('SonarQube analysis') {
            steps {
                withSonarQubeEnv('pruebasSonarqube') {
                    sh './gradlew sonarqube -Dsonar.projectKey=calculator -Dsonar.host.url=http://localhost:9000 -Dsonar.login=09a189df8100835f584e23505fa6a20c72df4b3f'
                }
            }
        }
    }
}
