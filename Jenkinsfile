
node('nodejs') {
    stage('test npm') {
        sh("node --version")
        sh("npm --version")
        sh("oc whoami")
    }
    stage ('pull code') {
        git branch: branch, url: gitRepo
    }
    stage ('build') {
        sh("npm install")
        sh("npm run build")
    }
    stage('check and prepare') {
        sh("cd /tmp")
        sh("pwd")
        sh("ls -alrth")
    }
    stage ('deploy') {
        try {
            sh("oc delete bc hello-react")
        } catch (Exception e) {
            sh("echo \"fail deleting bc \"")
        }
        try {
            sh("oc delete is hello-react")
        } catch (Exception e) {
            sh("echo \"fail deleting is \"")
        }
        try {
            sh("oc delete svc hello-react")
        } catch (Exception e) {
            sh("echo \"fail deleting svc \"")
        }
        try {
            sh("oc delete route hello-react")
        } catch (Exception e) {
            sh("echo \"fail deleting route \"")
        }
        sh("oc new-build --binary=true --name=hello-react --image-stream=nginx-116-rhel7:1-64.1638430384")
        sh("oc start-build hello-react --from-dir=build --follow --wait" )
 
        try {
            sh("oc new-app  hello-react --name=hello-react" )
        } catch (Exception e) {
            sh("echo \"fail creating new-app, dc exists \"")
        }
 
        sh("oc expose svc/hello-react --name=hello-react")
    }
}
