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
		stage ('Etapa 2'){
			steps{
				echo 'Hola mundo'
			}
		}
	}
}