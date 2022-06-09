<div id="top"></div>

<br />
<div align="center">
<h3 align="center">Xonotic Agones Deployment</h3>
</div>



<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
  </ol>
</details>




<!-- GETTING STARTED -->
## Getting Started

These are the steps to deploy xonotic to AWS EKS with Agones.

### Prerequisites

These prerequisites need installing before deploying
*  [AWS account](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account/)
*  [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
*  [eksctl](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html)
*  [aws-vault](https://github.com/99designs/aws-vault)
*  [kubectl](https://kubernetes.io/docs/tasks/tools/)
*  [Xonotic](https://xonotic.org/download/)

### Deploy

1. Create AWS EKS cluster. User the aws-vault profile name that you set up earlier. This example uses "Sandbox"
	```sh
	aws-vault exec Sandbox -- eksctl create cluster --name xono-cluster --region eu-west-1 --version 1.22 --nodegroup-name standard-workers --node-type t3.large --nodes 1 --nodes-min 1 --nodes-max 2
	```
2. If you have set up a cluster before you may need to switch kubeconfig to the new one
   ```sh
   aws-vault exec Sandbox -- aws eks update-kubeconfig --region eu-west-1 --name xono-cluster
   ```
3. Create namespace on cluster
   ```sh
   aws-vault exec Sandbox -- kubectl create namespace agones-system
   ```
4. Install agones on cluster
   ```sh
   aws-vault exec Sandbox -- kubectl apply -f https://raw.githubusercontent.com/googleforgames/agones/release-1.23.0/install/yaml/install.yaml
   ```
5. Download https://github.com/googleforgames/agones/tree/release-1.18.0/examples/xonotic and cd to the directory
   ```sh
   cd /documents/agones/tree/release-1.18.0/examples/xonotic
   ```
6. Deploy container
   ```sh
  aws-vault exec Sandbox -- kubectl create -f .\gameserver.yaml
   ```
7. Check container has deployed and what the port is
   ```sh
  aws-vault exec Sandbox -- kubectl get gameservers
  NAME      STATE   ADDRESS                                             PORT   NODE                                           AGE
xonotic   Ready   ec2-54-216-76-253.eu-west-1.compute.amazonaws.com   7412   ip-192-168-12-185.eu-west-1.compute.internal   5s
   ```
8. Load xonotic game, go to multiplaye, add the server address and port and press join. The address in the example would be "ec2-54-216-76-253.eu-west-1.compute.amazonaws.com:7412"

<p align="right">(<a href="#top">back to top</a>)</p>






