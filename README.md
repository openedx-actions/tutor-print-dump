<img src="https://avatars.githubusercontent.com/u/40179672" width="75">

[![hack.d Lawrence McDaniel](https://img.shields.io/badge/hack.d-Lawrence%20McDaniel-orange.svg)](https://lawrencemcdaniel.com)
[![discuss.overhang.io](https://img.shields.io/static/v1?logo=discourse&label=Forums&style=flat-square&color=ff0080&message=discuss.overhang.io)](https://discuss.overhang.io)
[![docs.tutor.overhang.io](https://img.shields.io/static/v1?logo=readthedocs&label=Documentation&style=flat-square&color=blue&message=docs.tutor.overhang.io)](https://docs.tutor.overhang.io)<br/>
[![AWS](https://img.shields.io/badge/AWS-%23FF9900.svg?style=for-the-badge&logo=amazon-aws&logoColor=white)](https://aws.amazon.com/)
[![Docker](https://img.shields.io/badge/docker-%230db7ed.svg?style=for-the-badge&logo=docker&logoColor=white)](https://www.docker.com/)
[![Kubernetes](https://img.shields.io/badge/kubernetes-%23326ce5.svg?style=for-the-badge&logo=kubernetes&logoColor=white)](https://kubernetes.io/)

# tutor-print-dump

Github Action to print a summary report (a "dump") of the tutor configuration contents to the console.

This action is designed to work seamlessly with Kubernetes secrets created by the Terraform modules contained in [Cookiecutter Tutor Open edX Production Devops Tools](https://github.com/lpm0073/cookiecutter-openedx-devops).

**IMPORTANT SECURITY DISCLAIMER**: Sensitive data contained in the Kubernetes secrets is masked in Github Actions logs and console output provided however, that all of these secrets were created with the Cookiecutter, and, that all of these secrets were extracted using openedx-actions/tutor-k8s-get-secret. If you created your Open edX installation using the Cookiecutter then this is your case and you have nothing more to worry about. If on the other hand you are working with Kubernetes secrets created outside of the Cookiecutter, or if you have created a custom workflow that does not use openedx-actions/tutor-k8s-get-secret then **be aware that you run a non-zero risk of sensitive data becoming exposed inside the Github Actions logs and/or console output**.


## Usage:


```yaml
name: Example workflow

on: workflow_dispatch

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      # required antecedent
      - uses: actions/checkout@v3.0.2

      # required antecedent
      - name: Configure AWS credentials
        uses: aws-actions/configure-aws-credentials@v1.6.1
        with:
          aws-access-key-id: ${{ secrets.THE_NAME_OF_YOUR_AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.THE_NAME_OF_YOUR_AWS_SECRET_ACCESS_KEY }}
          aws-region: us-east-2

      # install and configure tutor and kubectl
      - name: Initialize environment
        uses: openedx-actions/tutor-k8s-init@v1.0.0
        with:
          namespace: openedx-prod

      #
      # ... do some configuration work ...
      # 

      # This action.
      # - eks-namespace: optional. if set, persists configuration data to Kubernetes secrets
      - name: Print tutor dump report
        uses: openedx-actions/tutor-print-dump@v1.0.3
        with:
          namespace: openedx-prod
```
