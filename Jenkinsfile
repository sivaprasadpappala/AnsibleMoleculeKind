pipeline {
  agent any

  environment {
    REPO_URL   = "https://github.com/sivaprasadpappala/AnsibleMoleculeKind.git"
    BASE_BRANCH = "main"
    FEATURE_BRANCH = "ci-${BUILD_NUMBER}"
    GH_TOKEN = credentials('github-token')
  }

  stages {

    stage('Checkout') {
      steps {
        git branch: "${BASE_BRANCH}", url: "${REPO_URL}"
      }
    }

    stage('Create Feature Branch') {
      steps {
        sh """
          git checkout -b ${FEATURE_BRANCH}
        """
      }
    }

    stage('Molecule Test with kind') {
      steps {
        sh '''
          unset DOCKER_HOST
          unset DOCKER_TLS_VERIFY
          unset DOCKER_CERT_PATH

          export DOCKER_HOST=unix:///var/run/docker.sock

          python3 -m venv .venv
          . .venv/bin/activate

          pip install --upgrade pip
          pip install \
            ansible-core==2.15.13 \
            ansible==8.7.0 \
            molecule==6.0.2 \
            molecule-docker==2.1.0 \
            docker

          docker version
          docker ps

          cd ansible/roles/demo
          molecule test
        '''
      }
    }

    stage('Commit Changes') {
      steps {
        sh """
          git status
          git add .
          git commit -m "Validated with Molecule (kind)" || true
          git push origin ${FEATURE_BRANCH}
        """
      }
    }

    stage('Create Pull Request') {
      steps {
        sh """
          curl -s -X POST \
            -H "Authorization: token ${GH_TOKEN}" \
            -H "Accept: application/vnd.github+json" \
            https://api.github.com/repos/ORG/REPO/pulls \
            -d '{
              "title": "Auto PR from Jenkins CI",
              "head": "${FEATURE_BRANCH}",
              "base": "${BASE_BRANCH}",
              "body": "PR created automatically after Molecule validation using kind"
            }'
        """
      }
    }
  }
}