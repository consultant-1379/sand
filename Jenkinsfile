#!/usr/bin/env groovy

/* IMPORTANT:
 *
 * In order to make this pipeline work, the following configuration on Jenkins is required:
 * - slave with a specific label (see pipeline.agent.label below)
 * - credentials plugin should be installed and have the secrets with the following names:
 *  + lciadm100credentials (token to access Artifactory)
 */

def defaultBobImage = 'armdocker.rnd.ericsson.se/sandbox/adp-staging/adp-cicd/bob.2.0:1.5.2-0'
def bob = new BobCommand()
        .bobImage(defaultBobImage)
        .envVars([ISO_VERSION: '${ISO_VERSION}'])
        .needDockerSocket(true)
        .toString()
def GIT_COMMITTER_NAME = 'enmadm100'
def GIT_COMMITTER_EMAIL = 'enmadm100@ericsson.com'
def failedStage = ''

def emailReport() {
    def mail_list = ["eric-enmsg-access-control":"PDLTEAMTOT@pdl.internal.ericsson.com","eric-enmsg-amos":"PDLENMOUTS@pdl.internal.ericsson.com","eric-enmsg-auto-id-solr":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-autoid-service":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-autoprovisioning":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-cellserv":"PDLNMFEATU@pdl.internal.ericsson.com","eric-enmsg-cmevents":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-cmutilities":"PDLTEAMTOT@pdl.internal.ericsson.com","eric-enmsg-com-ecim-mscm":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-comecimpolicy":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-dc-history":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-domain-proxy-coordinator":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-dlms":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-dpmediation":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-eventbasedclient":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-flowautomation":"PDLTEAMTOT@pdl.internal.ericsson.com","eric-enmsg-fls":"PDLTEAMTOT@pdl.internal.ericsson.com","eric-enmsg-fm-alarm-processing":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-fm-history":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-fm-service":"PDLTEAMTOT@pdl.internal.ericsson.com","eric-enmsg-general-scripting":"PDLENMOUTS@pdl.internal.ericsson.com","eric-enmsg-gossiprouter-cache":"PDLTEAMTOT@pdl.internal.ericsson.com","eric-enmsg-gossiprouter-remoting":"PDLTEAMTOT@pdl.internal.ericsson.com","eric-enmsg-identity-mgmt-service":"PDLAPOLLO1@pdl.internal.ericsson.com","eric-enmsg-import-export-service":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-ip-service-management":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-jmsserver":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-kpi-calc-serv":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-kpi-service":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-lcmservice":"PDLTEAMTOT@pdl.internal.ericsson.com","eric-enmsg-medrouter":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-msap":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-msapgfm":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-mscm":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-mscmapg":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-mscmip":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-msfm":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-mskpirt":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-msnetlog":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-mspm":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-mspmip":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-mssnmpcm":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-mssnmpfm":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-nb-alarm-irp-agent-corba":"EricssonHyderabad.ENMGalaxy@tcs.com","eric-enmsg-nb-fm-snmp":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-nbi-bnsi-fm":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-nedo-serv":"PDLTEAMTOT@pdl.internal.ericsson.com","eric-enmsg-node-plugins":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-nodecli":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-openidm":"PDLNMFEATU@pdl.internal.ericsson.com","eric-enmsg-pki-ra-service":"PDLTEAMTOT@pdl.internal.ericsson.com","eric-enmsg-pmic-router-policy":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-pmservice":"PDLTEAMTOT@pdl.internal.ericsson.com","eric-enmsg-sa-service":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-security-service":"PDLTEAMTOT@pdl.internal.ericsson.com","eric-enmsg-sentinel":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-shm-core-service":"PDLTEAMTOT@pdl.internal.ericsson.com","eric-enmsg-shmservice":"PDLTEAMTOT@pdl.internal.ericsson.com","eric-enmsg-smrs-service":"PDLTEAMTOT@pdl.internal.ericsson.com","eric-enmsg-sps-service":"PDLTEAMTOT@pdl.internal.ericsson.com","eric-enmsg-sso":"PDLTEAMTOT@pdl.internal.ericsson.com","eric-enmsg-supervisionclient":"EricssonHyderabad.ENMMisty@tcs.com","eric-enm-troubleshooting-utils":"PDLTORDEPL@pdl.internal.ericsson.com","eric-enmsg-uiservice":"PDLTEAMTOT@pdl.internal.ericsson.com","eric-enmsg-visinaming-nb":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-visinaming-sb":"EricssonHyderabad.ENMMisty@tcs.com","eric-enmsg-vault-service":"PDLNMFEATU@pdl.internal.ericsson.com","eric-enmsg-web-push-service":"PDLTEAMTOT@pdl.internal.ericsson.com","eric-enmsg-ebs-flow":"PDLTEAMTOT@pdl.internal.ericsson.com","eric-enmsg-ebs-topology":"PDLTEAMTOT@pdl.internal.ericsson.com","eric-enmsg-ebs-controller":"PDLNMFEATU@pdl.internal.ericsson.com","eric-enm-data-migration":"PDLAPOLLO1@pdl.internal.ericsson.com"]
        mail_list.each { each_repo,each_email ->
            if (each_repo == env.REPO.split('/').last()) {
                def Repo_Mail = each_email;
                try {
                    mail to: "${Repo_Mail}",
                    subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                    body: "Failure on ${env.BUILD_URL}"
                } catch( err ){
                     echo "$err"
                }
            }
        };
}

pipeline {
    agent {
        label 'Cloud-Native'
    }
    parameters {
        string(name: 'ISO_VERSION', description: 'The ENM ISO version (e.g. 1.65.77)')
        string(name: 'SPRINT_TAG', description: 'Tag for GIT tagging the repository after build')
        string(name: 'PRODUCT_SET', description: 'cENM product set (e.g. 21.1.13-1)')
    }
    environment {
        GERRIT_HTTP_CREDENTIALS_FUser = credentials('FUser_gerrit_http_username_password')
    }
    stages {
        stage('Clean') {
            steps {
                deleteDir()
            }
        }
        stage('Inject Credential Files') {
            steps {
                withCredentials([file(credentialsId: 'lciadm100-docker-auth', variable: 'dockerConfig')]) {
                    //sh "install -m 600 ${dockerConfig} ${HOME}/.docker/config.json"
                    sh '''    
                        if [ ! -f ${HOME}/.docker/config.json ]; then
                            echo "File not found!  Installing permission change file"
                            install -m 600 ${dockerConfig} ${HOME}/.docker/config.json
                        else
                            echo "Config.json file exists . Moving to next stage"
                        fi
                    '''
                }
            }
        }
        stage('Checkout Cloud-Native SG Git Repository') {
            steps {
                git branch: 'master',
                     credentialsId: 'enmadm100_private_key',
                     url: '${GERRIT_MIRROR}/'+env.REPO
                sh '''
                    git remote set-url origin --push https://${GERRIT_HTTP_CREDENTIALS_FUser}@${GERRIT_CENTRAL_HTTP_E2E}/${REPO}
                '''
            }
        }
        stage('Checkout SG RPM Git Repository') {
            steps {
                script {
                    if (env.REPO.split('/').last() == "eric-enmsg-auto-id-solr" || env.REPO.split('/').last() == "eric-enmsg-amos" || env.REPO.split('/').last() == "eric-enmsg-general-scripting") {
                        sgcontainer_repo = "${REPO}".split('enmsg-').last()
                        if (sgcontainer_repo == "amos") {
                            sgcontainer_repo = "AMOS"
                        }
                        dir("${sgcontainer_repo}") {
                            git branch: 'master',
                                credentialsId: 'enmadm100_private_key',
                                url: '${GERRIT_MIRROR}/'+'OSS/com.ericsson.oss.servicegroupcontainers/'+"${sgcontainer_repo}"
                        }
                     }
                }
            }
        }
        stage('Helm Dep Up ') {
            steps {
                sh "${bob} helm-dep-up"
            }
            post {
                failure {
                    script {
                        failedStage = env.STAGE_NAME
                    }
                }
            }
        }
        stage('Merge values files') {
            steps{
                 script {
                     appconfig_values = sh (script: "ls ${WORKSPACE}/chart/${CHART_DIR}/appconfig/ | grep values.yaml", returnStatus: true)
                     if (appconfig_values == 0) {
                          sh("${bob} merge-values-files-with-appconfig")
                     } else {
                          sh("${bob} merge-values-files")
                     }
                     sh '''
                         if git status | grep 'values.yaml' > /dev/null; then
                            git add chart/${CHART_DIR}/values.yaml
                            git commit -m "NO JIRA - Merging Values.yaml file with common library values.yaml"
                         fi
                     '''
                }
            }
            post {
                failure {
                    script {
                        failedStage = env.STAGE_NAME
                    }
                }
            }
        }
        stage('Helm Lint') {
            steps {
                sh "${bob} lint-helm"
            }
            post {
                failure {
                    script {
                        failedStage = env.STAGE_NAME
                    }
                }
            }
        }
        stage('Linting Dockerfile') {
            steps {
                sh "${bob} lint-dockerfile"
                archiveArtifacts '*dockerfilelint.log'
            }
            post {
                failure {
                    script {
                        failedStage = env.STAGE_NAME
                    }
                }
            }
        }
        stage('ADP Helm Design Rule Check') {
            steps {
                sh "${bob} test-helm || true"
                archiveArtifacts 'design-rule-check-report.*'
            }
            post {
                failure {
                    script {
                        failedStage = env.STAGE_NAME
                    }
                }
            }
        }
        stage('Swap versions in Dockerfile and values.yaml file'){
            steps{
                echo sh(script: 'env', returnStdout:true)
                step ([$class: 'CopyArtifact', projectName: 'sync-build-trigger', filter: "*"]);
                sh "${bob} swap-latest-versions-with-numbers"
                sh '''
                    if git status | grep 'Dockerfile\\|values.yaml' > /dev/null; then
                        git commit -m "NO JIRA - Updating Dockerfile and Values.yaml files with base images version"
                        git push origin HEAD:master
                    else
                        echo `date` > timestamp
                        git add timestamp
                        git commit -m "NO JIRA - Time Stamp "
                        git push origin HEAD:master
                    fi
                '''
            }
        }
        stage('Build Image and Chart') {
            steps {
                sh "${bob} generate-new-version build-helm build-image-with-all-tags"
            }
            post {
                failure {
                    script {
                        failedStage = env.STAGE_NAME
                        sh "${bob} remove-image-with-all-tags"
                    }
                }
            }
        }
        stage('Retrieve image version') {
            steps {
                script {
                    env.IMAGE_TAG = sh(script: "cat .bob/var.version", returnStdout:true).trim()
                    echo "${IMAGE_TAG}"
                }
            }
            post {
                failure {
                    script {
                        failedStage = env.STAGE_NAME
                        sh "${bob} remove-image-with-all-tags"
                    }
                }
            }
        }
        stage('Generate ADP Parameters') {
            steps {
                sh "${bob} generate-output-parameters"
                archiveArtifacts 'artifact.properties'
            }
            post {
                failure {
                    script {
                        failedStage = env.STAGE_NAME
                    }
                }
            }
        }
        stage('Publish Images to Artifactory') {
            steps {
                sh "${bob} push-image-with-all-tags"
            }
            post {
                failure {
                    script {
                        failedStage = env.STAGE_NAME
                        sh "${bob} remove-image-with-all-tags"
                    }
                }
                always {
                    sh "${bob} remove-image-with-all-tags"
                }
            }
        }
        stage('Publish Helm Chart') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'lciadm100', variable: 'HELM_REPO_TOKEN')]) {
                        def bobWithHelmToken = new BobCommand()
                                .bobImage(defaultBobImage)
                                .needDockerSocket(true)
                                .envVars(['HELM_REPO_TOKEN': env.HELM_REPO_TOKEN])
                                .toString()
                        sh "${bobWithHelmToken} push-helm"
                    }
                }
            }
        }
        stage('Tag Cloud-Native SG Git Repository') {
            steps {
                wrap([$class: 'BuildUser']) {
                    script {
                        def bobWithCommitterInfo = new BobCommand()
                                .bobImage(defaultBobImage)
                                .needDockerSocket(true)
                                .envVars([
                                        'AUTHOR_NAME'        : "\${BUILD_USER:-${GIT_COMMITTER_NAME}}",
                                        'AUTHOR_EMAIL'       : "\${BUILD_USER_EMAIL:-${GIT_COMMITTER_EMAIL}}",
                                        'GIT_COMMITTER_NAME' : "${GIT_COMMITTER_NAME}",
                                        'GIT_COMMITTER_EMAIL': "${GIT_COMMITTER_EMAIL}"
                                ])
                                .toString()
                        sh "${bobWithCommitterInfo} create-git-tag"
                        sh """
                            tag_id=\$(cat .bob/var.version)
                            git push origin \${tag_id}
                        """
                    }
                }
            }
            post {
                failure {
                    script {
                        failedStage = env.STAGE_NAME
                    }
                }
                always {
                    script {
                        sh "${bob} remove-git-tag"
                    }
                }
            }
        }
        stage('Generate Metadata Parameters') {
            steps {
                sh "${bob} generate-metadata-parameters"
                archiveArtifacts 'image-metadata-artifact.json'
            }
        }
    }
    post {
        success {
            script {
                sh '''
                    set +x
                    git tag --annotate --message "Tagging latest in sprint" --force $SPRINT_TAG HEAD
                    git push --force origin $SPRINT_TAG
                    git tag --annotate --message "Tagging latest in sprint with ISO version" --force ${SPRINT_TAG}_iso_${ISO_VERSION} HEAD
                    git push --force origin ${SPRINT_TAG}_iso_${ISO_VERSION}
                    git tag --annotate --message "Tagging latest in sprint with Product Set version" --force ps_${PRODUCT_SET} HEAD
                    git push --force origin ps_${PRODUCT_SET}
                '''
            }
        }
        failure {
            script {
                emailReport()
            }
        }
    }
}
// More about @Builder: http://mrhaki.blogspot.com/2014/05/groovy-goodness-use-builder-ast.html
import groovy.transform.builder.Builder
import groovy.transform.builder.SimpleStrategy

