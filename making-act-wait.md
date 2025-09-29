# Making `act` Wait for You

## What is `act`?

`act` is an open-source tool that lets you run GitHub Actions workflows locally, mimicking the GitHub Actions environment. It’s a lifesaver for developers who want to test their workflows without pushing to a remote repository. By using `act`, you can catch issues early, saving time and avoiding costly mistakes in CI/CD pipelines.

## The Problem: `act` Runs Non-Stop

When you use `act` to test your GitHub Actions workflows locally, it barrels through the entire workflow as long as nothing fails. This can be an issue if your workflow includes steps that require manual intervention, like approving a deployment to a production environment. Without a way to pause and wait for user input, `act` just keeps going, which doesn’t always mirror real-world scenarios.

## The Solution: Forcing `act` to Pause

In a GitHub issue comment (see here), I shared a practical workaround to make `act` pause and wait for user input before proceeding. The trick is to add a step in your workflow that checks for the existence of a particular file that you will create when you are sure you want the workflow to continue. Here’s how you can implement it:

```yaml
name: Wait for User Input
on: [push]
jobs:
  wait-for-user:
    runs-on: ubuntu-latest
    steps:
      - name: Local approval gate (only when running with act)
        if: env.ACT == 'true'
        shell: bash
        run: |
          rm -f /tmp/approved
          echo "Waiting for local approval..."
          echo "In another terminal: find act container and 'docker exec' to touch /tmp/approved"
          cid=$(docker ps --format '{{.ID}}\t{{.Names}}' | awk '$2 ~ /^act-/ {print $1; exit}')
          echo "Example:"
          echo "docker exec -it \"$cid\" bash -lc 'touch /tmp/approved'"
          while [ ! -f /tmp/approved ]; do sleep 2; done

      # Your sensitive steps
      - name: Deploy to X (runs after approval)
        run: ./deploy.sh
```

This step outputs the precise command you need to run on your terminal (it will be a `docker` command) to allow it to continue, keeping things simple and effective. By adding this to your workflow, you can simulate manual gates—like approvals—locally, making your `act` tests more realistic. It’s a small tweak, but it gives you better control over your workflow’s flow when testing with `act`.
