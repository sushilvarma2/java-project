pipeline {
   agent none
   stages {
	stage('Unit Tests') { 
	agent {
	    label 'apache'
         }
	steps { 
	 sh 'ant -f test.xml -v'
         junit 'reports/result.xml'
}

}
     stage('build') {
	agent {
    	label 'apache'
         }

       steps {
         sh 'ant -f build.xml -v'
	    }
	post {
     always {
      archiveArtifacts artifacts: 'dist/*.jar', fingerprint:true
		}
	}		
		
		}
     stage('deploy') {
	agent {
    	label 'apache'
         }
	
       steps { 
	 sh "mkdir /var/www/html/rectangles/all/${env.BRANCH_NAME}"
         sh "cp dist/rectangle.jar /var/www/html/rectangles/all/${env.BRANCH_NAME}/"
	}	

}
	stage("Running on Centos") {
	agent {
	label 'CentOS'
		}
	steps {
	  sh "wget http://sushilvarma2-gmail-com6.mylabserver.com/rectangles/all/rectangle.jar"
          sh "java -jar rectangle.jar 3 4"

	      }	
}
	stage("Test on Debian") {
	agent { 
	   docker 'openjdk:8u131-jre'
		}
	steps { 
	   sh "wget http://sushilvarma2-gmail-com6.mylabserver.com/rectangles/all/${env.BRANCH_NAME}/rectangle.jar"
          sh "java -jar rectangle.jar 3 4" 
		}	

}
	stage('Promote to Green'){ 
	agent {
	  label 'apache'
		}
	when { 
	  branch 'master'
		}
	steps {
	  sh "cp /var/www/html/rectangles/all/rectangle.jar /var/www/html/rectangles/green/rectangle.jar"
		}
	}
	stage('Promote Development Branch to Master') { 
	  agent { 
	    label 'apache'
}
   	  when {
	    branch 'development'
		}
	  steps { 
	    echo "Stashing Any Local Changes"
            sh 'git stash'
            echo "checking Out Development Branch"
            sh 'git checkout development'
            echo 'Checking Out Master Branch'
            sh 'git checkout master'
  	    echo 'Merging Development into Master Branch'
	    sh 'git merge development'
	    echo 'Pushing to Origin Master'
	    sh 'git push origin master'
}

}	

}		
}
