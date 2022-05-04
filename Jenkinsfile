pipeline {
    agent { label 'master' }
    options {
        ansiColor('vga')
    }
    parameters {
        string(name: 'hostname', defaultValue: '', description: 'Enter your IP server here')
        string(name: 'zabbix_server', defaultValue: '103.127.206.135', description: 'Enter IP of Zabbix server here')
    }
    stages {
        stage ("Install zabbix-agent") {
            steps {
                ansiblePlaybook (
                    playbook: '${WORKSPACE}/tuantest-install-zabbix-agent.yml',
                    inventory: '${WORKSPACE}/hosts_all_server',
                    tags: 'install-zabbix-agent',
                    extraVars: [
                        hostname: [value: '${hostname}', hidden: false],
                        zabbix_server: [value: '${zabbix_server}', hidden: false]
                        ]
                )
            }
        }
        stage ("Copy config file") {
            steps {
                ansiblePlaybook (
                    playbook: '${WORKSPACE}/tuantest-install-zabbix-agent.yml',
                    inventory: '${WORKSPACE}/hosts_all_server',
                    tags: 'copy-config-file',
                    extraVars: [
                        hostname: [value: '${hostname}', hidden: false],
                        zabbix_server: [value: '${zabbix_server}', hidden: false]
                        ]
                )
            }
        }
    }
    post {
        always {
            // notifyEvents message: '''Tuan is testing
            // Build <a href="$PROJECT_URL">$PROJECT_NAME</a>
            // Build Number <a href="$BUILD_URL">$BUILD_NUMBER</a> result with status: <b>$BUILD_STATUS</b>
            // <a href="$BUILD_URL/console">Build log</a> on host ${hostname}''',
            //     token: 'zqMYpf7aCt0Wl3T_IMdsh-LOUzf7_G8T'

            emailext subject: 'Jenkins ${BUILD_STATUS} [#${BUILD_NUMBER}] - ${PROJECT_NAME}',
            body: '''Build <a href="$PROJECT_URL">$PROJECT_NAME</a> <br>
            Build Number <a href="$BUILD_URL">$BUILD_NUMBER</a> result with status: <b>$BUILD_STATUS</b> <br>
            <a href="$BUILD_URL/console">Build log</a> on host ${hostname}''',
            to: 'tech@tel4vn.com'

            telegramSend(message: '''Build [$PROJECT_NAME]($PROJECT_URL) \nBuild Number [$BUILD_NUMBER]($BUILD_URL) result with status: *$BUILD_STATUS* \n[Build log]($BUILD_URL/console) on host ${hostname}''',
            chatId:-535274016)
        }
    }
}
