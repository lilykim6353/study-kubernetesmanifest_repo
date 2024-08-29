node {
    def app

    stage('Clone repository') {     
        checkout scm
    }

    stage('Update GIT') {
        script {
            // withCredentials는 Jenkins Pipeline에서 사용되는 구문으로 credentials을 안전하게 처리
            // credentialsId는 Jenkins에서 생성한 github credential 값
            withCredentials([usernamePassword(credentialsId: 'github-credentials', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                
                // github email
                sh "git config user.email {github email}"
                
                // github user name
                sh "git config user.name {github user name}"

                sh "cat deployment.yaml"

                // sed 명령어를 이용하여 deployment.yaml 파일 내용 수정
                // deployment.yaml에서 image에 해당하는 '{aws ecr url}'을 찾은 후 '{aws ecr url}:buildimage job의 빌드 넘버'로 변경
                sh "sed -i 's+{aws ecr url}.*+{aws ecr url}:${DOCKERTAG}+g' deployment.yaml"
                
                sh "cat deployment.yaml"
                
                sh "git add ."
                
                sh "git commit -m 'Done by Jenkins Job changemanifest: ${env.BUILD_NUMBER}'"
                
                // kubernetesmanifest repo에 변경 사항 push
                // kubernetesmanifest용 git repository 이름이 study-kubernetesmanifest_repo가 아닐 경우 하기 코드에서 수정
                sh 'git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/${GIT_USERNAME}/study-kubernetesmanifest_repo.git HEAD:main'
            }                
        }
    }
}
