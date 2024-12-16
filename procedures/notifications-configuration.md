# Notifications Configuration

Superstream is designed to deliver a seamless experience, using lightweight notifications to keep you and your team informed about actions across your clusters.

To configure notifications, please head to the [**System** **Settings**](https://app.superstream.ai/system-settings) page.

<figure><img src="../.gitbook/assets/Screenshot 2024-12-16 at 20.46.49.png" alt=""><figcaption></figcaption></figure>

### For Slack notifications

1. **Log In to Slack**
   * Navigate to Slack and log in to your workspace using your credentials.
2. **Access Slack API Management**
   * Visit the Slack API page.
   * Click on the **"Your Apps"** button in the top-right corner.
3. **Create a New App**
   * On the **Your Apps** page, click **"Create an App"**.
   * Select **"From Scratch"**.
   * Provide a name for your app and choose the workspace where you want to send messages.
   * Click **"Create App"**.
4. **Enable Incoming Webhooks**
   * In the app settings, locate the **"Features"** section on the left-hand menu and click on **"Incoming Webhooks"**.
   * Toggle the **Activate Incoming Webhooks** option to **On**.
5. **Create a Webhook URL**
   * Scroll down to the **Webhook URLs for Your Workspace** section.
   * Click **"Add New Webhook to Workspace"**.
   * Choose the Slack channel where you want the messages to be sent.
   * Click **"Allow"** to grant the necessary permissions.
6. **Copy the Webhook URL**
   * Once created, you’ll see the webhook URL under the **Webhook URLs for Your Workspace** section.
   * Copy the URL to use it in your application or script.
7. **In Superstream Console**
   1. Toggle the REST channel
   2. Paste the URL
   3. Add the following header `Content-type: application/json`
