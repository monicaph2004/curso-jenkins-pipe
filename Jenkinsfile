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
		
		stage ('Validación y paso a pre'){
		    parallel{
		    	stage ('Validación checkstyle') {
					// Se va a hacer el checkstyle. Falta que luego se vean los resultados.
					// Se podría hacer por aquí, pero eso no lo damos en el curso.
					// En el curso nos han dicho que hagamos una tarea checkstyle y hagamos build job
					steps{
					    echo 'Hacer el checkstyle'
					    bat 'mvn checkstyle:checkstyle' 
					}
		    	}
				stage ('Paso a pre'){
					// Con este paso, se ejecuta una tarea jenkins llamada 'curso-jenkins-realizar-deploy-pre' 
					steps{
						echo 'Se hace el despliegue a pre'
						build job: 'curso-jenkins-realizar-deploy-pre'
					}
				}
			}
		}
		
		stage ('Paso a pro'){
			steps{
				/* Si transcurridos cinco días desde que se generó el war no pasa a producción,
				 * falla el proceso. Y no pregunta si quieres realizar el paso a producción, simplemente
				 * da error.
				 *
				 * Al realizar la pregunta ¿Aprobar el paso a producción? Si dice que Aceptar,
				 * hace el build job, si no, aborta el proceso
				*/
				timeout(time:5, unit:'DAYS'){
			        input message: '¿Aprobar el paso a producción?'
			    }
			    
			    build job: 'curso-jenkins-realizar-deploy-produccion'

			}
			
			post{
				// Se llega a aquí, si se aprueba el paso a producción con la pregunta "¿Aprobar el paso a producción?"
			    success{
			        echo 'Realizado el paso a producción.'
			    }
			    // Se lleva a aquí, cuando se aborta el paso a producción con la pregunta "¿Aprobar el paso a producción?"
			    failure{
			        echo 'Ha fallado el paso a producción.'
			    }

			}

		}
	}
}