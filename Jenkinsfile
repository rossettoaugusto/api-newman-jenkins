pipeline {
    agent any
    tools {
        nodejs 'node-18'
    }
    stages {
        stage('Install') {
            steps {
                dir('/Users/augustorossetto/projetos/api-tests') {
                    sh 'npm install'
                }
            }
        }
        stage('Run API Tests') {
            steps {
                dir('/Users/augustorossetto/projetos/api-tests') {
                    sh '''
                        npx newman run collections/desafio_augusto_rossetto.json \
                          --reporters cli,junit,htmlextra \
                          --reporter-junit-export results/newman-results.xml \
                          --reporter-htmlextra-export results/newman-report.html
                    '''
                }
            }
        }
    }
    post {
        always {
            dir('/Users/augustorossetto/projetos/api-tests') {
                junit 'results/newman-results.xml'
                archiveArtifacts artifacts: 'results/**', allowEmptyArchive: true
                publishHTML(target: [
                    allowMissing         : false,
                    alwaysLinkToLastBuild: true,
                    keepAll              : true,
                    reportDir            : 'results',
                    reportFiles          : 'newman-report.html',
                    reportName           : 'Newman Report'
                ])
            }
        }
    }
}
```

---

Depois do próximo build vai aparecer **"Newman Report"** no menu lateral do job assim:
```
api-tests/
├── Construir agora
├── Configurar
├── Newman Report       ← novo botão aqui
└── Histórico de builds