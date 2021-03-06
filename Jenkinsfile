node ('master'){
// Get Artifactory Server Instance Details
  def server = Artifactory.server "01"
  def buildInfo = Artifactory.newBuildInfo()
   

// Project Variables
  def project_path = "petclinic-code"

notify('Started')

try {

stage('Git-CheckOut') {
  git branch: 'preprod', url: 'https://github.com/sathishiyer/DevOPS-Mindtree-301-CS.git'
}


dir(project_path) {

stage('Maven-Clean') {
    sh 'mvn clean'
}

stage('Maven-Compile') {
sh 'mvn compile'
}

stage('Maven-Test') {
sh 'mvn test'
}

stage('Maven-Package') {
sh 'mvn package'
}

stage('SonarQube') {
  withSonarQubeEnv('Sonar') {
    sh 'mvn sonar:sonar'
  }
}

stage('Build Management') {
   def uploadSpec = """{ 
     "files": [
       {
        "pattern": "**/*.war",
         "target": "petclinic-war"
       }
      ]
   }"""
   server.upload spec: uploadSpec
}

stage('Publish Build Info'){
  server.publishBuildInfo buildInfo
}


stage('Archive-Artifacts') {
archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
}


stage('PreProd Deployment - Docker') {
sh 'docker-compose up -d --build'
}

} 
 notify('Completed')
} catch(err){
  notify("Error ${err}")
  currentBuild.result = 'FAILURE'

 }

}

def notify(status){
    emailext( 
      to: "sathish.sambamurthy@mindtree.com",
      subject: "${status}: JOB '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """<p>${status}: JOB '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
               <p> Check the Console output at <a href='${env.BUILD_URL}'>${env.JOB_NAME}[${env.BUILD_NUMBER}]</a></p>""",
    )
}
