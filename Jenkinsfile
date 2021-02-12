node{
def mvn_home
mvn_home=tool "mvn"

dir('01-Jenkins/petclinic-code') {

 

stage('Maven-Clean') {
    withEnv(["MVN_HOME=$mvn_home"])
    {
        sh '"$MVN_HOME/bin/mvn" clean'
    }
}

 

stage('Maven-Compile') {
    withEnv(["MVN_HOME=$mvn_home"])
    {
        sh '"$MVN_HOME/bin/mvn" compile'
    }
}

 

stage('Maven-Test') {
    withEnv(["MVN_HOME=$mvn_home"])
    {
        sh '"$MVN_HOME/bin/mvn" test'
    }
}

 

stage('Maven-Package') {
    withEnv(["MVN_HOME=$mvn_home"])
    {
        sh '"$MVN_HOME/bin/mvn" package'
    }
}

 

stage('Deployment - PreProd') {
sh 'docker-compose up -d --build'
}


}
 notify('Completed')
}


def notify(status){
    emailext( 
      to: "Sathish Sambamurthy",
      subject: "${status}: JOB '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """<p>${status}: JOB '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
               <p> Check the Console output at <a href='${env.BUILD_URL}'>${env.JOB_NAME}[${env.BUILD_NUMBER}]</a></p>""",
    )
}
