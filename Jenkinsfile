/*def remote = [:]
remote.name = "eq-demo-lm6"
remote.host = "10.0.0.9"
remote.allowAnyHosts = true */

node {

def IMAGE_NAME = params.IMAGE_NAME
def TAG_NAME = params.TAG_NAME
 def TAG_NAME_Latest = params.TAG_NAME_Latest
 def docker_login = params.docker_login
 def Dockerhub_URL = params.Dockerhub_URL
//def ARTIFACTORY_IP = params.ARTIFACTORY_IP
//def ARTIFACTORY_PORT = params.ARTIFACTORY_PORT
//def ARTIFACTORY_KEY = params.ARTIFACTORY_KEY
//def app_name = params.app_name
/*def NODE_PASSWD = params.NODE_PASSWD
*/
stage('Checkout') {
 git branch: 'master', credentialsId: 'Github_poc', url: 'https://github.com/Pocaccount1/ADOK8.git'   
 }


stage('Build') {
    withMaven(jdk: 'java', maven: 'maven') {
        
        println "build is running"
        sh 'mvn -f pom.xml clean package -Dmaven.test.skip=true'
    }
}

stage('Test') {
    withMaven(jdk: 'java', maven: 'maven') {
        
        println "Unit Test is running"
        sh 'mvn -f pom.xml test'
    }
}

stage('Build Image') {
    sh """
        
        docker build -t ${IMAGE_NAME}:${TAG_NAME} .
    """
}


stage('Publish Image') {
 withCredentials([usernamePassword(credentialsId: 'docker_login', passwordVariable: 'password', usernameVariable: 'username')]) {
 //withCredentials([usernamePassword(credentialsId: 'Artifactory_Creds', passwordVariable: 'password', usernameVariable: 'username')]) {
   
    /*sh """
        docker login -u ${username} -p ${password} ${ARTIFACTORY_KEY}
        docker tag ${IMAGE_NAME}:${TAG_NAME} ${ARTIFACTORY_KEY}/${IMAGE_NAME}:${TAG_NAME}
        docker push ${ARTIFACTORY_KEY}/${IMAGE_NAME}:${TAG_NAME}
        docker pull ${ARTIFACTORY_KEY}/${IMAGE_NAME}:${TAG_NAME}
    """ */
    sh """
        docker login -u ${username} -p ${password} 
        docker tag janardhan54/adok8:1 janardhan54/adok8:2
        /*docker push ${IMAGE_NAME}:${TAG_NAME_Latest}
        docker pull ${IMAGE_NAME}:${TAG_NAME_Latest}*/
        """

    }
}
/*stage('Deploy Task') {
 sshagent(['ssh-private']) {
    sh "scp -o StrictHostKeyChecking=no adok8.yaml CAadmin@20.121.23.190:/root"
 script{
  try{
    sh "ssh CAadmin@20.121.23.190 kubectl apply -f ."
  }catch(error){
         sh "ssh CAadmin@20.121.23.190 kubectl create -f ."
  }
 }
 
}
} */
    /*withCredentials([kubeconfigFile(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
    withCredentials([kubeconfigFile(credentialsId: 'KConfig', variable: 'KUBECONFIG')]) {

    def docker_image = "${IMAGE_NAME}:${TAG_NAME_Latest}"
    dir('k8s-templates') {
      git branch: 'main', credentialsId: 'gitlab_cred', url: 'https://gitlab.com/rajudruva2/k8s-templates-yaml.git'
       sh """
    sed -i 's|docker_image|${docker_image}|' springboot-template.yml
    sed -i "s/APP_NAME/\${APP_NAME}/g" springboot-template.yml   
    sed -i "s/APP_LABEL/\${APP_LABEL}/g" springboot-template.yml 
    sed -i "s/NAMESPACE/\${NAMESPACE}/g" springboot-template.yml    
    
"""
sh 'kubectl get deploy springboot-c-dep'
    }
   
}
}
stage('Deploy to k8s'){
  withCredentials([kubeconfigFile(credentialsId: 'kubeconfig', variable: 'KUBECONFIG')]) {
    
     script{
         def docker_image = "${ARTIFACTORY_IP}:${ARTIFACTORY_PORT}/${ARTIFACTORY_KEY}/${IAMGE_NAME}:${TAG_NAME}"
             try{   sh 'kubectl get deploy springboot-c-dep'
             
            sh """
            kubectl set image deployment/springboot-c-dep springboot-c='${docker_image}'
            """
            }
            catch(error){
              sh 'kubectl apply -f k8s-templates/springboot-template.yml' }

     }
}
}*/

stage("Deploy to node4") {
        withCredentials([sshUserPrivateKey(credentialsId: 'ssh-private', keyFileVariable: '', passphraseVariable: '', usernameVariable: 'username')]) {
        def remote = [:]
        remote.name = 'k8smaster1'
        remote.host = '20.231.51.90'
        remote.user = 'CAadmin'
        remote.password = 'Passw0rd@123'
        remote.allowAnyHosts = true
        
        sshPut remote: remote, from: 'adok8.yaml', into: '.'        
        sshCommand remote: remote, command: "kubectl apply -f adok8.yaml"

      /*  sshPut remote: remote, from: 'target/spring-boot-complete-0.0.1-SNAPSHOT.jar', into: '.'
        sshCommand remote: remote, command: "nohup java -jar spring-boot-complete-0.0.1-SNAPSHOT.jar --server.port=8083 > /dev/null 2>&1 &"*/
        }
     }


/*stage("Deploy to node4") 
        {
    withCredentials([sshUserPrivateKey(credentialsId: 'eqadmin-ID', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'eqadmin')]) {
       
       remote.user = eqadmin
       remote.identityFile = identity  
        
         sshPut remote: remote, from: 'complete/target/spring-boot-complete-0.0.1-SNAPSHOT.jar', into: '.'
         sshCommand remote: remote, command: "nohup java -jar spring-boot-complete-0.0.1-SNAPSHOT.jar --server.port=8083 > /dev/null 2>&1 &" 
        }
    }   */
} 


