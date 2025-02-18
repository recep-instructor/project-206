pipeline {
    agent any
    environment {
        OPENSHIFT_PROJECT = 'rcpmrl-dev' // OpenShift projenizin adı
        GITHUB_REPO = 'https://github.com/recep-instructor/project-206.git' // GitHub repository URL
        FRONTEND_IMAGE_NAME = 'my-frontend-image' // OpenShift'de oluşturulacak imaj adı
        BACKEND_IMAGE_NAME = 'my-backend-image'
    }
    stages {
        stage('Create BuildConfig-frontend') {
            steps {
                script {
                    // OpenShift BuildConfig YAML dosyasını oluşturun
                    def buildConfig = """
                    apiVersion: build.openshift.io/v1
                    kind: BuildConfig
                    metadata:
                      name: ${FRONTEND_IMAGE_NAME}-build
                      namespace: ${OPENSHIFT_PROJECT}
                    spec:
                      source:
                        type: Git
                        git:
                          uri: '${GITHUB_REPO}'
                      strategy:
                        type: Docker
                        dockerStrategy:
                          dockerfilePath: ./image_for_web_server/Dockerfile
                      output:
                        to:
                          kind: ImageStreamTag
                          name: ${FRONTEND_IMAGE_NAME}:latest
                    """
                    
                    // BuildConfig'i OpenShift'e uygulayın
                    writeFile file: 'buildconfig.yaml', text: buildConfig
                    sh "oc apply -f buildconfig.yaml -n ${OPENSHIFT_PROJECT}"
                }
            }
        }
        stage('Start Build') {
            steps {
                script {
                    // Build'i başlatın
                    sh "oc start-build ${FRONTEND_IMAGE_NAME}-build -n ${OPENSHIFT_PROJECT}"
                }
            }
        }
        stage('Create BuildConfig-backend') {
            steps {
                script {
                    // OpenShift BuildConfig YAML dosyasını oluşturun
                    def buildConfig = """
                    apiVersion: build.openshift.io/v1
                    kind: BuildConfig
                    metadata:
                      name: ${BACKEND_IMAGE_NAME}-build
                      namespace: ${OPENSHIFT_PROJECT}
                    spec:
                      source:
                        type: Git
                        git:
                          uri: '${GITHUB_REPO}'
                      strategy:
                        type: Docker
                        dockerStrategy:
                          dockerfilePath: ./image_for_result_server/Dockerfile
                      output:
                        to:
                          kind: ImageStreamTag
                          name: ${BACKEND_IMAGE_NAME}:latest
                    """
                    
                    // BuildConfig'i OpenShift'e uygulayın
                    writeFile file: 'buildconfig.yaml', text: buildConfig
                    sh "oc apply -f buildconfig.yaml -n ${OPENSHIFT_PROJECT}"
                }
            }
        }
        stage('Start Build') {
            steps {
                script {
                    // Build'i başlatın
                    sh "oc start-build ${BACKEND_IMAGE_NAME}-build -n ${OPENSHIFT_PROJECT}"
                }
            }
        }

    }
    post {
        success {
            echo 'Build completed successfully!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}