# Google Indexing API Batch Submitter

A lightweight web application built with Node.js and Express to batch submit URLs to the **Google Indexing API** for updating (`URL_UPDATED`) or deleting (`URL_DELETED`) requests.

---

## 1. Prerequisites: `service_account.json`

The Google Indexing API requires authentication via a Google Cloud Service Account with proper Search Console permissions.

### How to Get Your `service_account.json`:
1. **Create a Google Cloud Project**:
   - Go to [Google Cloud Console](https://console.cloud.google.com/).
   - Create a new project (or select an existing one).

2. **Enable the Google Indexing API**:
   - Go to **APIs & Services > Library**.
   - Search for **Web Search Indexing API** (or **Indexing API**) and click **Enable**.

3. **Create a Service Account**:
   - Navigate to **IAM & Admin > Service Accounts**.
   - Click **Create Service Account**, enter a name (e.g. `google-indexing-bot`), and click **Done**.

4. **Generate and Download the Key**:
   - Click on the created service account email.
   - Go to the **Keys** tab, click **Add Key > Create new key**.
   - Choose **JSON** format and click **Create**.
   - A `.json` file will download to your machine.
   - Rename that downloaded file to `service_account.json` and place it directly into the root folder of this project (`indexing-test/service_account.json`).

5. **Grant Ownership in Google Search Console (Crucial)**:
   - Copy the service account's client email address (found inside `service_account.json` under `"client_email"` or in Cloud Console, usually ends with `@...iam.gserviceaccount.com`).
   - Go to [Google Search Console](https://search.google.com/search-console).
   - Select your verified property (domain or URL prefix).
   - Go to **Settings > Users and permissions > Add user**.
   - Paste the service account email and set the permission to **Owner**.
   > **Note:** Without **Owner** permission in Search Console, Google will return a `403 Permission Denied` error when notifying URLs.

---

## 2. Installation & Setup

1. **Install dependencies**:
   ```bash
   npm install
   ```

2. **Verify Project Structure**:
   Make sure `service_account.json` is located in the root directory:
   ```text
   indexing-test/
   ├── index.js
   ├── package.json
   ├── service_account.json    <-- Required Google credentials file
   └── public/
       └── index.html
   ```

---

## 3. How to Run & Use the Application

### Start the Server:
```bash
npm start
```
Or for development with automatic reloads:
```bash
npm run dev
```

### Access the Web Interface:
1. Open your browser and navigate to:
   ```text
   http://localhost:3000
   ```
2. **Choose the Action**:
   - **Update URL (`URL_UPDATED`)**: Informs Google that a new URL has been added or existing content has been updated.
   - **Delete URL (`URL_DELETED`)**: Informs Google that a page has been removed from the site (returns 404/410).
3. **Input URLs**:
   - Paste your list of URLs into the text area with **one URL per line**.
   - *Example:*
     ```text
     https://yourdomain.com/page-1
     https://yourdomain.com/page-2
     https://yourdomain.com/job/job-title-123
     ```
4. **Submit**:
   - Click **Send Batch Request**.
   - Watch the live progress bar, counters, and detailed execution log showing Google's response for every single URL.

---

## 4. API Limits & Important Notes
- **Daily Quotas**: By default, Google Indexing API allows **200 requests per day** per project (can be increased via quota requests in Google Cloud Console).
- **Intended Use**: Google officially recommends the Indexing API primarily for pages with `JobPosting` or `BroadcastEvent` embedded structured data, but it can submit updates/deletions for any verified URLs under your property.
- **Security**: Never commit `service_account.json` to public repositories. It is already added to `.gitignore`.

