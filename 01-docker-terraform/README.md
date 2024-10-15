
# Pre-requisites

### Create a Google Cloud account
1. Visit Google Cloud Platform Console https://cloud.google.com

### Create a Google Cloud project
1. Open the Google Cloud Console, log in, and click on the project name or "Select a project."
2. Click New Project, name your project, and link a billing account (even for free tier).
3. Click Create, then select your new project to start using Google Cloud services.

### Create a Google Service Account
1. In the Google Cloud Console, navigate to IAM & Admin > Service Accounts.
2. Click Create Service Account, provide a name, ID, and description, then click Create and Continue.
3. Assign roles, adjust permissions if needed, and click Done to finish setting up the service account.

### Create cryptographic keys for the Google Service Account
1. In the Google Cloud Console, go to IAM & Admin > Service Accounts.
2. Select the service account, click Manage Keys, and then click Add Key > Create New Key.
3. Choose JSON format, click Create, and the key will download automatically to your system.

### Install the Google Cloud CLI
Follow the intructions provided for your platform : https://cloud.google.com/sdk/docs/install-sdk

### Initialize the gcloud CLI
```bash
(base) ➜  data-engineering-workshop git:(main) ✗ gcloud init
Welcome! This command will take you through the configuration of gcloud.

Your current configuration has been set to: [default]

You can skip diagnostics next time by using the following flag:
  gcloud init --skip-diagnostics

Network diagnostic detects and fixes local network connection issues.
Checking network connection...done.                                                                                                                                                                                                                                                                      
Reachability Check passed.
Network diagnostic passed (1/1 checks passed).

You must sign in to continue. Would you like to sign in (Y/n)?  Y

Your browser has been opened to visit:

    https://accounts.google.com/o/oauth2/auth?response_type=code&client_id=32555940559.apps.googleusercontent.com&redirect_uri=http%3A%2F%2Flocalhost%3A8085%2F&scope=openid+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.email+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fcloud-platform+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fappengine.admin+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fsqlservice.login+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fcompute+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Faccounts.reauth&state=uc7cyrydxDesqWhlmQ6L9uSeq8qOTw&access_type=offline&code_challenge=BoEGiX_mgiXjPWv5yIBZOT3_HDBtjo_oihOV4gaDjZU&code_challenge_method=S256

You are signed in as: [smbikina@gmail.com].

Pick cloud project to use: 
 [1] api-project-963735164668
 [2] ids-web
 [3] nsldatae9g
 [4] nslk8shardway
 [5] nslk8slabs
 [6] nslk8sspark
 [7] website-1475750712688
 [8] Enter a project ID
 [9] Create a new project
Please enter numeric choice or text value (must exactly match list item):  3

Your current project has been set to: [nsldatae9g].

Not setting default zone/region (this feature makes it easier to use
[gcloud compute] by setting an appropriate default value for the
--zone and --region flag).
See https://cloud.google.com/compute/docs/gcloud-compute section on how to set
default compute region and zone manually. If you would like [gcloud init] to be
able to do this for you the next time you run it, make sure the
Compute Engine API is enabled for your project on the
https://console.developers.google.com/apis page.

Created a default .boto configuration file at [/Users/mbikinaserge/.boto]. See this file and
[https://cloud.google.com/storage/docs/gsutil/commands/config] for more
information about configuring Google Cloud Storage.
The Google Cloud CLI is configured and ready to use!

* Commands that require authentication will use smbikina@gmail.com by default
* Commands will reference project `nsldatae9g` by default
Run `gcloud help config` to learn how to change individual settings

This gcloud configuration is called [default]. You can create additional configurations if you work with multiple accounts and/or projects.
Run `gcloud topic configurations` to learn more.

Some things to try next:

* Run `gcloud --help` to see the Cloud Platform services you can interact with. And run `gcloud help COMMAND` to get help on any gcloud command.
* Run `gcloud topic --help` to learn about advanced features of the CLI like arg files and output formatting
* Run `gcloud cheat-sheet` to see a roster of go-to `gcloud` commands.
```

### Run core commands

List accounts whose credentials are stored on the local system:
```bash
✗ gcloud auth list
  Credentialed Accounts
ACTIVE  ACCOUNT
*       smbikina@gmail.com

To set the active account, run:
    $ gcloud config set account `ACCOUNT`
```

List the properties in your active gcloud CLI configuration:
```bash
✗ gcloud config list
[core]
account = smbikina@gmail.com
disable_usage_reporting = True
project = nsldatae9g

Your active configuration is: [default]
```

