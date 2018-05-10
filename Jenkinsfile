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
				/* Si transcurridos cinco d�as desde que se gener� el war no pasa a producci�n,
				 * falla el proceso. Y no pregunta si quieres realizar el paso a producci�n, simplemente
				 * da error.
				 *
				 * Al realizar la pregunta �Aprobar el paso a producci�n? Si dice que Aceptar,
				 * hace el build job, si no, aborta el proceso
				*/
				timeout(time:5, unit:'DAYS'){
			        input message: '�Aprobar el paso a producci�n?'
			    }
			    
			    build job: 'curso-jenkins-realizar-deploy-produccion'

			}
			
			post{
				// Se llega a aqu�, si se aprueba el paso a producci�n con la pregunta "�Aprobar el paso a producci�n?"
			    success{
			        echo 'Realizado el paso a producci�n.'
			    }
			    // Se lleva a aqu�, cuando se aborta el paso a producci�n con la pregunta "�Aprobar el paso a producci�n?"
			    failure{
			        echo 'Ha fallado el paso a producci�n.'
			    }

			}

		}
	}
}