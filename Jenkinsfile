#!groovy
node("slave") {
    def isUnix = isUnix();
    stage "checkout"

    if (isUnix && !env.DISPLAY) {
       env.DISPLAY=":1"
    }
    
    checkout scm
    if (isUnix) {sh 'git submodule update --init'} else {bat "git submodule update --init"}
    
    stage "test"

    dir "./tests"
    def commandToRun = "oscript finder.os";
    echo pwd
    echo "${commandToRun}" 

    if (isUnix){
        sh "pushd ./tests/ \n${commandToRun}\npopd"
    } else {
        bat "chcp 1251\npushd ./tests/ \n${commandToRun}\npopd"
    }
    
    step([$class: 'JUnitResultArchiver', testResults: '**/tests/*.xml'])
}