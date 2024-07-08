pipeline {
    agent any 
    environment {
      // gitlab
        version = "${params.Version}"
        publisher = "${params.Publisher}"
        sendTo = "${params.SendTo}"
        prev_version = "default"
    }

    stages {
        stage ('Build and Upload Docker Image') {
            steps {
                script {
                    sh "docker login hyperregistry.tmaxcloud.org -u admin -p admin"
                    //sh "docker tag celesta30/super-app-tomcat:${version} hyperregistry.tmaxcloud.org/oss-sas-tomcat/super-app-server:${version}"
                    //sh "docker push hyperregistry.tmaxcloud.org/oss-sas-tomcat/super-app-server:${version}"
                    //sh "docker tag celesta30/super-app-wildfly:${version} hyperregistry.tmaxcloud.org/oss-sas-wildfly/super-app-server:${version}"
                    //sh "docker push hyperregistry.tmaxcloud.org/oss-sas-wildfly/super-app-server:${version}"
                }
                withCredentials([usernamePassword(credentialsId: 'aws', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    script {
                        sh "aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws"
                        //sh "docker tag hyperregistry.tmaxcloud.org/oss-sas-tomcat/super-app-server:${version} public.ecr.aws/l0p3k1b5/sas-tomcat:${version}"
                        //sh "docker push public.ecr.aws/l0p3k1b5/sas-tomcat:${version}"
                        //sh "docker tag hyperregistry.tmaxcloud.org/oss-sas-wildfly/super-app-server:${version} public.ecr.aws/l0p3k1b5/sas-wildfly:${version}"
                        //sh "docker push public.ecr.aws/l0p3k1b5/sas-wildfly:${version}"
                    }
               }    
           }
        }
        stage('Send Email') {
            steps {
                script {
                    def publisherEmails = [
                        '이채진': 'chaejin_lee@tmax.co.kr'
                    ]
                    def sendToEmails = [
                        'CL1 RnD, QA': 'clrnd1_div@tmax.co.kr; clpm1_qa_unit@tmax.co.kr;',
                        'CL1-1, QA': 'clrnd1_1_unit@tmax.co.kr; clpm1_qa_unit@tmax.co.kr;',
                        'CL1-1-1, QA': 'clrnd1_1_1@tmax.co.kr; clpm1_qa_unit@tmax.co.kr;',
                        'CL1-1-1': 'clrnd1_1_1@tmax.co.kr;',
                        'Publisher only': 'chaejin_lee@tmax.co.kr;'
                    ]
                    def fromEmail = publisherEmails[publisher]
                    def toEmail = sendToEmails[sendTo]
                        emailext (
                            subject: "[oss-super-app-server] Release Notes - oss-super-app-server:${version}",
                            body:
                                 """
  안녕하세요. CL1-1-1팀 ${publisher}입니다.
 
 금주 배포된 oss-super-app-server-tomcat:${version}, oss-super-app-server-wildfly:${version} release 버전에 대한 안내 및 가이드 메일 드립니다.

 OSS-SAS-${version}의 개선 및 추가된 사항은 아래 OSS-Super-App-Server Release Note 링크를 참고 부탁드립니다.
 http://192.168.1.150:10081/superobject/super-object/-/wikis/OSS-SAS

 ===

 구체적인 설치 및 테스트 가이드에 대한 내용은 해당 WIKI 가이드 참고 부탁드립니다.
 
 http://192.168.1.150:10081/ck1-1/oss-super-app-service/wikis/Tomcat-SAS-가이드
 http://192.168.1.150:10081/ck1-1/oss-super-app-service/wikis/Wildfly-SAS-가이드

 감사합니다.

 - ${publisher} 드림.

 ※ OSS SuperApp Server Container Image :
 
 hyperregistry.tmaxcloud.org/oss-sas-tomcat/super-app-server:${version}
 hyperregistry.tmaxcloud.org/oss-sas-wildfly/super-app-server:${version}

  ※ gitlab.ck:10081 접속 방법 :
 Default DNS 192.168.1.150 로 설정

 """,
                            to: toEmail,
                            from: fromEmail
                        )
                    }
                }
            }
        } 
    }
