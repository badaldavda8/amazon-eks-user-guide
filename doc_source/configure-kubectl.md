# Configure kubectl for Amazon EKS<a name="configure-kubectl"></a>

Amazon EKS uses IAM to provide authentication to your Kubernetes cluster through the [AWS IAM Authenticator for Kubernetes](https://github.com/kubernetes-sigs/aws-iam-authenticator)\. Beginning with Kubernetes version 1\.10, you can configure the stock kubectl client to work with Amazon EKS by installing the AWS IAM Authenticator for Kubernetes and modifying your kubectl configuration file to use it for authentication\.

**To install kubectl for Amazon EKS**
+ You have multiple options to download and install kubectl for your operating system\.
  + The `kubectl` binary is available in many operating system package managers, and this option is often much easier than a manual download and install process\. You can follow the instructions for your specific operating system or package manager in the [Kubernetes documentation](https://kubernetes.io/docs/tasks/tools/install-kubectl/) to install\.
  + Amazon EKS also vends kubectl binaries that you can use that are identical to the upstream kubectl binaries with the same version\. To install the Amazon EKS\-vended binary for your operating system, see [Installing `kubectl`](install-kubectl.md)\.

**To install `aws-iam-authenticator` for Amazon EKS**
+ Download and install the `aws-iam-authenticator` binary\.

  Amazon EKS vends `aws-iam-authenticator` binaries that you can use, or you can use go get to fetch the binary from the [AWS IAM Authenticator for Kubernetes](https://github.com/kubernetes-sigs/aws-iam-authenticator) project on GitHub for other operating systems\.
  + To download and install the Amazon EKS\-vended `aws-iam-authenticator` binary:

    1. Download the Amazon EKS\-vended `aws-iam-authenticator` binary from Amazon S3:
       + **Linux**: [https://amazon\-eks\.s3\-us\-west\-2\.amazonaws\.com/1\.11\.5/2018\-12\-06/bin/linux/amd64/aws\-iam\-authenticator](https://amazon-eks.s3-us-west-2.amazonaws.com/1.11.5/2018-12-06/bin/linux/amd64/aws-iam-authenticator)
       + **MacOS**: [https://amazon\-eks\.s3\-us\-west\-2\.amazonaws\.com/1\.11\.5/2018\-12\-06/bin/darwin/amd64/aws\-iam\-authenticator](https://amazon-eks.s3-us-west-2.amazonaws.com/1.11.5/2018-12-06/bin/darwin/amd64/aws-iam-authenticator)
       + **Windows**: [https://amazon\-eks\.s3\-us\-west\-2\.amazonaws\.com/1\.11\.5/2018\-12\-06/bin/windows/amd64/aws\-iam\-authenticator\.exe](https://amazon-eks.s3-us-west-2.amazonaws.com/1.11.5/2018-12-06/bin/windows/amd64/aws-iam-authenticator.exe)

       Use the command below to download the binary, substituting the correct URL for your platform\. The example below is for macOS clients\.

       ```
       curl -o aws-iam-authenticator https://amazon-eks.s3-us-west-2.amazonaws.com/1.11.5/2018-12-06/bin/darwin/amd64/aws-iam-authenticator
       ```

    1. \(Optional\) Verify the downloaded binary with the SHA\-256 sum provided in the same bucket prefix, substituting the correct URL for your platform\. 

       1. Download the SHA\-256 sum for your system\. The example below is to download the SHA\-256 sum for macOS clients\.

          ```
          curl -o aws-iam-authenticator.sha256 https://amazon-eks.s3-us-west-2.amazonaws.com/1.11.5/2018-12-06/bin/darwin/amd64/aws-iam-authenticator.sha256
          ```

       1. Check the SHA\-256 sum for your downloaded binary\. The example `openssl` command below was tested for macOS and Ubuntu clients\. Your operating system may use a different command or syntax to check SHA\-256 sums\. Consult your operating system documentation if necessary\.

          ```
          openssl sha -sha256 aws-iam-authenticator
          ```

       1. Compare the generated SHA\-256 sum in the command output against your downloaded `aws-iam-authenticator.sha256` file\. The two should match\.

    1. Apply execute permissions to the binary\.

       ```
       chmod +x ./aws-iam-authenticator
       ```

    1. Copy the binary to a folder in your `$PATH`\. We recommend creating a `$HOME/bin/aws-iam-authenticator` and ensuring that `$HOME/bin` comes first in your `$PATH`\.

       ```
       cp ./aws-iam-authenticator $HOME/bin/aws-iam-authenticator && export PATH=$HOME/bin:$PATH
       ```

    1. Add `$HOME/bin` to your `PATH` environment variable\.
       + For Bash shells on macOS:

         ```
         echo 'export PATH=$HOME/bin:$PATH' >> ~/.bash_profile
         ```
       + For Bash shells on Linux:

         ```
         echo 'export PATH=$HOME/bin:$PATH' >> ~/.bashrc
         ```

    1. Test that the `aws-iam-authenticator` binary works\.

       ```
       aws-iam-authenticator help
       ```
  + Or, to install the `aws-iam-authenticator` binary from GitHub using go get:

    1. Install the Go programming language for your operating system if you do not already have go installed\. For more information, see [Install the Go tools](https://golang.org/doc/install#install) in the Go documentation\.

    1. Use go get to install the `aws-iam-authenticator` binary\.

       ```
       go get -u -v github.com/kubernetes-sigs/aws-iam-authenticator/cmd/aws-iam-authenticator
       ```
**Note**  
If you receive the following error, you must upgrade your Go language to 1\.7 or greater\. For more information, see [Install the Go tools](https://golang.org/doc/install#install) in the Go documentation\.  

       ```
       package context: unrecognized import path "context" (import path does not begin with hostname)
       ```

    1. Add `$HOME/go/bin` to your `PATH` environment variable\.
       + For Bash shells on macOS:

         ```
         export PATH=$HOME/go/bin:$PATH && echo 'export PATH=$HOME/go/bin:$PATH' >> ~/.bash_profile
         ```
       + For Bash shells on Linux:

         ```
         export PATH=$HOME/go/bin:$PATH && echo 'export PATH=$HOME/go/bin:$PATH' >> ~/.bashrc
         ```

    1. Test that the `aws-iam-authenticator` binary works\.

       ```
       aws-iam-authenticator help
       ```