# Install SportsPicker for your club

This guide takes you from nothing to a working SportsPicker site for your club. You need a laptop and a credit card. You do not need to be a developer.

When you finish, you will have one web address (a URL) that you can send to your members. An invented league is already running on it, so members can sign up and make picks right away.

The whole install takes about 30 minutes of your time. Most of that is waiting.

## 1. What it costs

SportsPicker runs in your own Amazon Web Services (AWS) account. AWS bills you directly.

- Expect about $16.50 to $17.50 a month.
- Most of that, about $14 to $15, is the database. Three stored secrets cost 3 x $0.40. Each of the remaining items is under $1.
- The install sets a $20 monthly budget. AWS emails you when you reach 80% and 100% of it.
- Deleting the install stops the charges. Section 10 shows how.

The demo season (the invented league) runs for about 2 weeks from the day you install.

## 2. Create an AWS account

Skip this section if your club already has an AWS account.

1. Go to <https://aws.amazon.com> and click **Create an AWS Account**.
2. Enter an email address and a name for the account. Use a club email address that more than one person can read, so the account does not depend on one person.
<!-- screenshot: the AWS sign-up page with the email and account name fields -->
3. Check your email for a verification code and type it in.
4. Choose a root password. This is the master password for the account. Store it in a password manager.
5. Choose **Personal** or **Business** when asked. Either works. Fill in your name, address and phone number.
6. Enter your credit card. AWS may charge a small amount to check the card.
7. Verify your phone number when asked.
8. Choose the **Basic support - Free** plan.
<!-- screenshot: the support plan page with Basic support - Free selected -->
9. Wait for the "Your account is ready" email. This can take a few minutes. Sometimes it takes a few hours.
10. Sign in at <https://console.aws.amazon.com> as the **Root user**, with the email and password from step 2 and step 4.
<!-- screenshot: the AWS sign-in page with Root user selected -->

### Turn on a second sign-in check

AWS strongly suggests this. Click your account name at the top right, then **Security credentials**, then **Assign MFA device**. Follow the prompts to link an authenticator app on your phone.

### Pick your region and stay in it

AWS runs in many places around the world. Each place is a **region**. New AWS accounts are locked to one region, and you must install in that one.

1. Look at the top right of the console, next to your account name. It shows a region name such as "N. Virginia" or "Ohio".
2. Click the name. A list of regions opens.
3. Pick the region your account is set to. If you have not changed anything, the one already showing is the right one.
<!-- screenshot: the region menu at the top right of the AWS console, with the current region highlighted -->

Stay in that region for the rest of this guide. Every time you come back to the console later, check the region first. If your site "disappears", you are almost always looking at the wrong region.

## 3. Click Launch SportsPicker

Make sure you are signed in to the AWS console and the right region is showing. Then click this button:

