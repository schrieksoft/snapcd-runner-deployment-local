# SnapCD Runner - Local Deployment

This guide explains how to download and run the SnapCD Runner binary on your local machine.

## Prerequisites

- Linux x64 operating system. If on Windows, use Windows Subsystem for Linux (WSL).
- Network access [snapcd.io](https://snapcd.io).
- Organization ID and Runner ID, as well as Service Principal credentials (Client ID and Client Secret) from [snapcd.io](https://snapcd.io).

## Download

Download the latest runner binary from GitHub releases:

```bash
VERSION=0.1.6 # select the version you want here
curl -L -o SnapCd.Runner https://github.com/schrieksoft/snapcd-runner/releases/download/$VERSION/SnapCd.Runner
chmod +x SnapCd.Runner
```

## Configuration

Create an `appsettings.json` file in the same directory as the binary:

```bash
cp appsettings.example.json appsettings.json
```

Edit `appsettings.json` with your configuration values:

| Setting | Description |
|---------|-------------|
| `Runner.Id` | The unique ID of the runner (GUID format, obtained from SnapCD server) |
| `Runner.Instance` | A friendly name for this runner instance |
| `Runner.Credentials.ClientId` | OAuth client ID for authentication |
| `Runner.Credentials.ClientSecret` | OAuth client secret for authentication |
| `Runner.OrganizationId` | The organization ID this runner belongs to |
| `Server.Url` | The URL of your SnapCD server |
| `WorkingDirectory.WorkingDirectory` | Directory where the runner stores working files |
| `WorkingDirectory.TempDirectory` | Directory for temporary files |
| `HooksPreapproval.Enabled` | Enable/disable hooks preapproval feature |
| `HooksPreapproval.PreapprovedHooksDirectory` | Directory containing preapproved hook scripts |

## Directory Structure

The runner uses the following directories by default:

- `~/.snapcd/runner` - Working directory for runner operations
- `~/.snapcd/runner/.temp` - Temporary files
- `~/.snapcd/preapproved-hooks` - Preapproved hook scripts (if enabled)

Make sure these directories exist and are writable:

```bash
mkdir -p ~/.snapcd/runner/.temp
mkdir -p ~/.snapcd/preapproved-hooks
```


## Running

Start the runner:

```bash
./SnapCd.Runner
```

The runner will connect to the configured SnapCD server and begin processing jobs.