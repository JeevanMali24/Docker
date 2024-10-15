# Terraform

https://www.geeksforgeeks.org/devops-interview-questions/

# Terraform

### Input Variables in Terraform

**Input variables** in Terraform are used to parameterize configurations, making the code more flexible and reusable. You can pass values to these variables from command-line input, environment variables, or files. This allows you to customize how your infrastructure is provisioned.

---

### Defining Input Variables

Input variables are defined using the `variable` block. You can specify default values, types, and descriptions.

#### Basic Variable Declaration:
```hcl
variable "instance_type" {
  description = "Type of instance to be created"
  default     = "t2.micro"  # Default value
}
```

#### Example with Multiple Variables:
```hcl
variable "aws_region" {
  description = "AWS region"
  default     = "us-west-2"
}

variable "instance_count" {
  description = "Number of instances"
  default     = 2
}
```

### Types of Variables

Variables in Terraform can have explicit types. Some of the supported types are:

1. **String** (default type if not specified):
    ```hcl
    variable "environment" {
      type    = string
      default = "production"
    }
    ```

2. **Number**:
    ```hcl
    variable "instance_count" {
      type    = number
      default = 1
    }
    ```

3. **Boolean**:
    ```hcl
    variable "enable_monitoring" {
      type    = bool
      default = true
    }
    ```

4. **List**:
    ```hcl
    variable "instance_types" {
      type    = list(string)
      default = ["t2.micro", "t2.small"]
    }
    ```

5. **Map**:
    ```hcl
    variable "ami_ids" {
      type = map(string)
      default = {
        "us-east-1" = "ami-12345678"
        "us-west-2" = "ami-87654321"
      }
    }
    ```

6. **Object** (for complex structures):
    ```hcl
    variable "web_server_config" {
      type = object({
        instance_type = string
        disk_size     = number
      })
      default = {
        instance_type = "t2.micro"
        disk_size     = 30
      }
    }
    ```

---

### Using Input Variables in Resources

Once defined, variables can be referenced in your Terraform configurations using `var.<variable_name>`.

#### Example:
```hcl
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = var.instance_type  # Referencing the variable
  count         = var.instance_count # Use count based on variable
}
```

### Passing Variables to Terraform

You can pass values to variables in several ways:

#### 1. **Command Line**:
You can override the default values when running `terraform plan` or `terraform apply`:
```bash
terraform apply -var="instance_type=t2.large" -var="instance_count=3"
```

#### 2. **Variable Files**:
You can store variable values in a `.tfvars` file and pass the file when executing Terraform.

**`variables.tfvars`**:
```hcl
instance_type = "t2.large"
instance_count = 3
```

**Command to Use the File**:
```bash
terraform apply -var-file="variables.tfvars"
```

#### 3. **Environment Variables**:
You can also set variables through environment variables by prefixing them with `TF_VAR_`.

For example, for a variable `instance_type`:
```bash
export TF_VAR_instance_type="t2.large"
terraform apply
```

---

### Validation Rules

You can apply validation rules to input variables to enforce constraints.

```hcl
variable "instance_count" {
  type    = number
  default = 1

  validation {
    condition     = var.instance_count > 0
    error_message = "Instance count must be greater than 0."
  }
}
```


### Example Full Configuration with Input Variables

```
# variables.tf
variable "aws_region" {
  description = "AWS region"
  type        = string
  default     = "us-west-2"
}

variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t2.micro"
}

variable "instance_count" {
  description = "Number of EC2 instances"
  type        = number
  default     = 2
}

# main.tf
provider "aws" {
  region = var.aws_region
}

resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"
  instance_type = var.instance_type
  count         = var.instance_count

  tags = {
    Name = "TerraformInstance"
  }
}

# outputs.tf
output "instance_ids" {
  value = aws_instance.example[*].id
}

```


### Conclusion

Input variables in Terraform provide flexibility, reusability, and dynamic infrastructure creation by allowing you to customize configurations without changing the underlying code. You can pass variables through files, environment variables, or command-line inputs, and apply validation rules to ensure they meet your requirements.
