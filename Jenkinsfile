pipeline {
    agent any
    stages {
        stage('Clone'){
            steps {
                git url: 'https://github.com/jfrogdev/project-examples.git'
            }
        }

        stage('Artifactory download and upload'){
            steps {
                script{
                    // Obtain an Artifactory server instance, defined in Jenkins --> Manage:
                    def server = Artifactory.server 'ARTIFACTORY_1'

                    // Read the download and upload specs:
                    //def downloadSpec = readFile 'jenkins-examples/pipeline-examples/resources/props-download.json'
                    def downloadSpec = """{
                                         "files": [
                                          {
                                              "pattern": "generic-local/*.zip",
                                              "target": "generic/"
                                            }
                                         ]
                                        }"""
                    //def uploadSpec = readFile 'jenkins-examples/pipeline-examples/resources/props-upload.json'
                    def uploadSpec = """{
                                         "files": [
                                          {
                                              "pattern": "generic/*.zip",
                                              "target": "generic-local/*.zip""
                                            }
                                         ]
                                        }"""

                    // Download files from Artifactory:
                    def buildInfo1 = server.download spec: downloadSpec
                    // Upload files to Artifactory:
                    def buildInfo2 = server.upload spec: uploadSpec

                    // Merge the local download and upload build-info instances:
                    buildInfo1.append buildInfo2

                    // Publish the merged build-info to Artifactory
                    server.publishBuildInfo buildInfo1
                }
            }
        }
    }
}
