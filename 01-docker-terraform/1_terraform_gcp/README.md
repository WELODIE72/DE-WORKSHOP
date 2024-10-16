
# Pre-requisites


### Refresh service-account's auth-token for this session

```bash
✗ gcloud auth application-default login
```

The result of the command
```bash
The environment variable [GOOGLE_APPLICATION_CREDENTIALS] is set to:
  [/Users/mbikinaserge/Workspace/training/data-engineering-workshop/nsldatae9g-c7c2bc1a8679.json]
Credentials will still be generated to the default location:
  [/Users/mbikinaserge/.config/gcloud/application_default_credentials.json]
To use these credentials, unset this environment variable before
running your application.
Do you want to continue (Y/n)?  Y
Your browser has been opened to visit:
    https://accounts.google.com/o/oauth2/auth?response_type=code&client_id=764086051850-6qr4p6gpi6hn506pt8ejuq83di341hur.apps.googleusercontent.com&redirect_uri=http%3A%2F%2Flocalhost%3A8085%2F&scope=openid+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.email+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fcloud-platform+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fsqlservice.login&state=tWDRJTDqFJxEse5EJmH3rTU7XIiIAK&access_type=offline&code_challenge=C1zE9W7WITaus8WmYqQ_9kZh-kyhs-G9reSIxh4r1d4&code_challenge_method=S256
Credentials saved to file: [/Users/mbikinaserge/.config/gcloud/application_default_credentials.json]
These credentials will be used by any library that requests Application Default Credentials (ADC).
Quota project "nsldatae9g" was added to ADC which can be used by Google client libraries for billing and quota. Note that some services may still bill the project owning the resource.
```


### Initialize state file (.tfstate)

```bash
✗ terraform init
```

The result of the command
```bash
Initializing the backend...
Initializing provider plugins...
- Finding hashicorp/google versions matching "5.6.0"...
- Installing hashicorp/google v5.6.0...
- Installed hashicorp/google v5.6.0 (signed by HashiCorp)
Terraform has created a lock file .terraform.lock.hcl to record the provider
selections it made above. Include this file in your version control repository
so that Terraform can guarantee to make the same selections by default when
you run "terraform init" in the future.

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```


### Check changes to new infra plan

```bash
 ✗ terraform plan -var="project=nsldatae9g"
```

The result of the command
```bash
Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # google_bigquery_dataset.demo_dataset will be created
  + resource "google_bigquery_dataset" "demo_dataset" {
      + creation_time              = (known after apply)
      + dataset_id                 = "demo_dataset"
      + default_collation          = (known after apply)
      + delete_contents_on_destroy = false
      + effective_labels           = (known after apply)
      + etag                       = (known after apply)
      + id                         = (known after apply)
      + is_case_insensitive        = (known after apply)
      + last_modified_time         = (known after apply)
      + location                   = "EU"
      + max_time_travel_hours      = (known after apply)
      + project                    = "nsldatae9g"
      + self_link                  = (known after apply)
      + storage_billing_model      = (known after apply)
      + terraform_labels           = (known after apply)

      + access (known after apply)
    }

  # google_storage_bucket.demo-bucket will be created
  + resource "google_storage_bucket" "demo-bucket" {
      + effective_labels            = (known after apply)
      + force_destroy               = true
      + id                          = (known after apply)
      + location                    = "EU"
      + name                        = "terraform-demo-terra-bucket"
      + project                     = (known after apply)
      + public_access_prevention    = (known after apply)
      + self_link                   = (known after apply)
      + storage_class               = "STANDARD"
      + terraform_labels            = (known after apply)
      + uniform_bucket_level_access = (known after apply)
      + url                         = (known after apply)

      + lifecycle_rule {
          + action {
              + type          = "AbortIncompleteMultipartUpload"
                # (1 unchanged attribute hidden)
            }
          + condition {
              + age                    = 1
              + matches_prefix         = []
              + matches_storage_class  = []
              + matches_suffix         = []
              + with_state             = (known after apply)
                # (3 unchanged attributes hidden)
            }
        }

      + versioning (known after apply)

      + website (known after apply)
    }

Plan: 2 to add, 0 to change, 0 to destroy.
```

### Create new infra

```bash
✗ terraform apply -var="project=nsldatae9g"
```

