# GitHub Repository User Access Script

This shell script allows you to list all users who have **read access** to a specific GitHub repository using the GitHub REST API.

---

## 📌 Features
- Uses GitHub REST API to fetch repository collaborators.
- Filters users who have **read (pull)** permissions.
- Displays the list of users with read access.
- Requires GitHub **Personal Access Token (PAT)** for authentication.

---

## 📝 Prerequisites
1. **GitHub Personal Access Token**  
   - Generate from: [GitHub Settings → Developer settings → Personal access tokens](https://github.com/settings/tokens)  
   - Make sure the token has `repo` scope permissions.

2. **Dependencies**
   - `curl` → for API requests  
   - `jq` → for JSON parsing  

   Install `jq` (if not installed):
   ```bash
   sudo apt-get install jq -y
   ```

---

## ⚙️ Script Setup
Save the script as `list_users.sh` and make it executable:
```bash
chmod +x list_users.sh
```

Update the following environment variables in your shell:
```bash
export username="your_github_username"
export token="your_personal_access_token"
```

---

## 🚀 Usage
Run the script with repository **owner** and **name** as arguments:
```bash
./list_users.sh <repo_owner> <repo_name>
```

### Example:
```bash
./list_users.sh octocat hello-world
```

---

## 📂 Script Breakdown
### 1. GitHub API Endpoint
```bash
API_URL="https://api.github.com"
```
Defines the base URL for GitHub API requests.

### 2. Authentication
```bash
USERNAME=$username
TOKEN=$token
```
Takes username and personal access token from environment variables.

### 3. API Request Function
```bash
function github_api_get {
    curl -s -u "${USERNAME}:${TOKEN}" "$API_URL/$1"
}
```
Makes authenticated `GET` requests to the GitHub API.

### 4. Fetch Users with Read Access
```bash
function list_users_with_read_access {
    collaborators="$(github_api_get "repos/${REPO_OWNER}/${REPO_NAME}/collaborators"     | jq -r '.[] | select(.permissions.pull == true) | .login')"
}
```
Filters collaborators with `pull == true` (read access).

### 5. Output
Displays the list of users or a message if none are found.

---

## ✅ Sample Output
```bash
Listing users with read access to octocat/hello-world...
Users with read access to octocat/hello-world:
john-doe
jane-smith
```

---

## 🔒 Security Note
- **Do not hardcode** your token inside the script.  
- Always use environment variables for storing sensitive credentials.  
- Consider using a `.env` file (and add it to `.gitignore`) for better security.

---

## 📖 References
- [GitHub REST API - Collaborators](https://docs.github.com/en/rest/collaborators/collaborators?apiVersion=2022-11-28)
