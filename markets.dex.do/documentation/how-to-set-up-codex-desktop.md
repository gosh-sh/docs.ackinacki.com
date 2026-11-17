# How to Set Up Codex Desktop

This guide shows you how to connect `gpt-6.1-sol` through DEX.DO to Codex in the ChatGPT desktop app. Windows and macOS use the same Codex configuration. Use the file path and command for your operating system.

### 1. Install the app and get access to the model

1. Download the [ChatGPT desktop app](https://chatgpt.com/download/) for your operating system, install it, and sign in to your account.
2. Sign in or sign up on [DEX.DO Markets](https://markets.dex.do/). Fund your **Funding Account** with SHELL, purchase access to `gpt-6.1-sol` in **Markets**, and wait for your order status to change to **Serving**.\
   [More details](how-to-buy-ai-inference-on-dex.do.md)
3. In DEX.DO Markets, open **Inference keys**, select **All current and future models**, and copy the API key.

### 2. Configure DEX.DO as a model provider

Open your user-level `config.toml` at the path for your operating system. Create the `.codex` folder and the file if needed.

| Operating system | File path                          |
| ---------------- | ---------------------------------- |
| Windows          | `%USERPROFILE%\.codex\config.toml` |
| macOS            | `~/.codex/config.toml`             |

Add the following settings to the file.

```
model = "gpt-6.1-sol"
model_provider = "dexdo"
model_reasoning_effort = "high"

[model_providers.dexdo]
name = "DEX.DO"
base_url = "https://buyerplane.dex.do/inference/v1"
env_key = "DEXDO_API_KEY"
wire_api = "responses"
supports_websockets = false
```

Place the first three settings at the top of the file, before the first section heading in square brackets. If any of these settings or the `[model_providers.dexdo]` section already exist, update them in place. Save the file.

These settings make DEX.DO the default provider. They also apply to Codex CLI if it uses the same file.

### 3. Set the API key

Quit ChatGPT completely. Then run the command for your operating system, replacing `YOUR_API_KEY` with the key you copied from **Inference keys**.

#### Windows

Open **PowerShell** and run the following command.

```
setx DEXDO_API_KEY "YOUR_API_KEY"
```

Windows saves the variable for your user account. New processes, including ChatGPT the next time you launch it, can read the variable.

#### macOS

Open **Terminal** and run the following command.

```
launchctl setenv DEXDO_API_KEY "YOUR_API_KEY"
```

The app can read the key during your current macOS session. After signing out or restarting your Mac, run the command again before launching ChatGPT.

### 4. Verify the connection

Open ChatGPT, go to **Codex**, and start a new local task. Send a short prompt such as “Reply with the word ready.”

<table data-header-hidden><thead><tr><th width="274"></th><th></th></tr></thead><tbody><tr><td>Issue</td><td>What to check</td></tr><tr><td>Codex reports a missing API key</td><td>Make sure the variable name <code>DEXDO_API_KEY</code> matches the <code>env_key</code> value in <code>config.toml</code>. Quit ChatGPT completely and reopen it. If the issue persists, close any remaining ChatGPT or Codex processes in <strong>Task Manager</strong> on Windows or <strong>Activity Monitor</strong> on macOS, then reopen ChatGPT.</td></tr><tr><td>Codex returns a <code>401</code> error</td><td>Verify that the variable contains a valid key from <strong>Inference keys</strong> on DEX.DO Markets. If you update the key, restart ChatGPT.</td></tr><tr><td>The model is unavailable</td><td>Verify that the model ID in <code>config.toml</code> is exactly <code>gpt-6.1-sol</code> and that the order for that model is in <strong>Serving</strong> status.</td></tr></tbody></table>