[![Launch SportsPicker](https://s3.amazonaws.com/cloudformation-examples/cloudformation-launch-stack.png)](https://console.aws.amazon.com/cloudformation/home#/stacks/quickcreate?templateURL=https://sportspicker-releases.s3.us-east-2.amazonaws.com/v0.1.0/deployer.template.json&stackName=SportsPicker-deployer)

The AWS CloudFormation page opens. CloudFormation is the AWS tool that builds everything for you from a ready-made recipe. The recipe is already loaded.
<!-- screenshot: the CloudFormation quick-create page with the template loaded and the stack name SportsPicker-deployer -->

Leave the stack name as `SportsPicker-deployer`.

## 4. Fill in the fields

You will see a short form. Five fields matter.

| Field | What to type |
| --- | --- |
| **ClubName** | Your club's name, for example `Riverside Picks Club`, up to 64 characters. Members see this on the site. |
| **AdminUsername** | The name you will use to log in as administrator. |
| **AdminPassword** | A password for the administrator. It must be at least 12 characters. Store it in a password manager. |
| **BudgetEmail** | The email address that receives the cost alerts. Use one you read. |
| **SportsDataKeySecret** | Type the word `none` exactly as shown, in lowercase. |

About `SportsDataKeySecret`: this is where a paid sports-data key would go, and you do not have one yet. Typing `none` tells the install to run on the invented league only.

<!-- screenshot: the filled-in parameters form with the five fields completed (password hidden) -->

Scroll to the bottom. Under **Capabilities**, tick the box that says:

> I acknowledge that AWS CloudFormation might create IAM resources.

IAM is how AWS controls who and what is allowed to do things. The install creates the permissions that SportsPicker needs to run. Without this tick, the install cannot start.

<!-- screenshot: the Capabilities section with the IAM acknowledgement box ticked -->

Click **Create stack**.

## 5. Wait about 20 minutes

The next page shows your stack with the status **CREATE_IN_PROGRESS**. This is normal.

- It takes about 15 to 20 minutes. The database takes the longest.
- Click the **Events** tab to watch each piece being built. New lines appear as it goes.
- Click the refresh icon now and then. The page does not always update itself.
- You can close the tab and come back. Your stack stays in the CloudFormation list.

<!-- screenshot: the stack page showing CREATE_IN_PROGRESS with the Events tab open -->

You may see a second stack appear, named `CDKToolkit`. That is an AWS helper stack the install needs. Leave it alone.

When the status changes to **CREATE_COMPLETE**, the install is done.

If the status turns red and says anything with FAILED or ROLLBACK, go to section 12.

## 6. Find your site address

1. Click your `SportsPicker-deployer` stack.
2. Click the **Outputs** tab.
3. Find the row named **SiteUrl**. The value is your site's address.

<!-- screenshot: the Outputs tab with the SiteUrl row highlighted -->

Bookmark it. This is the address you send to your members.

To log in as administrator, add `admin/` to the end of the address. For example, if the SiteUrl is `https://example.cloudfront.net/`, the admin page is `https://example.cloudfront.net/admin/`. Use the AdminUsername and AdminPassword you chose in section 4.

<!-- screenshot: the admin login page -->

## 7. Update to a new version

When we publish a new version, the admin dashboard shows a notice: "Update available", with the new version number in a small badge beside it (for example `v1.2.0`), a **Release notes** link and an **Update** button.

1. Log in at your SiteUrl plus `admin/`.
2. Click **Update**. The AWS CloudFormation page for your stack opens. If AWS asks you to sign in, do so.
<!-- screenshot: the admin dashboard showing the "Update available" notice with its version badge and Update button -->
3. Choose **Update stack**, then **Make a direct update**.
4. Choose **Replace existing template**. The new template address is already filled in. Click **Next**.
5. On the parameters page, keep every value as it is. Do not change anything.
6. Tick the IAM acknowledgement box again. It resets each time.
7. Click **Next**, then **Next**, then **Submit**.
<!-- screenshot: the Update stack parameters page with existing values kept -->

Wait for the status to read **UPDATE_COMPLETE**. If an update fails, your old version keeps serving until it is fixed.

Some versions can only be reached from a recent one. If the update fails with a message that it needs an earlier version first (for example "needs v1.2.0 or later"), nothing was changed: update to that version first, then to the new one.

Note on the password: do not type a new AdminPassword on the update page. A new password typed there is stored in the install's secret but never used, and the next update overwrites it. Your admin password stays the one you chose at install. This version has no way to change it after install. Choose it carefully and keep it in a password manager.

## 8. Remove the demo league

The invented league is there so your members have something to play with. When your real season is ready, remove it.

This runs through a small helper program in your AWS account, called the ops function. You start it from the console.

> Note for the guide author: the exact console clicks below are written from understanding and are to be confirmed against the real console in Task 12.

First, find the function. Follow steps 1 and 2 below, then type `sportspicker-` in the search box above the function list. Your function is the one that ends in `-ops`. Its name is `sportspicker-<club-slug>-ops`.

For the curious, the club slug is built from your club name like this: it is made lowercase, each run of characters that is not a plain letter or digit (accented letters included) becomes a single `-`, dashes at the start and end are removed, and the result is cut to 30 characters, then any dash left at the end is removed again. For example, `Riverside Picks Club` becomes `riverside-picks-club`. You will need the slug for the event text below, and the function name shows it between `sportspicker-` and `-ops`.

Then:

1. In the console search bar at the top, type **Lambda** and open it.
2. Check your region is still the right one.
3. In the list, click the function named `sportspicker-<club-slug>-ops`.
<!-- screenshot: the Lambda function list with the ops function highlighted -->
4. Click the **Test** tab.
5. Under **Test event**, choose **Create new event**. Give it any name, such as `preview`.
6. In the **Event JSON** box, replace everything with the text below, using your own slug:

   ```json
   {"argv": ["purge-demo", "--club", "riverside-picks-club"]}
   ```
7. Click **Test**. Open the **Details** under the result. This is a preview. It lists what would be removed and deletes nothing. A green "Succeeded" banner alone does not mean the command worked. Look for `"exit_code": 0` in the result and read the `stdout` text.
<!-- screenshot: the Test tab with the preview event JSON and the result panel -->
8. If the list looks right, change the event JSON to add `"--yes"`:

   ```json
   {"argv": ["purge-demo", "--club", "riverside-picks-club", "--yes"]}
   ```
9. Click **Test** again. This time the demo data is deleted. It cannot be undone. Check for `"exit_code": 0` and read `stdout` again.

## 9. Take a snapshot before you delete (optional)

When you delete the install in section 10, the database is deleted for good. This includes your members, picks and scores. If you might want that data later, save a snapshot first.

1. In the console search bar, type **RDS** and open it.
2. Click **Databases** in the left menu.
3. Click the SportsPicker database.
4. Click **Actions**, then **Take snapshot**.
5. Give it a name such as `sportspicker-final`. Click **Take snapshot**.
<!-- screenshot: the RDS Actions menu with Take snapshot highlighted -->
6. Click **Snapshots** in the left menu and wait until the status is **Available**.

A snapshot you keep costs a small amount each month. Delete it from the same Snapshots page when you no longer need it.

## 10. Delete SportsPicker

This stops the monthly charges.

1. Open the CloudFormation console and check your region.
2. Click the `SportsPicker-deployer` stack.
3. Click **Delete**, then confirm.
<!-- screenshot: the stack page with Delete button and confirmation dialog -->
4. Wait for the stack to disappear from the list. This takes several minutes.

This removes everything the install created, including the database. Your site stops working.

About `CDKToolkit`, the AWS helper stack from section 5: it is removed only if your install created it. If a second club installed SportsPicker later in the same AWS account, that club uses the same helper. Deleting the first install then removes the helper the second one needs. If you share an AWS account with another install, do not delete the first one unless you have checked with the others first.

## 11. Later

These are optional and are for when your club is ready.

**A paid sports-data key (SportsRadar).** The install wires the key up for scheduled imports of real game data. Those imports also need the sport to be configured, and this version of the install does not do that step, so a key alone does not change what your site shows yet.

1. Buy a key from SportsRadar.
2. In the console, open **Secrets Manager** and click **Store a new secret**. Choose **Other type of secret**, then open the **Plaintext** tab and paste only the key, with nothing else. Do not use the key/value tab, which stores the key in a form the site cannot read. Pick any name you like, for example `my-sportsradar-key`.
3. Open your stack in CloudFormation and choose **Update stack**, **Replace existing template** as in section 7.
4. Set `SportsDataKeySecret` to the secret's name (`my-sportsradar-key` in the example). Keep every other value.
5. Tick the IAM box and submit.

A stored secret costs about $0.40 a month.

**A custom domain** (such as `picks.yourclub.org`). This version does not support one. Your site lives at the SiteUrl address. We will say so here when that changes.

## 12. If it fails

Open the CloudFormation console and click your stack. Click the **Events** tab. The failed line is red. The **Status reason** column says what went wrong.

If the failure came from the build step, the status reason includes a link to the **build log**. Click it to see the full details. Send it to us if you ask for help.

The two most common causes on brand-new AWS accounts are below. Both are fixed by asking AWS support, which is free.

### "Your account must be verified" (CloudFront)

AWS holds back some services on new accounts until it checks them.

1. In the console, click the **?** icon at the top right, then **Support Center**, then **Create case**.
2. Choose **Account and billing**.
3. Choose the service **Account**, and the category **Other account issues**.
4. In the description, paste:

   > My CloudFormation stack failed with the error "Your account must be verified before you can add new CloudFront resources". Please verify my account so I can create a CloudFront distribution. This is for a small website for a sports club.

5. Submit the case. A reply usually arrives within a day. Once AWS confirms, go to section 3 and start again.

### CodeBuild limit of 0

AWS sometimes starts new accounts with no allowed build jobs.

1. Open **Support Center** as above and click **Create case**.
2. Choose **Service limit increase**, and the service **CodeBuild**.
3. In the description, paste:

   > My CloudFormation stack failed because AWS CodeBuild reports a concurrent build limit of 0 in my account. Please raise the concurrent builds limit to at least 1 in my region. This is for deploying a small website for a sports club.

4. Submit the case. When AWS confirms, start again from section 3.

### Stuck in UPDATE_ROLLBACK_FAILED

An update went wrong and the undo also failed. The stack is locked.

1. Open the stack in CloudFormation.
2. Click **Stack actions**, then **Continue update rollback**.
3. Confirm. Wait a few minutes. The stack returns to a normal state, and you can try the update again.

### A failed first install

If your first install ends in **ROLLBACK_COMPLETE**, the site is not running and the main charges have stopped. A small AWS helper stack named `CDKToolkit` may stay behind at near-zero cost; leave it in place. It costs almost nothing, and the next install reuses it. Delete the failed stack (select it, then **Delete**), fix the cause above, then click the Launch button again.
