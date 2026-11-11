# Pipeline
Déclenchement
- [ ] Plan sur PR
	- [ ] contrôle sécu avec checkov
	- [ ] 
- [ ] Apply après merge
- [ ] Apply dev après commit sur la branche dev ?



`runs-on` -> définit la machine sur laquelle va être exécuté la pipeline. [Reference doc]([https://docs.github.com/fr/actions/reference/workflows-and-actions/workflow-syntax#choosing-github-hosted-runners](https://docs.github.com/fr/actions/reference/workflows-and-actions/workflow-syntax#jobsjob_idruns-on))

`uses` -> appel de script créer par Github ou par la communauté. Utilisation du hash de commit (SHA) à la place du tag pour éviter attaque de supply chain. [Reference doc](https://github.com/github/awesome-copilot/blob/main/instructions/github-actions-ci-cd-best-practices.instructions.md#3-steps-and-actions)

Checkov check rules to skip :
- CKV_AWS_337: "Ensure SSM parameters are using KMS CMK"
- CKV_AWS_382: "Ensure no security groups allow egress from 0.0.0.0:0 to port -1"
- CKV_AWS_260: "Ensure no security groups allow ingress from 0.0.0.0:0 to port 80"
- CKV_AWS_131: "Ensure that ALB drops HTTP headers"
- CKV_AWS_150: "Ensure that Load Balancer has deletion protection enabled"
- CKV_AWS_2: "Ensure ALB protocol is HTTPS"
- CKV_AWS_23: "Ensure every security group and rule has a description"
- CKV_AWS_126: "Ensure that detailed monitoring is enabled for EC2 instances"
- CKV_AWS_8: "Ensure all data stored in the Launch configuration or instance Elastic Blocks Store is securely encrypted"


Checkov check rules to skpi v2:
- CKV_TF_1: "Ensure Terraform module sources use a commit hash"
- CKV_AWS_135: "Ensure that EC2 is EBS optimized"
- CKV2_AWS_20: "Ensure that ALB redirects HTTP requests into HTTPS ones"
- CKV_AWS_103: "Ensure that load balancer is using at least TLS 1.2"
- CKV_AWS_378: "Ensure AWS Load Balancer doesn't use HTTP protocol"
- CKV2_AWS_28: "Ensure public facing ALB are protected by WAF"
- CKV2_AWS_41: "Ensure an IAM role is attached to EC2 instance"