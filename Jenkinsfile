
pipeline {
agent any

    tools {
         // Use Maven tool configured in Jenkins Global Tool Configuration
        maven 'apache-maven-3.9.11'
    }

    stages {

        stage('Build Project') {
            steps {
                sh '''
                    # Clean, compile, package the project
                    mvn clean package
                '''
            }
        }

        stage('Modify and Configure WAR for RDS') {
            steps {
                sh '''
                    cd target
                    # Extract WAR
                    unzip LoginWebApp.war -d temp
                    cd temp
                    # Replace localhost with AWS RDS endpoint
                    sed -i 's|localhost|database-1.c3igs6uku453.ap-south-1.rds.amazonaws.com|g' userRegistration.jsp
                    # Update DB username & password
                    sed -i 's|"root", "root"|"admin", "pravin12345"|g' userRegistration.jsp
                    # Recreate WAR with updated files
                    zip -r ../LoginWebApp.war *
                    cd ..
                    rm -rf temp
                '''
            }
        }

        stage('Deploy WAR File') {
            steps {
                sh '''
                    # Copy modified WAR to Tomcat webapps
                    cp target/LoginWebApp.war /mnt/servers/apache-tomcat-10.1.49/webapps/
                '''
            }
        }
    }
}
