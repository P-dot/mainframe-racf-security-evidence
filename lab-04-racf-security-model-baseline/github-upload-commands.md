# GitHub upload commands

From Git Bash or PowerShell, copy the lab folder into your repository and upload it:

```bash
cd /c/Carrera_Ciberseguridad/06_Portfolio_GitHub/mainframe-racf-security-evidence

mkdir -p lab-04-racf-security-model-baseline
cp -r /path/to/lab-04-racf-security-model-baseline/* lab-04-racf-security-model-baseline/

git status
git add lab-04-racf-security-model-baseline
git commit -m "Add RACF security model baseline lab"
git push
```

If the repository path is different, adjust the `cd` command.

## Suggested commit message

```text
Add RACF security model baseline lab
```
