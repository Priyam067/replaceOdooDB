# replacedb 🐘🗃️

A simple shell script to quickly replace an Odoo PostgreSQL database (along with its filestore) using a `.zip` backup. Designed to save time for Odoo developers working in test or development environments.

## 🔧 Features

- Drop an existing PostgreSQL database
- Restore a new database from a `dump.sql` file inside a `.zip`
- Replace the corresponding Odoo filestore
- Perform post-import cleanup (disable cron jobs, reset login credentials, etc.)
- Fully automated with minimal input

---

## 📥 Installation

1. Download the script:
   ```bash
   wget https://raw.githubusercontent.com/yourusername/replacedb/main/replacedb
   ```

2. Copy the script to your system path:

   ```bash
   sudo cp replacedb /usr/bin/
   ```

3. Make it executable:

   ```bash
   sudo chmod +x /usr/bin/replacedb
   ```

Now you can run `replacedb` from anywhere on your system.

---

## ▶️ Usage

```bash
replacedb <db_name> <backup.zip>
```

* `<db_name>`: Name of the PostgreSQL database you want to replace.
* `<backup.zip>`: Path to the `.zip` file that contains:

  * `dump.sql` – SQL dump of the database.
  * `filestore/` – The Odoo filestore directory.

**Example:**

```bash
replacedb my_test_db test_backup.zip
```

---

## 🧠 What the Script Does

1. **Extracts the zip file** to a temporary directory.
2. **Drops** the target PostgreSQL database (if it exists).
3. **Removes** the associated Odoo filestore directory (`~/.local/share/Odoo/filestore/<db_name>`).
4. **Creates a new PostgreSQL database** with the same name.
5. **Imports the SQL dump** (`dump.sql`) into the new database.
6. **Copies the filestore** into the appropriate Odoo location.
7. **Runs custom SQL cleanup** on the new database:

   * Deletes mail servers and fetchmail configs
   * Disables all cron jobs
   * Resets login for user with ID=2 to `admin`
8. **Cleans up** temporary files.

---

## ⚠️ Notes

* This script is intended for **development and testing** environments only.
* Use with caution — it will permanently delete the existing database.
* Ensure the `.zip` file contains both `dump.sql` and `filestore/`.

---

## 📂 File Structure Example

The `backup.zip` should contain:

```
backup.zip
├── dump.sql
└── filestore/
    ├── 1a
    ├── 2b
    └── ...
```

---

## 🙌 Contributions

Feel free to open issues or pull requests to improve this tool.
