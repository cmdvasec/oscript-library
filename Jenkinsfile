#!groovy
node("slave") {
    def isUnix = isUnix();
    stage "checkout"

    if (env.DISPLAY) {
        println env.DISPLAY;
    } else {
        env.DISPLAY=":1"
    }
    env.RUNNER_ENV="production";

    checkout scm
    if (isUnix) {sh 'git submodule update --init'} else {bat "git submodule update --init"}
    stage "init base"

    //checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [[$class: 'SubmoduleOption', disableSubmodules: false, recursiveSubmodules: true, reference: '', trackingSubmodules: true]], submoduleCfg: [], userRemoteConfigs: [[url: 'http://git.http.service.consul/shenja/vanessa-behavior.git']]])
    
    stage "test"

    dir "./tests"
    def command = "oscript finder.os";
    
    timestamps {
        if (isUnix){
            sh "${command}"
        } else {
            bat "chcp 1251\n${command}"
        }
    }

    step([$class: 'JUnitResultArchiver', testResults: '**/tests/*.xml'])
}