pipeline {
    agent any
    environment {
        SONAR_HOME = tool 'Sonar'
    }
    stages {
        stage("Git Clone") {
            steps {
                git url: "https://github.com/ankushregmi01/DevSevOps-wanderlust", branch: "main"
            }
        }
        stage("SonarQube Analysis") {
            steps {
                withSonarQubeEnv("Sonar") {
                    sh '''
                        $SONAR_HOME/bin/sonar-scanner \
                          -Dsonar.projectName=wanderlust-cicd \
                          -Dsonar.projectKey=wanderlust-cicd \
                          -Dsonar.sources=.
                    '''
                }
            }
        }
        stage("OWASP DP") {
            steps {
                dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'DP'
                dependencyCheckPublisher pattern: "**/dependency-check-report.xml"
            }
        }
        stage("Sonar Quality Gate Scan") {
            steps {
                timeout(time: 4, unit: "MINUTES") {
                    waitForQualityGate abortPipeline: false
                }
            }
        }
        stage("Trivy File System Scan") {
            steps {
                sh "trivy fs --format table -o trivy-fs-report.html ."
            }
        }
        stage("Deploy Docker-compose") {
            steps {
                sh "docker-compose up -d"
            }
        }
    }
}
