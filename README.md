# Broadcom (AVGO) - Entity Common Stock Shares Outstanding

This is a lightweight, frontend-only web application that fetches and displays the maximum and minimum number of outstanding shares for a given company from the SEC's EDGAR API. It specifically focuses on data reported after the fiscal year 2020.

## Features

*   Fetches company information (entity name, shares outstanding) from the SEC EDGAR API.
*   Processes and filters data to find the highest and lowest share counts for fiscal years greater than 2020.
*   Presents the data in a visually appealing HTML interface.
*   Supports dynamic loading of data for different companies by providing a CIK via the URL query parameter (e.g., `index.html?CIK=0001018724`).
*   Updates the UI dynamically without page reloads when a new CIK is provided.

## Setup

This application is designed to be deployed on GitHub Pages. No local setup or build process is required. Simply clone the repository and serve the `index.html` file.

1.  **Clone the repository:**
    ```bash
    git clone <repository_url>
    cd <repository_directory>
    ```
2.  **Deploy to GitHub Pages:**
    Follow the GitHub Pages documentation to deploy the contents of this repository to a GitHub Pages site. Ensure the `main` (or `gh-pages`) branch is configured for deployment.

## Usage

1.  **Access the application:**
    Open the deployed `index.html` URL in your web browser. By default, it will display data for Broadcom (CIK: 0001730168).

    `https://yourusername.github.io/your-repo-name/`

2.  **View data for another company:**
    To view data for a different company, append the `?CIK=` query parameter to the URL with the 10-digit CIK of the desired company. For example, to fetch data for a company with CIK `0001018724`:

    `https://yourusername.github.io/your-repo-name/?CIK=0001018724`

    The page will automatically update to show the information for the specified company.

## Code Explanation

*   **`index.html`**:
    *   Contains the entire user interface and the JavaScript logic.
    *   Uses basic HTML and CSS for presentation.
    *   The `<head>` includes a `<title>` that will be dynamically updated.
    *   The `<body>` contains a `<h1>` tag (with `id="share-entity-name"`) for the company name and `div` elements (with specific IDs like `share-max-value`, `share-max-fy`, etc.) to display the fetched share data.
    *   Includes placeholders for loading messages (`#loading-message`) and error messages (`#error-message`).
    *   The embedded `<script>` block handles all application logic.

*   **JavaScript (within `index.html`)**:
    *   **`SEC_API_BASE_URL`**: Constant for the SEC API endpoint.
    *   **`DEFAULT_CIK`**: The default CIK for Broadcom.
    *   **`fetchData(cik)`**: Asynchronously fetches data from the SEC API using the provided CIK. It sets a descriptive `User-Agent` as recommended by the SEC. Includes error handling for HTTP responses.
    *   **`processData(data)`**: Takes the raw JSON response from the SEC API.
        *   Extracts `entityName`.
        *   Filters the `shares` array to include only entries with a fiscal year (`fy`) greater than "2020" and a numeric value (`val`).
        *   Finds the maximum and minimum `val` among the filtered entries, breaking ties arbitrarily.
        *   Returns a structured object containing `entityName`, `max` (with `val` and `fy`), and `min` (with `val` and `fy`).
    *   **`renderUI(shareData)`**: Updates the HTML elements of the page with the processed `shareData`. It sets the page title, H1 content, and the values/years for max and min shares. It also hides loading messages and displays error messages if necessary.
    *   **`initializeApp()`**:
        *   Retrieves the CIK from the URL query parameters or uses the `DEFAULT_CIK`.
        *   Performs basic validation on the CIK.
        *   Calls `fetchData()` and `processData()`.
        *   Calls `renderUI()` to display the results.
        *   Includes a `try...catch` block to handle any errors during the fetching or processing stages and calls `displayError()`.
    *   **`displayError(message)`**: Shows an error message to the user and updates the UI accordingly.
    *   **Event Listeners**: An event listener for `popstate` is included to re-initialize the app if the user navigates using the browser's back/forward buttons after a CIK change.
    *   **Initial Load**: `initializeApp()` is called once when the page loads to fetch and display the initial data.

*   **`LICENSE`**:
    *   Contains the full text of the MIT License.

*   **`uid.txt`**:
    *   Contains a base64 encoded string as provided in the attachments. This file is included as-is.

## SEC API Guidance

This application adheres to SEC guidance regarding User-Agent strings for API access. The `User-Agent` is set to `AVGO AVGO (broadcom.com) AVGO-Share-Fetcher/1.0 (mailto:your.email@example.com)`. **Please replace `your.email@example.com` with your actual email address before deployment.**