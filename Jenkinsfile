properties([pipelineTriggers([githubPush()])])

node('linux'){
    stage('Unit Tests'){
        git 'https://github.com/mike2357/java-project.git'
        sh 'ant -f test.xml -v'
        junit 'reports/result.xml'
    }
    stage('Build'){
       sh 'ant -f build.xml -v'
    }
    stage('Deploy'){
       sh 'aws s3 cp /workspace/java-pipeline/dist/rectangle-${BUILD_NUMBER}.jar s3://homework10'
    }
    stage('Report'){
        withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: '97fe4282-d988-4796-92c1-35b122feeffa', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]) {
            sh 'aws cloudformation describe-stack-resources --region us-east-1 --stack-name jenkins'
        }
    }
}
