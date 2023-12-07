## Deploying a Static Website on AWS with Terraform

In this post, you'll learn how to deploy a static website to AWS using Terraform. This is a great option for serving static content like HTML, CSS, and JavaScript files. Terraform makes the process simple and easy to automate, which is perfect for developers who want to focus on building their website rather than managing infrastructure.

### Prerequisites

Before you start, you'll need to have the following:

-   An AWS account
-   Terraform installed
-   A static website codebase

### Step 1: Set Up Terraform

First, you need to initialize a Terraform project. To do this, create a directory for your project and then run the following command:

```
terraform init

```

This will download the necessary plugins and initialize the Terraform state file.

### Step 2: Create an S3 Bucket

Next, you need to create an S3 bucket to store your website's files. In your Terraform configuration file (e.g., main.tf), add the following code:

```
resource "aws_s3_bucket" "bucket" {
  bucket = "your-unique-bucket-name"
  acl = "public-read"

  website {
    index_document = "index.html"
  }
}

```

This code creates an S3 bucket named "your-unique-bucket-name" and sets its ACL to "public-read," meaning anyone can access the files in the bucket. It also sets the index document to "index.html," which is the file that will be served when someone visits the website root directory.

### Step 3: Configure Terraform to Upload Your Files

You'll need to tell Terraform how to upload your website files to the S3 bucket. You can do this by adding a provisioner to your Terraform configuration. The following code shows how to use the `local-exec` provisioner to upload your files:

```
provisioner "local-exec" {
  command = "aws s3 sync static/ s3://${aws_s3_bucket.bucket.bucket} --acl public-read --delete"
}

```

This provisioner runs the `aws s3 sync` command, which will upload all the files from the `static/` directory to the S3 bucket. The `--acl public-read` flag ensures that the files are publicly accessible. The `--delete` flag tells Terraform to delete any files from the bucket that are no longer present in the `static/` directory.

### Step 4: Deploy Your Website

Once you have your Terraform configuration ready, you can deploy your website by running the following commands:

```
terraform plan
terraform apply

```

The `terraform plan` command will show you what changes Terraform will make to your AWS infrastructure. The `terraform apply` command will actually make those changes.

### Step 5: Access Your Website

Once your website is deployed, you can access it by visiting the URL of your S3 bucket. The URL will look something like this:

```
http://your-unique-bucket-name.s3.amazonaws.com

```

### Conclusion

Deploying a static website to AWS with Terraform is a simple and efficient process. This method allows you to automate the infrastructure management process and focus on building your website.

### Additional Notes:

-   You can customize the Terraform configuration to meet your specific needs. For example, you can configure a custom domain name for your website.
-   Terraform can also be used to manage other AWS resources, such as CloudFront distributions, which can improve the performance of your website.

I hope this blog post has helped you learn how to deploy a static website to AWS with Terraform. If you have any questions, please feel free to leave a comment below.


I wrote a blog about this on my website.  [akiltipu.com](https://akiltipu.com).
