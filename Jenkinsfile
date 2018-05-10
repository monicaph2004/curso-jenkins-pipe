pipeline{
	agent any
	
	parameters{
	    string(name: 'tomcat_pre', defaultValue: 'localhost:8090', description: 'Servidor de pre')
	    string(name: 'tomcat_pro', defaultValue: 'localhost:9090', description: 'Servidor de pre')
	}
	
	triggers {
	    // Para que mire al GIT si hay cambios en el código
	    pollSCM('* * * * *')
	}


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
		stage('Despliegues'){
		    parallel{
				stage ('Paso a pre'){
					// Con este paso, se ejecuta una tarea jenkins llamada 'curso-jenkins-realizar-deploy-pre' 
					steps{
						echo 'Se hace el despliegue a pre'
						/* Tenemos que tener unas claves SSH, porque si no, no funciona.
						 * Al generar las claves SSH, es cuando se genera el fichero .pem
						*/
						bat "cp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2- user@$params.tomcat_pre}:/var/lib/tomcat7/webapps"
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
					    
						bat "cp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2- user@$params.tomcat_pro}:/var/lib/tomcat7/webapps"
		
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
	}
}