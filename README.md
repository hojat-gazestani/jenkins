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
    type: Freestyle project
        Restrict where this project can be run
            Label Expression: kubeagent

        Build Steps
            Execute shell: 
                command:
                    echo "hollo form pod"

```