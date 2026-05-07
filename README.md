# Snap CD Runner - Local Deployment

This guide explains how to download and run the Snap CD Runner binary on your local machine.

## Prerequisites

- Linux x64 operating system. If on Windows, use Windows Subsystem for Linux (WSL).
- Network access to your Snap CD Server. For SaaS this is [snapcd.io](https://snapcd.io); for a self-hosted Server it's whatever URL you've deployed it at.
- `unzip` available on your `$PATH`.
- Organization ID and Runner ID, as well as Service Principal credentials (Client ID and Client Secret), obtained from your Snap CD Server's Dashboard.

> The Runner is published as a **self-contained `linux-x64` build** — the .NET runtime is bundled. You do **not** need to install .NET on the host.

## Download

The Runner is published as a zip archive attached to GitHub Releases on the [schrieksoft/snapcd](https://github.com/schrieksoft/snapcd/releases) repository. Each release publishes two flavours:

| Asset | When to use |
|-------|-------------|
| `snapcd-runner-<version>.zip` | Standard build. Use this for most deployments. |
| `snapcd-runner-azure-<version>.zip` | Bundles the Azure CLI for hooks/providers that require `az`. Use only if your modules need it. |

Download and extract the latest archive:

```bash
VERSION=0.1.13   # pick the release you want from https://github.com/schrieksoft/snapcd/releases
curl -L -o snapcd-runner.zip \
  https://github.com/schrieksoft/snapcd/releases/download/$VERSION/snapcd-runner-$VERSION.zip
unzip snapcd-runner.zip -d snapcd-runner
chmod +x snapcd-runner/SnapCd.Runner
```

This leaves a `snapcd-runner/` directory containing the `SnapCd.Runner` executable plus its bundled runtime files.

## Configuration

Copy the example configuration into the extracted directory and edit it:

```bash
cp appsettings.example.json snapcd-runner/appsettings.json
```

Edit `snapcd-runner/appsettings.json` with your values:

| Setting | Description |
|---------|-------------|
| `Runner.Id` | The unique ID of the Runner (GUID, obtained from the Snap CD Server) |
| `Runner.Instance` | A friendly name for this Runner instance |
| `Runner.Credentials.ClientId` | OAuth client ID for the Runner's Service Principal |
| `Runner.Credentials.ClientSecret` | OAuth client secret for the Runner's Service Principal |
| `Runner.OrganizationId` | The organization ID this Runner belongs to |
| `Server.Url` | The URL of your Snap CD Server (`https://snapcd.io` for SaaS, or your self-hosted URL) |
| `WorkingDirectory.WorkingDirectory` | Directory where the Runner stores working files |
| `WorkingDirectory.TempDirectory` | Directory for temporary files |
| `HooksPreapproval.Enabled` | Enable/disable hooks preapproval feature |
| `HooksPreapproval.PreapprovedHooksDirectory` | Directory containing preapproved hook scripts |
| `Engine.AdditionalBinaryPaths` | Extra directories prepended to `$PATH` for hooks and engines (e.g. Pulumi) |

## Directory Structure

The Runner uses the following directories by default:

- `~/.snapcd/runner` - Working directory for runner operations
- `~/.snapcd/runner/.temp` - Temporary files
- `~/.snapcd/preapproved-hooks` - Preapproved hook scripts (if enabled)

Make sure these directories exist and are writable:

```bash
mkdir -p ~/.snapcd/runner/.temp
mkdir -p ~/.snapcd/preapproved-hooks
```

## Running

Start the Runner from the extracted directory:

```bash
cd snapcd-runner
./SnapCd.Runner
```

The Runner will connect to the configured Snap CD Server and begin processing jobs.

## Upgrading

To upgrade to a newer release, download and extract a new archive into a fresh directory, copy your existing `appsettings.json` over, and restart. The Runner is stateless beyond its `WorkingDirectory`, so swapping binaries is safe.
