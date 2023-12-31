pipeline {
    agent any
    stages {
        stage('verfiy version') {
            steps {
                sh 'ls /'
            }
        }
        stage('Static Analysis') {
                parallel {
                  stage('CodeSniffer') {
                      steps {
                          sh 'vendor/bin/phpcs --standard=phpcs.xml .'
                      }
                  }
                  stage('PHP Compatibility Checks') {
                      steps {
                          sh 'vendor/bin/phpcs --standard=phpcs-compatibility.xml .'
                      }
                  }
                  stage('PHPStan') {
                      steps {
                          sh 'vendor/bin/phpstan analyse --error-format=checkstyle --no-progress -n . > build/logs/phpstan.checkstyle.xml'
                      }
                  }
                }
            }
            post {
                always {
                    recordIssues([
                        sourceCodeEncoding: 'UTF-8',
                        enabledForFailure: true,
                        aggregatingResults: true,
                        blameDisabled: true,
                        referenceJobName: "repo-name/master",
                        tools: [
                            phpCodeSniffer(id: 'phpcs', name: 'CodeSniffer', pattern: 'build/logs/phpcs.checkstyle.xml', reportEncoding: 'UTF-8'),
                            phpStan(id: 'phpstan', name: 'PHPStan', pattern: 'build/logs/phpstan.checkstyle.xml', reportEncoding: 'UTF-8'),
                            phpCodeSniffer(id: 'phpcompat', name: 'PHP Compatibility', pattern: 'build/logs/phpcs-compat.checkstyle.xml', reportEncoding: 'UTF-8')
                        ]
                    ])
                }
            }
        }
    }
}
