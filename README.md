# CloudNotes Web Tier Security Brief

CloudNotes runs in one VPC network (`cloudnotes-vpc`) with three tiers. Each tier is marked by a network tag on its VM:

- `web`: public website, serves HTTP and HTTPS
- `app`: CloudNotes application, listens on port 5000
- `db`: PostgreSQL database, listens on port 5432

## Intended secure design

1. The internet reaches only the `web` tier, on ports 80 and 443.
2. Port 5000 on the `app` tier accepts traffic only from the `web` tier.
3. Port 5432 on the `db` tier accepts traffic only from the `app` tier. It is never open to the internet.
4. The CloudNotes app service account (`cloudnotes-app`) has least privilege. It only reads attachment files from the attachments bucket. It never writes, deletes, or manages anything.
5. The attachments bucket (`cloudnotes-attachments`) is private. It has no public members and uniform bucket-level access is on.
6. A lifecycle rule deletes attachments 30 days after they are created (action: Delete, age: 30 days).

## Files

| File | What it holds |
|---|---|
| `firewall-rules.txt` | VPC firewall rules for `cloudnotes-vpc` |
| `iam-binding.txt` | Role binding for the `cloudnotes-app` service account |
| `bucket-settings.txt` | Settings and members of `cloudnotes-attachments` |

## Rules for students

- Edit only the three files above. Do not edit this README.
- Keep every rule name, bucket name, service account name, and port unchanged, except where a fix requires a change.
- No paid tool, no cloud account, no real credentials. Edit the text files in the browser.
