pipeline{
	agent any
	stages{
		stage ('Build'){
			// Generar el fichero war
			steps{
				echo 'Generando fichero war'
				bat 'mvn clean package' 
			}
			post {
				// Si ha salido todo bien, entonces, guardamos el war
			    success {
			        echo 'Guardando war'
			        archiveArtifacts artifacts: '**/target/*.war'
			    }
			}

		}
		stage ('Paso a pre'){
			// Con este paso, se ejecuta una tarea jenkins llamada 'curso-jenkins-realizar-deploy-pre' 
			steps{
				echo 'Se hace el despliegue a pre'
				build job: 'curso-jenkins-realizar-deploy-pre'
			}
		}
		stage ('Paso a pro'){
			steps{
				// Si transcurridos cinco d�as desde que se gener� el war no pasa a producci�n,
				// falla el proceso.
				// Primero pregunta �Aprobar el paso a producci�n? Si dice que Aceptar, hace el
				// build job, si no, aborta el proceso
			    timeout(time:5, unit:'DAYS'){
			        input message: '�Aprobar el paso a producci�n?'
			    }
			    
			    build job: 'curso-jenkins-realizar-deploy-produccion'

			}
			
			post{
			    success{
			        echo 'Realizado el paso a producci�n.'
			    }
			    failure{
			        echo 'Ha fallado el paso a producci�n.'
			    }

			}

		}
	}
}