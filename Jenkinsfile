import org.yaml.snakeyaml.Yaml

node("build") {

    def envs = []
    envs.add(0, 'prod')
    envs.add(1, 'prod')
    envs.add(1, 'dev')

    properties([
            buildDiscarder(logRotator(numToKeepStr: '20')),
            parameters([
                    choice(choices: envs, description: 'project name to update configs', name: 'project')
            ])
    ])

    stage('Push configs') {
        checkout scm
        withAWS(credentials: 'aws-ecs-jenkins', region: 'eu-central-1') {
            configs = readYaml file: "${params.project}/application_settings.yml"

            configs.each { service ->
                sh "aws ssm put-parameter --name \"/apps/environment/prod/${service.key}\" --type \"String\" --value \'${service}\' --overwrite"
            }
        }
    }
}


