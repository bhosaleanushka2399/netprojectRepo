pipeline {
    agent any // Or specify a label like 'windows-agent'
 
    environment {
        // Change these to match your project structure
        SOLUTION_PATH = "newProject.sln"
        PROJECT_PATH  = "newProject/newProject.csproj"
        BUILD_CONFIG  = "Release"
        OUTPUT_DIR    = "publish"
    }
 
    stages {
        stage('Restore') {
            steps {
                echo 'Restoring NuGet packages...'
                bat "dotnet restore ${SOLUTION_PATH}"
            }
        }
 
        stage('Build') {
            steps {
                echo 'Building solution...'
                bat "dotnet build ${SOLUTION_PATH} --configuration ${BUILD_CONFIG} --no-restore"
            }
        }
 
        stage('Test') {
            steps {
                echo 'Running unit tests...'
                // Assumes you have a test project in your solution
               // bat "dotnet test ${SOLUTION_PATH} --configuration ${BUILD_CONFIG} --no-build --logger:trx"
            }
            // post {
            //     always {
            //         // Optional: archive test results if using the JUnit/MSTest plugin
            //         mstest testResultsFile: '**/*.trx', keepLongStdio: true
            //     }
            // }
        }
 
        stage('Publish') {
            steps {
                echo 'Publishing API...'
                bat "dotnet publish ${PROJECT_PATH} --configuration ${BUILD_CONFIG} --output ${OUTPUT_DIR} --no-build"
            }
        }
 
        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: "${OUTPUT_DIR}/**/*", fingerprint: true
            }
        }
    }
 
    post {
        failure {
            echo 'Build failed. Sending notification...'
        }
        success {
            echo 'Pipeline completed successfully!'
        }
    }
}