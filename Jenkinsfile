pipeline{
    agent{
        kubernetes{
            cloud "kubernetes"
            label "viktor"
        yaml """
        
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: curl
    image: vfarcic/curl
    command: ["cat"]
    tty: true
        
        """
        }
    }
environment { 

        CREDS = credentials('12') 

    }
    stages{
        stage("Deploy")
        {
            steps{
                container("curl"){
                    script{
			def props = readProperties interpolate: true, file: 'systemProps.txt'
			env.vision=props.vision
			env.NexusURL=props.NexusURL
			echo "${props.vision}"

			// change echo curl to sh curl
			echo "curl ${env.NexusURL}/vision/${env.vision}/vision-${env.vision}.dar -o visionDar"

                    }
                }
            
            }
            
        }
         stage("unit-testing")
        {
            steps{
                container("gradle"){
                    echo "${env.vision}"
                }
            
            }
        }
    }
}
