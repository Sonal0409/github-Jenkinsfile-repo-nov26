// This is a pipeline scipt in Jenkinsfile format
// as part of this pipeline we will build java code 
// we will compile the code, review the code and test the code  and package thecode
// we will also generate pmd report and test report and publish the reports in jenkins
// jenkins is java based 
// code is written in groovy language
// syntax of pipleine code is declarative pipeline syntax
// this is groovy based DSL (Domain Specific Language) which is used to write the pipeline code
pipeline{

agent any  // here any represerts the current ec2 server where the pipeline will run
 
tools{
   maven 'mymaven' // here we are defining the maven tool 
   // which is required for the pipeline
   // tool name has to be same as defined in jenkins global tool configuration
}

stages{
// fetch code --> Review the code --> compile code --> test --> package code
stage('Checkout Code'){
    steps{
        git 'https://github.com/Sonal0409/DevOpsCodeDemo.git'    
    }
}
// code will be present in jenkins workspace
stage('Code Review'){
    steps{
        sh 'mvn pmd:pmd'
    } // this will generate pmd report in target/pmd.xml.
    post{
        success{
           echo 'Code review is successful'
           echo 'Generating pmd trend report in jenkins'
         recordIssues sourceCodeRetention: 'LAST_BUILD', tools: [pmdParser(pattern: '**/pmd.xml')]
        }
        failure{
            echo 'Code review is failed'
            echo 'Please check the pmd report in target/pmd.xml'
        }
    }
}
stage('Compile the code'){
    steps{
        sh 'mvn compile'
    }
}

stage('Test the code'){
    steps{
        sh 'mvn test'
    }
    post{
        success{
            junit '**/target/surefire-reports/*.xml'
        }
     
    }

} 

stage('Package the code'){
    steps{
        sh 'mvn package'
    }

}

}

}
