node('maven') {
  stage('Build') {
    git url: "https://github.com/shalinyp/Demo-Springboot-Docker-Openshift.git"
    sh "mvn package"
    stash name:"jar", includes:"target/Demo-Springboot-Docker-Openshift.jar"
  }
 /*  stage('Test') {
    parallel(
      "Cart Tests": {
        sh "mvn verify -P cart-tests"
      },
      "Discount Tests": {
        sh "mvn verify -P discount-tests"
      }
    )
  } */
  stage('Build Image') {
    unstash name:"jar"
    sh "oc start-build Demo-Springboot-Docker-Openshift --from-file=target/Demo-Springboot-Docker-Openshift.jar --follow"
  }
  stage('Deploy') {
    openshiftDeploy depCfg: 'Demo-Springboot-Docker-Openshift'
    openshiftVerifyDeployment depCfg: 'Demo-Springboot-Docker-Openshift', replicaCount: 1, verifyReplicaCount: true
  }
 /*  stage('System Test') {
    sh "curl -s -X POST http://cart:8080/api/cart/dummy/666/1"
    sh "curl -s http://cart:8080/api/cart/dummy | grep 'Dummy Product'"
  } */
}