//testsave
pipeline{
    agent any
   //
    options {
        timestamps()
        // gitConnection('my-repo')  
    }
    
    stages{
        stage("CHEKOUT"){
            steps{
                echo "===============================================Executing CHEKOUT==============================================="
                deleteDir()
                checkout scm
              
            }
        }
    
        stage("Building for all"){
            steps{
                echo "===============================================Executing Building for all==============================================="
                sh "ls"
                sh"docker-compose down"
                sh "docker-compose build --no-cache "
                sh "docker-compose up -d"
                sh "docker images"
            }
        }
        stage("test build"){
            steps{
                script{
                    sh "curl http://43.0.20.203:5001"
                }
                
            }
        }
    
        stage("e2e test"){
            when{
                anyOf {
                        branch "main"
                        branch "feature/*"
                        branch "master"
                }
            }
            steps{
                echo "===============================================Executing e2e test==============================================="
                script{
                    dir("app/test"){
                        sh "./testing.sh 43.0.20.203 5001"
                    }

                }
            }
        }
        
        // stage("calc tag"){
        //     when{
        //         anyOf {
        //                 branch "main"
        //                 branch "master"
        //         }
        //     }
        //     steps{
        //         echo "===============================================Executing calc tag==============================================="
        //         script{
        //             Ver_Br=sh (script: "git describe --tags | cut -d '-' -f1 | cut -d '.' -f1-2",
        //             returnStdout: true).trim()
        //             echo "${Ver_Br}"
        //             Ver_Calc=sh (script: "bash calc.sh ${Ver_Br}",
        //             returnStdout: true).trim()
        //             echo "${Ver_Calc}"
                        // withCredentials([gitUsernamePassword(credentialsId: '2053d2c3-e0ab-4686-b031-9a1970106e8d', gitToolName: 'Default')]){
                        //     sh "git checkout release/${VER}"
                            
                        //     sh "git tag $NEXT_VER"
                        //     sh "git push  origin $NEXT_VER"

        //         }     
                
        //     }
           
        // }
        stage("Publish"){
            when{
                anyOf {
                        branch "main"
                        branch "master"
                }
            }
            
            steps{
                echo "===============================================Executing Publish==============================================="
                
                script{
                withCredentials([[
                                credentialsId: 'aws_shoval',
                                accessKeyVaeiable: 'AWS_ACCESS_KET_ID',
                                secretKeyVariable: 'AWS_SECRET_KEY_ID'
                                ]]) {
                                sh "docker tag shoval_private_ecr shoval_private_ecr"
                            docker.withRegistry("http://644435390668.dkr.ecr.eu-west-3.amazonaws.com/shoval_private_ecr", "ecr:eu-west-3:644435390668") {
                            docker.image("shoval_private_ecr").push()
                            }
                }
                }


            }
           
        }
    }
    post{

        success{
            script{
                
                script{
                    echo "yes"

                    // emailext    recipientProviders: [culprits()],
                    // subject: 'yes', body: 'ooooononononn',  
                    // attachLog: true


                    // emailext to: 'shoval123055@gmail.com',
                    // subject: 'Congratulations!', body: 'Well, this time you didnt mess up',  
                    // attachLog: true
                }     
            
            
                // gitlabCommitStatus(connection: gitLabConnection(gitLabConnection: 'my-repo' , jobCredentialId: ''),name: 'report'){
                //     echo "that good"
                // }
            }
        }
        failure{
            script{

                echo "no"
                // emailext   recipientProviders: [culprits()],
                // subject: 'YOU ARE BETTER THEN THAT !!! ', body: 'Dear programmer, you have broken the code, you are asked to immediately sit on the chair and leave the coffee corner.',  
                // attachLog: true


                // emailext   to: 'shoval123055@gmail.com',
                // subject: 'YOU ARE BETTER THEN THAT !!! ', body: 'Dear programmer, you have broken the code, you are asked to immediately sit on the chair and leave the coffee corner.',  
                // attachLog: true
            }      
           
            // gitlabCommitStatus(connection: gitLabConnection(gitLabConnection: 'my-repo' , jobCredentialId: ''),name: 'report'){
            //     echo "ahh"
            // }

        }
    }
}