# jenkins

### Jenkins configuration

```text
plugin kubernetes
Manage Jenkins > Nodes and Cloud > Cloud > add new cloud > kubernetes
    kubernetes cloud details
        Kubernetes Namespace : devops-tools
            test connection

        Jenkins URL : http://jenkins-service.devops-tools.svc.cluster.local:8080

        pod template
            Name: jnlp
            Namespace: devops-tools
            Labels: kubeagent

            Containers
                Name: jnlp
                Docker image: jenkins/inbound-agent:latest

newJob
    name: agenttest
    type: Pipeline

    Pipeline

        pipeline {
        agent {
            label "kubeagent"
        }
        stages {
            stage('Git clone') {
                steps {
                    container('jnlp'){
                        git branch: 'main', url: "https://github.com/hojat-gazestani/myapp.git"
                    }   
                }
            }
        }
    }