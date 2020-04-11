pipeline {
  agent any
  environment {
    PATH = "${PATH}:${getTerraformPath()}"
  }
stages {
  stage('S3 Create Bucket') {
    steps{
      sh "ansible-playbook s3-bucket.yml"
    }
  }
  stage('terraform init and apply -dev'){
    steps{
    sh returnStatus: true, script: 'terraform workspace new dev'
    sh "terraform init"
    sh "ansible-playbook terraform.yml"
      }
    }
   stage('terraform init and apply -prod'){
      steps{
      sh returnStatus: true, script: 'terraform workspace new prod'
      sh "terraform init"
      sh "ansible-playbook terraform.yml -e app_env=prod"
        }
      }
  }
}
def getTerraformPath(){
  tfHome = tool name: 'terraform-12', type: 'org.jenkinsci.plugins.terraform.TerraformInstallation'
  return tfHome
}