The result of the command
```bash
google_bigquery_dataset.demo_dataset: Refreshing state... [id=projects/nsldatae9g/datasets/demo_dataset]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # google_storage_bucket.nsldatae9g-demo-bucket will be created
  + resource "google_storage_bucket" "nsldatae9g-demo-bucket" {
      + effective_labels            = (known after apply)
      + force_destroy               = true
      + id                          = (known after apply)
      + location                    = "EU"
      + name                        = "nsldatae9g-terraform-demo-terra-bucket"
      + project                     = (known after apply)
      + public_access_prevention    = (known after apply)
      + self_link                   = (known after apply)
      + storage_class               = "STANDARD"
      + terraform_labels            = (known after apply)
      + uniform_bucket_level_access = (known after apply)
      + url                         = (known after apply)

      + lifecycle_rule {
          + action {
              + type          = "AbortIncompleteMultipartUpload"
                # (1 unchanged attribute hidden)
            }
          + condition {
              + age                    = 1
              + matches_prefix         = []
              + matches_storage_class  = []
              + matches_suffix         = []
              + with_state             = (known after apply)
                # (3 unchanged attributes hidden)
            }
        }

      + versioning (known after apply)

      + website (known after apply)
    }

Plan: 1 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

google_storage_bucket.nsldatae9g-demo-bucket: Creating...
google_storage_bucket.nsldatae9g-demo-bucket: Creation complete after 2s [id=nsldatae9g-terraform-demo-terra-bucket]

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

### Delete infra after your work, to avoid costs on any running services

```bash
✗ terraform destroy
```

The result of the command
```bash
google_storage_bucket.nsldatae9g-demo-bucket: Refreshing state... [id=nsldatae9g-terraform-demo-terra-bucket]
google_bigquery_dataset.demo_dataset: Refreshing state... [id=projects/nsldatae9g/datasets/demo_dataset]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # google_bigquery_dataset.demo_dataset will be destroyed
  - resource "google_bigquery_dataset" "demo_dataset" {
      - creation_time                   = 1729066938196 -> null
      - dataset_id                      = "demo_dataset" -> null
      - default_partition_expiration_ms = 0 -> null
      - default_table_expiration_ms     = 0 -> null
      - delete_contents_on_destroy      = false -> null
      - effective_labels                = {} -> null
      - etag                            = "EAR6LgINKi3YtnLX69zOBg==" -> null
      - id                              = "projects/nsldatae9g/datasets/demo_dataset" -> null
      - is_case_insensitive             = false -> null
      - labels                          = {} -> null
      - last_modified_time              = 1729067734196 -> null
      - location                        = "EU" -> null
      - max_time_travel_hours           = "168" -> null
      - project                         = "nsldatae9g" -> null
      - self_link                       = "https://bigquery.googleapis.com/bigquery/v2/projects/nsldatae9g/datasets/demo_dataset" -> null
      - terraform_labels                = {} -> null
        # (4 unchanged attributes hidden)

      - access {
          - role           = "OWNER" -> null
          - user_by_email  = "sa-data-engineering@nsldatae9g.iam.gserviceaccount.com" -> null
            # (4 unchanged attributes hidden)
        }
      - access {
          - role           = "OWNER" -> null
          - special_group  = "projectOwners" -> null
            # (4 unchanged attributes hidden)
        }
      - access {
          - role           = "READER" -> null
          - special_group  = "projectReaders" -> null
            # (4 unchanged attributes hidden)
        }
      - access {
          - role           = "WRITER" -> null
          - special_group  = "projectWriters" -> null
            # (4 unchanged attributes hidden)
        }
    }

  # google_storage_bucket.nsldatae9g-demo-bucket will be destroyed
  - resource "google_storage_bucket" "nsldatae9g-demo-bucket" {
      - default_event_based_hold    = false -> null
      - effective_labels            = {} -> null
      - enable_object_retention     = false -> null
      - force_destroy               = true -> null
      - id                          = "nsldatae9g-terraform-demo-terra-bucket" -> null
      - labels                      = {} -> null
      - location                    = "EU" -> null
      - name                        = "nsldatae9g-terraform-demo-terra-bucket" -> null
      - project                     = "nsldatae9g" -> null
      - public_access_prevention    = "inherited" -> null
      - requester_pays              = false -> null
      - self_link                   = "https://www.googleapis.com/storage/v1/b/nsldatae9g-terraform-demo-terra-bucket" -> null
      - storage_class               = "STANDARD" -> null
      - terraform_labels            = {} -> null
      - uniform_bucket_level_access = false -> null
      - url                         = "gs://nsldatae9g-terraform-demo-terra-bucket" -> null

      - lifecycle_rule {
          - action {
              - type          = "AbortIncompleteMultipartUpload" -> null
                # (1 unchanged attribute hidden)
            }
          - condition {
              - age                        = 1 -> null
              - days_since_custom_time     = 0 -> null
              - days_since_noncurrent_time = 0 -> null
              - matches_prefix             = [] -> null
              - matches_storage_class      = [] -> null
              - matches_suffix             = [] -> null
              - num_newer_versions         = 0 -> null
              - with_state                 = "ANY" -> null
                # (3 unchanged attributes hidden)
            }
        }
    }

Plan: 0 to add, 0 to change, 2 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

google_storage_bucket.nsldatae9g-demo-bucket: Destroying... [id=nsldatae9g-terraform-demo-terra-bucket]
google_bigquery_dataset.demo_dataset: Destroying... [id=projects/nsldatae9g/datasets/demo_dataset]
google_bigquery_dataset.demo_dataset: Destruction complete after 0s
google_storage_bucket.nsldatae9g-demo-bucket: Destruction complete after 1s

Destroy complete! Resources: 2 destroyed.
```



