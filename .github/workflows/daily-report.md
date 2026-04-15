---
on:
  schedule: daily
permissions:
      contents: read
      issues: read
      pull-requests: read
engine: copilot
network: defaults
tools:
  github:
    toolsets: [default]
safe-outputs:
  create-issue:
  close-issue:
  add-comment:
  update-issue:
---

# daily-report

Run daily at 9 AM and create an issue with a summary of repository activity from past 24 hours. Include commits, pull requests, issues, and CI/CD failures.


<!--
## TODO: Customize this workflow

The workflow has been generated based on your selections. Consider adding:

- [ ] More specific instructions for the AI
- [ ] Error handling requirements
- [ ] Output format specifications
- [ ] Integration with other workflows
- [ ] Testing and validation steps

## Configuration Summary

- **Trigger**: Daily schedule (fuzzy, scattered time)
- **AI Engine**: copilot
- **Tools**: github
- **Safe Outputs**: create-issue, close-issue, add-comment, update-issue
- **Network Access**: defaults

## Next Steps

1. Review and customize the workflow content above
2. Remove TODO sections when ready
3. Run `gh aw compile` to generate the GitHub Actions workflow
4. Test the workflow with a manual trigger or appropriate event
-->
