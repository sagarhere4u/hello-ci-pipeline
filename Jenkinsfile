pipeline {
  agent any

  stages {
    stage('hellosetup - Checkout Scm') {
      steps {
            checkout([
            $class: 'GitSCM',
            branches: [[name: 'main']],
            userRemoteConfigs: [[url: 'https://github.com/sagarhere4u/hello-flask.git']]
        ])
      }
    }

    stage('hellosetup - Shell script 0') {
      steps {
        sh 'sudo mkdir -p /var/www/hello ; sudo cp -fr * /var/www/hello/ ; sudo chmod -R 775 /var/www/hello ; sudo cp -fr /var/www/hello/hello.service /etc/systemd/system/ ; sudo systemctl daemon-reload ; sudo systemctl restart hello'
      }
    }


    stage('hellotest - Checkout Scm') {
      steps {
            checkout([
            $class: 'GitSCM',
            branches: [[name: 'main']],
            userRemoteConfigs: [[url: 'https://github.com/sagarhere4u/hello-flask-test.git']]
        ])
      }
    }

    stage('hellotest - Maven Build 0') {
      steps {
        sh 'mvn clean verify'
      }
      
      post{
      		always{
	      		dir("target/jmeter/report/"){
				      perfReport '**/*.jtl'
	    	 	}
      		
      		}
      	}
    }

    stage('hellosave - Shell script 0') {
      steps {
        sh 'sudo tar -cvf hello.tar /var/www/hello'
      }
    }

    stage('helloclean - Shell script 0') {
      steps {
        sh 'sudo systemctl stop hello ; sudo rm -fr /var/www/hello ; sudo rm -fr /etc/systemd/system/hello.service ; sudo systemctl daemon-reload'
      }
    }


  }
}
