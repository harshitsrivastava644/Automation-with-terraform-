# Automation-with-terraform-
EC2 automation with terraform

This guide provides a step-by-step approach to automating the provisioning and management of an **EC2 Instance** using **Terraform**. By following this guide, you will learn how to install Terraform, configure AWS credentials, write Terraform configuration files, and deploy an EC2 instance efficiently.

---

## **1. Install Terraform**

Terraform must be installed before proceeding with the automation process. Follow these steps to install and verify Terraform:

1. Download the latest version of Terraform from the official [Terraform Download](https://www.terraform.io/) page.
2. Extract and install Terraform by following the instructions provided for your operating system.
3. Verify the installation by executing the following command in your terminal:

```bash
terraform -v
```

![image](https://github.com/user-attachments/assets/d91ef44b-234b-4378-88f7-bad3275f5011)

If Terraform is installed correctly, this command will return the installed version.

---

## **2. Configure AWS Credentials**

Terraform requires valid AWS credentials to interact with AWS services. Configure your AWS credentials by running the following command:

```bash
aws configure
```

![image](https://github.com/user-attachments/assets/e2f736ab-2f77-44ec-ba57-69df1395f24e)

You will be prompted to enter the following details:
- **AWS Access Key ID**
- **AWS Secret Access Key**
- **Default region name** (e.g., `us-east-1` or `eu-west-2`)
- **Default output format** (leave empty for JSON or choose from `table`, `text`, or `json`)

After setting up the credentials, ensure that AWS CLI is correctly configured by running:

```bash
aws sts get-caller-identity
```

This command will return your AWS account details, confirming that the credentials are valid.

---

## **3. Create a Terraform Configuration File**

To begin, create a dedicated directory for your Terraform project and navigate into it:

```bash
mkdir terraform-ec2 && cd terraform-ec2
```

Inside this directory, create a new Terraform configuration file named `main.tf`:

```bash
touch main.tf
```

![image](https://github.com/user-attachments/assets/d29ed685-2c4c-4397-9713-1220c6b9aefe)

This file will contain the configuration for deploying an EC2 instance.

---

## **4. Define the EC2 Instance in Terraform**

Now, open `main.tf` in a text editor and define the Terraform configuration to deploy an EC2 instance:

```hcl
provider "aws" {
    profile = "default"  # Uses the AWS credentials profile
    region  = "eu-west-2"  # Change as per your preferred region
}

resource "aws_instance" "UGO_Server" {
    ami           = "ami-0cbf43fd299e3a464"  # Ensure this AMI ID is available in your selected region
    instance_type = "t2.micro"  # Instance type selection

    tags = {
        Name = "MyNCAAInstance"  # Assign a name to the instance
    }
}
```

![image](https://github.com/user-attachments/assets/398be90b-7c90-456a-8042-605dffae01ac)

Ensure that the **AMI ID** is valid for your AWS region by checking the AWS EC2 console or using the following AWS CLI command:

```bash
aws ec2 describe-images --owners amazon --filters "Name=name,Values=amzn2-ami-hvm-*-x86_64-gp2"
```

---

## **5. Initialize Terraform**

Before applying the configuration, Terraform must be initialized to download the required provider plugins. Run the following command:

```bash
terraform init
```

![image](https://github.com/user-attachments/assets/4c04a20d-a71a-4368-b6e1-139673e8ff27)

This command:
- Downloads the necessary provider plugins
- Prepares the working directory for Terraform execution
- Verifies that everything is correctly set up

---

## **6. Validate and Plan Configuration**

Before applying the configuration, validate and review the planned infrastructure changes:

1. **Validate the configuration syntax** to ensure there are no errors:

```bash
terraform validate
```

2. **Generate and review an execution plan** to preview the resources that Terraform will create:

```bash
terraform plan
```

![image](https://github.com/user-attachments/assets/c1209480-64fb-4aa2-9591-21c7e25cb373)

The execution plan provides a detailed breakdown of the resources that will be provisioned, modified, or destroyed.

---

## **7. Apply the Configuration**

To deploy the EC2 instance, apply the Terraform configuration:

```bash
terraform apply -auto-approve
```

![image](https://github.com/user-attachments/assets/808936ce-36b2-475a-b921-f7252078fee1)
![image](https://github.com/user-attachments/assets/5c1a2928-999e-4786-bd84-0bd02bf9ab57)

Terraform will provision the EC2 instance as per the defined configuration.

---

## **8. Verify the Instance**

Once the EC2 instance is deployed, verify its status using AWS CLI:

```bash
aws ec2 describe-instances --query 'Reservations[*].Instances[*].[InstanceId,State.Name,PublicIpAddress]' --output table
```

![image](https://github.com/user-attachments/assets/9b259335-5126-403d-a6ac-fc7bc906fa53)

Alternatively, you can check the instance details in the **AWS Management Console** under the **EC2 Dashboard**.

![image](https://github.com/user-attachments/assets/72817dae-48e7-4097-86ff-2c65ad26aab9)

---

## **9. Destroy the Instance**

To remove the EC2 instance and clean up resources, use the following command:

```bash
terraform destroy -auto-approve
```

![image](https://github.com/user-attachments/assets/241ee758-0aaf-4aad-b311-1527999d7e77)
![image](https://github.com/user-attachments/assets/b9885ac3-a076-45f5-b140-4c2a44248442)

This command will:
- Identify the infrastructure managed by Terraform
- Prompt for confirmation (if `-auto-approve` is not used)
- Terminate the EC2 instance

---

## **10. Conclusion**

Congratulations! You have successfully automated the provisioning and management of an **EC2 instance** using **Terraform**. This guide demonstrated:

- Installing and configuring Terraform
- Setting up AWS credentials
- Writing a Terraform configuration file
- Initializing and applying the Terraform configuration
- Verifying and destroying the EC2 instance

For more advanced configurations, refer to the [Terraform AWS Provider Documentation](https://registry.terraform.io/providers/hashicorp/aws/latest/docs).

