# test-infra

Nephio project's test infrastructure configuration &amp; tools will be maintained in this repository.

**Note**: Issues should be opened in the [sig-release](https://github.com/nephio-project/sig-release)
repository.

### AWS CLI

Use this to get token for authenticantion

`aws sts get-session-token --duration-seconds 129600 --serial-number arn:aws:iam::935317843233:mfa/VtiPhone-14P --token-code 890658`

### Running Nephio on AWS and Installing OAI 5G

Find full guide on this page [here](./bootstrap_5g_guide.md)
