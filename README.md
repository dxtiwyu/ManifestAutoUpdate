# ManifestAutoUpdate - A Steam Manifest Cache Repository

## Project Introduction
* Uses `Actions` to automatically crawl `Steam` game manifests

## How to Deploy
1. Fork this repository
2. Initialization
   * The first time the program runs, it will perform initialization operations
   * Initialization will generate a `data` branch, using `worktree` to check out to the `data` directory
   * Generate a key to encrypt `users.json`
   * The key generation path is located at: `data/KEY`
   * At the same time, the program will output the hexadecimal string of the key, which needs to be stored in the GitHub repository secret, with the name saved as `KEY`
   * Open your repository -> `Settings` -> `Secrets` -> `Actions` -> `New repository secret`
   * Or add `/settings/secrets/actions/new` after your repository address
   * Add account passwords to `data/users.json`:
   * If you need to use `Actions` afterwards, you need to push it to the remote repository
   * Run the program again, and it will automatically push to the `data` branch when the program ends
   * Manual push steps are as follows:
     1. `cd data`: Switch to the `data` directory
     2. `git add -u`: Add modified content
     3. `git commit -m "update"`: Commit changes
     4. `git push origin data`: Push to the remote `data` branch

## Operation Process
1. Actions initialization and operation
   * Configure `workflow` read and write permissions: Repository -> *`Settings`* -> *`Actions`* -> *`General`* -> *`Workflow permissions`* -> *`Read and write permissions`*
   * Open `Actions` in the repository, select the corresponding `Workflow`, click `Run workflow`, select the parameters and run
   * *`INIT`*: Initialization
   * `users`: Accounts, multiple can be specified, separated by commas
   * *`password`*: Passwords, multiple can be specified, separated by commas
   * `ssfn`: [ssfn](https://ssfnbox.com/), need to upload this file to the `credential_location` directory in advance, multiple can be specified, separated by commas
   * *`2fa`*: [shared_secret](https://zhuanlan.zhihu.com/p/28257212), multiple can be specified, separated by commas
   * `update`: Whether to update the account
   * *`update_users`*: Accounts that need to be updated
   * After the first initialization, remember to save the key to the repository secret, otherwise it will report an error due to the lack of a key on the next run, and remember to delete the results of this `Workflow` run to prevent key leakage, or use local initialization for more security
   * *`Trigger Workflow`*: Trigger workflow
   * Process `CI` -> `PR` -> `MERGE`
   * *`PR`*: Automatically PR manifests to the specified repository
   * Since GitHub prohibits Actions from recursively creating PRs, you need to create a personal access token and save it to the repository secret token
