node ('master'){
// Get Artifactory Server Instance Details
  def server = Artifactory.server "01"
  def buildInfo = Artifactory.newBuildInfo()
   

// Project Variables
  def project_path = "01-Jenkins/petclinic-code"

notify('Started')

try {
stage('Git-CheckOut') {
  git branch: 'production', url: 'https://github.com/sathishiyer/DevOPS-Mindtree-301-CS.git'
}

stage('Download Package') {
   def downloadSpec = """{ 
     "files": [
       {
        "pattern": "petclinic-war/*.war",
         "target": "ansible-code/roles/petclinic/files/"
       }
      ]
   }"""
   server.download spec: downloadSpec
}

stage('Getting Ready For Ansible Deployment'){
  sh "echo \'<h1>JENKINS TASK BUILD ID: ${env.BUILD_DISPLAY_NAME}</h1>\' > ansible-code/roles/petclinic/files/jenkins.html"
}


stage('Petclinic Ansible Deployment-Prod') {
  sh '/usr/bin/terraform init'
  sh '/usr/bin/terraform apply -input=false --auto-approve'
}
 
 notify('Completed')
} catch(err){
  notify("Error ${err}")
  currentBuild.result = 'FAILURE'

 }

}

def notify(status){
    emailext( 
      to: "sathishiyer@gmail.com",
      subject: "${status}: JOB '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
      body: """<p>${status}: JOB '${env.JOB_NAME} [${env.BUILD_NUMBER}]':</p>
               <p> Check the Console output at <a href='${env.BUILD_URL}'>${env.JOB_NAME}[${env.BUILD_NUMBER}]</a></p>""",
    )
}
