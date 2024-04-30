# SD 2023-05-09 "Migrating Dashboard to React"

## Current Situation

The current Jobbr.Dashboard has been written during the time where the frontend framework [Aurelia](https://aurelia.io/) was being hyped.
Unfortunately the framework has seen it's last update to resolve a dependency issue in 2022, but had essentially died somewhen in 2017/18.

Dependabot and other scans indicate vulnerability issues with the current `package.json` config, but updating certain packages leads to complete failure in building it.

## Target Situation

As Aurelia has read its end-of-life, there's no point in trying to fix build issues when updating to the latest package.
Instead it's more useful to migrate or rather rewrite the whole project in React.

The decision on React instead of Angular is a bit arbitrarily, but it seems like the more "modern" stack in a sense.

### Development

See the feature branch: [`feature/react-rewrite`](https://github.com/jobbrIO/jobbr-dashboard/tree/feature/react-rewrite)

Progress:

- ✅ Remove Aurelia
- ✅ Create a basic react app
- ✅ Copy the basic layout from the original Dashboard
- 🟨 Implement the Dashboard Landing page
- 🟨 Implement the Jobs page
- 🟨 Implement the JobRuns page
- 🟨 Implement the Job Details page
