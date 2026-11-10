# Pipeline
Déclenchement
- [ ] Plan sur PR
	- [ ] contrôle sécu avec checkov
	- [ ] 
- [ ] Apply après merge
- [ ] Apply dev après commit sur la branche dev ?



`runs-on` -> définit la machine sur laquelle va être exécuté la pipeline. [Reference doc]([https://docs.github.com/fr/actions/reference/workflows-and-actions/workflow-syntax#choosing-github-hosted-runners](https://docs.github.com/fr/actions/reference/workflows-and-actions/workflow-syntax#jobsjob_idruns-on))

`uses` -> appel de script créer par Github ou par la communauté. Utilisation du hash de commit (SHA) à la place du tag pour éviter attaque de supply chain. [Reference doc](https://github.com/github/awesome-copilot/blob/main/instructions/github-actions-ci-cd-best-practices.instructions.md#3-steps-and-actions)

