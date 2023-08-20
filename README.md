# Deploy a static website using Google Cloud Storage and Terraform
In this repo, I will provide with you all the details of deploying a static website using Google Cloud Storage and Terraform

![Terraform with GCP](https://drive.google.com/file/d/1yuH3w9779FlJ8ym-iIvOmwxMsyeFx_1t/view?usp=sharing)

## Prerequisites

  * Create a Google Cloud Platform (GCP) account if you don't have one.
  * Install Terraform on your local machine.

## Configure GCP Credentials:

  * Create a service account and download the JSON key file.
  * Set the GOOGLE_APPLICATION_CREDENTIALS environment variable to the path of the downloaded JSON key

## Create a Folder Structure

  * Open VS Code and create necessary files like following:

   ```
static-website-project/
    └── website/
        ├── index.html
    └── terraform/
        ├── main.tf
        ├── provider.tf
        ├── keys.json
    
  ```

## Write Terraform Configuration:

  * Inside index.html paste following code:
  ```
  <!DOCTYPE html>
<html>
<head>
    <title>Static website deployement!</title>
</head>
<body>
    <h1>Hello, DevOps Enthusiasts!</h1>
    <p>Lorem, ipsum dolor sit amet consectetur adipisicing elit. Tempora qui dolorem velit, blanditiis accusantium et in doloremque architecto debitis eos nesciunt officia ratione tenetur sit error assumenda veritatis? Temporibus, iste consequatur voluptas obcaecati aspernatur error esse odio nihil. Fugiat eos reprehenderit quas dolorum quibusdam laboriosam corporis quae saepe esse nam.</p>
</body>
</html>
  ```

  * Inside main.tf paste following code:

```
resource "google_storage_bucket" "website_bucket" {
  name          = "my-stat-web1"
  location      = "US"
  force_destroy = true
  website {
    main_page_suffix = "index.html"
  }
}

resource "google_storage_bucket_iam_member" "public_access" {
  bucket = google_storage_bucket.website_bucket.name
  role   = "roles/storage.objectViewer"
  member = "allUsers"
}

resource "google_storage_bucket_object" "index" {
  name        = "index.html"
  bucket      = google_storage_bucket.website_bucket.name
  source      = "../website/index.html"  # Path to your index.html file
  content_type = "text/html"
}

output "website_url" {
  value = "https://${google_storage_bucket.website_bucket.name}.storage.googleapis.com"
}

```

  * Inside provider.tf paste following code

```
terraform {
  required_providers {
    google = {
      source = "hashicorp/google"
      version = "4.78.0"
    }
  }
}

provider "google" {
 project = "your_project_id"
 region = "us-central-1"
 credentials = file("keys.json")
}
```

  * Go to Google Cloud Console and create a service account and generate a key
  * Upload Key inside terraform folder
  * I have a full video if you can not do above two steps: [Project Setup Terraform on GCP](https://youtu.be/AaRmcE2YdFk)

## Initialize and Apply Terraform:

```
terraform init:
Initialize your Terraform project. This command sets up your working directory, downloads necessary plugins, and prepares for configuration.

terraform plan:
After initialization, run this command to preview changes that Terraform will make to your infrastructure. It helps you understand modifications before applying them.

terraform apply:
Once you're satisfied with the planned changes, use this command to apply those changes to your infrastructure. It transforms your infrastructure to match the defined configuration.

terraform destroy:
When you're done with your resources and want to tear them down, run this command. It will destroy the resources created by your Terraform configuration, cleaning up your environment.
```

## Check the Website URL
  * Go to Google Cloud Console
  * Then click on Cloud Storage
  * Click on your bucket
  * Copy URL and check it in a Incognito Window

## You have sucessefully depoyed a static website using GCP with Terraform

  * [If you have any problems or questions reach me on LinkedIn or YouTube](https://linktr.ee/otabekinha)
  * Thanks for reaching until here and all the best with your future works
