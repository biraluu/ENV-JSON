
This repo has the one line code to either convert .env file into a json file or a json file into env

🕹️ More Cheatcodes Coming Soon

Stay updated — and if you found this useful, drop a ⭐ on the repo 😄

# 🔄 ENV ↔ JSON Conversion Guide

A clean, simple, and **developer‑friendly** guide for converting between `.env` files and JSON using shell commands. Perfect for teams, DevOps workflows, or API config management.

---

## 📦 1. Install `jq`
`jq` is required for JSON → ENV conversion.

for more: https://jqlang.org/download/

### **Linux**
```sh
sudo apt install jq
```
### **Mac**
```sh
brew install jq
```
### **Windows (Chocolatey)**
```sh
choco install jq
```

---

## 🌱 2. Convert `.env` → JSON

### **Steps**
1. **Navigate to your `.env` file directory:**
   ```sh
   cd /path/to/your/env
   ```
2. Ensure your `.env` contains valid key-value pairs.
3. Run the following command (unchanged from your original):

```sh
cat .env | grep -v '^#' | grep -v '^\s*$' | awk -F= '{gsub(/"/, "\\\"", $2); print "\"" $1 "\": \"" $2 "\","}' | sed '$ s/,$//' | awk 'BEGIN {print "{"} {print} END {print "}"}'
```
# IMPORTANT⚠️: This command doesnot works directly in windows, you have to be in Git Bash, WSL, etc. terminal

If you are doing this in **Powershell**, here is the seperate command. Rest is same !

```sh
(Get-Content .env |
    Where-Object { $_ -notmatch '^\s*$' -and $_ -notmatch '^\s*#' } |
    ForEach-Object {
        $parts = $_ -split '=', 2
        '"' + $parts[0] + '": "' + ($parts[1].Replace('"', '\"')) + '"'
    }) -join ",`n" | 
    ForEach-Object { "{`n$_`n}" }
```

### **Output**
The resulting JSON will be **printed directly in the terminal**.

---

## 🛠️ 3. Convert JSON → `.env`

### **Steps**
1. Save your JSON as a file (e.g., `input.json`).
2. **Navigate to the file directory:**
   ```sh
   cd /path/to/input.json
   ```
3. Run the conversion command:

```sh
jq -r 'to_entries[] | "\(.key)=\(.value)"' input.json
```
# IMPORTANT⚠️: This command doesnot works directly in windows, you have to be in Git Bash, WSL, etc. terminal

If you are doing this in **Powershell**, here is the seperate command. Rest is same !

```sh
(Get-Content input.json | ConvertFrom-Json).psobject.Properties |
    ForEach-Object { "$($_.Name)=$($_.Value)" }
```

### **Output**
The `.env` formatted variables will be **displayed directly in your terminal**.

Copy & paste the output into your `.env` file if needed.

---

## ✨ You're All Set!
Easily switch between `.env` and JSON formats without installing heavy tools or writing scripts. Ideal for automation, CI/CD pipelines, or configuration cleanup.

---

**Happy hacking! 🚀**
**Biraluu**

