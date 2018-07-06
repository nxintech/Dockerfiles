import java.text.SimpleDateFormat

// get name of image, format: 'registry/job_name:tag'
def registry = "cargo.caicloudprivatetest.com"
def tag = "${env.BUILD_NUMBER}_${new SimpleDateFormat("yyyyMddHHmm").format(new Date())}"
def img = "${registry}/${env.JOB_NAME}:${tag}"


node {
    stage('Checkout') {
        checkout scm
    }

    stage('Compile'){
        // 'gradle4.31' is gradle tool name defined in Jenkins Tools configure
        def gradleHome = tool 'gradle4.31'
        
        // jar springboot
        if (params.env == null) {
          sh "${gradleHome}/bin/gradle clean bootJar -S"
        }
        else {
          sh "${gradleHome}/bin/gradle ${params.env} clean bootJar -S"
        }
        sh 'mv build/libs/name-VERSION-SNAPSHOT.jar app.jar'
        
        // war
        if (params.env == null) {
          sh "${gradleHome}/bin/gradle clean war"
        }
        else {
          sh "${gradleHome}/bin/gradle clean ${params.env} war"
        }
        sh 'mv build/libs/name-VERSION-SNAPSHOT.war app.war'
    }

    stage('Sonar'){
    }

    stage('Image Build'){
        sh "docker build -t ${img} --no-cache ."
        echo "Image build complete"
    }

    stage('Push Image'){
        // 'docker-registry-login' is the username/password credentials ID
        // as defined in Jenkins Credentials.
        docker.withRegistry('http://${registry}', 'docker-registry-login') {
            // login harbor before push on jenkins
            sh "docker push ${img}"
            echo "Image push complete"
        }
    }
}
