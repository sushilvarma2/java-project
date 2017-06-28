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
         sh "cp dist/rectangle.jar /var/www/html/rectangles/all/"
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
}		
}
