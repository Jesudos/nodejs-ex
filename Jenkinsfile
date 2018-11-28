pipeline{
  agent{
      label 'nodejs'
  }
            stages{
            stage('preamble'){
              steps{
                script{
                openshift.withCluster() {
                    openshift.withProject() {
                        echo "Using project: ${openshift.project()}"
                    }
                }
                    }
                }
            }
            stage('cleanup'){
              steps{
                script{
                 openshift.withCluster() {
                    openshift.withProject() {
                       openshift.selector("all", [ app : 'nodejs-ex' ]).delete() 
                  if (openshift.selector("secrets", 'nodejs-ex').exists()) { 
                    openshift.selector("secrets", 'nodejs-ex').delete()
                  }
                    }
                 }
                }
                }
            }
            stage('Create App'){
              steps{
                script{
                openshift.withCluster() {
                    openshift.withProject() {
                        // Run `oc new-app https://github.com/openshift/ruby-hello-world` . It
    // returns a Selector which will select the objects it created for you.
    def created = openshift.newApp( 'https://github.com/Jesudos/nodejs-ex.git' )
    
    // This Selector exposes the same operations you have already seen.
    // (And many more that you haven't!).
    // created.describe()

    // We can create a Selector from the larger set which only selects
    // the build config which new-app just created.
    
    def bc = created.narrow('bc')
    openshift.startBuild('bc')
    // Let's output the build logs to the Jenkins console. bc.logs()
    // would run `oc logs bc/ruby-hello-world`, but that might only
    // output a partial log if the build is in progress. Instead, we will
    // pass '-f' to `oc logs` to follow the build until it terminates.
    // Arguments to logs get passed directly on to the oc command line.
                      //doesn't work for pipeline strategy
    //def result = bc.logs('-f')
     
  

    // You can even see exactly what oc command was executed.
    //echo "Logs executed: ${result.actions[0].cmd}"

    // And even obtain the standard output and standard error of the command.
    //def logsString = result.actions[0].out
    //def logsErr = result.actions[0].err
    
    echo "Deployment configuration"
    
    def dc = created.narrow('dc')
    // dcs is a Selector which selects the deployment config created by new-app. How do
    // we get more information about this DC? Turn it into a Groovy object using object().
    // If there was a chance here that more than one DC was created, we should use objects()
    // which would return a List of Groovy objects; however, in this example, there
    // should only be one.
  

    // dc is not a Selector -- It is a Groovy Map which models the content of the DC
    // new-app created at the time object() was called. Changes to the model are not
    // reflected back to the API server, but the DC's content is at our fingertips.
   // echo "new-app created a ${dc.kind} with name ${dc.metadata.name}"
  //  echo "The object has labels: ${dc.metadata.labels}"
    
    echo "Expose service"
    
    created.narrow('svc').expose()
                    }
                }
                    }
                }
            }
    
    
        }
    
        
}
