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
				// Si transcurridos cinco días desde que se generó el war no pasa a producción,
				// falla el proceso.
				// Primero pregunta ¿Aprobar el paso a producción? Si dice que Aceptar, hace el
				// build job, si no, aborta el proceso
			    timeout(time:5, unit:'DAYS'){
			        input message: '¿Aprobar el paso a producción?'
			    }
			    
			    build job: 'curso-jenkins-realizar-deploy-produccion'

			}
			
			post{
			    success{
			        echo 'Realizado el paso a producción.'
			    }
			    failure{
			        echo 'Ha fallado el paso a producción.'
			    }

			}

		}
	}
}