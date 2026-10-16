# HelixReplicates Live Feed

A looping conference display for the five supplied pilot replication cards. The page cycles through the studies, ranks them by estimated power for each primary claim, and refreshes feed data every 20 seconds.

## Publish with GitHub Pages

The included GitHub Actions workflow publishes the site whenever the page, feed data, brand assets, or workflow changes on the repository's main branch.

1. Create a GitHub repository and add the files in this folder to its root.
2. In the repository settings, open **Pages** and set the publishing source to **GitHub Actions**.
3. Push the files to main. The Actions workflow will publish the page and show its URL in the workflow run.

GitHub Pages sites are publicly reachable when enabled. Check the repository's visibility and your organization's Pages policy before publishing.

## Run locally

On macOS, double-click **Start Live Feed.command**. A browser window opens and the feed begins rotating through the papers. Use the **Full screen** button for a conference display. Close the Terminal window to stop the local feed.

If macOS blocks the file on first open, right-click it and choose **Open**. Opening index.html directly also shows the embedded starting snapshot, but it cannot receive updates from papers.json without an HTTP server.

## Add a paper

Add a study object to the studies array in papers.json. Once that file is committed and deployed, open displays pick up the update on their next 20-second check without a page reload.

Each entry should include:

- id: unique short identifier
- title, condition, journal, year, pmid, doi
- kind: gwas or burden
- statusLabel, claim, summary, cases, controls
- powerScore: decimal from 0 to 1
- powerBasis: short explanation of the source metric
- For GWAS, metrics: testedLoci, directionCount, replicatedCount, expectedHits, and effectSlope
- secondarySignals and caveat
- For gene-burden studies, primaryFinding and secondarySignals

The leaderboard uses the report's estimated power for the primary claim. For GWAS entries, powerScore is expected corrected-significant findings divided by tested loci. For gene-burden entries, it is the report's power estimate for the primary claim. Add only aggregate findings suitable for display; preserve suppression of small carrier counts.

## Paper submission preview

The QR code opens the on-page intake preview. The form validates its required fields in the browser but does not upload, store, or send names, email addresses, or selected files. It is a visual prototype only.

To accept real submissions, connect the form to a Helix-managed intake service. A typical flow would request a short-lived upload URL from an authenticated AWS API, upload the paper to a private encrypted S3 bucket, and validate metadata and file type in a Lambda function. Only then should the service send an alert to a private Slack destination, with the file attached or linked under access controls. Keep Slack credentials in AWS Secrets Manager; add file-size/type checks, abuse protection, a retention period, and clear consent language before enabling public intake. Do not collect participant-level or other sensitive research data through this form.

## Interpretation notes

Power describes the ability to detect the paper's primary claim; it is not a replication score. The display keeps power, direction agreement, and threshold-level findings separate. The source reports label their verdicts as pending and note that the verdict rule was set after results were seen.
