node {
    
    stage('Clone Repository') {
        checkout scm
    }  
    
    stage('Dependency Check') {
      environment {
          analyzer.bundle.audit.enabled=false
      }
      sh 'echo "Trying to run depedency check"'
      sh 'echo $analyzer.bundle.audit.enabled'
      dependencyCheckAnalyzer datadir: 'dependency-check-data', isFailOnErrorDisabled: true, hintsFile: '', includeCsvReports: false, includeHtmlReports: true, includeJsonReports: false, isAutoupdateDisabled: false, outdir: '', scanpath: 'package.json', skipOnScmChange: false, skipOnUpstreamChange: false, suppressionFile: '', zipExtensions: ''
      dependencyCheckPublisher canComputeNew: false, defaultEncoding: '', healthy: '', pattern: '', unHealthy: ''
      archiveArtifacts allowEmptyArchive: true, artifacts: '**/dependency-check-report.*', onlyIfSuccessful: true
      step([$class: 'DependencyCheckPublisher', unstableTotalAll: '0'])
    }
    
    stage('SonarQube analysis') {
    def scannerHome = tool 'sonarqubeScanner';
    withSonarQubeEnv('sonarqubeServer') {
      sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=hrostenpipeline -Dsonar.sources=./app -Dsonar.host.url=http://10.48.253.181:9000 -Dsonar.login=d62a31f8ae0f6e6fd2482f07e80611edc1fc03f6"
    }
  }
}
