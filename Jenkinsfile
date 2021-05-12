#!/usr/bin/env groovy
import groovy.yaml.YamlSlurper





node("build") {
    def envs = []
    envs.add(0, '')
    envs.add(1, 'dev')

    properties([
            buildDiscarder(logRotator(numToKeepStr: '20')),
            parameters([
                    choice(choices: envs, description: 'project name to update configs', name: 'project')
            ])
    ])

    stage('checkout') {
        checkout scm

        configs = new YamlSlurper().parseText(new File("prod/application_settings.yml").text)

        configs.each { service ->
            service_name = service.key
            println("/service/${service_name} --> ${service}")
        }
    }
}


