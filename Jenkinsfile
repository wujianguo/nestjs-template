pipeline {
  agent any
  stages {
    stage('检出') {
      steps {
        checkout([
          $class: 'GitSCM',
          branches: [[name: GIT_BUILD_REF]],
          userRemoteConfigs: [[
            url: GIT_REPO_URL,
            credentialsId: CREDENTIALS_ID
          ]]])
        }
      }

      stage("Install dependencies") {
          steps {
            sh 'rm -rf /usr/lib/node_modules/npm/'
            dir ('/root/.cache/downloads') {
              sh 'wget -nc "https://coding-public-generic.pkg.coding.net/public/downloads/node-linux-x64.tar.xz?version=v18.9.1" -O node-v18.9.1-linux-x64.tar.xz | true'
              sh 'tar -xf node-v18.9.1-linux-x64.tar.xz -C /usr --strip-components 1'
              }
            // sh 'npm install -g @nestjs/cli'
            // sh 'npm ci'
            // sh 'npm run build'
            // sh 'npm ci --omit=dev'
            // sh 'npm cache clean --force'
          }
      }

      stage('Deploy to serverless') {
        steps {
          // sh 'zip -r code.zip .'
          sh 'npm install @serverless-devs/s -g --registry=https://registry.npmmirror.com'
          sh 's config add --AccessKeyID ${AccessKeyId} --AccessKeySecret ${AccessKeySecret} --AccountID ${AccountID} --access default'
          sh 's deploy'
        }
      }
    }
  }
