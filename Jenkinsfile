pipeline{

    agent any
    tools {
    git 'Default'
    maven 'maven_3.8.6'
    }
    stages{
        stage("checkout code"){
            steps{
                echo "========executing checkout code========"
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Boopathivellaichamy/simple-java-maven-app.git']]])
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========checkout code executed successfully========"
                }
                failure{
                    echo "========checkout code execution failed========"
                }
            }
        }
        stage("Build"){
            steps{
                echo "========Build checkout code========"
                sh 'mvn clean install'
            }
            post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========Build executed successfully ========"
                    archiveArtifacts artifacts: 'target/*jar', followSymlinks: false
                }
                failure{
                    echo "========Build execution failed========"
                }
            }
        }
                stage("Test"){
                        steps{
                            echo "========executing Test========"
                            sh 'mvn clean test'
                        }
                post{
                    always{
                        echo "========always========"
                    }
                    success{
                        echo "========Build executed successfully ========"
                        junit 'target/surefire-reports/*.xml'
                    }
                    failure{
                        echo "========Build execution failed========"
                        junit 'target/surefire-reports/*.xml'
                    }
                }
            }
                stage ("sonar scanning") {
                        steps {
                            echo "========executing sonar========"
                            withSonarQubeEnv("mysonarqube") {
                              sh "${tool("mysonar")}/bin/sonar-scanner \
                                -Dsonar.projectKey=simple_java_maven_project \
                                -Dsonar.sources=. \
                                -Dsonar.java.binaries=target/* \
                                -Dsonar.host.url=http://35.87.242.111:9000 \
                                -Dsonar.login=sqa_fd87f5aa26e05c7338297a740c0ce44238aaf253"                                    
                            }        
                        }
                                post{
                                always{
                                    echo "========always========"
                                }
                                success{
                                    echo "========Sonar Scanning executed successfully ========"
                                }
                                failure{
                                    echo "========Sonar Scanning execution failed========"
                                }
                                }
                }   
            stage ("Upload to Nexus") {
                steps {
                echo "========Uploading Artifacts========"
                sh 'mvn -g settings.xml deploy'
                }

                post{
                always{
                    echo "========always========"
                }
                success{
                    echo "========Uploading File successfully ========"
                }
                failure{
                    echo "========Uploading File failed========"
                }
                }
            }     
                            
    }
}