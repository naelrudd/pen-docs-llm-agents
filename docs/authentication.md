# Authentication

## Sign in to pen.dev

Sign in with an email code or password when pen.dev prompts you.

In the desktop app, open Settings (⚙️) → Account to check the signed-in
account or select Sign Out. Use **Current workspace** to select a workspace
from your organizations. Your own workspace appears under **Personal**.

For the CLI, run `pen login` to sign in to pen.dev, then `pen status` to check
your session. See the [CLI reference](../reference/pen-cli.md) for
`PEN_CLI_KEY` authentication in scripts.

In the desktop model picker, models labeled **pen.dev Pro** use your pen.dev
account. Select **Upgrade**, when shown, to view plans.

## Connect an integrated AI provider

Choose a provider in Settings (⚙️) → Agents. The authentication method
depends on the provider.

### Authentication options

| Provider | Options |
|----------|---------|
| OpenAI ChatGPT | Existing Codex settings, OpenAI API key or ChatGPT Plus/Pro sign-in |
| Anthropic Claude Code | Existing Claude Code settings, Anthropic API key, Claude Pro/Max sign-in, AWS Bedrock, Google Vertex, Microsoft Foundry or a custom Claude model |
| Google Gemini | Google AI Studio API key |
| Cursor | Cursor Integrations API key |
| GitHub Copilot | GitHub sign-in, with an optional GitHub Enterprise domain |
| xAI, Open Router, Moonshot AI, Kimi For Coding, Deepseek, Together AI, Fireworks AI, Z.AI, OpenCode Zen and OpenCode Go | Provider API key |
| Custom provider | Optional API key for a custom endpoint using OpenAI Completions, OpenAI Responses or Anthropic Messages (desktop only) |

## Check a provider connection

1. Complete the provider-specific setup in Settings (⚙️) → Agents
2. Return to the provider list
3. Check for **Connected Automatically**, **Connected**, **Signed in**, **API key set** or **Configured**
4. Close Settings and choose a model from that provider in the agent composer

Follow [Create your first design](create-your-first-design.md) to check
that the agent can edit a document. To switch providers, choose a model from
another connected provider in the model picker.

AWS Bedrock, Google Vertex, Microsoft Foundry and custom Claude configurations
show **External config** because pen.dev does not test those connections.

To remove a saved API key, open its provider and select **Clear**.

## Troubleshooting

### The pen.dev verification code does not arrive

Select **Resend code** after the cooldown. If the account has a password, select
**I have a password**.

### A provider is not connected

Confirm that the selected authentication method matches the credential you
supplied. If the provider has a refresh control, use it after correcting the
credentials. Claude Code and Codex have a **Re-check connection** button.

For existing Claude Code or Codex settings, confirm that the corresponding CLI
is signed in. For an external Claude configuration, check the credentials and
configuration outside pen.dev.

### A model is missing

Refresh the provider status, then check the model picker again.