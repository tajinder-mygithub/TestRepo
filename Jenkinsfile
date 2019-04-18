

node {
   def gradleHome
   parameters { 
       string(name: 'DEPLOY_ENV', defaultValue: 'staging', description: '') 
       choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')
   }
   
   environment {
    GitBranch = 'master'
    
   }
      stage('ParamsValue') {
		echo "Choice: ${params.CHOICE}"
		echo "Deployment Type: ${params.DEPLOY_ENV}"
		
		step([$class: 'WsCleanup'])
	  
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
      
   }
   stage('Build') {
      // Run the maven build
      if (isUnix()) {
         sh "'${gradleHome}/bin/gradle' clean build"
      } else {
         bat(/"${gradleHome}\bin\gradle.bat" clean build/)
      }
   }
   stage('Results') {
      junit '**/TEST-*.xml'
      //archive 'build/*.jar'
	  
   }
      stage('Sonar Analysis') {
      if (isUnix()) {
         sh "'${gradleHome}/bin/gradle' sonar"
      } else {
         bat(/"${gradleHome}\bin\gradle.bat" sonar/)
      }
	  
   }

      stage('Artifact Publishing') {
      if (isUnix()) {
         sh "'${gradleHome}/bin/gradle' publish"
      } else {
         bat(/"${gradleHome}\bin\gradle.bat" publish/)
      }
	  
   }

}