View information about your gcloud CLI installation and the active configuration:
```bash
✗ gcloud info
Google Cloud SDK [496.0.0]

Platform: [Mac OS X, arm] uname_result(system='Darwin', node='MacBookPro.fritz.box', release='24.0.0', version='Darwin Kernel Version 24.0.0: Tue Sep 24 23:39:07 PDT 2024; root:xnu-11215.1.12~1/RELEASE_ARM64_T6000', machine='arm64')
Locale: ('en_US', 'UTF-8')
Python Version: [3.11.9 (main, Apr 19 2024, 11:43:47) [Clang 14.0.6 ]]
Python Location: [/opt/homebrew/anaconda3/bin/python3]
OpenSSL: [OpenSSL 3.0.13 30 Jan 2024]
Requests Version: [2.25.1]
urllib3 Version: [1.26.9]
Default CA certs file: [/Users/mbikinaserge/Workspace/training/data-engineering-workshop/google-cloud-sdk/lib/third_party/certifi/cacert.pem]
Site Packages: [Disabled]

Installation Root: [/Users/mbikinaserge/Workspace/training/data-engineering-workshop/google-cloud-sdk]
Installed Components:
  gsutil: [5.30]
  core: [2024.10.04]
  bq: [2.1.9]
  gcloud-crc32c: [1.0.0]
System PATH: [/Users/mbikinaserge/Workspace/training/data-engineering-workshop/google-cloud-sdk/bin:/opt/homebrew/anaconda3/bin:/usr/local/anaconda3/bin:/Users/mbikinaserge/.rd/bin:/Users/mbikinaserge/.cargo/bin:/Users/mbikinaserge/.nvm/versions/node/v16.14.0/bin:/Library/Frameworks/Python.framework/Versions/3.12/bin:/opt/homebrew/bin:/opt/homebrew/sbin:/usr/local/bin:/System/Cryptexes/App/usr/bin:/usr/bin:/bin:/usr/sbin:/sbin:/var/run/com.apple.security.cryptexd/codex.system/bootstrap/usr/local/bin:/var/run/com.apple.security.cryptexd/codex.system/bootstrap/usr/bin:/var/run/com.apple.security.cryptexd/codex.system/bootstrap/usr/appleinternal/bin:/Library/Apple/usr/bin:/opt/homebrew/anaconda3/bin:/opt/homebrew/anaconda3/condabin:/usr/local/anaconda3/bin:/Users/mbikinaserge/.rd/bin:/Users/mbikinaserge/.cargo/bin:/Users/mbikinaserge/.nvm/versions/node/v16.14.0/bin:/Library/Frameworks/Python.framework/Versions/3.12/bin:/Users/mbikinaserge/.local/bin:/Users/mbikinaserge/Library/Python/3.9/bin:/Users/mbikinaserge/.local/bin:/Users/mbikinaserge/Library/Python/3.9/bin:/Users/mbikinaserge/.local/bin:/Users/mbikinaserge/Library/Python/3.9/bin]
Python PATH: [/Users/mbikinaserge/Workspace/training/data-engineering-workshop/google-cloud-sdk/lib/third_party:/Users/mbikinaserge/Workspace/training/data-engineering-workshop/google-cloud-sdk/lib:/opt/homebrew/anaconda3/lib/python311.zip:/opt/homebrew/anaconda3/lib/python3.11:/opt/homebrew/anaconda3/lib/python3.11/lib-dynload]
Cloud SDK on PATH: [True]
Kubectl on PATH: [/Users/mbikinaserge/.rd/bin/kubectl]

Installation Properties: [/Users/mbikinaserge/Workspace/training/data-engineering-workshop/google-cloud-sdk/properties]
User Config Directory: [/Users/mbikinaserge/.config/gcloud]
Active Configuration Name: [default]
Active Configuration Path: [/Users/mbikinaserge/.config/gcloud/configurations/config_default]

Account: [smbikina@gmail.com]
Project: [nsldatae9g]
Universe Domain: [googleapis.com]

Current Properties:
  [core]
    account: [smbikina@gmail.com] (property file)
    disable_usage_reporting: [True] (property file)
    project: [nsldatae9g] (property file)

Logs Directory: [/Users/mbikinaserge/.config/gcloud/logs]
Last Log File: [/Users/mbikinaserge/.config/gcloud/logs/2024.10.15/10.54.30.278066.log]

git: [git version 2.37.3]
ssh: [OpenSSH_9.8p1, LibreSSL 3.3.6]
```

### Set Key access env variable

```bash
export GOOGLE_APPLICATION_CREDENTIALS="/Users/mbikinaserge/Workspace/training/data-engineering-workshop/nsldatae9g-c7c2bc1a8679.json"
```

```bash
✗ gcloud auth application-default login

The environment variable [GOOGLE_APPLICATION_CREDENTIALS] is set to:
  [/Users/mbikinaserge/Workspace/training/data-engineering-workshop/nsldatae9g-c7c2bc1a8679.json]
Credentials will still be generated to the default location:
  [/Users/mbikinaserge/.config/gcloud/application_default_credentials.json]
To use these credentials, unset this environment variable before
running your application.

Do you want to continue (Y/n)?  Y

Your browser has been opened to visit:

    https://accounts.google.com/o/oauth2/auth?response_type=code&client_id=764086051850-6qr4p6gpi6hn506pt8ejuq83di341hur.apps.googleusercontent.com&redirect_uri=http%3A%2F%2Flocalhost%3A8085%2F&scope=openid+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.email+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fcloud-platform+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fsqlservice.login&state=0kGeRtognbJorUk2xfoE0manYDlH2X&access_type=offline&code_challenge=RMc-2U0JAmRlBRzIfclD4gjgnFySIlB0SEZl71T_3M0&code_challenge_method=S256


Credentials saved to file: [/Users/mbikinaserge/.config/gcloud/application_default_credentials.json]

These credentials will be used by any library that requests Application Default Credentials (ADC).

Quota project "nsldatae9g" was added to ADC which can be used by Google client libraries for billing and quota. Note that some services may still bill the project owning the resource.
```

### IAM Roles for Service account:

1. Go to the IAM section of IAM & Admin: https://console.cloud.google.com/iam-admin/iam
2. Click the Edit principal icon for your service account.
3. Add these roles in addition to Viewer:
    - Storage Admin
    - Storage Object Admin
    - BigQuery Admin

### Enable these APIs for your project:

1. In the Google Cloud Console, navigate to APIs & Services > Library.
2. Search for the API you want to enable and click on it.
3. Click Enable to activate thes following APIs:

    https://console.cloud.google.com/apis/library/iam.googleapis.com
    https://console.cloud.google.com/apis/library/iamcredentials.googleapis.com