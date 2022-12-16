# TI Avonnni Components

For the original README, switch to the `main` branch.

This repository bundles the Avonni repo into a Salesforce Unlocked Package for use at Tradesmen International. As an unlocked package, these Avonni components are namespaced separately from our own code.

# Contributions

## Persistent Branches

`main` - This branch should always be the latest from the source repo.

`package` - The primary branch used in this repo to track changes made on top of the source repo, `main`.

## Steps to update with new Avonni Changes

Because Avonni does not bundle their code for packaging, this repo takes some manual steps after an Avonni update.

1. The sfdx-project.json should remain unchanged across Avonni versions.
1. Avonni does not set `isExposed` on their LWC meta.xml files. Use the following vscode regex search to find and replace the setting in this repo.
    * Search For: `<isExposed>false</isExposed>`
    * Replace with: `<isExposed>true</isExposed>`
    * Files to Include: `**/lwc/avonni**/*.xml`

## Deploy new version

1. Verify all `**/lwc/avonni**/*.xml` have `isExposed` set to `true`.
1. Create new Version
    * `sfdx force:package:version:create --wait 10 --package avonni --installationkeybypass --codecoverage`
    * `--codecoverage` is required to promote to a released package, but can be omitted to iterate during dev.
1. Get the Subscriber Package Version ID
    * Starts with `04t`
    * Reported output of force:package:version:create
    * Can also be retrieved later using `sfdx force:package:version:list`
1. Install new Version to Salesforce Sandbox/Prod
    * Using the Subscriber Package Version Id (Starts with `04t`).
    * `sfdx force:package:version:promote --package <04t>
    * `sfdx force:package:install --wait 10 --package <04t...> --username <username of sf target>`