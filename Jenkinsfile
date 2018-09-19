node {
  stage('Source Checkout') {
    git url: "https://github.com/irfanjs/openshift.git"
  }
  
   stage('Build') {
      git url: "https://github.com/irfanjs/openshift.git"
      sh "pwd"
      sh "/var/lib/jenkins/apache-maven-3.5.4/bin/mvn clean package jacoco:prepare-agent jacoco:report -Dorg.jenkinsci.plugins.durabletask.BourneShellScript.HEARTBEAT_CHECK_INTERVAL=300"
  /*  stash name:"jar", includes:"/var/lib/jenkins/jobs/coe-mern-project/jobs/coe-mern-project-petclinic-pipeline/workspace/target/spring-petclinic-openshift-2.0.0.BUILD-SNAPSHOT.jar" */
      stash includes: "target/*.jar", name:"jar"
    }
  
  stage('publish test cases result') {
   junit '**/target/surefire-reports/TEST*.xml' 
  }
  
  stage('Record Jacoco coverage report') {
   jacoco() 
  }
  
  stage('Execute Code quality') {
   sh "/var/lib/jenkins/apache-maven-3.5.4/bin/mvn sonar:sonar -Dsonar.host=http://sonar-server-coe-mern-project.apps.na39.openshift.opentlc.com:9000"
  }
  
    
  /* stage('Execute Unit test(s)') {
     sh " cd /var/lib/jenkins/jobs/coe-mern-project/jobs/coe-mern-project-petclinic-pipeline/workspace"
     sh "/var/lib/jenkins/apache-maven-3.5.4/bin/mvn test -Dorg.jenkinsci.plugins.durabletask.BourneShellScript.HEARTBEAT_CHECK_INTERVAL=300"
      stash name:"jar", includes:"target/spring-petclinic-2.0.0.BUILD-SNAPSHOT.jar"
    } */

  stage('Build Image') {
    unstash name:"jar"
   sh "oc start-build testapp --from-file=target/spring-petclinic-openshift-2.0.0.BUILD-SNAPSHOT.jar --follow --loglevel 10"
    sh "sleep 30"
   /*openshiftBuild apiURL: 'https://master.na39.openshift.opentlc.com', authToken: 'IdXwlyWZphf_A00Z9X02qMv57gyictVDMoBdA1PobQo', buildName: 'target/spring-petclinic-openshift-2.0.0.BUILD-SNAPSHOT.jar', bldCfg: 'mypet', checkForTriggeredDeployments: 'false', commitID: '', namespace: 'coe-mern-project', showBuildLogs: 'false', verbose: 'false', waitTime: '', waitUnit: 'sec'*/
   /*openshiftExec apiURL: 'https://master.na39.openshift.opentlc.com', arguments: [[value: 'petclinic-pipeline --from-file=target/spring-petclinic-openshift-2.0.0.BUILD-SNAPSHOT.jar --follow']], authToken: 'IdXwlyWZphf_A00Z9X02qMv57gyictVDMoBdA1PobQo', command: 'oc start-build', container: '', namespace: 'coe-mern-project', pod: 'jenkins-5-h7p9s', verbose: 'true', waitTime: '', waitUnit: 'sec'*/

  }
 } 
