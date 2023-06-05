# SampleFastApiAppTools




### Create S3 State Bucket

```shell
aws s3api create-bucket --bucket "BUCKET_NAME"  --region us-east-1
```


### input your bucket name into the backend.tf file from backend.tf.example
```terraform
    bucket         = "BUCKET_NAME"
    key            = "terraform.tfstate"
    region         = "us-east-1"
```

### Initialize backend 
```shell
terraform init 
```

### input your aws credentials and choose instance name into terraform.tfvars from terraform.tfvars.example
```terraform
access_key = "XXXXX"
secret_key = "XXXX"
instance_name = "INSTANCE_NAME"
```
### Execute plan and verify everything looks good
```shell
terraform plan  
```



### Deploy cloud resources
```shell
terraform apply  
```


### Grab instance name and private key from the output
```shell
Outputs:

instance_dns_name = "ec2-XX-XX-XX-XX.compute-1.amazonaws.com"

```

### input ansible host and private key into inventory.ini file from inventory.ini.example
```shell
[webservers]
web1 ansible_host=ec2-XX-XX-XX-XX.compute-1.amazonaws.com ansible_user=ubuntu

[all:vars]
ansible_ssh_private_key_file=./INSTANCE_NAME.pem

```


### Execute ansible setup playbook
```shell
ansible-playbook -vv -i inventory.ini instance-setup.yaml

```


### Verify instance is deployed on exposed port 8000
```
http://ec2-XX-XX-XX-XX.compute-1.amazonaws.com:8000

```

### If there are any issues, you may ssh to the instance
```shell
  chmod 400 fibonacci-demo.pem
  ssh -i "INSTANCE_NAME.pem" ubuntu@ec2-XX-XX-XX-XX.compute-1.amazonaws.com

```

### Once logged on to the server you can verify the status of the service
```shell
sudo su 
systemctl status fibonacci

```
