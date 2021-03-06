# Manage Spinnaker

Use this section to manage your Spinnaker deployment going forward.

## Select GCP project

Select the project in which your Spinnaker is installed, then click **Next**.

<walkthrough-project-billing-setup>
</walkthrough-project-billing-setup>

## Manage Spinnaker via Halyard from Cloud Shell

This management console lets you run [Halyard
commands](https://www.spinnaker.io/reference/halyard/) to configure and manage
your Spinnaker installation.

### Ensure you are connected to the correct Kubernetes context

```bash
PROJECT_ID={{project-id}} ~/spinnaker-for-gcp/scripts/manage/check_cluster_config.sh
```

### Pull Spinnaker config

Paste and run this command to pull the configuration from your Spinnaker
deployment into your Cloud Shell.


```bash
~/spinnaker-for-gcp/scripts/manage/pull_config.sh
```

### Update the console

**This is a required step if you've just pulled config from a different Spinnaker deployment.**

This will include details on how to connect to Spinnaker.

```bash
~/spinnaker-for-gcp/scripts/manage/update_console.sh
```

### Configure Spinnaker via Halyard

All [halyard](https://www.spinnaker.io/reference/halyard/commands/) commands are available.

```bash
hal config
```

As with provisioning Spinnaker, don't use `hal deploy connect` when managing
Spinnaker. Also, don't use `hal deploy apply`. Instead, use the `push_and_apply.sh`
command shown below.

### Notes on Halyard commands that reference local files

If you add a Kubernetes account that references a kubeconfig file, that file must live within
the '`~/.hal/default/credentials`' directory on your Cloud Shell VM. The
kubeconfig is specified using the `--kubeconfig-file` argument to the
`hal config provider kubernetes account add` and ...`edit` commands.

Change the `default` path segment if you are using a different name for your deployment.

The same requirement applies for any Google JSON key file specified via the
`--json-path` argument to various commands.

### Push and apply updated config to Spinnaker deployment

If you change any of the configuration, paste and run this command to push
and apply those changes to your Spinnaker deployment.

```bash
~/spinnaker-for-gcp/scripts/manage/push_and_apply.sh
```

## Included command-line tools

### Halyard CLI

The [Halyard CLI](https://www.spinnaker.io/reference/halyard/) (`hal`) and
daemon are installed in your Cloud Shell.

If you want to use a specific version of Halyard, use:

```bash
~/spinnaker-for-gcp/scripts/cli/install_hal.sh --version $HALYARD_VERSION
```

If you want to upgrade to the latest version of Halyard, use:

```bash
~/spinnaker-for-gcp/scripts/cli/update_hal.sh
```

### Spinnaker CLI

The [Spinnaker CLI](https://www.spinnaker.io/guides/spin/app/) 
(`spin`) is installed in your Cloud Shell.

If you want to upgrade to the latest version, use:

```bash
~/spinnaker-for-gcp/scripts/cli/install_spin.sh
```

## Scripts for Common Commands

Remember that any configuration changes you make locally (e.g. adding
accounts) must be pushed and applied to your deployment to take effect:

```bash
~/spinnaker-for-gcp/scripts/manage/push_and_apply.sh
```

### Add Spinnaker account for GKE

Before you run this command, make sure you've configured the context you intend
to use to manage your GKE resources.

The public Spinnaker documentation contains details on [configuring GKE
clusters](https://www.spinnaker.io/setup/install/providers/kubernetes-v2/gke/).

```bash
~/spinnaker-for-gcp/scripts/manage/add_gke_account.sh
```

### Add Spinnaker account for GCE

```bash
~/spinnaker-for-gcp/scripts/manage/add_gce_account.sh
```

### Add Spinnaker account for GAE

```bash
~/spinnaker-for-gcp/scripts/manage/add_gae_account.sh
```

### Upgrade Spinnaker

First, modify `SPINNAKER_VERSION` in your `properties` file to reflect the desired version of Spinnaker:

<walkthrough-editor-open-file
    filePath="spinnaker-for-gcp/scripts/install/properties"
    text="Open properties file">
</walkthrough-editor-open-file>

Next, use Halyard to apply the changes:

```bash
~/spinnaker-for-gcp/scripts/manage/update_spinnaker_version.sh
```

### Upgrade Halyard daemon running in cluster

First, modify `HALYARD_VERSION` in your `properties` file to reflect the desired version of Halyard:

<walkthrough-editor-open-file
    filePath="spinnaker-for-gcp/scripts/install/properties"
    text="Open properties file">
</walkthrough-editor-open-file>

Next, apply this change to the Statefulset managing the Halyard daemon:

```bash
~/spinnaker-for-gcp/scripts/manage/update_halyard_daemon.sh
```

### Connect to Redis

```bash
~/spinnaker-for-gcp/scripts/manage/connect_to_redis.sh
```

## Configure Operator Access

To add additional operators, grant them the `Owner` role on GCP Project {{project-id}}: [IAM Permissions](https://console.developers.google.com/iam-admin/iam?project={{project-id}})

Once they have been added to the project, they can locate Spinnaker by navigating to the newly-registered [Kubernetes Application](https://console.developers.google.com/kubernetes/application/$ZONE/$DEPLOYMENT_NAME/spinnaker/$DEPLOYMENT_NAME?project={{project-id}}).

Note that if you have secured Spinnaker via IAP, granting someone the `Owner` role does not implicitly grant them access as a user. For configuring user access, please continue on to the *Configure User Access (IAP)* section.

The application's *Next Steps* section contains the relevant links and operator instructions.

