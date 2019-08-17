#!/usr/bin/env groovy 

// Console Output Display Block 
def seperator60 = '\u2739' * 60
def seperator20 = '\u2739' * 20


node('misc') {
    echo "${seperator60}\n${seperator20} Clone repo to workspace \n${seperator60}"
    stage("Repo Pull") {
        checkout scm 
    }

    echo "${seperator60}\n${seperator20} Check AWS connection \n${seperator60}"
    stage("AWS") {
        check_aws_connection()     
    } 
    
    echo "${seperator60}\n${seperator20} Patch and setup kubeconfig \n${seperator60}"
    stage("EKS") {
        setup_k8s_kube()     
    } 

}



// CUSTOM DSL METHODS
def setup_k8s_kube() {
    stage('Prep k8s'){
      withCredentials([usernamePassword(credentialsId: 'cicd-token', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID' )]){
        sh """
           aws sts get-caller-identity
           aws eks update-kubeconfig --name fmbah01 --region eu-west-1
           kubectl get nodes
           kubectl get ns
           
           helm init
           helm version
           helm ls

           kubectl create serviceaccount --namespace kube-system tiller
           kubectl create clusterrolebinding tiller-cluster-rule --clusterrole=cluster-admin --serviceaccount=kube-system:tiller
           kubectl patch deploy --namespace kube-system tiller-deploy -p '{"spec":{"template":{"spec":{"serviceAccount":"tiller"}}}}'
           helm init --service-account tiller --upgrade
           sleep 30
           helm las 
        """
      }
    }
}

def check_aws_connection() {
    stage('AWS Creds'){
      withCredentials([usernamePassword(credentialsId: 'cicd-token', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID' )]){
        sh """
           aws ec2 describe-instances --region eu-west-1
        """
      }
    }
}


