pipeline {
  agent any
  environment {
    HOME = "/root/"
  }
  stages {
    stage('') {
      steps {
        script {
            def userInput = input(
            id: 'userInput', message: 'Enter Policy Cookbook Info:',
            parameters: [
                string(defaultValue: 'None',
                        description: 'Policy name (Policy cookbook will inherit this name)',
                        name: 'POLICYNAME'),
                string(defaultValue: 'None',
                        description: 'Supported systems (https://docs.chef.io/config_rb_metadata.html under "supports" section)',
                        name: 'SUPPORTS'),
                string(defaultValue: 'None',
                        description: 'Maintainer name',
                        name: 'MAINTAINER'),
                string(defaultValue: 'None',
                        description: 'Maintainer email',
                        name: 'MAINTAINEREMAIL'),
                string(defaultValue: 'None',
                        description: 'License type (https://docs.chef.io/config_rb_metadata.html under "license" section)',
                        name: 'LICENSE'),
                string(defaultValue: 'None',
                        description: 'Short description of the Policy cookbook',
                        name: 'SHORTDESCRIPTION'),
                string(defaultValue: 'None',
                        description: 'Longer description of the Policy cookbook',
                        name: 'LONGDESCRIPTION'),
                string(defaultValue: '0.0.1',
                        description: 'Initial cookbook version (default is 0.0.1)',
                        name: 'COOKBOOKVERSION'),
                string(defaultValue: 'None',
                        description: 'Chef version supported (https://docs.chef.io/config_rb_metadata.html under "chef_version" section)',
                        name: 'CHEFVERSION'),
            ])
            inputPOLICYNAME         = userInput.POLICYNAME?:''
            inputSUPPORTS           = userInput.SUPPORTS?:''
            inputMAINTAINER         = userInput.MAINTAINER?:''
            inputMAINTAINEREMAIL    = userInput.MAINTAINEREMAIL?:''
            inputLICENSE            = userInput.LICENSE?:''
            inputSHORTDESCRIPTION   = userInput.SHORTDESCRIPTION?:''
            inputLONGDESCRIPTION    = userInput.LONGDESCRIPTION?:''
            inputCOOKBOOKVERSION    = userInput.COOKBOOKVERSION?:''
            inputCHEFVERSION        = userInput.CHEFVERSION?:''

            echo inputPOLICYNAME
            echo inputSUPPORTS
            echo inputMAINTAINER
            echo inputMAINTAINEREMAIL
            echo inputLICENSE
            echo inputSHORTDESCRIPTION
            echo inputLONGDESCRIPTION
            echo inputCOOKBOOKVERSION
            echo inputCHEFVERSION

            dir ("${inputPOLICYNAME}") {
                git branch: 'master',
                    credentialsId: 'jenkins-bot',
                    url: "https://github.com/danielcbright/policyfile-template-PFP.git"
                withCredentials([usernamePassword(credentialsId: 'jenkins-bot', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                    env.GITHUB_TOKEN = "$PASSWORD"
                    env.USERNAME = "$USERNAME"
                }

            issuesURL = "https://github.com/danielcbright/${inputPOLICYNAME}/issues"
            sourceURL = "https://github.com/danielcbright/${inputPOLICYNAME}"

            sh  """
                rm -rf .git
                mv _Jenkinsfile Jenkinsfile
                find . -type f -print0 | xargs -0 sed -i 's/POLICYNAME/${inputPOLICYNAME}/g'\n
                find . -type f -print0 | xargs -0 sed -i 's/SUPPORTS/${inputSUPPORTS}/g'\n
                find . -type f -print0 | xargs -0 sed -i 's/MAINTAINER/${inputMAINTAINER}/g'\n
                find . -type f -print0 | xargs -0 sed -i 's/EMAIL/${inputMAINTAINEREMAIL}/g'\n
                find . -type f -print0 | xargs -0 sed -i 's/LICENSE/${inputLICENSE}/g'\n
                find . -type f -print0 | xargs -0 sed -i 's/SHORTDESCRIPTION/${inputSHORTDESCRIPTION}/g'\n
                find . -type f -print0 | xargs -0 sed -i 's/LONGDESCRIPTION/${inputLONGDESCRIPTION}/g'\n
                find . -type f -print0 | xargs -0 sed -i 's/COOKBOOKVERSION/${inputCOOKBOOKVERSION}/g'\n
                find . -type f -print0 | xargs -0 sed -i 's/CHEFVERSION/${inputCHEFVERSION}/g'\n
            """
            sh "find . -type f -print0 | xargs -0 sed -i \"s|ISSUESURL|${issuesURL}|g\"\n"
            sh "find . -type f -print0 | xargs -0 sed -i \"s|SOURCEURL|${sourceURL}|g\"\n"
            sh """
                pwd
                git config --global hub.protocol https
                git init
                git add .
                git commit -m "Initial commit of ${inputPOLICYNAME} by Jenkins"
                /usr/local/bin/hub create danielcbright/${inputPOLICYNAME}
                """
            sh "git remote set-url origin https://$USERNAME:$GITHUB_TOKEN@github.com/danielcbright/${inputPOLICYNAME}.git"
            sh "git push --set-upstream origin master"
          }
        }
      }
    }
  }
}
