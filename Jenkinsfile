def regions = [master:'eu-west-1', preprod:'eu-west-1', develop: 'eu-west-2']
def accounts = [master:'production', preprod:'staging', develop:'sandbox']

node('master'){
    stage('Checkout'){
        checkout scm
    }

    stage('Authentication'){
        sh "aws eks update-kubeconfig --name ${accounts[env.BRANCH_NAME]} --region ${regions[env.BRANCH_NAME]}"
    }

    stage('Deploy'){
        sh """
           helm upgrade --install quote ./quote  \
                --set metadata.jenkins.buildTag=${env.BUILD_TAG} \
                --set metadata.git.commitId=${getCommitId()}
        """
    }
}

def getCommitId() {
    sh 'git rev-parse HEAD > .git/commitID'
    def commitID = readFile('.git/commitID').trim()
    sh 'rm .git/commitID'
    commitID
}