@Builder(builderStrategy = SimpleStrategy, prefix = '')
class BobCommand {
    def bobImage = 'bob.2.0:latest'
    def envVars = [:]
    def needDockerSocket = false

    String toString() {
        def env = envVars
                .collect({ entry -> "-e ${entry.key}=\"${entry.value}\"" })
                .join(' ')

        def cmd = """\
            |docker run
            |--init
            |--rm
            |--workdir \${PWD}
            |--user \$(id -u):\$(id -g)
            |-v \${PWD}:\${PWD}
            |-v /home/enmadm100/doc_push/group:/etc/group:ro
            |-v /home/enmadm100/doc_push/passwd:/etc/passwd:ro
            |-v \${HOME}/.m2:\${HOME}/.m2
            |-v \${HOME}/.docker:\${HOME}/.docker
            |${needDockerSocket ? '-v /var/run/docker.sock:/var/run/docker.sock' : ''}
            |${env}
            |\$(for group in \$(id -G); do printf ' --group-add %s' "\$group"; done)
            |--group-add \$(stat -c '%g' /var/run/docker.sock)
            |${bobImage}
            |"""
        return cmd
                .stripMargin()           // remove indentation
                .replace('\n', ' ')      // join lines
                .replaceAll(/[ ]+/, ' ') // replace multiple spaces by one
    }
}
