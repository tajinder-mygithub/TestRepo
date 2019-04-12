

node {
   def gradleHome
   parameters { 
       string(name: 'DEPLOY_ENV', defaultValue: 'staging', description: '') 
       
   }
   
   environment {
    GitBranch = 'master'
    
   }

   stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      //git 'https://github.com/tajinder-mygithub/TestRepo'
       git branch: 'master',
                credentialsId: 'tajgithubcreds',
                url: 'https://github.com/tajinder-mygithub/TestRepo'
      // Get the Maven tool.
      // ** NOTE: This 'M3' Maven tool must be configured
      // **       in the global configuration.           
      gradleHome = tool 'gradle31'
      echo "Deployment Type: ${params.DEPLOY_ENV}"
   }
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
         sh "'${gradleHome}/bin/gradle' build"
      } else {
         bat(/"${gradleHome}\bin\gradle.bat" build/)
      }
   }
   stage('Results') {
      junit '**/TEST-*.xml'
      //archive 'build/*.jar'
   }
}