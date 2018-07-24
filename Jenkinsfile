import java.text.SimpleDateFormat

// get name of image, format: 'registry/job_name:tag'
def registry = "cargo.caicloudprivatetest.com"
def tag = "${env.BUILD_NUMBER}_${new SimpleDateFormat("yyyyMddHHmm").format(new Date())}"
def img = "${registry}/release/${env.JOB_NAME}:${tag}"



node {
    stage('Checkout') {
        checkout scm
    }

    stage('Compile'){
        // 'gradle4.31' is gradle tool name defined in Jenkins Tools configure
        def gradleHome = tool "gradle4.31"
        
        // 1 jar springboot
        if (params.env == null) {
          sh "${gradleHome}/bin/gradle clean bootJar -S"
        }
        else {
          sh "${gradleHome}/bin/gradle clean ${params.env} bootJar -S"
        }
        
        // 2 war
        if (params.env == null) {
          sh "${gradleHome}/bin/gradle clean war"
        }
        else {
          sh "${gradleHome}/bin/gradle clean ${params.env} war"
        }
        
        // rename
        sh "mv build/libs/name-VERSION-SNAPSHOT.war_or_jar app.war_or_jar"
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
        docker.withRegistry("http://${registry}", "docker-registry-login") {
            // login harbor before push on jenkins
            sh "docker push ${img}"
            echo "Image push complete"
        }
    }
    
    
    // Sub modele build
    ws("${WORKSPACE}/${module}") {
      def module = "module name"  
      
      stage('Compile'){
        def gradleHome = tool "gradle4.31"
        
        if (params.env == null) {
          sh "${gradleHome}/bin/gradle clean war"
          // sh "${gradleHome}/bin/gradle clean bootJar -S"
        }
        else {
          sh "${gradleHome}/bin/gradle clean ${params.env} war"  
          // sh "${gradleHome}/bin/gradle ${params.env} clean bootJar -S"
        }
            
        sh "mv build/libs/name-VERSION-SNAPSHOT.war_or_jar app.war_or_jar"
      }

      stage('Push Image'){
        sh "docker build -t ${img} --no-cache ."
        docker.withRegistry("http://${registry}", "docker-registry-login") {
            sh "docker push ${img}"
        }
    }
        
    stage('Auto Deploy') {
      // auto deploy on kubenates
      sh("""
        kubectl -n zone1 get release <your_app> -o yaml|sed -r 's#"image":"[^"]*#"image":"${img}#' | kubectl apply -f -
        """)  
    }    
}
