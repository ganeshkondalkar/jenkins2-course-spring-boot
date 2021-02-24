node {
    def PATH = "spring-boot-samples/spring-boot-sample-atmosphere"
    def REPOSITORY = "https://github.com/ganeshkondalkar/jenkins2-course-spring-boot.git"
    def BRANCH = "fix/maven-pom-fixes"

    stage('GIT - checkout') {
        // git branch: BRANCH, url: REPOSITORY
        checkout scm
    }

    stage('MAVEN - compile, test, package') {
        dir(PATH) {
            bat 'mvn clean package'
        }
    }

    stage('JUNIT - TEST RESULTS') {
        dir(PATH) {
            publishHTML(target: [
                allowMissing: true,
                alwaysLinkToLastBuild: true,
                keepAll: true,
                reportDir: 'target/site/jacoco/',
                reportFiles: 'index.html',
                reportName: 'Code Coverage Report',
                reportTitles: 'Atmosphere (Pipeline) Code Coverage Report'
            ])
            step([
                $class: 'JUnitResultArchiver',
                checksName: '',
                testResults: 'target/surefire-reports/TEST-*.xml'
            ])
        }
    }

    // stage('ARCHIEVE') {
    //     dir(PATH) {
    //         archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false, onlyIfSuccessful: true
    //     }
    // }

}

input id: 'Deploy_to_stage',
        message: 'Deploy to Staging?',
        ok: 'Approve'
node {
    def PATH = "spring-boot-samples/spring-boot-sample-atmosphere"

    stage('ARCHIVE') {
        dir(PATH) {
            archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false, onlyIfSuccessful: true
        }
    }
}
