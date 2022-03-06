pipeline {
	agent any
		stages{
			stage(compile){
				steps {
					echo 'compiling.....'
					git 'https://github.com/SahiDevOps/samplejavaapp'
					sh '/opt/apache-maven-3.8.4/bin/mvn compile'
				      }
			          }
			stage(codereview){
				steps {
					echo 'codereview.....'
					sh '/opt/apache-maven-3.8.4/bin/mvn -P metrics pmd:pmd'
				      }
				post {
				  success {
					recordIssues(tools: [pmdParser(pattern: '**/pmd.xml')])
				          }
				     }
				}
			stage(unittest){
				steps {
					echo 'unittest.....'
				        sh '/opt/apache-maven-3.8.4/bin/mvn test'
				      }
				post {
				  success {
					junit 'target/surefire-reports/*.xml'
				          }
				     }
				}
			stage(codecoverage){
				steps {
					echo 'codecoverage.....'
				        sh '/opt/apache-maven-3.8.4/bin/mvn cobertura:cobertura -Dcobertura.report.format=xml'
				      }
				post {
				  success {
					cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: 'target/site/cobertura/coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false
				          }
				     }
				}
			stage(package){
				steps {
					echo 'package.....'
					sh '/opt/apache-maven-3.8.4/bin/mvn package'
				      }
				}
			}
	}

