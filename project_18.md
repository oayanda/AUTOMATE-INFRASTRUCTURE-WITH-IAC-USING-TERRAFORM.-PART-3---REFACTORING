# TERRAFORM MODULARIZATION FOR 2 WEB APPLICATION WITH AUTOSCALING PROJECT

***[View Repo Here](https://github.com/oayanda/Terraform-PBL16/tree/main/PBL18)***

## Terraform backends

The backends.tf or the Backend Configuration files defines where Terraform stores its state data files. Terraform uses persisted state data to keep track of the resources it manages.

Introducing Backend on AWS S3 for Team collaboration.

```bash 

#############################
##creating bucket for s3 backend
#########################

resource "aws_s3_bucket" "terraform-state" {
  bucket        = "pbl18-dev"
  force_destroy = true
}
resource "aws_s3_bucket_versioning" "version" {
  bucket = aws_s3_bucket.terraform-state.id
  versioning_configuration {
    status = "Enabled"
  }
}
resource "aws_s3_bucket_server_side_encryption_configuration" "first" {
  bucket = aws_s3_bucket.terraform-state.id
  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm = "AES256"
    }
  }
}
## create a DynamoDB table to handle locks and perform consistency checks

resource "aws_dynamodb_table" "terraform_locks" {
  name         = "terraform-locks"
  billing_mode = "PAY_PER_REQUEST"
  hash_key     = "LockID"
  attribute {
    name = "LockID"
    type = "S"
  }
}
```

`Lock file in DynamoDB`
![s3](/images/lock.png)

`State File in Amazon S3 bucket`
![s3](/images/statefile.png)

`Run Terraform Plan`
![s3](/images/plan.png)

`Terraform State List`
![s3](/images/state.png)

Modules
![s3](/images/modules.png)